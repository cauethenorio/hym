---
name: open-pr
description: Use when implementation is complete and you need to create or open a pull request for review. Use after verify-before-completing.
---

# Open PR

## Overview

Prepare, create, and open a pull request for review. Ensures the PR includes the RFC, QA steps, and a clear description of what was done.

## Before Anything Else

1. Read CLAUDE.md and `.claude/project-instructions/open-pr.md` if it exists
   - This tells you PR title format, reviewer conventions, branch naming
   - These instructions are ADDITIVE — they do not replace this skill
2. Read `active-plan/rfc.md` for context on what was built

## Process

1. **Verify preconditions**
   - Tests passing
   - Lint/build clean
   - `active-plan/rfc.md` updated with final decisions
   - All changes committed

2. **Generate QA steps**
   - Invoke `hym:generate-qa-steps` to create the QA testing report
   - Review the output with the dev

3. **Prepare the PR**
   - Compare what was done vs what was planned (RFC)
   - Generate a summary of changes
   - Identify points that deserve reviewer attention
   - Include `active-plan/rfc.md` content in the PR description
   - Attach QA steps per project instructions (PR description, comment, or ticket)

4. **Create the PR**
   - Use the tool specified in project instructions (gh CLI by default)
   - Title format: follow project conventions
   - Body: summary + RFC content + QA steps location
   - Present the PR to the dev before creating

5. **After creation**
   - Return the PR URL
   - Suggest assigning reviewers if project instructions specify who

## Next Step

After the PR is created, tell the dev: when review feedback comes in, use `hym:address-pr-feedback` to process it.

## Key Principles

- **RFC goes in the PR** — the context for the review is the design
- **QA steps are always generated** — even if small, they help reviewers
- **Explain the why** — PR description focuses on reasoning, not line-by-line changes
- **Confirm before creating** — show the dev the PR details first
