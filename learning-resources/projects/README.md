# 实战项目

[返回分类导航](../README.md)

优先练习规模可控、能自己修改并解释清楚的项目。阅读开源项目时，从一个请求或一个功能开始跟踪。

## 一、现在动手：学习资料 ReAct RAG 助手

### 参考资料

- [Hello-Agents](https://github.com/datawhalechina/hello-agents)：理解模型与工具循环。
- [LLM Universe](https://github.com/datawhalechina/llm-universe)：参考知识库处理流程。
- 自己已经写过的检索与模型调用代码。

### 第一版功能

1. 准备 5～10 份自己的 Markdown 或文本笔记。
2. 加载、切分、向量化和入库；可以沿用已有的 Milvus。
3. 单独测试 retrieve(query)，检查文本和来源是否合理。
4. 将检索封装成一个工具，注册给 Agent。
5. 在终端输入问题，输出答案和资料来源。
6. 记录工具调用参数与结果，处理检索无匹配的情况。
7. 设置运行步数或工具调用上限。

### 完成标准

- 知识库内的问题能检索到相关证据并据此作答。
- 引用来源能对上实际片段。
- 证据不足时能明确说明。
- 能解释一次请求里模型、工具和向量库分别做了什么。
- 用一组固定问题检查正确回答、无答案、检索失败等情况。

### 逐步扩展

终端版完成后，再加 FastAPI 接口、会话历史、流式输出和部署。框架异步用法应与具体客户端能力配合使用，避免把同步阻塞调用直接放进异步处理函数。

## 二、JavaWeb / Spring Boot 入门：学习笔记管理系统

### 第一版功能

- 新增、编辑、删除学习笔记。
- 按关键词查询与分页查询。
- MySQL 保存数据。
- 接收和返回 JSON。
- 简单页面或接口工具调用。

### 后续扩展

- 参数校验与统一异常处理。
- 登录和会话归属检查。
- 请求日志。
- 基础索引与查询分析。
- Linux 或 Docker 部署。

目标：能从一个请求追踪到 Controller、Service、数据库，再看到返回结果。

## 三、Spring Boot 与 MyBatis 入门后：mall-learning 分模块练习

- 仓库：[macrozheng/mall-learning](https://github.com/macrozheng/mall-learning)
- 教程：[macrozheng.com](https://www.macrozheng.com/)
- 可参考入口：[mall-tiny-boot](https://github.com/macrozheng/mall-learning/tree/master/mall-tiny-boot)、[mall-tiny-mybatis](https://github.com/macrozheng/mall-learning/tree/master/mall-tiny-mybatis)、[mall-tiny-redis](https://github.com/macrozheng/mall-learning/tree/master/mall-tiny-redis)。
- 建议：先理解 Spring Boot 与 MyBatis 示例的一个接口，再根据所学加入 Redis。

### 版本与维护注意

[父 pom.xml](https://github.com/macrozheng/mall-learning/blob/master/pom.xml) 使用 Spring Boot 2.7.5 与 Java 8 配置；仓库最近推送快照为 2024-09。

按教程复现时对齐依赖，迁移到课程使用的其他版本时检查差异。这里的推荐用途是分模块学习，完整商城涉及的组件较多，留到基础项目之后。

## 四、基础增删改查后：Spring Petclinic

- 仓库：[spring-projects/spring-petclinic](https://github.com/spring-projects/spring-petclinic)
- 内容：宠物诊所管理的 Spring Boot 示例。
- 默认使用 H2 内存数据库，也提供 MySQL/PostgreSQL 配置。
- 整理时的 [pom.xml](https://github.com/spring-projects/spring-petclinic/blob/main/pom.xml) 为 Spring Boot 4.1.0、Java 17 配置。实际运行请同时核对 README 与构建文件要求。
- 使用 Spring Data JPA，与 MyBatis 的数据访问方式有区别。
- 建议用途：理解现成应用怎样组织功能、校验输入和访问数据库。

### 练习方法

1. 先运行项目，选择一个查询或编辑功能。
2. 从页面追踪到处理请求的方法。
3. 找到数据访问代码与实体。
4. 修改一个字段或增加简单查询条件。
5. 检查修改后原有功能是否正常。

## 五、Spring Boot 熟悉后：Java + Agent

- 仓库：[alibaba/spring-ai-alibaba](https://github.com/alibaba/spring-ai-alibaba)
- 定位：Java Agent 框架，包含工具调用、工作流、状态持久化和流式能力等。
- 从仓库 README 的 Examples 入口寻找简单示例。
- 建议第一步：运行一个简单模型调用，再增加一个工具，之后尝试自己的知识检索工具。

先通过 Python 项目理解 Agent 循环，再尝试用 Java 实现相同的小功能，有助于比较两个技术栈的实现方式。

## 阅读与练习提醒

本导航提供上游项目链接和学习建议，没有复制其源码。使用、修改或发布上游内容时，应查看对应仓库的许可证和说明。这里列出的项目未逐个安装运行。
