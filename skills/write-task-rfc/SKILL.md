---
name: write-task-rfc
description: Use when you need to align on the design direction before coding, create a lightweight RFC for a task, or document the proposed solution and trade-offs.
---

# Task RFC

## Overview

Create a lightweight RFC (Request for Comments) that aligns direction before coding. A living document that accompanies the task through implementation.

## Before Anything Else

1. Read repository state:
   - CLAUDE.md (root + subprojects)
   - Recent commits
   - Relevant source code for what will be built
2. Read `.claude/project-instructions/write-task-rfc.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Gather requirements through questions**
   - One question at a time
   - Prefer multiple choice when possible
   - Focus on understanding the solution, not just the problem
   - Read relevant source files to ask informed questions

2. **Write the RFC section by section**
   - Present each section (200-300 words) to the dev
   - Ask if it looks right before moving to the next
   - Sections:
     - Problem / Context
     - Proposed solution
     - Alternatives considered and trade-offs
     - Scope: what's in and what's out
     - Acceptance criteria

3. **Save the document**
   - Save to `active-plan/rfc.md`
   - Do NOT commit — the RFC is a working document, not a deliverable

## Document Format

```markdown
# RFC: [task title]

Ticket: [link]
Date: [YYYY-MM-DD]
Status: active

## Problem

## Proposed Solution

## Alternatives and Trade-offs

## Scope

## Acceptance Criteria

## Implementation Notes (updated during dev)
```

## Lifecycle

- Created by this skill
- Updated during implementation (decisions change, scope adjusts)
- Transferred to the PR when `hym:open-pr` is invoked
- Removed from the repo after merge by `hym:wrap-up`

## Next Step

After the RFC is saved and approved, display the full RFC content to the dev and suggest: `hym:write-implementation-plan` to break it into executable steps.

## Key Principles

- **Lightweight** — 1-2 pages, not a dissertation
- **Incremental validation** — section by section, not all at once
- **Living document** — it changes as understanding deepens
- **Focuses on direction** — detailed implementation comes in `hym:write-implementation-plan`
- **Acceptance criteria are tests** — write them as verifiable outcomes
