---
name: archive-task
description: Use when a PR has been merged and you need to archive task artifacts, clean up the development branch, and finalize the task.
---

# Archive Task

## Overview

Finalize work on a development branch after merge. Archive artifacts to institutional memory, clean up working files, and leave the repo in a clean state.

## Before Anything Else

1. Read `.claude/project-instructions/archive-task.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Verify state**
   - PR is approved and merged
   - Confirm with the dev if unsure

2. **Generate summary**
   - Read `tasks/current/blueprint.md` header (title, ticket, date)
   - Write a one-paragraph distillation of what was done and why
   - Get the PR link from the merged PR
   - Save as `tasks/current/summary.md` using the template below

3. **Archive artifacts**
   - Determine the archive path: `tasks/archive/YYYY-MM/YYYY-MM-DD-TICKET-short-name/`
     - `YYYY-MM-DD` is today's date
     - `TICKET` is the ticket identifier (e.g., `PROJ-123`)
     - `short-name` is a 2-4 word slug derived from the task title (e.g., `add-user-auth`)
   - Create the archive directory
   - Use `git mv` to move all files from `tasks/current/` into the archive directory (preserves git history):
     - `summary.md`
     - `blueprint.md`
     - `recipe.md`
     - `qa-steps.md`
     - Any other task artifacts present
   - Commit the archived directory with message: `chore: archive [TICKET] — [short description]`

4. **Clean up**
   - Ensure `tasks/current/` is empty (all files were moved in step 3)
   - Delete the feature branch after merge

5. **Confirm**
   - Run `git status` to verify clean state
   - Tell the dev everything is wrapped up and where the archive lives

## Summary Template

```markdown
# [ticket] — [task title]
Date: [YYYY-MM-DD]
Ticket: [link]
PR: [link]

## Summary
[One paragraph: what was done and why]

## Artifacts
- blueprint.md — Design document
- recipe.md — Implementation steps
- qa-steps.md — QA testing report
```

## Next Step

After cleanup is done, suggest:
- Ready to deploy? → `hym:announce-release` or `hym:draft-release-thread`
- Starting a new task? → `hym:start-work`

## Key Principles

- **Only after merge** — never archive or clean up before the PR is merged
- **Always archive first** — move artifacts to `tasks/archive/` before clearing `tasks/current/`
- **Commit the archive** — the archive directory is committed to git as institutional memory
- **Summary is scannable** — keep it to ~10-20 lines so AI can scan past tasks without loading full artifacts
- **Confirm before proceeding** — ask the dev if in doubt
- **Leave it clean** — no leftover working files in `tasks/current/`, no dangling branches
