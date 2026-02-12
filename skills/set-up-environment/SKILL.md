---
name: set-up-environment
description: Use when a developer needs to set up their local development environment, install dependencies, configure services, or verify their setup works.
---

# Setup Environment

## Overview

Help the dev set up their local development environment. Check prerequisites, guide through setup, verify everything works.

## Before Anything Else

1. Read CLAUDE.md — it usually has setup instructions
2. Read `.claude/project-instructions/set-up-environment.md` if it exists
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Check prerequisites**
   - Runtime versions (Node, Go, Python, etc.)
   - Package managers (npm, pip, etc.)
   - Docker and docker-compose
   - Other required tools (make, etc.)
   - Report what's installed and what's missing

2. **Guide through setup**
   - Install missing dependencies
   - Configure environment variables (.env files)
   - Set up local services (databases, caches, etc.)
   - Configure hosts file, certificates, etc.
   - One step at a time, verifying each

3. **Run verification checks**
   - Build the project
   - Run tests
   - Start the local server
   - Hit a health endpoint if available

4. **Report**
   - What's working
   - What needs attention
   - Known issues or workarounds

## Next Step

After environment is verified, suggest: `hym:start-work` to begin a task.

## Key Principles

- **Don't assume** — check what's installed before suggesting commands
- **One step at a time** — verify each step before moving to the next
- **CLAUDE.md is the source** — it should have the setup instructions
- **Report honestly** — if something doesn't work, say so with context
