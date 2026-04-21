---
name: openspec-proposal-creation-cn
description: 使用官方 OpenSpec getting-started 风格创建结构化变更提案。适用于规划新功能、修改现有行为、移除旧能力、补写规格，或任何需要先产出 `proposal.md`、`design.md`、`tasks.md`、`spec-delta.md` 再进入实施的场景。触发词包括“openspec提案”“规划”“创建提案”“规划变更”“新功能”“新特性”“新需求”“添加功能规划”“设计规范”。
---

# OpenSpec 提案创建

该技能用于把一个变更想法落成更贴近官方 getting-started 的 OpenSpec 提案。提案阶段不仅要说明为什么改，还要把设计思路和实施任务显式落到 change 工作区里。

## 目标产物

每个提案都应在 `openspec/changes/{change-id}/` 下生成以下文件：

1. `proposal.md`：说明 Why / What Changes / Impact
2. `design.md`：说明关键设计决策、备选方案与限制
3. `tasks.md`：按执行顺序排列的实施任务清单
4. `specs/{capability}/spec-delta.md`：需求差异，使用 EARS + 场景格式

如有需要，可为一个 change 涉及多个 capability，各自放在 `specs/<capability>/spec-delta.md`。

## 工作原则

- 先读现有规范，再写变更
- 一个 change 只聚焦一个连贯目标
- `design.md` 用于沉淀设计理由，而不是把设计细节散落在 proposal 或任务里
- `tasks.md` 是实施清单，应明确顺序与完成条件
- 需求差异必须使用 `ADDED/MODIFIED/REMOVED Requirements`
- 每条 Requirement 至少包含 2 个场景，通常是正向 + 错误路径

## 推荐流程

```text
提案进度:
- [ ] 第 1 步：检查现有规范与进行中的变更
- [ ] 第 2 步：生成唯一 change-id
- [ ] 第 3 步：创建标准目录结构
- [ ] 第 4 步：起草 proposal.md
- [ ] 第 5 步：补充 design.md
- [ ] 第 6 步：编写 tasks.md
- [ ] 第 7 步：编写 spec-delta.md
- [ ] 第 8 步：验证结构与格式
- [ ] 第 9 步：向用户展示并等待审批
```

## 第 1 步：检查现有规范与进行中的变更

重点查看：

- `openspec/specs/*/spec.md`
- `openspec/changes/*/proposal.md`
- `openspec/changes/*/specs/**/spec-delta.md`

可参考：

```bash
find openspec/specs -mindepth 1 -maxdepth 1 -type d -exec basename {} \;
find openspec/changes -maxdepth 1 -type d -not -path "openspec/changes" -not -path "*/archive"
grep -r "### Requirement:" openspec/specs/
```

## 第 2 步：生成唯一 change-id

change-id 必须 URL-safe、短而清晰，能直接说明这次变更做什么。

推荐格式：

- `add-user-authentication`
- `fix-password-reset`
- `update-notification-routing`
- `remove-legacy-login`

## 第 3 步：创建标准目录结构

```text
openspec/changes/{change-id}/
├── proposal.md
├── design.md
├── tasks.md
└── specs/
    └── {capability}/
        └── spec-delta.md
```

## 第 4 步：起草 proposal.md

使用 [templates/proposal.md](templates/proposal.md)。

要求：

- `Why` 讲问题、目标、上下文
- `What Changes` 讲变更边界
- `Impact` 讲 spec、代码、接口与迁移影响

## 第 5 步：补充 design.md

使用 [templates/design.md](templates/design.md)。

当官方流程需要明确“怎么做、为什么这样做”时，设计信息应写入 `design.md`，而不是混入 `proposal.md` 或 `tasks.md`。

典型内容：

- 关键设计决策
- 备选方案与取舍
- 数据 / 接口 / 流程影响
- 风险与限制

## 第 6 步：编写 tasks.md

使用 [templates/tasks.md](templates/tasks.md)。

`tasks.md` 是有序任务清单，不是结构化状态数据库。它应该能被人或 agent 逐项执行，并且完成状态直接在 Markdown checklist 中体现。

任务编写要求：

- 任务按依赖顺序排列
- 每项任务聚焦一个可验证结果
- 覆盖实现、验证、文档或迁移等必要工作
- 不要写成与某个技术栈强绑定的脚手架模板

## 第 7 步：编写 spec-delta.md

使用 [templates/spec-delta.md](templates/spec-delta.md)。

差异标题必须使用：

- `## ADDED Requirements`
- `## MODIFIED Requirements`
- `## REMOVED Requirements`

Requirement 结构：

```markdown
### Requirement: {name}
WHEN/IF/WHERE/WHILE ...
系统 SHALL ...

#### Scenario: {scenario name}
GIVEN ...
WHEN ...
THEN ...
```

## 第 8 步：验证结构与格式

至少检查：

```text
- [ ] openspec/changes/{change-id}/ 目录存在
- [ ] proposal.md 含 Why / What Changes / Impact
- [ ] design.md 已记录关键设计决策
- [ ] tasks.md 为可执行的有序 checklist
- [ ] spec-delta.md 使用 ADDED/MODIFIED/REMOVED Requirements
- [ ] 每条需求使用 ### Requirement:
- [ ] 每个场景使用 #### Scenario:
- [ ] 每条需求至少两个场景
```

验证模式见 [reference/VALIDATION_PATTERNS.md](reference/VALIDATION_PATTERNS.md)。

## 第 9 步：向用户展示并等待审批

至少说明：

- change-id
- 涉及的 capability
- 创建的文件
- 提案解决的问题
- 下一步如何进入实施

```markdown
## Proposal Summary

**Change ID**: {change-id}
**Capabilities**: {capabilities}

**Created files**:
- openspec/changes/{change-id}/proposal.md
- openspec/changes/{change-id}/design.md
- openspec/changes/{change-id}/tasks.md
- openspec/changes/{change-id}/specs/{capability}/spec-delta.md
```

## 反模式

不要：

- 未读现有规范就直接起草提案
- 在一个 change 中混合多个无关目标
- 把 `tasks.md` 写成空泛待办列表
- 忽略 `design.md`，把关键设计理由丢失掉
- 写没有场景的 Requirement

要：

- 保持 proposal 面向审批、design 面向设计决策、tasks 面向实施、delta 面向规范
- 在交付前验证结构

## 参考资料

- [templates/proposal.md](templates/proposal.md)
- [templates/design.md](templates/design.md)
- [templates/tasks.md](templates/tasks.md)
- [templates/spec-delta.md](templates/spec-delta.md)
- [reference/EARS_FORMAT.md](reference/EARS_FORMAT.md)
- [reference/VALIDATION_PATTERNS.md](reference/VALIDATION_PATTERNS.md)
