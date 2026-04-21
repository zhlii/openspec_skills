---
name: openspec-context-loading-cn
description: 加载官方 OpenSpec 项目上下文，列出现有规范、进行中的变更与归档历史，帮助用户理解 `openspec/` 目录下的能力、change 和需求。适用于“openspec上下文”“有哪些规范”“显示变更”“列出能力”“项目上下文”“查找规范”“展示规范”等场景。
---

# OpenSpec 上下文加载

该技能用于围绕官方 OpenSpec 目录结构提供上下文发现与摘要，重点是 `openspec/specs/`、`openspec/changes/` 与 `openspec/changes/archive/`。

## 基本模式

搜索 → 阅读 → 总结。

先给高层概览，再根据用户问题下钻到具体 capability、change 或 requirement。

## 发现命令

### 列出所有规范

```bash
find openspec/specs -name "spec.md" -type f
find openspec/specs -mindepth 1 -maxdepth 1 -type d
ls -R openspec/specs/
```

### 列出进行中的变更

```bash
find openspec/changes -maxdepth 1 -type d -not -path "openspec/changes" -not -path "*/archive"
```

### 列出已归档的变更

```bash
ls -1 openspec/changes/archive/
find openspec/changes/archive/ -maxdepth 1 -type d -mtime -7
```

### 搜索需求与场景

```bash
grep -r "### Requirement:" openspec/specs/
grep -r "#### Scenario:" openspec/specs/
```

### 查看单个 change

```bash
cat openspec/changes/{change-id}/proposal.md
cat openspec/changes/{change-id}/design.md
cat openspec/changes/{change-id}/tasks.md
find openspec/changes/{change-id}/specs -name "*.md" -exec cat {} \;
```

## 常见查询

### 项目有哪些规范？

先列 capability，再统计每个 spec 中的 Requirement 数量。

### 当前有哪些 change？

先列 `openspec/changes/*/`，再读取每个 `proposal.md` 的 Why 段落做摘要。

### 某个 change 是否接近完成？

检查 `tasks.md` 的 checklist 状态，而不是寻找额外完成标记文件。

```bash
grep -n "^- \[ \]\|^- \[x\]" openspec/changes/{change-id}/tasks.md
```

### 某个能力的规范在哪里？

```bash
cat openspec/specs/{capability}/spec.md
```

## 输出建议

不要直接倾倒整份文档。优先提供：

- capability 列表
- 进行中 change 列表
- 某个 spec 的 requirement 摘要
- 某个 change 的 proposal / design / tasks / delta 摘要

## 反模式

不要：

- 默认输出整个 `spec.md`
- 把 `tasks.md` 之外的文件当作完成状态来源
- 在仓库不存在 `openspec/` 目录时假装结构已经存在

要：

- 明确说明当前项目是否采用官方 OpenSpec 结构
- 用 capability / change / requirement 三层组织答案
- 给用户下一步导航提示

## 参考资料

- [reference/SEARCH_PATTERNS.md](reference/SEARCH_PATTERNS.md)
