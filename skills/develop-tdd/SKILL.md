---
name: develop-tdd
description: Use when implementing any feature or bugfix to follow TDD - write tests first, then implementation, then refactor. Use before writing implementation code.
---

# Test-Driven Development

## Overview

Implement functionality following TDD — tests first, then code, then refactor. Red-green-refactor.

## Before Anything Else

1. Read `.claude/project-instructions/develop-tdd.md` if it exists
   - This tells you testing frameworks, conventions, file locations
   - These instructions are ADDITIVE — they do not replace this skill
2. Read `active-plan/rfc.md` for acceptance criteria (if it exists)

## Process

### Red: Write Failing Tests

1. Pick one acceptance criterion or behavior
2. Write a test that describes the expected behavior
3. Run the test — confirm it fails
4. If it passes, the behavior already exists — move to the next

### Green: Make It Pass

1. Write the minimum code to make the test pass
2. Don't over-engineer — just make it green
3. Run the test — confirm it passes
4. Run all tests — confirm nothing else broke

### Refactor

1. Look at the code you just wrote
2. Is there duplication? Unclear naming? Unnecessary complexity?
3. Refactor if needed — tests protect you
4. Run all tests again after refactoring

### Repeat

- Pick the next acceptance criterion
- Continue until all criteria are covered

## Key Principles

- **Test first, always** — write the test before the implementation
- **Minimum to pass** — don't write more code than the test requires
- **One behavior at a time** — don't test everything at once
- **Tests are documentation** — they describe what the code does
- **Refactor with confidence** — green tests mean you can change safely
- **Run all tests often** — catch regressions early
