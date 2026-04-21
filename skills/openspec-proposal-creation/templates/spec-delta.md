# 规范差异：{capability-name}

本文件定义对 `openspec/specs/{capability-name}/spec.md` 的需求变更。

## ADDED Requirements

### Requirement: {新增需求名称}
WHEN {触发条件},
系统 SHALL {动作与结果}。

#### Scenario: {成功场景名称}
GIVEN {前置条件}
WHEN {动作}
THEN {期望结果}

#### Scenario: {错误场景名称}
GIVEN {错误前置条件}
WHEN {动作}
THEN {错误处理结果}

## MODIFIED Requirements

### Requirement: {现有需求名称}
WHEN {更新后的触发条件},
系统 SHALL {更新后的动作与结果}。

#### Scenario: {更新后的成功场景}
GIVEN {前置条件}
WHEN {动作}
THEN {期望结果}

#### Scenario: {更新后的错误场景}
GIVEN {错误前置条件}
WHEN {动作}
THEN {错误处理结果}

> 对于 MODIFIED，必须提供完整更新后的 Requirement 与全部场景，不要只写“旧行为改成新行为”。

## REMOVED Requirements

### Requirement: {被移除的需求名称}
**Reason**: {为什么移除}

**Migration**: {用户、调用方或后续实现应如何迁移}

## Notes

- 新能力使用 `ADDED Requirements`
- 修改现有行为使用 `MODIFIED Requirements`
- 弃用或删除能力使用 `REMOVED Requirements`
- 每条 Requirement 至少包含两个 Scenario
- Requirement 标题必须使用 `### Requirement:`
- Scenario 标题必须使用 `#### Scenario:`
