---
name: debug-systematically
description: Use when encountering any bug, test failure, or unexpected behavior. Use before proposing fixes to ensure you understand the root cause first.
---

# Systematic Debugging

## Overview

Investigate bugs in a structured way — hypothesis, evidence, fix. Never guess at fixes without understanding the root cause.

## Before Anything Else

1. Read `.claude/project-instructions/debug-systematically.md` if it exists
   - This tells you available debugging tools, database access, logging
   - These instructions are ADDITIVE — they do not replace this skill
2. Read CLAUDE.md for project-specific context

## Process

### 1. Understand the Problem

- What is the expected behavior?
- What is the actual behavior?
- When did it start happening?
- Can you reproduce it?

### 2. Reproduce

- Follow the reproduction steps
- If you can't reproduce, gather more information
- Use available tools (browser, database, logs)

### 3. Hypothesize

- Based on the symptoms, form 2-3 hypotheses about the cause
- Rank by likelihood
- Start with the most likely

### 4. Gather Evidence

- Read the relevant code
- Check logs
- Query the database (if available via tool skills)
- Add temporary logging if needed
- Each hypothesis should have evidence for or against

### 5. Identify Root Cause

- Follow the evidence to the actual cause
- Don't stop at symptoms — find why it happens
- Verify your understanding: "If X is the cause, then Y should also be true"

### 6. Fix and Verify

- Propose the fix — explain why it addresses the root cause
- Implement the fix
- Write a test that catches this bug (regression test)
- Verify the fix works
- Verify no regression was introduced

## Key Principles

- **Hypothesize before fixing** — never change code without a theory
- **Evidence over intuition** — prove your hypothesis with data
- **Root cause, not symptoms** — don't patch over the real problem
- **Regression test** — ensure this bug can't come back
- **One change at a time** — don't make multiple changes and hope one works
