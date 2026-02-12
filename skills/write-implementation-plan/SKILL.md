---
name: write-implementation-plan
description: Use when you have an approved RFC and need to create a detailed technical implementation plan before coding. Use after write-task-rfc and before execute-implementation.
---

# Implementation Plan

## Overview

Create a detailed technical implementation plan from the RFC. This is what implementation agents and developers follow step by step.

## Before Anything Else

1. Read `active-plan/rfc.md` — this is the source of truth for what to build
2. Read CLAUDE.md and `.claude/project-instructions/write-implementation-plan.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill
3. Read relevant source code (files that will be modified or created)

## Process

1. **Analyze the RFC**
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
   - Show the full plan
   - Highlight any decisions you made that weren't in the RFC
   - Ask for adjustments

4. **Save the plan**
   - Save to `active-plan/implementation-plan.md`

## Plan Format

```markdown
# Implementation Plan: [task title]
RFC: active-plan/rfc.md
Date: [YYYY-MM-DD]

## Step 1: [description]
- Files: [list]
- Tests: [what to test]
- Notes: [any context]

## Step 2: [description]
...
```

## Next Step

After the plan is saved and approved, suggest the execution strategy:
- Tasks are mostly independent → `hym:execute-with-subagents`
- Tasks are tightly coupled → `hym:execute-implementation`

## Key Principles

- **Concrete, not abstract** — "modify handler X in file Y" not "update the API"
- **Ordered by dependency** — steps that depend on others come after
- **Each step is verifiable** — tests, build, or manual check
- **Small steps** — each step should be completable in one sitting
- **No surprises** — if it's not in the RFC, flag it before adding to the plan
