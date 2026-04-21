# OpenSpec 搜索模式

本文件汇总针对官方 OpenSpec 目录结构的常见搜索模式，用于快速回答“有哪些规范”“哪些变更正在进行”“某项能力定义在哪里”等问题。

## 1. 列出能力

```bash
find openspec/specs -mindepth 1 -maxdepth 1 -type d -exec basename {} \;
```

## 2. 列出进行中的 change

```bash
find openspec/changes -maxdepth 1 -type d -not -path "openspec/changes" -not -path "*/archive"
```

## 3. 列出归档历史

```bash
find openspec/changes/archive -maxdepth 1 -mindepth 1 -type d -exec basename {} \;
```

## 4. 搜索 Requirement

```bash
grep -r "### Requirement:" openspec/specs/
```

## 5. 搜索 Scenario

```bash
grep -r "#### Scenario:" openspec/specs/
```

## 6. 搜索进行中 change 的摘要

```bash
for change in openspec/changes/*/; do
  if [ "$change" != "openspec/changes/archive/" ]; then
    echo "=== $(basename "$change") ==="
    grep -A 3 "## Why" "$change/proposal.md"
  fi
done
```

## 7. 判断 change 是否接近可归档

```bash
grep -n "^- \[ \]\|^- \[x\]" openspec/changes/{change-id}/tasks.md
```

## 8. 搜索特定 capability 的规范

```bash
cat openspec/specs/{capability}/spec.md
```

## 使用建议

- 先给高层概览，再按用户需要下钻
- 优先总结，不要直接倾倒大段原文
- 如果仓库不存在 `openspec/` 目录，要明确告诉用户当前项目尚未建立官方 OpenSpec 结构
