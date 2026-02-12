---
name: onboard
description: Use when a developer is new to the project and needs to understand the codebase, workflows, or set up their environment. Use for guided project onboarding.
---

# Onboarding

## Overview

Guide a new developer through understanding the project and getting set up. Orchestrates the onboarding experience by invoking specialized skills.

## Before Anything Else

1. Read CLAUDE.md (root + subprojects)
2. Read `.claude/project-instructions/onboard.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Welcome**
   - Briefly explain the project (from CLAUDE.md)
   - Explain that this project uses Hym to guide development

2. **Ask what the dev needs**
   - Understand the project architecture and domain? → explain it
   - Learn the development workflow? → invoke `hym:explain-workflows`
   - Set up the local environment? → invoke `hym:set-up-environment`
   - All of the above? → guide through each step

3. **Guide through chosen topics**
   - One topic at a time
   - Check if the dev has questions before moving on
   - Don't rush — understanding is the goal

4. **Wrap up**
   - Summarize what was covered
   - Suggest: "Ready to start? Try `hym:start-work`"

## Key Principles

- **One topic at a time** — don't overwhelm a new dev
- **Check for questions** — pause after each section
- **Use what exists** — CLAUDE.md already has a lot of context, use it
- **Delegate to specialists** — invoke explain-workflows and set-up-environment instead of doing everything inline
