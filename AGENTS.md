# Agent 工作协议

本文件适用于仓库内所有人类成员和 AI Agent。任何更深目录中的 `AGENTS.md` 可以补充局部规则，但不得放宽本文件的质量、安全和人工审批门禁。

## 强制启动步骤

开始任何任务前必须：

1. 阅读触发任务的 Multica Issue、父 Issue、相关评论、已确认需求版本和关联 PR。
2. 阅读 `docs/workflow/README.md`、当前阶段规范及 `docs/templates/README.md`。
3. 检查仓库现状和已有改动，保留不属于当前任务的用户修改。
4. 确认当前 Issue 的阶段、输入、输出、验收标准、依赖与人工门禁。
5. 若缺少已确认的需求 commit、必要权限、仓库上下文或人工批准，停止有副作用的操作并在 Issue 中说明阻塞。

## 通用工作规则

- 一个任务必须有一个主 Multica Issue；子任务使用父子 Issue 和 stage 表达依赖。
- `backlog` 是停车场。未满足前置门禁的后续任务必须保持 `backlog`。
- 需求、开发、评审、QA、UAT、发布与发布后验证不得相互替代。
- Agent 不得代替用户确认需求、选择评审建议、批准 UAT 或批准生产发布。
- 所有文档必须使用对应模板；复制模板到目标目录后再填写，不得直接修改模板源文件来交付某个需求。
- 高风险、不可逆、生产数据、权限、Secret、迁移和回滚操作必须获得明确人工批准。
- Secret 只能存在于批准的 Secret/CI 系统，不得写入代码、日志、Issue、PR 或文档。
- CI/CD 尚未接入。不得声称已自动构建、部署或验证；必须记录实际执行方式和证据。

## 固定需求目录

每个需求使用以下结构，其中 `<ISSUE-KEY>` 示例为 `RUO-123`：

```text
docs/requirements/<ISSUE-KEY>/
├── README.md
├── prd.md
├── acceptance-criteria.md
├── decisions/
├── prototype/
│   ├── README.md
│   ├── assets/
│   └── screenshots/
├── development/
├── qa/
└── release/
```

## Git 与 PR

- 默认分支为 `main`，禁止未经明确授权直接提交到 `main`。
- 分支建议：`agent/<issue-key>-<short-description>`。
- PR 必须使用 `.github/pull_request_template.md`。
- PR 必须记录原始需求、确认 commit、关联 Issue、验收标准映射、测试、数据库/基础设施影响、风险和回滚。
- 只对合并后确实应该完成的主开发 Issue 使用 `Closes RUO-123`；其他关联项写入关联表。
- 修改评审意见期间将 PR 置为 Draft；完成修改并自检后标记 Ready，触发新一轮评审。

## 完成定义

Agent 声明完成前必须给出：

- 实际产物或变更文件；
- 执行过的检查及结果；
- 未执行的检查与原因；
- 风险、遗留问题和需要人工决定的事项；
- PR URL（有代码或版本化文档变更时）；
- 对应 Multica Issue 的状态与下一门禁。
