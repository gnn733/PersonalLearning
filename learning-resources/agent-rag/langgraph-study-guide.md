# LangGraph 怎么学：从普通函数到 Agent 流程

更新日期：2026-10-15。

[返回 Agent 分类](README.md) · [完整后续路线](after-rag-roadmap.md) · [具体教材入口](advanced-resources.md)

这份说明面向已有 Python 与基础 RAG 经验、暂时不熟悉后端术语的学习者。重点是每个 API 在流程里承担什么职责。

## 1. 先分清三个工具的作用

| 工具或方法 | 在你的项目里负责什么 |
| --- | --- |
| RAG | 从外部资料找依据，再用于回答 |
| LangChain create_agent | 帮你组织模型与工具之间的循环 |
| LangGraph | 明确定义步骤、分支、循环和状态，控制执行流程 |
| FastAPI | 接收 HTTP 请求，让外部程序能调用你的功能 |

LangChain 的 create_agent 建立在 LangGraph 之上。学习 LangGraph 是为了理解和定制流程，基础聊天或工具循环仍可以使用 create_agent。

## 2. 前置 Python 只补这些

- 函数作为参数：add_node("retrieve", retrieve) 传入的是函数本身；retrieve(...) 才是执行函数。
- 字典：state["question"] 取出字段，节点返回一个更新字典。
- 类型注解：TypedDict 描述字典字段；它本身不会在运行时自动验证所有输入。
- Annotated：给某个字段附加元信息，LangGraph 可据此选择 reducer。
- 列表与函数返回值。
- 基础生成器：stream 返回的数据需要迭代读取。
- async/await 可以在异步与服务化阶段补。

## 3. State、Node、Edge，用普通函数理解

设想流程是“读取问题 → 检索 → 根据资料作答”。

- State：沿途携带的数据，例如 question、docs、answer。
- Node：处理数据的函数，例如 retrieve、compose。
- Edge：步骤之间的连接，例如 retrieve 后去 compose。

各次图调用有自己的执行状态；这不等于所有用户共享一个全局字典。跨次调用的延续取决于 checkpoint、thread_id 和调用方式。

### 最小练习：不调用模型、不连接数据库

以下是原创教学示例。依赖 LangGraph，检索与回答用固定逻辑模拟；它用于理解图执行，不是真实 RAG。

~~~python
from typing import TypedDict

from langgraph.graph import END, START, StateGraph


class QAState(TypedDict):
    question: str
    docs: list[str]
    answer: str


def retrieve(state: QAState):
    print("检索问题：", state["question"])
    return {"docs": ["来源：事务笔记.md；事务包含提交与回滚。"]}


def compose(state: QAState):
    return {"answer": "找到的资料：" + state["docs"][0]}


builder = StateGraph(QAState)
builder.add_node("retrieve", retrieve)
builder.add_node("compose", compose)
builder.add_edge(START, "retrieve")
builder.add_edge("retrieve", "compose")
builder.add_edge("compose", END)

graph = builder.compile()
result = graph.invoke({
    "question": "事务是什么？",
    "docs": [],
    "answer": "",
})
print(result["answer"])
~~~

### 按调用顺序解释

1. StateGraph(QAState)：声明这个图有哪些状态字段。
2. add_node：登记节点名字和对应函数，此时还没有执行检索。
3. add_edge：登记执行顺序。
4. compile：检查和构建可执行图，此时还没有回答用户问题。
5. invoke：传入本次初始数据，开始运行图。
6. retrieve：读取 question，返回 docs 的更新。
7. LangGraph 合并更新，compose 读取最新 docs。
8. 到达 END，本次调用结束，返回最终状态。

compose 节点本质上仍是普通 Python 函数。框架负责按照图的定义调用它，并处理状态更新。

### 第一轮验收

- 在 compose 前增加一个打印资料数量的节点。
- 将固定资料替换为已有 retrieve(query) 的结果。
- 故意改错一条边，观察结构或执行错误。
- 画出顺序并解释 question 为什么仍保留在后续状态中。

参考跟练：[Academy simple-graph](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/simple-graph.ipynb)。

## 4. 节点返回“更新”，reducer 决定怎样合并

例子中的 retrieve 只返回 docs。没有更新的 question 会保留。

普通字段通常用新值覆盖旧值。假设 docs 当前有两条，返回 {"docs": ["新资料"]} 后，docs 变成一条新资料。

如果某字段需要追加或其他合并规则，可以为它配置 reducer。Reducer 可以理解成一个接收“旧值”和“本次更新”、返回合并结果的函数。

### 消息列表特别重要

MessagesState 为 messages 提供 add_messages 合并规则。它按消息 ID 处理追加或更新，不只是无条件拼接列表。

因此，模型节点一般只返回本次的新消息：

~~~python
def model_node(state):
    reply = model_with_tools.invoke(state["messages"])
    return {"messages": [reply]}
~~~

这里假定 model_with_tools 已配置；这是节点片段，不是独立可运行脚本。

如果已使用消息 reducer，又把整个旧历史当成新消息返回，可能产生重复或意外合并。重复行为与消息 ID 有关，应检查实际消息。

### 第二轮验收

- 写两个节点分别更新同一普通字段，观察后值覆盖前值。
- 再给一个列表配置追加 reducer，比较结果。
- 查看 messages 每一轮新增了什么。
- 并行节点写同一字段时，明确选择合并规则；并行状态更新不能假定自动拼接。

参考：[state-schema](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/state-schema.ipynb)、[state-reducers](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/state-reducers.ipynb)。

## 5. 条件边：相当于为流程增加 if/else

固定边：retrieve 完成后总去 compose。

条件边：检查状态，再决定去哪个节点。可以先用普通规则实现：

~~~python
def choose_next(state):
    return "compose" if state["docs"] else "no_evidence"
~~~

将它通过 add_conditional_edges 接在 retrieve 后面。路由结果需对应实际节点或明确的映射。分支节点完成后也需要有适当的结束或后续连接。

如果同一节点同时配置普通出边和条件出边，两者都可能调度后续节点。初学时先为一个分支出口选一种清晰方式。

### 第三轮验收

让“有资料”进入回答节点，“无资料”进入证据不足节点。覆盖两个分支。之后再增加“改写问题 → 重新检索”的回路，并用计数字段限制次数。

## 6. 工具循环：把 ReAct 流程拆开

### 几个对象分别负责什么

| 对象或方法 | 职责 |
| --- | --- |
| model.bind_tools | 告诉模型有哪些工具与参数，让模型能够返回调用请求 |
| AIMessage.tool_calls | 模型提出的工具名、参数与调用 ID |
| ToolNode | 根据工具调用请求执行注册工具，生成对应结果消息 |
| ToolMessage | 把执行结果交回模型，与 tool_call_id 对应 |
| tools_condition | 检查消息是否包含工具请求，并决定走工具节点还是结束 |
| MessagesState | 保存并合并流程里的消息 |

模型调用请求与执行工具是两个步骤。工具拿到的是实际问题字符串等参数，注意检查没有误传 Python 的 str 类型对象。

### 手写一次图，理解循环

~~~text
START → model
          ├─ 有 tool_calls → tools → model
          └─ 无 tool_calls → END
~~~

练习顺序：

1. 先用加法等简单本地工具。
2. 查看 AIMessage 和 ToolMessage 的对应关系。
3. 把加法工具替换为自己的知识检索工具。
4. 限制运行步数，并记录每次实际工具调用。
5. 比较这张图和 create_agent 的职责。

参考：[Academy agent.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/agent.ipynb)、[LangGraph 101 基础练习](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/101/101_langchain_langgraph.ipynb)。

## 7. Checkpointer、thread_id、Store、Runtime 分别是什么

| 概念 | 初学时怎么理解 | 例子 |
| --- | --- | --- |
| State | 流程当前处理中的数据 | 问题、消息、检索结果、重试次数 |
| Checkpointer | 在运行过程中保存状态检查点 | 继续会话、暂停后恢复 |
| thread_id | 查找一条执行线程及其检查点的标识 | conversation-001 |
| Store | 保存跨线程共享的数据 | 某用户偏好简短回答 |
| Runtime context | 本次运行提供给节点的依赖和上下文 | 已验证的用户身份、服务配置 |
| config | 图运行相关配置 | configurable.thread_id、recursion_limit |

thread_id 本身不证明用户身份。服务端还需要验证当前用户是否能访问该会话。

进程内 InMemorySaver 适合学习。关闭进程后数据不会自动保留；跨进程恢复需要相应持久化后端和一致的运行配置。

### 什么时候回头学 Runtime

当你的节点需要“当前用户是谁”，可以在运行图时传入 context，通过 Runtime.context 读取。State 随步骤更新；context 用于传运行依赖和上下文。类型注解不会自动完成用户认证。

不必在第一个两节点图里同时加入 Runtime、Store 和数据库。先理解数据从哪里传来，再增加对象。

### 第四轮验收

- 相同 thread_id 连续两轮对话，检查历史延续。
- 换一个 thread_id，检查历史隔离。
- 对照使用的存储后端，观察重启行为。
- 给节点传入一个用户标识，并解释它由谁验证。

## 8. Streaming：先看到进展，再做网页流式输出

常见学习方式：

- stream_mode="updates"：观察节点返回的状态更新。
- stream_mode="values"：观察合并后的状态快照。
- stream_mode="messages"：观察支持流式的模型输出片段与相关元数据。
- ainvoke/astream：在异步调用环境中使用；节点和客户端也要配合正确的异步用法。

第一轮可以在终端迭代 graph.stream 并打印。节点更新与模型 token 输出是不同层次。

网页的 SSE 是 HTTP 传输方式，还需要后端把图产生的事件转换成响应。使用 LangGraph 的 stream 不会自动建立网页接口。向页面输出时选择合适字段，不直接暴露整个内部状态。

## 9. 人工确认：暂停后怎样继续

学习两个核心动作：

- interrupt(payload)：告诉调用方需要外部输入，暂停图。
- Command(resume=answer)：把确认或修改结果送回同一线程。

它需要 checkpointer，并在恢复时使用相同 thread_id。

关键点：恢复时会从被中断节点的开头重新执行；interrupt 前的代码可能再次运行。外部写入等副作用需要设计幂等性或拆分节点。暂停并不等于自动回滚外部系统。

第一次练习可以生成一段总结 → 等待确认 → 根据确认结果打印或结束。

### 旧课怎么处理

Academy module-3 的 dynamic-breakpoints.ipynb 在本次检查时仍使用 NodeInterrupt。该课适合理解旧机制，但这轮人工确认实践优先跟 [LangGraph 101 的 102_middleware.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/101/102_middleware.ipynb)，其中能看到 interrupt 与 Command(resume=...)。

## 10. 子图、并行与多 Agent 放在后面

先完成前面的小项目，再学习：

- 子图：把可复用的流程封装起来。
- 并行：相互独立的节点同时处理，并明确结果合并。
- Send：按输入动态分发多个任务。
- Command：在适当位置组合状态更新与流程控制。
- 多 Agent：为不同职责设计上下文与交接，并限制总调用预算。

LangGraph 可以只调用普通函数或单个模型。增加多个 Agent 应当有具体收益，并与简单方案做对比。

## 11. 推荐的跟练顺序

1. Academy module-1：simple-graph → chain → router。
2. Academy module-2：state-schema → state-reducers。
3. 回到 module-1：agent → agent-memory。
4. LangGraph 101：101 基础练习，比较 create_agent 和手写图。
5. LangGraph 101：102_middleware，练习暂停、批准与拒绝。
6. Academy module-2：消息裁剪和摘要。
7. Academy module-4：子图和并行。
8. module-5 长期记忆、module-6 部署，根据项目需要选学。

Academy 的教学模块顺序和你当前项目的需要不同，这里的穿插顺序用于减少一次接触的概念数量。

## 12. 一轮学习的验收清单

- [ ] 能从空文件写两节点图。
- [ ] 能解释 compile 与 invoke 的区别。
- [ ] 能增加一个条件分支。
- [ ] 能解释普通字段覆盖与消息 reducer。
- [ ] 能实现并解释一个工具循环。
- [ ] 能设置正常退出条件和执行上限。
- [ ] 能用 thread_id 延续和隔离会话。
- [ ] 能观察节点更新与模型流式输出。
- [ ] 能暂停、批准或拒绝，并解释恢复时的重新执行。
- [ ] 能画出自己的 Agentic RAG 流程并定位一例失败。

## 查错依据

主学习材料采用 Notebook 与教程。下列页面只用于核对概念和查 API：

- [Graph API：状态、节点、边与 reducer](https://docs.langchain.com/oss/python/langgraph/graph-api)
- [记忆与持久化](https://docs.langchain.com/oss/python/langgraph/add-memory)
- [Interrupt：暂停与恢复](https://docs.langchain.com/oss/python/langgraph/interrupts)

本文的教学代码做了语法检查，未在本机安装 LangGraph 运行验证。真实模型、数据库、流式和恢复行为，需要在对应项目环境中验证。
