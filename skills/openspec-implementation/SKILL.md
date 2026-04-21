---
name: openspec-implementation-cn
description: 按已批准的官方 OpenSpec 提案顺序实施变更，并使用 TodoWrite 跟踪执行过程。适用于“openspec开发”“开发”“实施”“实现提案”“应用变更”“执行规范”“按顺序完成任务”“开始实施”等场景，尤其适合已经存在 `proposal.md`、`design.md`、`tasks.md`、`spec-delta.md` 的 change。
---

# OpenSpec 实施

该技能用于执行已批准提案。实施阶段的目标是根据 `proposal.md`、`design.md`、`tasks.md` 与 `spec-delta.md` 落地变更，并逐项验证结果。

## 核心原则

- 先读取完整上下文，再开始改动
- 用 TodoWrite 跟踪执行进度
- `tasks.md` 是权威实施清单
- 实施阶段不要把 spec delta 直接合并到 living spec
- 只有当 `tasks.md` 中相关任务完成后，change 才能进入 archive 流程

## 标准流程

```text
实施进度:
- [ ] 第 1 步：读取 proposal / design / tasks / spec delta
- [ ] 第 2 步：将 tasks.md 映射到 TodoWrite
- [ ] 第 3 步：按顺序执行任务
- [ ] 第 4 步：逐任务验证结果
- [ ] 第 5 步：记录剩余风险或补充说明
- [ ] 第 6 步：确认 tasks.md 可进入 archive
```

## 第 1 步：读取完整上下文

开始前必须读取：

- `openspec/changes/{change-id}/proposal.md`
- `openspec/changes/{change-id}/design.md`
- `openspec/changes/{change-id}/tasks.md`
- `openspec/changes/{change-id}/specs/**/spec-delta.md`

重点搞清楚：

- 为什么要做这次变更
- 设计决策与约束是什么
- 需要完成哪些任务
- 哪些 Requirement / Scenario 是验收依据

## 第 2 步：将 tasks.md 映射到 TodoWrite

实施阶段的运行时状态应写在 TodoWrite，并与 `tasks.md` 的 checklist 顺序保持一致。

推荐映射方式：

- 每个 checklist 项生成一个 TodoWrite 项
- 如果某一项过大，可拆成更小的 TodoWrite 子任务
- 同一时刻只保留一个 `in_progress`

## 第 3 步：按顺序执行任务

默认按 `tasks.md` 顺序执行，除非你已经明确验证某些任务完全独立。

每个任务都遵循：

1. 读取相关文件与上下文
2. 实施最小必要改动
3. 运行最相关的验证
4. 确认满足对应 Requirement / Scenario
5. 标记 TodoWrite 完成后再进入下一个任务

## 第 4 步：逐任务验证结果

常见验证方式：

- 相关自动化测试
- 类型检查 / lint / 构建检查
- 手动验证关键路径
- 逐条对照 `spec-delta.md` 的 Scenario

如果本仓库是文档或模板仓库，也要做对应验证，例如：

- Markdown 结构检查
- 模板字段完整性检查
- 路径、文件名、引用链接一致性检查

## 第 5 步：记录剩余风险或补充说明

实施完成时，应明确说明：

- 哪些场景已经验证
- 是否存在未覆盖但已知的后续工作
- 是否需要用户进一步确认

## 第 6 步：确认 tasks.md 可进入 archive

实施阶段不再使用额外的完成标记文件。完成信号应来自：

- `tasks.md` 中相关任务都已完成
- 验证已执行
- 交付说明已明确当前状态

## 交付时应说明

至少向用户报告：

- 已完成的任务
- 已验证的范围
- `tasks.md` 当前完成状态
- 下一步可进入 archive 阶段

```markdown
## 实施完成

**Change**: {change-id}

已完成：
- {task summary 1}
- {task summary 2}

已验证：
- {verification summary}

下一步：如需合并规范差异到 living spec，可执行“openspec归档 {change-id}”。
```

## 阻塞处理

如果任务被阻塞：

- 不要伪造完成状态
- 在 TodoWrite 中保留阻塞项
- 清楚写出阻塞原因
- 在 `tasks.md` 与交付说明中体现当前进度

## 反模式

不要：

- 将无关重构混入当前提案
- 在实施阶段把 spec delta 直接合并到 `openspec/specs/`
- 没有验证就宣布完成
- 引入额外的完成标记文件替代 `tasks.md`

要：

- 让 TodoWrite 与 `tasks.md` 顺序保持一致
- 使用 Scenario 作为验收依据
- 为后续 archive 保留清晰边界

## 参考资料

- [reference/TASK_PATTERNS.md](reference/TASK_PATTERNS.md)
- [reference/TESTING_STRATEGIES.md](reference/TESTING_STRATEGIES.md)
