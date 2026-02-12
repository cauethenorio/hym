---
name: create-ticket
description: Use when creating a new ticket, filing an issue, or when a task needs to be tracked in the project's issue management system.
---

# Create Ticket

## Overview

Create a ticket in the project's issue tracking system from collected context.

## Before Anything Else

1. Read `.claude/project-instructions/create-ticket.md` if it exists
   - This tells you which tool to use (Linear, GitHub Issues, Jira, etc.)
   - And the project's conventions (labels, priorities, templates)
   - These instructions are ADDITIVE — they do not replace this skill
2. Read CLAUDE.md for general project context

## Process

1. **Gather context**
   - Use conversation context if invoked from `hym:start-work`
   - If invoked standalone, ask the dev to describe the task
   - Understand: what, why, who it affects, expected outcome

2. **Draft the ticket**
   - Title: imperative verb + specific noun + context
   - Description:
     - Context / Background
     - Expected behavior
     - Current behavior (for bugs)
     - Scope: what's in and what's out
   - Acceptance criteria: observable, verifiable outcomes

3. **Present for review**
   - Show the draft to the dev
   - Ask for adjustments
   - Do not create until the dev approves

4. **Create the ticket**
   - Use the tool specified in project instructions
   - If no tool is specified, ask the dev which system to use
   - Return the ticket link

## Next Step

After the ticket is created, suggest: `hym:write-task-rfc` to design the solution.

## Guidelines

- One ticket = one reviewable PR
- If the title needs "and", it's probably two tickets
- Acceptance criteria should read like test cases
- Write for someone who has zero context
- Don't over-specify implementation details — focus on the outcome
