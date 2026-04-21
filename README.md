# OpenSpec Skills Repository

这是一个用于 OpenCode / OhMyOpenCode 的 **OpenSpec 技能仓库**，提供一套围绕规范驱动开发（spec-driven development）的技能与模板，帮助 agent 按统一流程完成提案、实施、上下文查询与归档。

仓库当前包含 4 个核心技能：

| Skill | 用途 | 典型触发词 |
|---|---|---|
| `openspec-proposal-creation` | 创建变更提案、设计文档、任务清单与 spec delta | `openspec提案`、`规划`、`创建提案`、`新功能` |
| `openspec-implementation` | 按已批准提案实施变更，并基于任务清单逐项验证 | `openspec开发`、`开发`、`实施`、`实现提案` |
| `openspec-context-loading` | 查询项目中的 specs、changes 与 requirement 上下文 | `openspec上下文`、`有哪些规范`、`显示变更` |
| `openspec-archiving` | 在实施完成后归档 change，并将 delta 合并回 living spec | `openspec归档`、`归档提案`、`合并规范` |

## Repository Purpose

这个仓库的目标不是提供业务代码，而是沉淀一套可复用的 OpenSpec 工作方式：

- 先写提案，再实施变更
- 用结构化文档承载设计决策和验收依据
- 通过 spec delta 管理规范差异
- 在完成后将 change 归档并合并回 living specs

这让 agent 在处理需求时，不只是“改文件”，而是按一条可审阅、可追踪、可归档的流程推进。

## OpenSpec Workflow

标准工作流如下：

```text
Proposal Creation → Implementation → Archiving
     ↓                   ↓                ↓
  openspec/changes/   tasks.md      openspec/changes/archive/
  + design.md         + TodoWrite   + merge to openspec/specs/
```

### 1. Proposal Creation

在 `openspec/changes/{change-id}/` 下创建：

- `proposal.md`：说明 Why / What Changes / Impact
- `design.md`：记录关键设计决策、取舍与限制
- `tasks.md`：实施清单，按依赖顺序组织
- `specs/{capability}/spec-delta.md`：记录需求差异

### 2. Implementation

按 `tasks.md` 顺序执行任务，并基于 `spec-delta.md` 的 Requirement / Scenario 做验证。实施阶段的完成信号来自任务状态和验证结果，而不是额外的标记文件。

### 3. Archiving

当变更实施完成后，将 `spec-delta.md` 的内容合并回 `openspec/specs/{capability}/spec.md`，随后把整个 change 目录移动到 `openspec/changes/archive/` 下作为历史记录。

## Expected Project Structure

OpenSpec 项目通常使用以下目录结构：

```text
openspec/
├── specs/
│   ├── {capability}/
│   │   └── spec.md
│   └── ...
├── changes/
│   ├── {change-id}/
│   │   ├── proposal.md
│   │   ├── design.md
│   │   ├── tasks.md
│   │   ├── specs/
│   │   │   └── {capability}/
│   │   │       └── spec-delta.md
│   └── archive/
│       └── {YYYY-MM-DD}-{change-id}/
```

本仓库本身是技能仓库，因此你会在 `skills/` 目录中看到技能定义、模板和参考资料，而不是某个业务系统的 `openspec/` 实例数据。

## Skills Directory

当前技能位于：

```text
skills/
├── openspec-archiving/
├── openspec-context-loading/
├── openspec-implementation/
└── openspec-proposal-creation/
```

每个技能通常包含：

- `SKILL.md`：技能说明、步骤和约束
- `templates/`：提案、设计、任务、spec delta 模板（按需提供）
- `reference/`：格式、搜索、校验等参考资料

## Requirement Format

OpenSpec 需求遵循 EARS 风格，推荐格式如下：

```markdown
### Requirement: {name}
WHEN {trigger condition},
系统 SHALL {action and result}。

#### Scenario: {positive scenario}
GIVEN {preconditions}
WHEN {action}
THEN {expected result}
```

关键约定：

- 使用 **SHALL** 表示强约束需求
- 每条 Requirement 至少包含 2 个 Scenario
- Scenario 标题必须使用 `#### Scenario:`
- 需求差异必须使用 `ADDED / MODIFIED / REMOVED Requirements`

## Important Conventions

使用本仓库中的技能时，请遵守以下约定：

1. `change-id` 必须清晰、简短、URL-safe，例如 `add-user-authentication`
2. 提案阶段先读现有规范，再写 proposal / design / tasks / delta
3. 实施阶段以 `tasks.md` 为权威清单，按顺序执行并验证
4. 实施阶段不要直接把 spec delta 合并到 living specs
5. 归档阶段先合并规范，再把 change 移入 `archive/`
6. 已归档 change 视为历史记录，不应随意修改

## Repository Notes

- 这是一个文档 / 模板 / 技能仓库，不是常规应用仓库
- 仓库当前没有业务构建或测试流程
- 技能内容、模板与参考资料主要使用简体中文编写

如果你要把这套流程应用到自己的项目中，建议先从 `openspec-proposal-creation` 开始，为第一个 change 建立完整的 proposal、design、tasks 和 spec delta。
