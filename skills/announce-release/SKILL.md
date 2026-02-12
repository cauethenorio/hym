---
name: announce-release
description: Use when you need to create a release summary message to notify the team about what was deployed. Use after a deploy for quick team communication.
---

# Release Announcement

## Overview

Create a release message to communicate the team about what was deployed. A concise summary — not a detailed thread.

## Before Anything Else

1. Read `.claude/project-instructions/announce-release.md` if it exists
   - This tells you the format, channel, and tone for the project
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Identify what was deployed**
   - Read recent commits, PRs, or release tags
   - Match changes to tickets if possible

2. **Generate the message**
   - Summary of what changed (2-5 bullet points)
   - Impact for users
   - Relevant links (PR, ticket, docs)
   - Keep it concise — this is a notification, not documentation

3. **Present for review**
   - Show the message to the dev
   - Ask for adjustments

4. **Publish**
   - Use the channel/tool specified in project instructions
   - If no tool specified, just output the message for the dev to post manually

## Next Step

After the announcement is posted, suggest: `hym:draft-release-thread` if the team wants a detailed per-ticket breakdown too.

## Key Principles

- **Concise** — a few bullets, not a wall of text
- **Impact-focused** — what changed for users, not what code was modified
- **Links for context** — anyone who wants details can click through
