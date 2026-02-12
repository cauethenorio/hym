---
name: start-work
description: Use when starting a new task, beginning work on a ticket, or when you need to establish context before writing code. Use before any implementation begins.
---

# Start Work

## Overview

Orchestrate the start of a task. Ensure context and understanding exist before any code is written.

## Before Anything Else

1. Read the repository state silently:
   - CLAUDE.md (root + subprojects)
   - Recent commits (`git log --oneline -20`)
   - Directory structure
   - Check if `active-plan/rfc.md` exists

2. Read project instructions:
   - Look for `.claude/project-instructions/start-work.md`
   - If found, incorporate those instructions into your behavior
   - These instructions are ADDITIVE — they do not replace this skill

3. If `active-plan/rfc.md` already exists:
   - Tell the dev there's active work in progress
   - Show a brief summary of what it contains
   - Ask: continue that work, or start something new?

## Understanding the Task

Start a conversation to understand what the dev wants to do.

- Open with: "What are you going to work on?"
- One question at a time
- Prefer multiple choice when possible
- Ask about:
  - What needs to be done and why
  - Who asked for it, what's the motivation
  - Is there a ticket, PRD, Figma, Slack thread, or any reference?
  - What's the expected outcome from the user's perspective
- If the dev provides a link, read it and use as context
- If the dev describes the task without links, work with what they give
- Do NOT ask for a link if the dev has already explained enough

## Ticket

Once you understand the task:

- If the dev mentioned an existing ticket — read it, confirm understanding
- If no ticket exists — offer to create one using `hym:create-ticket`
  - Pass all context gathered so far
- If the dev doesn't want a ticket — that's fine, continue

## Deepening Understanding

With repo context + ticket + what the dev said, go deeper:

- If the idea is still vague or the dev is exploring — invoke `hym:brainstorm` to clarify direction first
- If the task is clear enough, continue:
  - Cross-reference what you know about the codebase with the task
  - Ask about what the ticket or dev did NOT say explicitly:
    - Ambiguities and open decisions
    - Edge cases and expected behaviors
    - Scope: what's in and what's out
    - Impact on existing functionality
  - Read relevant source files if it helps you ask better questions
  - Stop when you feel you have enough clarity to describe the solution

## Handoff to Design

When understanding is sufficient:

- Summarize what you understood in 3-5 bullet points
- Ask the dev to confirm
- If confirmed — invoke `hym:write-task-rfc` with all accumulated context
- If not — keep asking until alignment is reached

## Key Principles

- **Read before asking** — always read repo state before the first question
- **Conversation, not interrogation** — this should feel natural
- **One question at a time** — never overwhelm
- **Respect what exists** — if there's an active plan, acknowledge it
- **Project instructions are additive** — they enrich, never override
- **Don't force links** — a verbal description is valid context
- **Know when to stop asking** — move to design when clarity is sufficient
