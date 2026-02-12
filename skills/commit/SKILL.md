---
name: commit
description: Use when committing code changes, staging files, or writing commit messages. Use to ensure commits follow project conventions and are atomic.
---

# Commit

## Overview

Create clean, atomic commits that follow the project's conventions. One logical change per commit.

## Before Anything Else

1. Read `.claude/project-instructions/commit.md` if it exists
   - This tells you the project's commit message format and conventions
   - These instructions are ADDITIVE — they do not replace this skill
2. If no project instructions exist, use conventional commits as default

## Process

1. **Review changes**
   - Run `git status` to see all changed files
   - Run `git diff` to see staged and unstaged changes
   - Understand what changed and why

2. **Stage files**
   - Stage files that belong to the same logical change
   - Prefer `git add <specific files>` over `git add .`
   - Never stage files that contain secrets (.env, credentials)
   - If changes span multiple logical units, split into multiple commits

3. **Draft commit message**
   - Follow the project's format (from project instructions)
   - Default format if none specified:
     ```
     <type>: <short description>

     <optional body explaining why>
     ```
   - Types: `feat`, `fix`, `ref`, `docs`, `test`, `chore`
   - Focus on the "why", not the "what" — the diff shows the what

4. **Confirm with the dev**
   - Show the staged files and proposed message
   - Wait for approval before executing
   - Never commit without confirmation

5. **Execute the commit**
   - Create the commit
   - Run `git status` to verify success

## Key Principles

- **Atomic** — one logical change per commit
- **Never commit secrets** — check for .env, credentials, API keys
- **Confirm first** — always show the dev what you're about to commit
- **Message explains why** — the diff shows what changed, the message explains the reason
- **Project conventions win** — if project instructions specify a format, use it
