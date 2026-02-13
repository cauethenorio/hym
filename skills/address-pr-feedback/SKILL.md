---
name: address-pr-feedback
description: Use when you received code review comments on a PR and need to process and address the feedback. Use to evaluate suggestions with technical rigor before implementing.
---

# Address PR Feedback

## Overview

Process code review feedback with technical rigor. Evaluate each suggestion before implementing — don't blindly accept everything.

## Before Anything Else

1. Read `.claude/project-instructions/address-pr-feedback.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill
2. Read `tasks/current/blueprint.md` for context on design decisions

## Process

1. **Read all feedback**
   - Read every comment on the PR
   - Group by: must-fix, suggestion, question, nit

2. **Evaluate each suggestion**
   - Understand what is being asked
   - Evaluate if it makes sense technically
   - Cross-reference with the blueprint — does the suggestion contradict a design decision?
   - Consider: does this improve the code or is it preference?

3. **For each suggestion, decide:**
   - **Agree** — implement the change
   - **Partially agree** — implement a modified version, explain what you changed and why
   - **Disagree** — explain why to the dev before changing anything
   - **Don't understand** — ask for clarification, don't guess

4. **Implement changes**
   - Make the agreed changes
   - Run tests to verify nothing broke
   - Commit with clear messages referencing the feedback

5. **Respond**
   - For each comment addressed, explain what you did
   - For disagreements, provide technical reasoning

## Next Step

- More feedback to address → `hym:address-pr-feedback` again
- PR approved and merged → suggest `hym:archive-task` to clean up artifacts and branch

## Key Principles

- **Technical rigor over politeness** — don't implement bad suggestions just to avoid conflict
- **Understand before acting** — never implement something you don't understand
- **Blueprint is context** — if a reviewer questions a design decision, the blueprint explains why
- **Verify after changes** — run tests after addressing feedback
- **One commit per logical group** — don't squash all feedback into one commit
