# Hym

> Joaquin Phoenix had Her. We have Hym.

## Overview

**Hym** is a system of composable skills for Claude Code that accompanies the developer through the entire development cycle — from "what am I going to work on?" to "deployed and team notified".

**Core principle:** No code is written without context, understanding, and a documented design. Hym ensures this through composable skills that guide each phase.

### Concepts

- **Workflow skills** — Skills that guide a phase of development (e.g., `start-work`, `write-task-blueprint`, `debug-systematically`). Core logic that works in any project. Installed per project.
- **Tool skills** — Skills that encapsulate how to use a specific tool (e.g., `linear`, `mongodb-atlas`, `playwright`). They abstract away whether the tool is an MCP server, a CLI, or another skill.
- **Project instructions** — Files in `.claude/project-instructions/` that each project uses to adapt skills to its context (tools, conventions, workflows).
- **Living documents** — The task blueprint (`tasks/current/blueprint.md`) and recipe (`tasks/current/recipe.md`) accompany the task, are updated during implementation, and are transferred to the PR at the end.

---

## Architecture

### Directory Structure

```
<project>/
  .claude/
    plugins/hym/
      skills/                        # Workflow skills (guide the process)
        onboard.md
        explain-workflows.md
        set-up-environment.md
        explore.md
        start-work.md
        create-ticket.md
        brainstorm.md
        write-task-blueprint.md
        write-task-recipe.md
        follow-recipe.md
        develop-tdd.md
        debug-systematically.md
        open-pr.md
        generate-qa-steps.md
        address-pr-feedback.md
        verify-work.md
        wrap-up.md
        deploy.md
        announce-release.md

      tools/                         # Tool skills (encapsulate tool knowledge)
        linear.md
        mongodb-atlas.md
        playwright.md
        github.md
        slack.md

    project-instructions/            # Project-specific instructions
      start-work.md
      create-ticket.md
      write-task-blueprint.md
      ...

  tasks/                             # Living documents (per task)
    current/
      blueprint.md
      recipe.md
      qa-steps.md
    archive/
      YYYY-MM/
        YYYY-MM-DD-TICKET-short-name/
          summary.md
          blueprint.md
          recipe.md
          qa-steps.md
```

### Rules

1. **The global skill always runs** — it is never replaced.
2. **Project instructions are additive** — they enrich, never override.
3. **Every skill, on start, reads two things:**
   - The project's CLAUDE.md (general context)
   - `.claude/project-instructions/<skill-name>.md` if it exists
4. **Skills are independent** — each one can be invoked standalone.
5. **Skills can invoke other skills** — `start-work` can call `create-ticket` and `write-task-blueprint`.
6. **Context carries forward** — when a skill invokes another, accumulated context follows naturally through the conversation history.

---

## Skills

### Onboarding & Learning

#### hym:onboard

**Purpose:** Guide a new developer through understanding the project and getting set up. Orchestrates the onboarding experience.

**Flow:**
1. Read CLAUDE.md and project instructions
2. Welcome the dev, explain what Hym is and how the project uses it
3. Ask what the dev needs:
   - Understand the project? → explain architecture + domain model
   - Understand the workflow? → invoke `hym:explain-workflows`
   - Set up the environment? → invoke `hym:set-up-environment`
   - All of the above? → guide through each step
4. One topic at a time, checking if the dev has questions before moving on
5. At the end, suggest: "Ready to start? Try `hym:start-work`"

**Inputs:** None
**Outputs:** Dev with context, environment ready

---

#### hym:explain-workflows

**Purpose:** Explain how Hym works and what skills are available. Teach the dev the development workflow used in this project.

**Flow:**
1. Read all installed Hym skills and project instructions
2. Explain the development cycle and which skills are used at each phase
3. Show examples of how to invoke skills
4. Answer questions about the workflow

**Inputs:** None
**Outputs:** Dev understands the workflow

---

#### hym:set-up-environment

**Purpose:** Help the dev set up their local development environment.

**Flow:**
1. Read CLAUDE.md and `.claude/project-instructions/set-up-environment.md` if it exists
2. Check prerequisites (runtime versions, tools, Docker, etc.)
3. Guide through setup step by step:
   - Install dependencies
   - Configure environment variables
   - Set up local services
   - Configure hosts file, certificates, etc.
   - Create `tasks/current` and `tasks/archive` directories if they don't exist
4. Run verification checks (build, tests, local server)
5. Report what's working and what needs attention

**Inputs:** None
**Outputs:** Working local environment

---

#### hym:explore

**Purpose:** Free-form exploration of the codebase. No task, no goal — just understanding.

**Flow:**
1. Read CLAUDE.md and repo structure
2. Ask the dev what they want to understand — or help them figure out what they're trying to understand
3. Through questions, help the dev clarify their own goal:
   - Are they trying to understand a specific feature?
   - How a flow works end to end?
   - How data moves through the system?
   - General architecture?
   - Something they can't articulate yet?
4. Explore the code together — read files, trace flows, explain patterns
5. One topic at a time, conversational
6. No output artifact — this is a learning conversation

**Inputs:** Curiosity
**Outputs:** Understanding

---

### Requisites & Context

#### hym:start-work

**Purpose:** Orchestrate the start of a task. Ensure context and understanding exist before any code is written.

**Flow:**
1. Read repository state (CLAUDE.md, recent commits, directory structure, `tasks/current/blueprint.md`)
2. Read `.claude/project-instructions/start-work.md` if it exists
3. If `tasks/current/blueprint.md` exists — ask the dev if they want to resume that work or start something new
4. Exploratory conversation — understand the task:
   - What needs to be done and why
   - Who asked for it, what's the motivation
   - Is there a ticket, PRD, Figma, Slack thread, or any reference?
   - What's the expected outcome from the user's perspective
   - If the dev provides a link, read it and use as context
   - If the dev describes the task without links, work with what they give
   - Do NOT ask for a link if the dev has already explained enough
5. If no ticket exists — invoke `hym:create-ticket`
6. Deepen understanding — cross-reference repo + ticket + conversation:
   - Ambiguities and open decisions
   - Edge cases and expected behaviors
   - Scope: what's in and what's out
   - Impact on existing functionality
   - Read relevant source files if it helps ask better questions
7. Summarize understanding in 3-5 bullet points, confirm with the dev
8. Invoke `hym:write-task-blueprint` with all accumulated context

**Inputs:** None required (everything comes from the conversation)
**Outputs:** Ticket created/identified + context ready for blueprint

---

#### hym:create-ticket

**Purpose:** Create a ticket in the project's issue tracking system from collected context.

**Flow:**
1. Read `.claude/project-instructions/create-ticket.md` if it exists
2. Use conversation context (or ask if invoked standalone)
3. Draft the ticket: title, description, acceptance criteria
4. Present to the dev for review
5. Create the ticket in the project's tool (Linear, GitHub Issues, etc.)
6. Return the ticket link

**Inputs:** Conversation context or verbal description
**Outputs:** Created ticket with link

---

#### hym:brainstorm

**Purpose:** Explore ideas before having enough clarity for a ticket or design. Open, creative conversation.

**Flow:**
1. Read repository state and project instructions
2. Understand the dev's idea through questions
3. Propose 2-3 approaches with trade-offs
4. Refine the chosen approach
5. When mature enough — suggest next step (ticket, blueprint, or more exploration)

**Inputs:** An idea, however vague
**Outputs:** Clear direction, ready to become a ticket or design

---

### Design

#### hym:write-task-blueprint

**Purpose:** Create a lightweight blueprint that aligns direction before coding. A living document that accompanies the task.

**Flow:**
1. Read repository state and `.claude/project-instructions/write-task-blueprint.md` if it exists
2. Read relevant source code for what will be built
3. Ask questions to gather solution requirements (one at a time, multiple choice when possible)
4. Write the blueprint section by section, validating each with the dev:
   - Problem / Context
   - Proposed solution
   - Alternatives considered and trade-offs
   - Scope: what's in and what's out
   - Acceptance criteria
5. Save to `tasks/current/blueprint.md`
6. Commit the document

**Document format:**
```markdown
# Blueprint: [task title]
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

**Lifecycle:**
- Created by `hym:write-task-blueprint`
- Updated during implementation (decisions change, scope adjusts)
- Transferred to the PR when `hym:open-pr` is invoked
- Archived to `tasks/archive/` after merge by `hym:wrap-up`

**Inputs:** Accumulated context from `start-work` or standalone conversation
**Outputs:** `tasks/current/blueprint.md` committed

---

#### hym:write-task-recipe

**Purpose:** Create a detailed technical recipe from the blueprint. This is what implementation agents follow.

**Flow:**
1. Read `tasks/current/blueprint.md` and project instructions
2. Read relevant source code (files that will be modified/created)
3. Break the solution into ordered, concrete steps:
   - Which files to create/modify
   - Which tests to write
   - Dependencies between steps
   - Migrations needed
4. Present the recipe to the dev for approval
5. Save to `tasks/current/recipe.md`

**Inputs:** Existing blueprint
**Outputs:** Detailed recipe

---

### Implementation

#### hym:follow-recipe

**Purpose:** Execute the recipe by dispatching fresh subagents per task with two-stage review (spec compliance + code quality).

**Flow:**
1. Read `tasks/current/recipe.md` and project instructions
2. For each task in the recipe:
   - Dispatch implementer subagent with full task text and context
   - Implementer builds, tests, and commits
   - Dispatch spec reviewer subagent — verify code matches recipe requirements
   - Dispatch code quality reviewer subagent — verify implementation quality
   - If reviewers find issues, implementer fixes and re-review loops until approved
3. After all tasks: final cross-task quality review
4. Invoke `hym:verify-work` to check acceptance criteria
5. Update `tasks/current/blueprint.md` if decisions change during implementation

**Inputs:** Existing recipe
**Outputs:** Implemented code, updated recipe

---

#### hym:develop-tdd

**Purpose:** Implement functionality following TDD — tests first, then code, then refactor.

**Flow:**
1. Read project instructions and task context
2. Write tests that describe the expected behavior
3. Run tests — confirm they fail
4. Implement the minimum to pass
5. Refactor if necessary
6. Repeat until acceptance criteria are covered

**Inputs:** Acceptance criteria (from blueprint or conversation)
**Outputs:** Tests + implementation

---

#### hym:debug-systematically

**Purpose:** Investigate bugs in a structured way — hypothesis, evidence, fix.

**Flow:**
1. Read project instructions and bug context
2. Reproduce the problem (or understand reproduction steps)
3. Formulate hypotheses about the cause
4. Collect evidence — logs, code, database state
5. Identify root cause
6. Propose fix and validate
7. Verify no regression was introduced

**Inputs:** Bug description, reproduction steps
**Outputs:** Root cause identified + fix applied

---

### Review & Quality

#### hym:open-pr

**Purpose:** Prepare, create, and open a pull request for review.

**Flow:**
1. Read project instructions and task context
2. Verify preconditions:
   - Tests passing
   - Lint/build clean
   - `tasks/current/blueprint.md` updated with final decisions
3. Invoke `hym:generate-qa-steps` to create the QA testing report
4. Generate summary of what was done vs what was planned
5. Identify points that deserve reviewer attention
6. Include `tasks/current/blueprint.md` content in the PR description
7. Attach QA steps per project instructions (PR description, comment, or ticket)
8. Create the PR

**Inputs:** Implemented code, blueprint, recipe
**Outputs:** PR ready for review with description and attention points

---

#### hym:generate-qa-steps

**Purpose:** Generate a detailed QA testing report based on the blueprint, recipe, and actual code changes. Covers happy paths, edge cases, and areas touched by refactors.

**Flow:**
1. Read project instructions and `.claude/project-instructions/generate-qa-steps.md` if it exists
2. Read `tasks/current/blueprint.md` (acceptance criteria)
3. Read `tasks/current/recipe.md` (what was planned)
4. Analyze actual code changes (`git diff` against base branch)
5. Generate QA steps covering:
   - Happy path for each acceptance criterion
   - Edge cases
   - Areas touched by refactors (regression risk)
   - Integration points affected
6. Save to `tasks/current/qa-steps.md`
7. Ask the dev where to publish:
   - PR description/comment
   - Ticket comment
   - Local file only

**Default behavior:** Invoked automatically by `hym:open-pr`. Can also be invoked standalone.

**Inputs:** Blueprint, recipe, git diff
**Outputs:** `tasks/current/qa-steps.md`

---

#### hym:address-pr-feedback

**Purpose:** Process code review feedback with technical rigor. Don't blindly accept — evaluate each suggestion.

**Flow:**
1. Read the feedback received
2. For each suggestion:
   - Understand what is being asked
   - Evaluate if it makes sense technically
   - If agree — implement
   - If disagree — explain why to the dev before changing
3. Never implement a suggestion that isn't understood or seems incorrect without discussion

**Inputs:** Code review feedback
**Outputs:** Adjustments implemented or grounded discussion

---

#### hym:verify-work

**Purpose:** Final check before declaring work as complete. Evidence before assertion.

**Flow:**
1. Read project instructions
2. Run verifications:
   - Tests pass
   - Build compiles
   - Lint clean
   - Acceptance criteria from blueprint are met
3. Compare what was implemented vs what was planned
4. Only declare "done" if all verifications pass
5. If something fails — report it, don't pretend it passed

**Inputs:** Finalized code, blueprint with acceptance criteria
**Outputs:** Verification report (pass/fail)

---

### Delivery

#### hym:wrap-up

**Purpose:** Finalize work on a development branch after merge. Archive task artifacts for institutional memory.

**Flow:**
1. Read project instructions
2. Verify branch state:
   - Tests pass, build ok
   - PR approved and merged
3. Archive task artifacts:
   - Generate `summary.md` from blueprint header (ticket, date, problem, solution)
   - Move files from `tasks/current/` to `tasks/archive/YYYY-MM/YYYY-MM-DD-TICKET-short-name/`
   - Commit the archive
4. Clean up:
   - Clean `tasks/current/`
   - Delete branch after merge
5. Confirm everything is clean

**Inputs:** Branch with completed and merged work
**Outputs:** Artifacts archived to `tasks/archive/`, branch deleted

---

#### hym:deploy

**Purpose:** Guide the deploy process after merge.

**Status:** TBD — each project deploys differently. To be designed with more context.

---

### Communication

#### hym:announce-release

**Purpose:** Create a release message to communicate the team about what was deployed.

**Flow:**
1. Read `.claude/project-instructions/announce-release.md` if it exists
2. Identify what was deployed (commits, PRs, tickets)
3. Generate message in the project's format:
   - Summary of what changed
   - Impact for users
   - Relevant links (PR, ticket, docs)
4. Present to the dev for review
5. Publish to the configured channel (Slack, etc.)

**Inputs:** Completed release/deploy
**Outputs:** Published message

---

## Tool Integration

### Two-Tier Skill Model

Hym has two types of skills:

```
Workflow skills (guide the process)
    │
    │  "create a ticket" / "check the database" / "test in the browser"
    │
    └── Tool skills (encapsulate tool knowledge)
            │
            ├── MCP servers (Linear, MongoDB Atlas, Playwright)
            ├── CLI tools (gh, mongosh, docker)
            └── Other skills or APIs
```

**Workflow skills** define *what* to do. **Tool skills** define *how* to do it with specific tools.

### How It Works

Workflow skills don't know about specific tools. Project instructions wire them to tool skills:

```markdown
# .claude/project-instructions/create-ticket.md

## Tools
- Use hym:linear for ticket management
```

```markdown
# .claude/project-instructions/debug-systematically.md

## Tools
- Use hym:mongodb-atlas for database inspection
- Use hym:playwright for browser reproduction
```

The tool skill encapsulates all the knowledge about how to use that tool:

```markdown
# hym:mongodb-atlas (tool skill)

## Available connections
- Production: mcp__mongodb-atlas-prod
- Staging: mcp__mongodb-atlas-staging
- Local: mongosh via CLI

## Conventions
- Always check staging before production
- Never modify production data directly
- Use read-only queries
```

### Benefits

- **Workflow skills stay generic** — they work in any project regardless of tooling
- **Tool skills are reusable** — `hym:mongodb-atlas` can be used by `debug-systematically`, `start-work`, or any skill that needs database access
- **Easy to swap tools** — switching from Linear to GitHub Issues means changing the project instructions and the tool skill, not every workflow skill
- **Tool knowledge is centralized** — connection details, conventions, and safety rules live in one place per tool

### Example Tool Skills

| Tool Skill | Abstracts |
|---|---|
| `hym:linear` | Linear MCP server, Linear API conventions |
| `hym:mongodb-atlas` | MongoDB Atlas MCP (prod/staging), mongosh CLI |
| `hym:playwright` | Playwright MCP for browser automation and testing |
| `hym:github` | `gh` CLI for PRs, issues, releases |
| `hym:slack` | Slack MCP or CLI for messaging |

---

## Adding Hym to a New Project

**Step 1 — Install Hym in the project**

Copy or link the Hym skills into the project:
```
<project>/.claude/plugins/hym/skills/
```

**Step 2 — Create the project instructions directory**
```bash
mkdir -p .claude/project-instructions
mkdir -p tasks/current tasks/archive
echo 'tasks/current/' >> .gitignore
```

**Step 3 — Create project instructions**

Only create files for skills that need customization. If the global skill's default behavior is sufficient, no file is needed.

Recommended minimum to start:
```
.claude/project-instructions/
  start-work.md          # how the project organizes tasks
  create-ticket.md       # which ticket tool and conventions
  open-pr.md             # PR conventions for the project
```

**Step 4 — Add a Workflow section to CLAUDE.md**
```markdown
## Workflow

This project uses Hym to guide the development cycle.
- Tickets: [tool and conventions]
- Branches: [naming pattern]
- PRs: [conventions]
- Deploy: [process]
```

**Step 5 — Start using**
```
hym:start-work
```

**Principle:** Start small. Add project instructions as you feel the need. Hym works without any project instructions — it's just less adapted to the specific context.

---

## Roadmap

### v1 — Core
- [ ] `hym:start-work`
- [ ] `hym:create-ticket`
- [ ] `hym:write-task-blueprint`
- [ ] `hym:write-task-recipe`
- [ ] `hym:follow-recipe`
- [ ] `hym:open-pr`
- [ ] `hym:generate-qa-steps`
- [ ] `hym:verify-work`
- [ ] `hym:wrap-up`

### v1.1 — Onboarding, learning & migrated skills
- [ ] `hym:onboard`
- [ ] `hym:explain-workflows`
- [ ] `hym:set-up-environment`
- [ ] `hym:explore`
- [ ] `hym:brainstorm`
- [ ] `hym:develop-tdd`
- [ ] `hym:debug-systematically`
- [ ] `hym:address-pr-feedback`
- [ ] `hym:announce-release`

### v1.2 — Tool skills
- [ ] `hym:linear` (ticket management via Linear MCP)
- [ ] `hym:mongodb-atlas` (database inspection via MongoDB Atlas MCP)
- [ ] `hym:playwright` (browser automation via Playwright MCP)
- [ ] `hym:github` (PRs, issues, releases via gh CLI)
- [ ] `hym:slack` (messaging via Slack MCP or CLI)

### v2 — Future
- [ ] `hym:deploy`
- [ ] Metrics (cycle time, tickets to merge)
- [ ] Project instruction templates by project type (Go API, React SPA, etc.)
- [ ] Richer inter-skill integration (automatic handoff)
