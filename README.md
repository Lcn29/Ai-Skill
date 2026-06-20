# Java Coding Standards Skill

这是一个面向 Codex/Agent 的 Java 编码规范 Skill，用于在编写、修改、审查和重构 Java 代码时，持续遵循一套可移植的 Java 规则。

## 适用场景

- 编写、修改或审查 `.java` 文件。
- 设计 Java API、接口、抽象类、DTO、枚举、异常和序列化形式。
- 审查 Maven 项目结构、依赖声明、分层约定和工程规范。
- 编写或评审 Spring 风格的业务服务、DAO、MySQL 持久化代码。
- 处理并发、线程池、异步 API、回调监听器、公共 SDK 或跨服务契约。
- 希望 Agent 在 Java 任务中同时参考《阿里巴巴 Java 开发手册》和 Effective Java 风格的实践。

## 覆盖范围

该 Skill 覆盖以下规则族：

- 命名、常量、格式、注释、控制流和面向对象规则。
- 集合、泛型、枚举、Optional、日期时间、BigDecimal 和金额处理。
- 异常设计、日志、参数校验、返回值契约和空值处理。
- 单元测试、安全、HTTP/前端契约、MySQL 和 ORM 风险。
- Maven 坐标、二方库/共享库、分层结构和工程约束。
- API 兼容性、公开接口演进、DTO 演进、枚举边界、跨服务 payload。
- 并发、线程池、ThreadLocal、锁、回调、异步生命周期。
- Java 原生序列化风险和 Effective Java 90 条实践。

## 目录结构

```text
java-coding-standards/
├── SKILL.md
├── README.md
├── pressure-test-report.md
├── agents/
│   └── openai.yaml
└── references/
    ├── java-handbook-rules.md
    └── effective-java-rules.md
```

## 文件说明

- `SKILL.md`：Agent 运行入口。定义触发条件、工作流、核心默认规则、冲突处理、公开 API 边界和审查清单。
- `references/java-handbook-rules.md`：团队 Java 规范参考，覆盖编码、日志、测试、安全、MySQL、工程结构和设计规则。
- `references/effective-java-rules.md`：Effective Java 90 条实践的可执行化整理。
- `agents/openai.yaml`：Codex/OpenAI UI 元数据。
- `pressure-test-report.md`：使用 sub-agent 做压力测试后的场景、结果、发现和修复记录。
- `README.md`：给人类维护者阅读的中文说明文档，不是 Agent 执行入口。

## 使用方式

当用户请求涉及 Java 代码编写、审查、重构、测试或 API 设计时，Agent 应加载 `SKILL.md`，再按任务类型读取对应参考文件：

- 普通 Java 实现或代码审查：读取 `references/java-handbook-rules.md`。
- API 设计、泛型、枚举、lambda/stream、异常、并发、性能或序列化：同时读取 `references/effective-java-rules.md`。
- 数据库、ORM、安全、日志、测试、Maven、工程结构或分层：重点使用 `references/java-handbook-rules.md` 中对应章节。

项目本身的 `AGENTS.md`、构建文件、现有代码风格、框架约束和生成代码规则优先级更高。发生冲突时，应遵循项目约定，同时尽量保留 Java 安全性和可维护性意图。

## 关键原则

- 优先做最小正确改动，避免无关重构和格式化。
- 默认最小可见性，避免公开可变字段和泄露内部实现。
- 公共 API、DTO、序列化形式、跨服务 payload 和 Maven 坐标都视为兼容性边界。
- 并发代码必须显式说明线程池、队列、拒绝策略、取消、超时和线程安全约束。
- 金额和精确小数不要使用 `float`/`double`。
- 日志使用 SLF4J 风格，不使用 `System.out`、`printStackTrace` 或字符串拼接日志。
- 集合返回值优先返回空集合/数组，而不是 `null`。
- 避免 Java 原生序列化；如无法避免，需要显式设计序列化形式和安全边界。

## 可移植性说明

Skill 中的规则已被整理为独立参考文件，不要求用户拥有原始资料、私有知识库或特定目录结构。

其中一些术语做了通用化处理：

- “二方库”可理解为组织内部或多个团队共享的二进制依赖、SDK 或公共库。
- “公司/业务线 GAV”可理解为当前项目自己的 Maven `groupId`、`artifactId` 和版本命名体系。
- “线上应用”可理解为生产或类生产环境运行的软件。
- “RPC”泛指 HTTP、gRPC、Dubbo 类 RPC、消息请求/响应和内部 SDK 调用等跨进程调用。

## 压力测试结果

该 Skill 已使用 sub-agent 做过压力测试，覆盖：

- 其他用户机器上的可移植性审计。
- 有明显缺陷的 Java 服务代码审查。
- 公开 Java 库/API 设计审查。
- 移除私有路径依赖后的可移植性复测。
- 增强 API 边界规则后的公开库设计复测。

压力测试中发现的问题已经回填到 `SKILL.md` 和 `references/` 中，最终验证结果记录在 `pressure-test-report.md`。
