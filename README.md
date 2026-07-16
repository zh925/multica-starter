# Multica Starter

这是一个面向 Multica 人类成员与 AI Agent 的研发工作流起始仓库。仓库当前主要固化协作规范和模板；业务代码可以在此基础上逐步加入。

## 开始工作前

1. 所有成员和 Agent 必须先阅读 [AGENTS.md](AGENTS.md)。
2. 阅读 [完整工作流](docs/workflow/README.md) 与当前阶段的门禁要求。
3. 从 [模板索引](docs/templates/README.md) 复制所需模板，不得覆盖模板源文件。
4. 所有工作必须关联一个 Multica Issue，分支、PR 标题或正文中应包含 `RUO-<数字>`。

## 当前自动化状态

- 默认分支：`main`
- CI/CD：尚未接入
- PR 代码评审、Staging QA、Production Verification Autopilot：已创建但保持暂停
- 在 CI/CD 接入前，不得假设构建、部署、回滚或环境验证已经自动完成

## 规范入口

- [研发生命周期](docs/workflow/lifecycle.md)
- [角色与职责](docs/workflow/roles.md)
- [状态与人工门禁](docs/workflow/gates.md)
- [GitHub 与代码评审](docs/workflow/github-review.md)
- [QA 与发布](docs/workflow/qa-release.md)
- [新人接入](docs/workflow/onboarding.md)
