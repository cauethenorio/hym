---
name: draft-release-thread
description: Use when you need to create a detailed per-ticket Slack thread for a release, with individual messages explaining each change's impact. Use for thorough team communication.
---

# Release Thread

## Overview

Create a detailed Slack release thread with a thread opener and individual per-ticket messages. More detailed than `hym:announce-release` — each change gets its own message explaining what, how, and impact.

## Before Anything Else

1. Read `.claude/project-instructions/draft-release-thread.md` if it exists
   - This tells you the format, channel, tone, and ticket system
   - These instructions are ADDITIVE — they do not replace this skill

## Process

1. **Gather ticket context**
   - Identify all tickets included in the release
   - Read each ticket for context (description, acceptance criteria)
   - Read the associated PRs for what actually changed

2. **Create thread opener**
   - 3-5 sentence overview of the release
   - What areas were affected
   - Any notable changes or risks

3. **Create per-ticket messages**
   - For each ticket:
     ```
     **What changed:** [1-2 sentences]
     **How it works:** [brief technical explanation]
     **Impact:** [who is affected and how]
     ```
   - Focus on business impact and user experience
   - Include screenshots or links if available

4. **Present for review**
   - Show all messages to the dev
   - Ask for adjustments

5. **Save output**
   - Save messages as markdown files per project instructions
   - Or output for manual posting

## Next Step

After the thread is drafted and posted, this workflow is complete. Suggest `hym:start-work` if the dev is picking up a new task.

## Key Principles

- **One message per ticket** — keeps the thread scannable
- **Business impact first** — explain pain eliminated, not code written
- **Thread opener sets context** — someone should understand the release from the opener alone
- **Include visuals** — screenshots, links to demos when available
