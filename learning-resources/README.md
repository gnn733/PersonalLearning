# Java + Agent 学习资料导航

整理日期：**2026-10-15**。

适用基础：JavaSE 已接触，正在开始 JavaWeb；MySQL 学过基础、多表查询和事务；Python 有基础，学过 LangChain，准备做简单的 ReAct RAG 项目；每周约 30 小时学习。

## 分类入口

| 分类 | 内容 |
| --- | --- |
| [Java 基础与后端](java/README.md) | 复习资料、理解型教程、当前应学的知识范围 |
| [Agent 与 RAG](agent-rag/README.md) | ReAct 原理、知识库流程、版本注意事项 |
| [RAG 后续与 Agent 进阶](agent-rag/after-rag-roadmap.md) | 从基础 RAG 到工程化、MCP 与多 Agent |
| [LangGraph 分步学习](agent-rag/langgraph-study-guide.md) | 状态、节点、路由、工具循环、持久化与人工确认 |
| [新增教程与项目](agent-rag/advanced-resources.md) | Academy、LangGraph 101、All-in-RAG 与维护状态 |
| [实战项目](projects/README.md) | Java 项目、RAG 项目、后续 Java Agent 示例 |
| [建议学习顺序](study-plan.md) | 阶段目标、完成标准、每周时间分配 |

## 现在优先使用

1. **JavaGuide**：对照当前课程复习 Java 与 MySQL。
2. **Hello-Agents 第四章**：理解模型为什么调用工具，以及工具结果怎样返回。
3. **自己的 ReAct RAG 项目**：把已有检索函数封装成工具，接进 Agent。

其他资料按具体问题查阅，先集中完成一个 Java 小项目和一个 Agent 小项目。

## 仓库总表

Star 与推送月份是整理时查询的快照，可能变化。最近推送仅表示仓库有代码或文档推送，不代表每个示例都同步更新。适合阶段是针对上述基础的学习建议。

| 仓库 | 类型 | Star 约数 | 最近推送月份 | 建议阶段 |
| --- | --- | ---: | --- | --- |
| [JavaGuide](https://github.com/Snailclimb/JavaGuide) | 中文复习与面试资料 | 15.9 万 | 2026-10 | 现在 |
| [二哥的 Java 进阶之路](https://github.com/itwanger/toBeBetterJavaer) | 中文知识讲解 | 1.77 万 | 2026-10 | 现在按需查 |
| [Hello-Agents](https://github.com/datawhalechina/hello-agents) | 中文 Agent 教程与配套代码 | 8.32 万 | 2026-10 | 现在重点看 |
| [LLM Universe](https://github.com/datawhalechina/llm-universe) | 中文 RAG 教程与代码 | 1.41 万 | 2026-08 | 做基础 RAG 时参考 |
| [mall-learning](https://github.com/macrozheng/mall-learning) | 中文后端教程与分模块练习 | 1.34 万 | 2024-09 | Spring Boot、MyBatis 入门后 |
| [Spring Petclinic](https://github.com/spring-projects/spring-petclinic) | Spring Boot 完整示例 | 9600 | 2026-09 | 基础增删改查完成后 |
| [RAG From Scratch](https://github.com/langchain-ai/rag-from-scratch) | 英文 RAG Notebook 与视频 | 9500 | 2025-06 | 基础 RAG 做通后 |
| [Spring AI Alibaba](https://github.com/alibaba/spring-ai-alibaba) | Java Agent 框架与示例 | 1.1 万 | 2026-10 | Spring Boot 熟悉后 |

## 如何使用这份导航

- Star、Fork、社区贡献和维护记录是参考信号；内容仍需结合自己的项目验证。
- 本次核对了仓库目录、部分依赖文件和推送记录，没有逐个安装运行。
- 这是资源链接与原创学习建议，不包含对上游教程正文或项目源码的镜像。
- 开源部分足够开始；部分作者页面另有付费内容，按学习需要自行判断。
- 实际运行模型示例时，API 调用可能产生费用。先检查示例配置与自己的账号额度。

[返回仓库首页](../README.md)
