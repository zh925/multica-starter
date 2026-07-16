# 生命周期与 Issue 结构

## 父 Issue

每个用户需求创建一个父 Issue，描述业务目标、提出人、期望结果、约束和初始优先级。所有阶段 Issue、PR、QA 缺陷和发布记录都应能追溯到该父 Issue。

## 推荐阶段

| Stage | 工作 | 负责人 | 初始状态 | 完成条件 |
|---|---|---|---|---|
| 1 | 需求分析、PRD、原型 | 需求组 | `todo` | 对应阶段 PR 已合并，阶段 Issue 已 `done` |
| 2 | 需求内部评审 | 需求组 | `backlog` | 问题已处理，阶段 Issue 已 `done` |
| 3 | 用户需求确认 | 用户 + 当前交付 assignee | `backlog` | 明确批准版本和合并授权，需求基线 PR 已合并 |
| 4 | 开发与开发测试 | 开发组 | `backlog` | PR Ready，开发检查完成 |
| 5 | 代码评审 | 代码评审组 | 动态创建 | 用户已选择建议，必须项已闭环 |
| 6 | Staging QA | QA/发布组 | `backlog` | QA 结论为 PASS |
| 7 | 用户 UAT | 用户 | `backlog` | 指定版本 UAT 通过 |
| 8 | 生产发布 | QA/发布组 | `backlog` | 获得生产批准并完成发布 |
| 9 | 发布后验证 | QA/发布组 | `backlog` | 观察窗口通过，无阻断异常 |

## Stage 推进规则

1. 子 Issue 完成自身交付和门禁后进入 `done`。
2. 同一 stage 的全部子 Issue 都达到 `done` 或 `cancelled` 后，Multica 唤醒父 Issue assignee。
3. 父 Issue leader 必须重新读取子 Issue，不得只依赖通知文本。
4. 只推进最低一个未完成 stage；将其中依赖满足的 `backlog` Issue改为 `todo`。
5. 不得同时启动多个串行 stage，也不得因为单个子 Issue 完成就越过同 stage 的其他子 Issue。
6. 没有下一 stage 时，父 Issue进入其定义的下一门禁；不得自动把父需求标为 `done`。

## 阶段回流

- 需求变更：回到需求组，生成新 PR 和新确认 commit。
- 代码评审发现问题：用户选择后创建开发修改 Issue。
- QA 发现缺陷：创建独立缺陷 Issue，回流开发，重新代码评审与 QA。
- UAT 失败：根据原因回流需求或开发，不得直接在生产前临时改需求。
- 生产异常：根据影响执行经批准的回滚或 Hotfix，Hotfix 仍需评审和必要 QA。
