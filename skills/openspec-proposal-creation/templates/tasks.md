# 实施任务：{change-title}

## 阶段 1：范围与设计对齐

- [ ] 阅读 `proposal.md` 与 `design.md`，确认目标、边界与设计决策
- [ ] 审阅 `openspec/specs/{capability}/spec.md` 与相关进行中 change，避免冲突
- [ ] 明确本次 change 要新增、修改、移除的 Requirement

## 阶段 2：实现变更

- [ ] 按依赖顺序完成核心实现
- [ ] 如影响多个模块，优先完成被依赖的底层改动
- [ ] 保持实现范围与 `proposal.md`、`design.md`、`spec-delta.md` 一致

## 阶段 3：验证结果

- [ ] 运行与本次改动最相关的测试、检查或人工验证
- [ ] 逐项对照 `spec-delta.md` 中的 Scenario 确认行为
- [ ] 记录已知限制、风险或后续事项

## 阶段 4：交付准备

- [ ] 确认任务项均已完成
- [ ] 如有必要更新用户文档或开发文档
- [ ] 确认该 change 已具备进入 archive 的条件
