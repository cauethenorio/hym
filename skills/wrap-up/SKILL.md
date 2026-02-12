---
name: wrap-up
description: Use when a PR has been merged and you need to clean up the development branch, remove plan artifacts, and finalize the task.
---

# Wrap Up

## Overview

Finalize work on a development branch after merge. Clean up artifacts and leave the repo in a clean state.

## Before Anything Else

1. Read `.claude/project-instructions/wrap-up.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Verify state**
   - PR is approved and merged
   - Confirm with the dev if unsure

2. **Clean up artifacts**
   - Remove `active-plan/` directory:
     - `active-plan/rfc.md`
     - `active-plan/implementation-plan.md`
     - `active-plan/qa-steps.md`
   - Delete the feature branch after merge

3. **Confirm**
   - Run `git status` to verify clean state
   - Tell the dev everything is wrapped up

## Next Step

After cleanup is done, suggest:
- Ready to deploy? → `hym:announce-release` or `hym:draft-release-thread`
- Starting a new task? → `hym:start-work`

## Key Principles

- **Only after merge** — never clean up before the PR is merged
- **Confirm before deleting** — ask the dev if in doubt
- **Leave it clean** — no leftover plan files, no dangling branches
