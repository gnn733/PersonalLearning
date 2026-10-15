# Agent 与 RAG

[返回分类导航](../README.md)

当前目标：把模型调用、工具调用与向量检索连接起来，做一个能回答自己学习资料问题的终端项目。

## 1. Hello-Agents：当前优先

- 仓库：[datawhalechina/hello-agents](https://github.com/datawhalechina/hello-agents)
- 内容：Agent 基础、ReAct 等范式、框架实践、记忆与检索、上下文工程、评估、综合案例。
- 提供中文正文与配套代码，面向有 Python 基础的学习者。
- 适合度判断：适合解决“API 学了很多，但不理解整体流程”的问题。

### 建议阅读顺序

| 顺序 | 入口 | 这一轮要解决的问题 |
| --- | --- | --- |
| 1 | [第一章：初识智能体](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter1) | Agent 由哪些部分组成？ |
| 2 | [第四章：智能体经典范式构建](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter4) | 模型怎样决定调用工具？结果怎样送回模型？重点看 ReAct |
| 3 | [第八章：记忆与检索](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter8) | 历史消息、记忆和知识检索有什么用途？ |
| 4 | [第九章：上下文工程](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter9) | 应该把哪些信息交给模型？ |
| 5 | [第十二章：智能体性能评估](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter12) | 怎样发现和比较项目效果的问题？ |

第一轮先完成前三项和自己的项目。框架源码、多 Agent、训练等部分可留到后续。

### 重要理解

```text
用户提问
  → 模型返回工具调用请求
  → 程序执行工具
  → 程序把工具结果交回模型
  → 模型回答，或再请求调用工具
```

模型提出调用请求，程序负责实际执行。调试时观察工具名称、参数、返回内容和最终答案即可；无需要求模型展示内部思考。

## 2. LLM Universe：参考知识库构建流程

- 仓库：[datawhalechina/llm-universe](https://github.com/datawhalechina/llm-universe)
- 内容：模型 API、文档加载与处理、向量库、RAG、Streamlit 展示及验证迭代。
- 建议用途：对照已有代码检查“加载 → 切分 → 向量化 → 入库 → 检索 → 回答”的完整流程。

### 版本注意

整理时的 [requirements.txt](https://github.com/datawhalechina/llm-universe/blob/main/requirements.txt) 固定了 LangChain 0.3.0。此前提供的学习代码使用 LangChain 1.2.x，二者的 API 和导入路径可能不同。

- 原样复现：单独创建环境，使用教程依赖。
- 改进自己的项目：保留已有版本，参考流程后改写相应 API。
- 教程涉及的模型名称与服务配置，运行前需要核对自己的账号是否支持。

## 3. RAG From Scratch：做通后再补充

- 仓库：[langchain-ai/rag-from-scratch](https://github.com/langchain-ai/rag-from-scratch)
- 形式：英文 Notebook 与配套视频，从索引、检索、生成展开。
- 第一份练习：[rag_from_scratch_1_to_4.ipynb](https://github.com/langchain-ai/rag-from-scratch/blob/main/rag_from_scratch_1_to_4.ipynb)
- 最近推送快照为 2025-06，遇到版本差异时按项目环境调整。
- 建议用途：自己的基础 RAG 做通后，带着检索效果问题阅读。

## 现阶段需要掌握的最小范围

- 模型调用与消息格式。
- Python 函数、类型注解和工具描述。
- 工具调用请求与执行结果之间的对应关系。
- 文档加载、切分、Embedding、向量检索。
- 把检索函数封装成工具，注册给 Agent。
- 记录来源、处理无匹配结果、限制运行步数。

Runtime 和中间件可以结合需求学习：传用户信息时学习运行上下文；记录调用耗时、处理重试时学习中间件。

## 下一步

[按项目逐步练习](../projects/README.md) · [学习顺序](../study-plan.md)
