---
name: write-task-recipe
description: Use when you have an approved blueprint and need to create a detailed technical recipe before coding. Use after write-task-blueprint and before follow-recipe.
---

# Task Recipe

## Overview

Create a detailed technical recipe from the blueprint. This is what implementation agents and developers follow step by step.

## Before Anything Else

1. Read `tasks/current/blueprint.md` — this is the source of truth for what to build
2. Read CLAUDE.md and `.claude/project-instructions/write-task-recipe.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill
3. Read relevant source code (files that will be modified or created)
4. If `tasks/current/` does not exist, create it and add `tasks/current/` to the project's `.gitignore`

## Process

1. **Analyze the blueprint**
   - Understand each acceptance criterion
   - Identify which parts of the codebase are affected
   - Note dependencies and ordering constraints

2. **Break into ordered steps**
   - Each step should be:
     - Concrete: which files to create/modify
     - Testable: what tests to write for this step
     - Independent where possible: minimize cross-step dependencies
   - Include:
     - Database migrations if needed
     - Configuration changes
     - Dependency additions

3. **Present to the dev for approval**
   - Show the full recipe
   - Highlight any decisions you made that weren't in the blueprint
   - Ask for adjustments

4. **Save the recipe**
   - Save to `tasks/current/recipe.md`

## Recipe Format

```markdown
# Recipe: [task title]
Blueprint: tasks/current/blueprint.md
Date: [YYYY-MM-DD]

## Step 1: [description]
- Files: [list]
- Tests: [what to test]
- Notes: [any context]

## Step 2: [description]
...
```

## Next Step

After the recipe is saved and approved, suggest `hym:follow-recipe` to start execution.

## Key Principles

- **Concrete, not abstract** — "modify handler X in file Y" not "update the API"
- **Ordered by dependency** — steps that depend on others come after
- **Each step is verifiable** — tests, build, or manual check
- **Small steps** — each step should be completable in one sitting
- **No surprises** — if it's not in the blueprint, flag it before adding to the recipe
