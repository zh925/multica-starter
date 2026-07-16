# 批准执行与阶段推进

本规范解决“用户已经确认，但 PR、Issue 和下一阶段没有继续变化”的责任空档。

## 两类责任人

- **门禁执行者**：当前交付 Issue 的 assignee。负责核验人工批准、合并当前 PR、核验关单。
- **阶段推进者**：父 Issue 的 leader。负责在整个 stage 结束后推进最低一个未完成 stage。

两者不得互相替代。门禁执行者不能跳过 stage，阶段推进者不能在交付未合并时提前启动后续任务。

## 可执行批准格式

```text
确认对象：https://github.com/<owner>/<repo>/pull/<number>
确认版本：<完整 head SHA>
确认结论：通过
允许合并：是
合并策略：squash / merge / rebase
决策备注：<可选>
```

“看过了”“可以”“评审通过”但没有 PR、SHA 和合并授权时，只能记录为反馈，不能执行合并。

## 门禁执行状态机

```text
收到人类评论
├─ 字段不完整 → 保持 in_review，回复缺失字段
├─ head SHA 已变化 → 保持 in_review，要求重新确认
├─ PR Draft/冲突/检查失败 → 保持 in_review 或 blocked，报告原因
├─ 通过但不允许合并 → 保持 in_review
└─ 通过且允许合并
   → 确保 Closes 当前交付 Issue
   → 按授权策略合并
   → 核验 GitHub state=merged
   → 核验 Multica Issue=done
   → Webhook 未关单时手动置 done
   → 评论记录 merged commit 和结果
```

禁止在 GitHub 尚未确认 `merged` 时把 Issue 标为 `done`。

## Stage 推进状态机

```text
收到 stage 完成通知
→ 重新读取父 Issue 全部 children
→ 验证当前最低 stage 全部为 done/cancelled
→ 找到最低一个未完成 stage
→ 检查每个子 Issue 的显式依赖
→ 只把依赖满足的 backlog Issue 改为 todo
→ 评论记录本次推进和仍停留 backlog 的原因
```

如果没有下一 stage，阶段推进者把父 Issue放入其流程定义的下一门禁；不得仅因子 Issue 完成就关闭父需求。

## 幂等与失败恢复

- 重复收到同一批准时，先读取 PR 状态；已经 merged 时不得重复合并，只完成缺失的关单/推进核验。
- GitHub Webhook 延迟时，以 GitHub 的真实 merged 状态为前提，再手动补 Multica 状态。
- 合并失败时记录失败原因，不重复盲试；权限、冲突、保护规则或检查失败需要人工/开发处理。
- 父 leader 被重复唤醒时，已是 `todo/in_progress/done` 的下一阶段 Issue 不得再次触发或重建。

