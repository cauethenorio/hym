---
name: generate-qa-steps
description: Use when you need to generate QA testing steps from code changes, after implementation is complete, or as part of opening a PR. Use to create detailed testing reports.
---

# Generate QA Steps

## Overview

Generate a detailed QA testing report based on the RFC, implementation plan, and actual code changes. Covers happy paths, edge cases, and areas touched by refactors.

## Before Anything Else

1. Read `.claude/project-instructions/generate-qa-steps.md` if it exists
   - This tells you the project's QA format, where to publish, testing focus
   - These instructions are ADDITIVE — they do not replace this skill
2. Read `active-plan/rfc.md` (acceptance criteria)
3. Read `active-plan/implementation-plan.md` (what was planned)

## Process

1. **Analyze changes**
   - Run `git diff` against the base branch
   - Identify all modified files and their purpose
   - Cross-reference with the RFC acceptance criteria
   - Note areas touched by refactors (regression risk)

2. **Generate QA steps**
   - For each acceptance criterion:
     - Happy path steps (step-by-step, verifiable)
     - Edge cases to check
   - For refactored areas:
     - Regression checks (existing behavior still works)
   - For integration points:
     - Cross-feature impacts

3. **Format the report**
   ```markdown
   # QA Steps: [task title]
   Date: [YYYY-MM-DD]

   ## Prerequisites
   [Setup needed before testing]

   ## Scenario 1: [acceptance criterion]
   ### Happy Path
   1. [step]
   2. [step]
   - Expected: [outcome]

   ### Edge Cases
   - [case]: [expected behavior]

   ## Regression Checks
   - [area]: [what to verify]
   ```

4. **Save and publish**
   - Save to `active-plan/qa-steps.md`
   - Ask the dev where to publish:
     - PR description/comment
     - Ticket comment
     - Local file only

## Default Behavior

This skill is invoked automatically by `hym:open-pr`. Can also be invoked standalone.

## Key Principles

- **Test what changed** — focus on actual changes, not the whole app
- **Refactors need regression checks** — code that moved or was restructured can break
- **Steps must be actionable** — a QA person should be able to follow them without asking questions
- **Include prerequisites** — what needs to be set up before testing
