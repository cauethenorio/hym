---
name: load
description: Load Hym into the current session. Run this before any other Hym skill to enable workflow guidance and skill routing.
---

# Initialize Hym

## Overview

Load the Hym skill map into context. This skill must run at the start of every Hym session so the agent knows which skills exist, how they chain together, and which to suggest at each phase.

## After Loading

Read CLAUDE.md and orient yourself in the project. Do NOT start asking questions immediately — wait for the dev to tell you what they need, then route to the appropriate skill.

If the dev doesn't know where to start, suggest:
- **Have a task?** → `hym:start-work`
- **Want to understand the codebase?** → `hym:explore`
- **New to the project?** → `hym:onboard`

---

## Skill Map

### Entry Points

- `hym:start-work` — Starting a new task. Guides from "what are you working on?" to design.
- `hym:explore` — No task, no goal. Freeform codebase exploration and learning.
- `hym:onboard` — First time in the project. Guided setup and orientation.

### Workflow Chain

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
                    └── archive-task     ← merged PR → archives tasks/current/ to tasks/archive/, deletes branch
```

### After Deploy

- `hym:announce-release` — Concise summary message from deployed changes.
- `hym:draft-release-thread` — Detailed per-ticket thread from deployed changes.

### Standalone Skills (usable at any point)

- `hym:explore` — Freeform codebase exploration. No artifacts, just understanding.
- `hym:brainstorm` — Creative exploration before committing to a direction. Suggests → `create-ticket` or `write-task-blueprint` when the idea is mature.
- `hym:debug-systematically` — Can be invoked standalone for any bug, not just during implementation.
- `hym:develop-tdd` — Can be invoked standalone for any feature, not just during plan execution.

### Onboarding (separate flow)

```
onboard
├── explain-workflows                    (how Hym works, what skills exist)
└── set-up-environment                   (prerequisites, dependencies, verification)
```

### Artifact Flow

```
blueprint.md ─────────→ write-task-recipe, follow-recipe,
                        verify-work,
                        open-pr, generate-qa-steps

recipe.md ────────────→ follow-recipe,
                        verify-work, generate-qa-steps

qa-steps.md ──────────→ open-pr (attached to PR description)
```

All artifacts live in `tasks/current/` during active work and are archived to `tasks/archive/` by `archive-task` after merge.

---

## Key Principles

- **Route, don't do** — this skill sets context, then hands off to the right skill
- **Wait for the dev** — don't assume what they need, let them tell you
- **Follow the chain** — skills produce artifacts that feed the next skill, respect the flow
- **Suggest, don't force** — if the dev wants to skip a step, let them
