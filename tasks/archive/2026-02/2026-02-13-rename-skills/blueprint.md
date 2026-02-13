# Blueprint: Rename execute-implementation and verify-before-completing

Date: 2026-02-13
Status: active

## Problem

Two skill names don't meet the project's naming conventions:

- **`execute-implementation`** — redundant (execute and implement mean nearly the same thing), doesn't connect to the artifact it consumes (`recipe.md`)
- **`verify-before-completing`** — a temporal clause, not a verb-action pattern. Every other skill uses verb-first naming (`start-work`, `open-pr`, `commit`)

The workflow chain should read naturally as a sequence of actions. Currently:
```
write-task-recipe → execute-implementation → verify-before-completing → open-pr
```

## Proposed Solution

Rename both skills and update all references:

- `execute-implementation` → **`follow-recipe`** — pairs naturally with `write-task-recipe`, names the artifact it consumes
- `verify-before-completing` → **`verify-work`** — verb-first, concise, describes the action

The chain becomes:
```
write-task-recipe → follow-recipe → verify-work → open-pr
```

Changes required:
1. **Rename directories** — `skills/execute-implementation/` → `skills/follow-recipe/`, `skills/verify-before-completing/` → `skills/verify-work/`
2. **Update SKILL.md frontmatter** — `name:` field in both skills
3. **Update all cross-references** — 11 files reference one or both names (CLAUDE.md, README.md, docs/hym-design.md, load/SKILL.md, write-task-recipe/SKILL.md, open-pr/SKILL.md, execute-implementation/SKILL.md, verify-before-completing/SKILL.md, active-plan/implementation-plan.md)

No behavioral changes. Pure rename — find-and-replace across all files.

## Scope

**In scope:**
- Rename `skills/execute-implementation/` directory → `skills/follow-recipe/`
- Rename `skills/verify-before-completing/` directory → `skills/verify-work/`
- Update `name:` frontmatter in both SKILL.md files
- Update all references across: CLAUDE.md, README.md, docs/hym-design.md, load/SKILL.md, write-task-recipe/SKILL.md, open-pr/SKILL.md, active-plan/implementation-plan.md
- Update internal cross-references within the renamed skills themselves

**Out of scope:**
- Behavioral changes to either skill
- Renaming any other skills
- Updating `.claude-plugin/plugin.json` (skill names aren't registered there)

## Acceptance Criteria

1. `skills/follow-recipe/` exists with all files from `execute-implementation/` (SKILL.md + 3 prompt templates)
2. `skills/verify-work/` exists with SKILL.md from `verify-before-completing/`
3. `skills/execute-implementation/` and `skills/verify-before-completing/` no longer exist
4. Zero occurrences of `execute-implementation` or `verify-before-completing` in the entire repo
5. All references use the new names (`follow-recipe`, `verify-work`)
6. No behavioral changes — skill content remains the same apart from name references
