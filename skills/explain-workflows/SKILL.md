---
name: explain-workflows
description: Use when a developer wants to understand how Hym works, what skills are available, or how the development workflow is organized in this project.
---

# Explain Workflows

## Overview

Explain how Hym works and what skills are available. Teach the dev the development workflow used in this project.

## Before Anything Else

1. Read all installed Hym skills (scan the skills directory)
2. Read `.claude/project-instructions/` to understand project-specific adaptations
3. Read CLAUDE.md for workflow section

## Process

1. **Explain the development cycle**
   - Walk through the phases: context → design → implementation → review → delivery
   - Show which skills are used at each phase
   - Explain how skills chain together

2. **Show how to invoke skills**
   - Example: `hym:start-work`, `hym:write-task-rfc`, etc.
   - Explain that skills can be used standalone or in sequence

3. **Explain project-specific adaptations**
   - What project instructions exist and what they customize
   - Which tool skills are configured (Linear, GitHub, etc.)

4. **Answer questions**
   - Let the dev ask about specific skills or workflows
   - Be patient — this is a learning conversation

## Next Step

After explaining workflows, suggest: `hym:start-work` to begin a task, or `hym:set-up-environment` if they haven't set up yet.

## Key Principles

- **Start with the big picture** — show the full cycle before diving into details
- **Use project context** — explain what THIS project uses, not abstract concepts
- **Be practical** — show real invocation examples
