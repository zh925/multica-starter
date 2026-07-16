# 人工批准与门禁执行记录

## 人工决定

- 决策人：
- 确认对象：PR URL
- 确认版本：完整 head SHA
- 确认结论：通过 / 退回修改
- 允许合并：是 / 否
- 合并策略：squash / merge / rebase
- 决策备注：

## 执行前核验

- [ ] 评论作者具有决策权
- [ ] PR URL 与当前 Issue 一致
- [ ] 当前 head SHA 与确认版本完全一致
- [ ] PR Ready 且可合并
- [ ] checks 已通过，或已如实记录未配置/豁免
- [ ] `Closes` 仅指向当前交付 Issue

## 执行结果

- PR 最终状态：
- Merged commit：
- 当前 Issue 最终状态：
- Stage barrier 状态：
- 下一阶段 Issue：
- 异常与未执行项：
