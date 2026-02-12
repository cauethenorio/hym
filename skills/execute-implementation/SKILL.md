---
name: execute-implementation
description: Use when you have an approved implementation plan and are ready to start coding. Use after write-implementation-plan to execute step by step with review checkpoints.
---

# Execute Implementation

## Overview

Execute the implementation plan step by step, with review checkpoints. Follow the plan, verify each step, and update living documents as decisions evolve.

## Before Anything Else

1. Read `active-plan/implementation-plan.md` — this is what you're executing
2. Read `active-plan/rfc.md` — this is the context and acceptance criteria
3. Read CLAUDE.md and `.claude/project-instructions/execute-implementation.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

For each step in the plan:

1. **Mark as in progress**
   - Tell the dev which step you're working on

2. **Implement**
   - Follow the step's instructions
   - Write tests as specified in the plan
   - If you need to deviate from the plan, tell the dev why before proceeding

3. **Verify**
   - Run tests
   - Run lint/build if applicable
   - Confirm the step works as expected

4. **Checkpoint**
   - For significant steps, pause and show the dev what was done
   - Ask if it looks right before continuing
   - Not every step needs a checkpoint — use judgment

5. **Update living documents**
   - If decisions changed during implementation, update `active-plan/rfc.md`
   - Mark the step as completed in the plan

## When to Stop and Ask

- The plan doesn't cover something you encountered
- You disagree with a step in the plan
- Tests are failing and you're not sure why
- A step is significantly more complex than expected
- You discovered something that changes the design

## When All Steps Are Complete

After executing every step in the plan:

1. Invoke `hym:verify-before-completing` to check acceptance criteria against the RFC
2. If verification passes → suggest `hym:open-pr` to the dev
3. If verification fails → fix the issues before suggesting the PR

## Key Principles

- **Follow the plan** — don't improvise unless necessary
- **Verify before moving on** — each step should pass before the next begins
- **Communicate deviations** — if you change something, say so
- **Update the RFC** — if reality differs from the plan, keep the RFC current
- **Small commits** — commit after each step or logical unit of work
