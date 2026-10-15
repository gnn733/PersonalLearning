# RAG 后续与 LangGraph：教材、代码和项目

检索整理日期：2026-10-15。

[返回 Agent 分类](README.md) · [先看学习路线](after-rag-roadmap.md) · [LangGraph 分步说明](langgraph-study-guide.md)

以下 Star、推送月份和归档状态来自整理时的 GitHub 查询。热度与维护是参考信号，是否适合当前学习阶段还要看前置知识和示例复杂度。此次检查了 README、目录、部分 Notebook 和依赖文件，没有逐个运行项目。

## 总表

| 资源 | Star 约数 | 最近推送月份 | 状态 | 用途 |
| --- | ---: | --- | --- | --- |
| [All-in-RAG](https://github.com/datawhalechina/all-in-rag) | 1.21 万 | 2026-09 | 未归档 | 中文 RAG 进阶与评估 |
| [LangChain Academy](https://github.com/langchain-ai/langchain-academy) | 2950 | 2026-06 | 未归档 | LangGraph 系统 Notebook 跟练 |
| [LangGraph 101](https://github.com/langchain-ai/langgraph-101) | 686 | 2026-10 | 未归档 | 当前 Agent API、中间件与人工确认示例 |
| [Agent Chat UI](https://github.com/langchain-ai/agent-chat-ui) | 3220 | 2026-10 | 未归档 | 给 LangGraph 服务接现成聊天界面 |
| [Ragas](https://github.com/vibrantlabsai/ragas) | 1.60 万 | 2026-02 | 未归档 | RAG 与 LLM 应用评估工具 |
| [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) | 2.46 万 | 2026-10 | 未归档 | 后续学习工具服务与客户端 |
| [Deep Agents](https://github.com/langchain-ai/deepagents) | 3.03 万 | 2026-10 | 未归档 | 后期学习规划、上下文与委派 |
| [Open Deep Research](https://github.com/langchain-ai/open_deep_research) | 1.27 万 | 2026-08 | 已归档 | 后期研究型 Agent 源码参考 |

最近推送日期不保证每个 Notebook 的 API 已更新。LangGraph 101 热度相对较低，但由 LangChain 团队维护，部分示例更贴合当前用法，作为 Academy 的补充。

## 1. All-in-RAG：补检索效果与评估

中文教程，提供代码，覆盖分块、索引、混合检索、查询重构、生成和评估。

优先入口：

- [文本分块](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter2/05_text_chunking.md)
- [混合检索](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter4/11_hybrid_search.md)
- [查询改写](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter4/14_query_rewriting.md)
- [评估介绍](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter6/18_system_evaluation.md)
- [评估工具](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter6/19_common_tools.md)

练习：选一个检索失败问题，对比两种分块或检索方法。多模态与知识图谱章节可留到有需要时。

## 2. LangChain Academy：LangGraph 主教材

- [课程仓库](https://github.com/langchain-ai/langchain-academy)
- [配套视频课程](https://academy.langchain.com/courses/intro-to-langgraph)
- 形式：英文视频、Notebook、Studio 示例。
- 各 Notebook 顶部有课程入口；可以先阅读和运行代码，再看对应视频。

### 按你当前基础选择文件

| 顺序 | 入口 | 重点 |
| --- | --- | --- |
| 1 | [simple-graph.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/simple-graph.ipynb) | 普通函数、State、Node、Edge |
| 2 | [chain.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/chain.ipynb) | 串联模型与步骤 |
| 3 | [router.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/router.ipynb) | 条件路由 |
| 4 | [state-schema.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/state-schema.ipynb) | 状态定义 |
| 5 | [state-reducers.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/state-reducers.ipynb) | 更新与合并 |
| 6 | [agent.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/agent.ipynb) | 模型与工具循环 |
| 7 | [agent-memory.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-1/agent-memory.ipynb) | Checkpointer 与会话 |
| 8 | [trim-filter-messages.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/trim-filter-messages.ipynb) | 历史消息处理 |
| 9 | [chatbot-summarization.ipynb](https://github.com/langchain-ai/langchain-academy/blob/main/module-2/chatbot-summarization.ipynb) | 摘要与上下文 |
| 10 | [module-4](https://github.com/langchain-ai/langchain-academy/tree/main/module-4) | 子图、并行、研究助手 |

### 已发现的版本问题

整理时 requirements.txt 中 langgraph 未固定版本，langchain-core 下限为 1.2.28，高于你之前代码里的 1.2.18；安装整份依赖会改变版本组合。请单独建环境并记录实际安装版本。

module-3 的 dynamic-breakpoints.ipynb 仍能看到 NodeInterrupt。人工确认练习优先使用下一项 LangGraph 101 的新示例。

这些示例预设了一些模型与搜索服务。替换服务时核对模型是否支持示例需要的工具调用、结构化输出或流式能力。

## 3. LangGraph 101：跟进当前写法

- [仓库](https://github.com/langchain-ai/langgraph-101)
- [101_langchain_langgraph.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/101/101_langchain_langgraph.ipynb)：模型、工具、create_agent、手写图、记忆与流式。
- [102_middleware.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/101/102_middleware.ipynb)：中间件、interrupt、Command(resume=...)。
- [email_agent.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/201/email_agent.ipynb)：状态化分流与人工确认思路。
- [multi_agent.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/201/multi_agent.ipynb)：多 Agent，后期选学。
- [research_agent.ipynb](https://github.com/langchain-ai/langgraph-101/blob/main/notebooks/201/research_agent.ipynb)：研究任务拆分，后期选学。

### 使用提醒

这是一套压缩工作坊材料。先用 Academy 理解 State 与图，再看这些较综合的代码。

整理时 pyproject.toml 要求 Python >=3.11、langchain >=1.3.10、langgraph >=1.0.1 等；与原来的 LangChain 1.2.x 环境不同。整个依赖集合还含多个模型厂商客户端，按仓库说明单独建立环境。

Notebook 从 utils 导入模型配置，第一次运行先检查 utils/models.py 和示例实际会调用的工具。练习人工确认时可改成自己的本地打印任务。

## 4. 中文辅助：Hello-Agents 第六章

- [框架开发实践](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter6)
- 适合在英文 Notebook 中遇到抽象概念时补理解。
- 该章同时介绍其他框架，按目录查找 LangGraph 部分即可。

中文说明、Academy 基础练习和 LangGraph 101 的较新用法可以配合使用。

## 5. Agent Chat UI：后期节省页面开发时间

- [仓库](https://github.com/langchain-ai/agent-chat-ui)
- 是现成 Next.js 聊天界面，可连接兼容的 LangGraph 服务，并要求相应消息状态与服务配置。
- 适合：图与服务端已经能运行，希望展示聊天、工具调用和运行进展。

它不能直接连接任意自定义 FastAPI /chat JSON 接口。若采用自定义接口，可以先写简单页面；若使用此 UI，需对接它期望的 LangGraph 服务协议。

先使用原界面完成接入，按需改样式。部署时按项目说明处理服务端密钥和访问控制。

## 6. Ragas：先有测试问题，再学评估工具

- [仓库](https://github.com/vibrantlabsai/ragas)
- 提供评估指标与测试数据生成等能力。
- 先理解自己的问题、参考资料、回答和预期结果，再选择对应指标。
- 第一轮只评估一组小型固定案例，保留人工核验和改进前后结果。
- SDK 不同版本的接口有差异，按安装版本选择示例；模型评分也会消耗模型调用。

## 7. MCP Python SDK：工具服务阶段再学

- [仓库](https://github.com/modelcontextprotocol/python-sdk)
- [当前 README 引用的最小示例](https://github.com/modelcontextprotocol/python-sdk/blob/main/docs_src/index/tutorial001.py)
- 建议顺序：本地工具函数 → MCP server 暴露工具 → client 调用 → Agent 使用工具。
- 学习 host/client/server、工具发现、参数与结果、stdio 和 Streamable HTTP 的基本用途。
- 整理时主分支示例使用 MCPServer，文档提供 v2 迁移信息。旧课可能使用不同类名；不要混用不同版本示例。

### 旧适配器的维护状态

[langchain-mcp-adapters](https://github.com/langchain-ai/langchain-mcp-adapters) 整理时已归档，README 提示 MCP 支持转入 [langchain.mcp 源码目录](https://github.com/langchain-ai/langchain/tree/master/libs/langchain_v1/langchain/mcp)。

原来的 LangChain 1.2.x 环境未必包含新命名空间。原课复现可使用对应旧版本，新项目接入时先确认当前版本与迁移说明，不直接在旧环境里替换导入。

## 8. Deep Agents：规划与上下文进阶

- [仓库](https://github.com/langchain-ai/deepagents)
- [示例入口](https://github.com/langchain-ai/deepagents/tree/main/examples)
- 内容涉及规划、文件后端、上下文管理、人工确认与子 Agent 委派。
- 适合：已能解释 LangGraph 工具循环与状态，希望处理较长的研究任务。

先选一个示例，画出输入、任务分配、工具调用和输出关系，再替换成自己的学习资料任务。能运行只是第一步，还要检查预算与失败路径。

## 9. Open Deep Research：归档源码阅读

- [仓库](https://github.com/langchain-ai/open_deep_research)
- 整理时 archived=true，适合作为历史项目阅读参考。
- [state.py](https://github.com/langchain-ai/open_deep_research/blob/main/src/open_deep_research/state.py)：状态和结构化数据定义。
- 后续再看配置、任务分配、研究与汇总流程。
- 不作为当前第一份复现项目；默认模型与搜索设置需要另行核对。

## 环境与验证范围

- 每个上游项目使用独立环境，保留自己的旧项目环境。
- 示例涉及的模型、搜索和平台服务，按所需功能逐项配置。
- .env 和真实 API key 不提交到 GitHub。
- 本导航只保存链接、版本观察与原创学习建议，没有复制上游教材正文。
- 主教材是教程与 Notebook；官方 API 页面只在学习说明的查错依据中提供。
