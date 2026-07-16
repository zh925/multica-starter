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
