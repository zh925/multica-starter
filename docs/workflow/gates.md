# 状态与人工门禁

Multica 状态本身不强制流程，因此 leader 和参与者必须执行以下门禁。

## 状态语义

- `backlog`：前置条件未满足，不触发 Agent。
- `todo`：前置条件已满足，可以开始。
- `in_progress`：正在执行。
- `in_review`：等待同阶段评审或人工决策。
- `blocked`：外部条件阻塞，必须写明 `waiting_on` 和原因。
- `done`：阶段产物和证据完整，且所需门禁已通过。
- `cancelled`：明确取消，保留历史。

## 人工确认格式

需求确认：

```text
确认对象：需求产物 PR URL
确认版本：commit SHA
确认结论：通过 / 退回修改
允许合并：是 / 否
合并策略：squash / merge / rebase（可选，默认遵循仓库策略）
决策备注：
```

评审建议选择：

```text
接受：R1, R3
拒绝：R2
延期：R4（目标 Issue）
补充要求：
```

UAT 与发布批准：

```text
验收版本：commit SHA / build ID
UAT 结论：通过 / 拒绝
允许生产发布：是 / 否
批准窗口：
备注：
```

缺少上述必要字段时，Agent 必须继续等待，不得自行推断为通过。

## 评论后的强制动作

人工批准不是信息归档事件。当前交付 Issue 的 assignee 收到完整批准后必须：

1. 核对评论作者是有决策权的人类成员。
2. 重新读取 Issue、关联 PR 和 PR 当前 head SHA。
3. 确认批准版本与当前 head 完全一致；不一致则停止并要求重新确认。
4. 确认 PR Ready、可合并，并如实报告 checks；CI/CD 未接入时不得伪造通过结果。
5. 若 `允许合并：是`，确认 PR 正文包含 `Closes <当前交付 Issue>` 后执行授权的合并策略。
6. 只有确认 GitHub PR 状态为 `merged` 后，才核验或设置当前 Issue 为 `done`。
7. 将 PR URL、merged commit、执行结果和未执行项写入 Issue 评论。

批准为“通过”但 `允许合并：否` 时，Issue 保持 `in_review`，等待后续明确授权。

完整状态机见 [批准执行与阶段推进](approval-execution.md)。
