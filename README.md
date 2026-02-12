# Hym

> Joaquin Phoenix had Her. We have Hym.

**Hym** is a system of composable skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that accompanies the developer through the entire development cycle — from "what am I going to work on?" to "deployed and team notified".

No code is written without context, understanding, and a documented design. Hym ensures this through skills that guide each phase, producing living documents that carry forward through the workflow.

## Why Hym?

Most AI coding tools help you write code. Hym helps you **build software** — the whole process:

- **Before coding:** Understand the task, create a ticket, write a lightweight RFC, plan the implementation
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

Skills that guide a phase of development. They define **what** to do, not how to use specific tools. Examples: `start-work`, `write-task-rfc`, `debug-systematically`.

### Tool Skills

Skills that encapsulate how to use a specific tool — Linear, MongoDB Atlas, Playwright, GitHub CLI, Slack. They abstract away whether the tool is an MCP server, a CLI, or an API.

Workflow skills stay generic. Project instructions wire them to tool skills.

### Project Instructions

Files in `.claude/project-instructions/` that adapt skills to a specific project's context — which ticket system, what PR conventions, how to deploy. Skills read these on startup and incorporate them as additive context.

### Living Documents

The task RFC (`active-plan/rfc.md`) and implementation plan (`active-plan/implementation-plan.md`) accompany the task through its lifecycle. They're created during design, updated during implementation, transferred to the PR, and removed after merge.

## The Workflow

### Full Chain

```
start-work
├── create-ticket                        (if no ticket exists)
└── write-task-rfc                       → produces active-plan/rfc.md
    └── write-implementation-plan        → produces active-plan/implementation-plan.md
        ├── execute-implementation       (step by step from the plan)
        │   ├── develop-tdd              (per step, when writing new code)
        │   ├── debug-systematically     (per step, when something fails)
        │   └── verify-before-completing (internal, after all steps)
        └── execute-with-subagents       (fresh subagent per task + two-stage review)
            ├── develop-tdd              (per task, when writing new code)
            ├── debug-systematically     (per task, when something fails)
            └── verify-before-completing (internal, after all tasks + final review)
            │
            └── open-pr                  ← reads rfc.md + implemented code
                └── generate-qa-steps    ← reads rfc.md + implementation-plan.md + git diff
                │
                └── address-pr-feedback  (loop until approved)
                    └── wrap-up          ← merged PR → removes active-plan/, deletes branch
```

### Artifact Flow

```
rfc.md ──────────────→ write-implementation-plan, execute-implementation,
                       execute-with-subagents, verify-before-completing,
                       open-pr, generate-qa-steps

implementation-plan.md → execute-implementation, execute-with-subagents,
                         verify-before-completing, generate-qa-steps

qa-steps.md ──────────→ open-pr (attached to PR description)
```

All artifacts live in `active-plan/` and are removed by `wrap-up` after merge.

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
| `hym:write-task-rfc`            | Write a lightweight RFC — problem, proposed solution, alternatives, scope, acceptance criteria. Saved to `active-plan/rfc.md`. |
| `hym:write-implementation-plan` | Break the RFC into ordered, concrete implementation steps. Saved to `active-plan/implementation-plan.md`.                      |

### Implementation

| Skill                        | Purpose                                                                                                                             |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `hym:execute-implementation` | Execute the plan step by step with review checkpoints. Best for tightly coupled steps.                                              |
| `hym:execute-with-subagents` | Execute the plan using fresh subagents per task with two-stage review (spec compliance + code quality). Best for independent tasks. |
| `hym:develop-tdd`            | Write tests first, then implementation, then refactor. Usable standalone or during plan execution.                                  |
| `hym:debug-systematically`   | Investigate bugs with structure — hypothesis, evidence, root cause, fix, verify. Usable standalone.                                 |
| `hym:commit`                 | Create clean, atomic commits following project conventions.                                                                         |

### Review & Quality

| Skill                          | Purpose                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| `hym:verify-before-completing` | Final verification before declaring work complete — tests, build, lint, acceptance criteria. |
| `hym:open-pr`                  | Prepare and create a PR with RFC context, QA steps, and reviewer attention points.           |
| `hym:generate-qa-steps`        | Generate QA testing steps from the RFC, plan, and actual code changes.                       |
| `hym:address-pr-feedback`      | Process code review feedback with technical rigor — evaluate before implementing.            |

### Delivery & Communication

| Skill                      | Purpose                                                                    |
| -------------------------- | -------------------------------------------------------------------------- |
| `hym:wrap-up`              | Clean up after merge — remove `active-plan/`, delete branch.               |
| `hym:announce-release`     | Create a concise release summary message for team communication.           |
| `hym:draft-release-thread` | Create a detailed per-ticket Slack thread explaining each change's impact. |

## Execution Strategies

Hym offers two ways to execute an implementation plan:

### Execute Implementation (inline)

`hym:execute-implementation` — Executes steps sequentially in the current session with developer checkpoints between significant steps.

**Best for:** Tightly coupled changes, tasks requiring ongoing developer input, smaller plans.

### Execute with Subagents

`hym:execute-with-subagents` — Dispatches a fresh subagent per task with a two-stage review process after each: spec compliance review first, then code quality review.

**Best for:** Plans with independent tasks, larger implementations, when you want automated quality gates.

**The review process:**

1. Implementer subagent builds and tests the task
2. Spec reviewer checks code matches the plan requirements
3. Code quality reviewer checks implementation quality
4. If either reviewer finds issues, the implementer fixes and re-review happens
5. After all tasks: final cross-task quality review + `verify-before-completing`

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
- PRs: Squash merge, include RFC in description
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
        write-task-rfc/
        write-implementation-plan/
        execute-implementation/
        execute-with-subagents/
        develop-tdd/
        debug-systematically/
        commit/
        verify-before-completing/
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

  active-plan/                       # Living documents (per task)
    rfc.md
    implementation-plan.md
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

**Skills library vs. workflow system.** Superpowers skills are independent units that chain through convention — each skill recommends the next, but there's no formal orchestration. Hym defines an explicit workflow with a dependency graph and artifact pipeline. Skills produce named artifacts (`active-plan/rfc.md`, `active-plan/implementation-plan.md`, `active-plan/qa-steps.md`) that are consumed by downstream skills and cleaned up after merge. This makes Hym more opinionated about the order of operations but also more predictable.

**Tool integration.** Superpowers stays within the git-and-code boundary — it doesn't address how to interact with issue trackers, databases, browsers, or messaging tools. Hym separates workflow skills (what to do) from tool skills (how to do it with Linear, MongoDB Atlas, Playwright, Slack, etc.), letting project instructions wire them together. This makes Hym more adaptable to different toolchains but also more complex to configure.

**Customization model.** Superpowers relies on CLAUDE.md conventions and the agent's skill-checking behavior. Hym provides a formal `project-instructions/` directory where per-skill files enrich (never override) the global skill behavior, giving each project fine-grained control over how skills adapt to its context.

## References

- [Superpowers](https://github.com/obra/superpowers) — A collection of skills and techniques for getting the most out of Claude Code
