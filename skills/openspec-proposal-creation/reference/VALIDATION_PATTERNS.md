# 验证模式

使用 grep 与 bash 的模式，在不依赖外部 CLI 工具的情况下验证提案结构。

## 目录结构验证

```bash
test -d openspec/changes/{change-id} && echo "✓ 目录存在" || echo "✗ 目录缺失"
```

## 提案文件验证

```bash
grep -c "## Why" openspec/changes/{change-id}/proposal.md
grep -c "## What Changes" openspec/changes/{change-id}/proposal.md
grep -c "## Impact" openspec/changes/{change-id}/proposal.md
```

## 设计文件验证

```bash
grep -c "## 设计决策\|## Design Decisions" openspec/changes/{change-id}/design.md
grep -c "## 备选方案\|## Alternatives" openspec/changes/{change-id}/design.md
```

## 任务文件验证

```bash
grep -n "^- \[ \]\|^- \[x\]" openspec/changes/{change-id}/tasks.md
```

**预期**：任务应为可执行的 Markdown checklist，且顺序能直接支持实施阶段执行。

## 规范差异验证

```bash
grep -c "## ADDED\|MODIFIED\|REMOVED" openspec/changes/{change-id}/specs/**/*.md
grep -n "### Requirement:" openspec/changes/{change-id}/specs/**/*.md
grep -n "#### Scenario:" openspec/changes/{change-id}/specs/**/*.md
```

## 场景数量验证

```bash
REQS=$(grep -c "### Requirement:" openspec/changes/{change-id}/specs/**/*.md)
SCENARIOS=$(grep -c "#### Scenario:" openspec/changes/{change-id}/specs/**/*.md)
MIN_SCENARIOS=$((REQS * 2))

if [ "$SCENARIOS" -lt "$MIN_SCENARIOS" ]; then
  echo "⚠ 场景数不足：$SCENARIOS / $MIN_SCENARIOS"
fi
```

## 结构清单

```text
- [ ] proposal.md 包含 Why / What Changes / Impact
- [ ] design.md 记录关键设计决策与备选方案
- [ ] tasks.md 是有序 checklist
- [ ] spec delta 使用 ADDED/MODIFIED/REMOVED Requirements
- [ ] 每条 Requirement 至少两个 Scenario
```
