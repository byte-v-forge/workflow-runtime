# AGENTS.md

本仓是 `workflow-runtime`，只承载 Temporal/worker 运行时基础设施。

- 本仓不承载任何业务 workflow、业务 activity、业务状态机或业务数据模型。
- 本仓计划开源；新增依赖时禁止引入私有契约仓、私有 SDK 仓或闭源业务仓。
- 跨仓共享的 workflow runtime 模型来自本仓 `proto/byte/v/forge/contracts/workflowruntime/v1/`。
- 业务 workflow、activity、worker 和内部 proto 留在各自业务仓。
- 本仓可以提供 Temporal client、worker bootstrap、task queue 命名校验、默认 retry/timeout、配置加载、日志/观测接入点和开发辅助。
- 可提供通用步骤投影、activity retry policy、retryable/non-retryable activity error 和 retry signal 辅助；业务步骤顺序和补偿逻辑仍归业务仓。
- 本仓当前不暴露 gRPC 服务；只有形成真实远程服务边界时才新增 gRPC service，并以本仓 proto 为源头。
- Workflow 代码必须保持确定性；网络、数据库、HTTP/gRPC、浏览器、文件 IO、随机数和 wall clock 等 side effect 必须放在 activity。
- Temporal history 只作为执行状态，不作为业务事实源；业务真相归业务服务自己的数据库、事件和读模型。
- Task queue 按服务所有权边界声明；基础仓只校验和复用命名，不集中定义业务队列。
- 后端优先使用 Go，按官方 Temporal Go SDK 文档和 Clean Code、DI、面向抽象设计实现。
- SDK、Go 版本和外部依赖默认使用最新稳定版本或 LTS。
- `gen/` 承载本仓 proto 生成物，随契约一起提交；检查报告、临时二进制和其他构建产物不提交。
