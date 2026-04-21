# AGENTS.md - OpenSpec Skills Repository

## Repository Purpose

This is an **OpenCode skills repository** containing 4 skills that implement an OpenSpec specification-driven development workflow. These skills are used by OpenCode agents to create, implement, and archive structured change proposals with formal requirements.

## Skills Overview

| Skill | Purpose | Trigger Keywords |
|-------|---------|------------------|
| `openspec-proposal-creation` | Create change proposals with design, tasks, and spec deltas | `openspec提案`, `规划`, `创建提案`, `新功能` |
| `openspec-implementation` | Implement approved proposals against design and task checklist | `openspec开发`, `开发`, `实施`, `实现提案` |
| `openspec-context-loading` | Query specs, changes, capabilities | `openspec上下文`, `有哪些规范`, `显示变更` |
| `openspec-archiving` | Archive completed changes and merge spec deltas | `openspec归档`, `归档提案`, `合并规范` |

Each skill is self-contained in `skills/<skill-name>/SKILL.md` with reference materials in subdirectories.

## OpenSpec Workflow

```
Proposal Creation → Implementation → Archiving
     ↓                   ↓                ↓
  openspec/changes/   tasks.md      openspec/changes/archive/
  + design.md         + TodoWrite   + merge to openspec/specs/
```

## Expected Project Structure

OpenSpec methodology expects projects to use this directory structure:

```
openspec/
├── specs/                    # Living specifications (capabilities)
│   ├── {capability}/         # e.g., authentication, billing
│   │   └── spec.md           # Capability specification
│   └── ...
├── changes/                  # In-progress changes
│   ├── {change-id}/          # e.g., add-user-auth
│   │   ├── proposal.md       # Why/What/Impact
│   │   ├── design.md         # Design decisions and tradeoffs
│   │   ├── tasks.md          # Implementation checklist
│   │   ├── specs/            # Spec deltas
│   │   │   └── {capability}/
│   │   │       └── spec-delta.md
│   └── archive/              # Archived changes (timestamped)
│       └── {YYYY-MM-DD}-{change-id}/
```

## Key Commands

### Context Loading
```bash
# List all capabilities
find openspec/specs -mindepth 1 -maxdepth 1 -type d -exec basename {} \;

# List in-progress changes
find openspec/changes -maxdepth 1 -type d -not -path "openspec/changes" -not -path "*/archive"

# Search requirements
grep -r "### Requirement:" openspec/specs/

# Show change summary
cat openspec/changes/{change-id}/proposal.md
```

### Proposal Validation
```bash
# Check delta operations exist
grep -c "## ADDED\|MODIFIED\|REMOVED" openspec/changes/{change-id}/specs/**/*.md

# Verify requirement format
grep -n "### Requirement:" openspec/changes/{change-id}/specs/**/*.md

# Verify scenario format (must use 4 #)
grep -n "#### Scenario:" openspec/changes/{change-id}/specs/**/*.md
```

## EARS Requirements Format

Requirements use the EARS (Easy Approach to Requirements Syntax) format:

```markdown
### Requirement: {name}
WHEN {trigger condition},
系统 SHALL {action and result}。

#### Scenario: {positive scenario}
GIVEN {preconditions}
WHEN {action}
THEN {expected result}
```

Key constraints:
- Use **SHALL** for binding requirements (avoid SHOULD/MAY)
- Each requirement needs **at least 2 scenarios** (positive + error)
- Scenarios use `#### Scenario:` (4 hash marks, not 3)

Delta operations:
- `## ADDED Requirements` - new capabilities
- `## MODIFIED Requirements` - behavior changes (include full updated text)
- `## REMOVED Requirements` - deprecated features

## Skills are Chinese-Language

All skill content, templates, and references are in **Simplified Chinese**. Keywords for triggering skills are also Chinese.

## Working with This Repository

- **No build/test commands** - this is a documentation/templates repo, not a software project
- Skills are loaded by OpenCode when matching keywords are detected
- Reference files provide detailed patterns for each skill domain
- Templates for proposals, design, tasks, and spec-deltas are in `skills/openspec-proposal-creation/templates/`

## Important Conventions

1. **Change IDs** must be URL-safe and descriptive: `add-feature`, `fix-bug`, `update-component`
2. **Tasks in implementation** must be executed in order; each task verified before marking complete
3. **Archiving is final** - archived changes are immutable historical records
4. **Spec deltas merge to living specs** during archiving, not during implementation
5. **Implementation completion** is reflected by the `tasks.md` checklist and implementation report, not an extra marker file
