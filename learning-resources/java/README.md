# Java 基础与后端

[返回分类导航](../README.md)

当前目标：理解一次请求如何经过 Java 后端、数据库，再返回给页面，并能实现基本增删改查。

## 1. JavaGuide：复习主资料

- 仓库：[Snailclimb/JavaGuide](https://github.com/Snailclimb/JavaGuide)
- 在线阅读：[javaguide.cn](https://javaguide.cn/)
- 定位：Java 与通用后端知识点、常见面试问答，适合按章节复习。
- 适合度判断：符合知识密度较高、便于查找和回顾的需求。

### 现在看什么

| 范围 | 重点 |
| --- | --- |
| Java 基础 | 面向对象、equals/hashCode、String、异常、泛型、反射的用途 |
| 集合 | List/Set/Map 的选择，ArrayList 与 HashMap 的基本原理 |
| MySQL | 巩固多表查询和事务，逐步补基础索引、事务隔离级别 |
| 网络 | HTTP 请求响应、GET/POST、状态码、Cookie 与 Session |
| 工具 | Maven 依赖管理、Git 基本操作 |
| Spring | 随 Spring Boot 课程学习 IoC、依赖注入、常用注解 |

每学完一个主题，再读对应问答。用一个小例子说明知识点，比单独背答案更容易记住。

并发深入、JVM 调优和分布式部分，可在基础项目完成后逐步补充。

## 2. 二哥的 Java 进阶之路：补理解

- 仓库：[itwanger/toBeBetterJavaer](https://github.com/itwanger/toBeBetterJavaer)
- 在线阅读：[javabetter.cn](https://javabetter.cn/)
- 定位：中文 Java 教程和后端知识讲解。
- 使用方式：遇到集合、泛型、反射等抽象知识时，找对应文章配合代码理解。
- 与 JavaGuide 的分工：JavaGuide 用于查缺补漏，这份资料用于展开理解；按一个主题切换即可。

## JavaWeb 学习范围

1. HTML 的输入框、按钮、表单。
2. JavaScript 基础与按钮事件。
3. HTTP、JSON、请求响应。
4. Maven、Tomcat、Servlet。
5. JDBC、连接池、数据库访问。
6. Cookie、Session、Filter。
7. fetch/Ajax：从页面调用后端接口。
8. 简单分层与综合增删改查。

### 前端的完成标准

能修改简单页面；能获取输入并调用接口；能显示结果；出错时会查看 Console 与 Network。以后再按项目需要补前端框架。

### JavaWeb 的完成标准

能解释并实现：

```text
页面输入 → HTTP 请求 → Java 接收参数 → 查询 MySQL → 返回 JSON → 页面展示
```

## 后续项目入口

- [实战项目分类](../projects/README.md)
- [学习阶段与完成标准](../study-plan.md)
