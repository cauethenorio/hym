# Hym

> Joaquin Phoenix had Her. We have Hym.

**Hym** is a system of composable skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that accompanies the developer through the entire development cycle — from "what am I going to work on?" to "deployed and team notified".

No code is written without context, understanding, and a documented design. Hym ensures this through skills that guide each phase, producing living documents that carry forward through the workflow.

## Why Hym?

Most AI coding tools help you write code. Hym helps you **build software** — the whole process:

- **Before coding:** Understand the task, create a ticket, write a lightweight blueprint, plan the recipe
- **During coding:** Follow TDD, debug systematically, execute plans step by step with review checkpoints
- **After coding:** Open PRs with context, generate QA steps, address review feedback, clean up after merge
- **Communication:** Announce releases, draft detailed release threads for the team

Each phase has a dedicated skill. Skills produce artifacts that feed into the next skill. The developer stays in control — Hym guides, it doesn't force.

## Quick Start

### 1. Install as a Claude Code plugin

Add Hym as a plugin in your project's `.claude/settings.json`:

```json
{
  "plugins": ["/path/to/hym/.claude-plugin"]
}
```

### 2. Load and go

In any Claude Code session:

```
/hym:load
```

This loads the skill map into context. Then tell Hym what you need:

- **Have a task?** `/hym:start-work`
- **Want to understand the codebase?** `/hym:explore`
- **New to the project?** `/hym:onboard`

## Core Concepts

### Workflow Skills

Skills that guide a phase of development. They define **what** to do, not how to use specific tools. Examples: `start-work`, `write-task-blueprint`, `debug-systematically`.

### Tool Skills

Skills that encapsulate how to use a specific tool — Linear, MongoDB Atlas, Playwright, GitHub CLI, Slack. They abstract away whether the tool is an MCP server, a CLI, or an API.

Workflow skills stay generic. Project instructions wire them to tool skills.

### Project Instructions

Files in `.claude/project-instructions/` that adapt skills to a specific project's context — which ticket system, what PR conventions, how to deploy. Skills read these on startup and incorporate them as additive context.

### Living Documents

Hym produces three artifacts during a task's lifecycle. Each one builds on the previous — no artifact exists in isolation. They live in `tasks/current/`, accompany the task through implementation, and after merge are archived to `tasks/archive/` by `wrap-up`.

#### 1. `tasks/current/blueprint.md` — The Design Document

**Produced by:** `write-task-blueprint`
**Consumed by:** `write-task-recipe`, `follow-recipe`, `verify-work`, `open-pr`, `generate-qa-steps`

A lightweight blueprint (1-2 pages) that aligns direction before coding. Contains:

- **Problem / Context** — what's broken or missing
- **Proposed Solution** — the chosen direction
- **Alternatives and Trade-offs** — what else was considered and why it was rejected
- **Scope** — what's in and what's explicitly out
- **Acceptance Criteria** — verifiable outcomes that define "done"

This is a living document — it gets updated during implementation as understanding deepens, then gets transferred into the PR description by `open-pr`, and after merge archived to `tasks/archive/` for institutional memory. It's the source of truth that carries context forward to every downstream skill.

#### 2. `tasks/current/recipe.md` — The Execution Playbook

**Produced by:** `write-task-recipe`
**Consumed by:** `follow-recipe`, `verify-work`, `generate-qa-steps`

Turns the blueprint's "what to build" into ordered, concrete steps — which files to create/modify, what tests to write, in what order. Each step is:

- **Concrete** — "modify handler X in file Y", not "update the API"
- **Testable** — includes what to verify after each step
- **Ordered by dependency** — steps that depend on others come after
- **Small** — completable in one sitting

This is what `follow-recipe` dispatches to subagents — one step at a time, each reviewed for spec compliance and code quality before moving on.

#### 3. `tasks/current/qa-steps.md` — The QA Testing Report

**Produced by:** `generate-qa-steps`
**Consumed by:** `open-pr` (attached to the PR description)

Generated after implementation by cross-referencing three things: the blueprint's acceptance criteria, the recipe, and the actual `git diff`. Contains:

- **Happy path steps** — step-by-step instructions for each acceptance criterion, followable by a QA person without asking questions
- **Edge cases** — boundary conditions and unusual inputs to check
- **Regression checks** — verification that areas touched by refactors still work
- **Prerequisites** — setup needed before testing

## The Workflow

### Full Chain

```
start-work
├── create-ticket                        (if no ticket exists)
└── write-task-blueprint                 → produces tasks/current/blueprint.md
    └── write-task-recipe                → produces tasks/current/recipe.md
        └── follow-recipe               (fresh subagent per task + two-stage review)
            ├── develop-tdd              (per task, when writing new code)
            ├── debug-systematically     (per task, when something fails)
            └── verify-work              (internal, after all tasks + final review)
            │
            └── open-pr                  ← reads blueprint.md + implemented code
                └── generate-qa-steps    ← reads blueprint.md + recipe.md + git diff
                │
                └── address-pr-feedback  (loop until approved)
                    └── wrap-up          ← merged PR → archives tasks/current/ to tasks/archive/, deletes branch
```


## Skills Reference

### Entry Points

| Skill            | Purpose                                                                                                                 |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `hym:load`       | Load the skill map into context. Run this first in every session.                                                       |
| `hym:start-work` | Start a new task. Guides from "what are you working on?" through understanding, ticket creation, and handoff to design. |
| `hym:explore`    | Freeform codebase exploration. No task, no goal — just understanding.                                                   |
| `hym:onboard`    | Guide a new developer through understanding the project and getting set up.                                             |

### Onboarding & Learning

| Skill                    | Purpose                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------ |
| `hym:onboard`            | Orchestrates the onboarding experience — architecture, workflows, environment setup. |
| `hym:explain-workflows`  | Explains how Hym works and what skills are available.                                |
| `hym:set-up-environment` | Guides local environment setup — dependencies, services, verification.               |
| `hym:explore`            | Conversational codebase exploration. Read files, trace flows, understand patterns.   |

### Planning & Design

| Skill                           | Purpose                                                                                                                        |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `hym:brainstorm`                | Creative exploration before committing to a direction. Proposes approaches with trade-offs.                                    |
| `hym:create-ticket`             | Create a ticket in the project's issue tracker from collected context.                                                         |
| `hym:write-task-blueprint`      | Write a lightweight blueprint — problem, proposed solution, alternatives, scope, acceptance criteria. Saved to `tasks/current/blueprint.md`. |
| `hym:write-task-recipe`         | Break the blueprint into ordered, concrete implementation steps. Saved to `tasks/current/recipe.md`.                                         |

### Implementation

| Skill                        | Purpose                                                                                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `hym:follow-recipe`         | Execute the plan using fresh subagents per task with two-stage review (spec compliance + code quality). Dispatches one task at a time.   |
| `hym:develop-tdd`            | Write tests first, then implementation, then refactor. Usable standalone or during plan execution.                                       |
| `hym:debug-systematically`   | Investigate bugs with structure — hypothesis, evidence, root cause, fix, verify. Usable standalone.                                      |
| `hym:commit`                 | Create clean, atomic commits following project conventions.                                                                              |

### Review & Quality

| Skill                          | Purpose                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| `hym:verify-work`              | Final verification before declaring work complete — tests, build, lint, acceptance criteria. |
| `hym:open-pr`                  | Prepare and create a PR with blueprint context, QA steps, and reviewer attention points.     |
| `hym:generate-qa-steps`        | Generate QA testing steps from the blueprint, recipe, and actual code changes.               |
| `hym:address-pr-feedback`      | Process code review feedback with technical rigor — evaluate before implementing.            |

### Delivery & Communication

| Skill                      | Purpose                                                                    |
| -------------------------- | -------------------------------------------------------------------------- |
| `hym:wrap-up`              | Clean up after merge — archive `tasks/current/` to `tasks/archive/`, delete branch. |
| `hym:announce-release`     | Create a concise release summary message for team communication.           |
| `hym:draft-release-thread` | Create a detailed per-ticket Slack thread explaining each change's impact. |

## Configuration

### Project Instructions

Create files in `.claude/project-instructions/` to adapt skills to your project. Only create files for skills that need customization — defaults work fine without them.

```
.claude/project-instructions/
  start-work.md              # How the project organizes tasks
  create-ticket.md           # Which ticket tool and conventions
  commit.md                  # Commit message format
  open-pr.md                 # PR conventions
  generate-qa-steps.md       # QA format and where to publish
  announce-release.md        # Release message format and channel
  ...
```

**Example: Wiring a tool skill**

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

### CLAUDE.md Integration

Add a workflow section to your project's `CLAUDE.md`:

```markdown
## Workflow

This project uses Hym to guide the development cycle.

- Tickets: Linear (use hym:linear)
- Branches: feature/<ticket-id>-<short-description>
- PRs: Squash merge, include blueprint in description
- Deploy: Merge to main triggers deploy
```

## Architecture

```
<project>/
  .claude/
    plugins/hym/
      skills/                        # Workflow skills
        load/
        start-work/
        create-ticket/
        brainstorm/
        write-task-blueprint/
        write-task-recipe/
        follow-recipe/
        develop-tdd/
        debug-systematically/
        commit/
        verify-work/
        open-pr/
        generate-qa-steps/
        address-pr-feedback/
        wrap-up/
        onboard/
        explain-workflows/
        set-up-environment/
        explore/
        announce-release/
        draft-release-thread/

      tools/                         # Tool skills
        linear/
        mongodb-atlas/
        playwright/
        github/
        slack/

    project-instructions/            # Project-specific customization
      start-work.md
      create-ticket.md
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

### Design Principles

1. **The global skill always runs** — it is never replaced by project instructions
2. **Project instructions are additive** — they enrich, never override
3. **Every skill reads context on start** — CLAUDE.md + project instructions for that skill
4. **Skills are independent** — each one can be invoked standalone
5. **Skills can invoke other skills** — context carries forward through conversation history
6. **Start small** — add project instructions as you feel the need; Hym works without any

## Hym vs. Superpowers

Hym was heavily inspired by [Superpowers](https://github.com/obra/superpowers). Several Hym skills — brainstorming, subagent-driven development, systematic debugging, TDD, verification before completion — trace their lineage directly to Superpowers. If you haven't looked at Superpowers, you should.

That said, the two projects have different goals and make different trade-offs.

### Shared Ground

Both projects believe that AI agents produce better work when guided by structured workflows rather than left to improvise. Both enforce planning before coding, test-driven development, systematic debugging, verification before claiming completion, and code review. Both offer subagent-driven execution with spec and quality review stages.

### Where They Differ

**Scope of the lifecycle.** Superpowers covers brainstorm-through-merge: design, plan, execute, test, review, finish the branch. Hym extends the lifecycle in both directions — earlier (ticket creation, onboarding, codebase exploration) and later (QA step generation, PR feedback loops, post-merge cleanup, release announcements). Superpowers is laser-focused on the engineering cycle; Hym treats development as a broader process that includes project management and team communication.

**Skills library vs. workflow system.** Superpowers skills are independent units that chain through convention — each skill recommends the next, but there's no formal orchestration. Hym defines an explicit workflow with a dependency graph and artifact pipeline. Skills produce named artifacts (`tasks/current/blueprint.md`, `tasks/current/recipe.md`, `tasks/current/qa-steps.md`) that are consumed by downstream skills and archived after merge. This makes Hym more opinionated about the order of operations but also more predictable.

**Tool integration.** Superpowers stays within the git-and-code boundary — it doesn't address how to interact with issue trackers, databases, browsers, or messaging tools. Hym separates workflow skills (what to do) from tool skills (how to do it with Linear, MongoDB Atlas, Playwright, Slack, etc.), letting project instructions wire them together. This makes Hym more adaptable to different toolchains but also more complex to configure.

**Customization model.** Superpowers relies on CLAUDE.md conventions and the agent's skill-checking behavior. Hym provides a formal `project-instructions/` directory where per-skill files enrich (never override) the global skill behavior, giving each project fine-grained control over how skills adapt to its context.

## References

- [Superpowers](https://github.com/obra/superpowers) — A collection of skills and techniques for getting the most out of Claude Code
