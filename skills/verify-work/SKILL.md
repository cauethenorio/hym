---
name: verify-work
description: Use when about to claim work is complete, fixed, or passing. Use before committing final changes or opening a PR to verify everything actually works.
---

# Verify Work

## Overview

Final check before declaring work as complete. Evidence before assertion. Never claim something works without proving it.

## Before Anything Else

1. Read `.claude/project-instructions/verify-work.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill
2. Read `tasks/current/blueprint.md` for acceptance criteria

## Process

1. **Run all verifications**
   - Tests pass (`go test`, `npm test`, etc.)
   - Build compiles without errors
   - Lint passes without warnings
   - Type checks pass (if applicable)

2. **Check acceptance criteria**
   - For each criterion in the blueprint:
     - Is it implemented?
     - Is it tested?
     - Does it work?
   - If any criterion is not met, report it

3. **Compare plan vs reality**
   - What was planned in `tasks/current/recipe.md`
   - What was actually implemented
   - Note any deviations and whether they were justified

4. **Update blueprint if implementation deviated**
   - If the implementation differs from what the blueprint describes (scope changed, approach changed, criteria adjusted):
     - Update the relevant sections in `tasks/current/blueprint.md` to reflect reality
     - Append a changelog entry to the end of the document:
       ```markdown
       ## Changelog
       - [YYYY-MM-DD] [description of what changed and why]
       ```
     - If a changelog section already exists, append to it
   - If no deviations — skip this step

5. **Report**
   - List each verification and its result (pass/fail)
   - List each acceptance criterion and its status
   - List any blueprint updates made (if applicable)
   - Be honest — if something failed, say so

6. **Decision**
   - All pass → declare ready
   - Something failed → report what failed, do NOT declare ready

## Next Step

- All verifications pass → suggest `hym:open-pr` to open the pull request
- Something failed → report failures, do NOT suggest open-pr

## Key Principles

- **Evidence before assertion** — run the command, show the output
- **Never fake it** — if tests fail, report the failure
- **Check everything** — tests, build, lint, acceptance criteria
- **Deviations are OK if justified** — but they must be noted
- **This is the last line of defense** — catch problems before the reviewer does
