# Recipe: Rename execute-implementation and verify-before-completing

Blueprint: tasks/current/blueprint.md
Date: 2026-02-13

## Step 1: Rename directories
- Use `git mv` to preserve history:
  - `git mv skills/execute-implementation skills/follow-recipe`
  - `git mv skills/verify-before-completing skills/verify-work`
- Tests: Both new directories exist, old ones don't

## Step 2: Update frontmatter and internal references in renamed skills
- Files:
  - `skills/follow-recipe/SKILL.md` — update `name:` frontmatter, heading, and all internal references to `verify-before-completing` → `verify-work`
  - `skills/verify-work/SKILL.md` — update `name:` frontmatter and heading
- Tests: `name:` field matches new directory name in both

## Step 3: Update all cross-references in other files
- Files:
  - `CLAUDE.md`
  - `README.md`
  - `docs/hym-design.md`
  - `skills/load/SKILL.md`
  - `skills/write-task-recipe/SKILL.md`
  - `skills/open-pr/SKILL.md`
  - `active-plan/implementation-plan.md`
- Action: Replace all occurrences of `execute-implementation` → `follow-recipe` and `verify-before-completing` → `verify-work`
- Tests: `grep -r "execute-implementation\|verify-before-completing"` returns zero results
