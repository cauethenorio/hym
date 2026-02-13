---
name: brainstorm
description: Use before any creative work - creating features, building components, adding functionality, or modifying behavior. Use to explore ideas before committing to a direction.
---

# Brainstorming

## Overview

Explore ideas before having enough clarity for a ticket or design. Turn vague ideas into clear directions through collaborative dialogue.

## Before Anything Else

1. Read CLAUDE.md and repo structure
2. Read `.claude/project-instructions/brainstorm.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill
3. Check recent commits to understand current project state

## Process

### Understanding the Idea

- Ask questions one at a time to understand the idea
- Prefer multiple choice when possible, but open-ended is fine
- Only one question per message
- Focus on: purpose, constraints, success criteria
- Read relevant source code if it helps ask better questions

### Exploring Approaches

- Propose 2-3 different approaches with trade-offs
- Lead with your recommended option and explain why
- Present options conversationally
- Let the dev choose — don't push

### Presenting the Design

- Once you understand what you're exploring, present it in sections
- Each section: 200-300 words
- Ask after each section: "Does this look right so far?"
- Be ready to go back and revise

### Suggesting Next Steps

When the idea is mature enough, suggest:

- Ready for a ticket? → `hym:create-ticket`
- Ready for design? → `hym:write-task-blueprint`
- Need more exploration? → keep brainstorming

## Key Principles

- **One question at a time** — don't overwhelm
- **Multiple choice preferred** — easier to answer
- **YAGNI ruthlessly** — remove unnecessary features from all designs
- **Explore alternatives** — always propose 2-3 approaches before settling
- **Incremental validation** — present in sections, validate each
- **No output pressure** — brainstorming can end as just a conversation
