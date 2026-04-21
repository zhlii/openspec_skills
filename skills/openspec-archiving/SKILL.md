---
name: openspec-archiving-cn
description: 将已完成实施的 OpenSpec change 归档，并把规范差异合并回 living spec。适用于“openspec归档”“归档提案”“合并规范”“完成提案后归档”等场景，前提是 `tasks.md` 所示实施工作已完成。
---

# OpenSpec 归档

该技能用于在实施完成后执行收尾：把 `openspec/changes/{change-id}` 中的规范差异合并到 `openspec/specs/`，再把整个 change 目录移动到 `openspec/changes/archive/` 下作为历史记录。

## 核心原则

- 只有实施已完成的 change 才能归档
- 先合并规范，再移动归档目录
- 归档目录位于 `openspec/changes/archive/`
- 归档是历史记录，应视为只读

## 标准流程

```text
归档进度:
- [ ] 第 1 步：验证 tasks.md 显示实施已完成
- [ ] 第 2 步：阅读 proposal、design、tasks 与 spec delta
- [ ] 第 3 步：将 delta 合并到 living spec
- [ ] 第 4 步：验证合并后的规范结构
- [ ] 第 5 步：移动 change 到 openspec/changes/archive/
- [ ] 第 6 步：向用户汇报归档完成
```

## 第 1 步：验证 tasks.md 显示实施已完成

至少确认：

- `openspec/changes/{change-id}/tasks.md` 已完成本次实施范围
- 用户没有表示该 change 仍在实施中

## 第 2 步：阅读 proposal、design、tasks 与 spec delta

归档前要重新读取以下文件：

- `openspec/changes/{change-id}/proposal.md`
- `openspec/changes/{change-id}/design.md`
- `openspec/changes/{change-id}/tasks.md`
- `openspec/changes/{change-id}/specs/**/spec-delta.md`

## 第 3 步：将 delta 合并到 living spec

按 capability 合并到：

```text
openspec/specs/{capability}/spec.md
```

### ADDED Requirements

- 将新增 Requirement 与全部 Scenario 加入目标 spec

### MODIFIED Requirements

- 在目标 spec 中找到同名 Requirement
- 用 spec delta 中的完整新版本替换整个 Requirement 块

### REMOVED Requirements

- 在目标 spec 中定位目标 Requirement
- 删除该 Requirement 块

## 第 4 步：验证合并后的规范结构

至少检查：

- 仍使用 `### Requirement:`
- 仍使用 `#### Scenario:`
- 没有重复 Requirement
- 修改后的 capability 文件路径正确

## 第 5 步：移动 change 到归档目录

归档位置为：

```text
openspec/changes/archive/{YYYY-MM-DD}-{change-id}
```

移动的是整个 change 目录，以保留 proposal、design、tasks 与 delta 的完整历史。

## 第 6 步：向用户汇报归档完成

至少说明：

- 更新了哪些 living spec
- 归档目录的最终位置
- 已归档的 change-id

```markdown
## 归档完成

**Change**: {change-id}
**Archived to**: openspec/changes/archive/{date}-{change-id}

已更新：
- openspec/specs/{capability}/spec.md
```

## 反模式

不要：

- 归档仍有未完成 checklist 项的 change
- 在移动前只归档部分文件
- 修改已归档目录中的历史记录
- 默认执行 git 提交，除非用户明确要求

要：

- 先合并 living spec，再移动到 archive
- 保留整套 change 历史

## 冲突处理

如果多个 change 修改同一 Requirement：

- 先暂停归档
- 明确指出冲突发生在哪个 capability / Requirement
- 手动决定以哪个版本为准，再继续归档
