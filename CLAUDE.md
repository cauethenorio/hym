# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What is Hym

Hym is a Claude Code plugin — a system of composable skills that guide developers through the entire development cycle. It contains no runtime code; the project is entirely markdown skill definitions and documentation.

There is nothing to build, test, lint, or install. The "codebase" is markdown files.

## Architecture

### Two types of skills

- **Workflow skills** (`skills/`) — Guide a phase of development (e.g., `start-work`, `write-task-blueprint`). Generic, project-agnostic.
- **Tool skills** (`tools/`) — Encapsulate how to use a specific tool (e.g., `linear`, `playwright`). Not yet implemented.

Workflow skills don't know about specific tools. Consuming projects wire them together via `.claude/project-instructions/` files.

### Skill structure

Each skill lives in `skills/<name>/SKILL.md`. All follow the same pattern:
1. YAML frontmatter (name, description)
2. Overview section
3. "Before Anything Else" — read repo state silently before acting
4. Process steps
5. "Next Step" — which skill comes after
6. "Key Principles"

Exception: `follow-recipe` has additional subagent prompt templates (`implementer-prompt.md`, `spec-reviewer-prompt.md`, `code-quality-reviewer-prompt.md`).

### Artifact pipeline

Three living documents accompany a task through its lifecycle:

| Artifact | Produced by | Lives at |
|---|---|---|
| `blueprint.md` | `write-task-blueprint` | `tasks/current/blueprint.md` |
| `recipe.md` | `write-task-recipe` | `tasks/current/recipe.md` |
| `qa-steps.md` | `generate-qa-steps` | `tasks/current/qa-steps.md` |

`tasks/current/` is gitignored (active work). After merge, `archive-task` archives everything to `tasks/archive/YYYY-MM/YYYY-MM-DD-TICKET-short-name/`.

### Workflow chain

```
start-work → create-ticket → write-task-blueprint → write-task-recipe
  → follow-recipe → verify-work → open-pr
  → generate-qa-steps → address-pr-feedback (loop) → archive-task
```

Standalone skills (invokable anytime): `explore`, `brainstorm`, `develop-tdd`, `debug-systematically`.

### Project instructions model

Consuming projects customize Hym by adding files to `.claude/project-instructions/<skill-name>.md`. These are **additive** — they enrich skills, never replace them. If no project instruction exists for a skill, the skill's default behavior runs unchanged.

## Conventions when editing skills

- Maintain the existing SKILL.md structure (frontmatter → overview → before anything else → process → next step → key principles)
- Skills should be invokable standalone — don't assume prior skills ran
- Skills read context on start (CLAUDE.md + project instructions) — don't hardcode project-specific details
- "Route, don't do" — orchestrator skills (`load`, `start-work`, `onboard`) set context and hand off to specialized skills
- One question at a time in conversations; prefer multiple choice over open-ended
- Present information section by section, validate before moving on

## Plugin metadata

Defined in `.claude-plugin/plugin.json`. Name: `hym`, version: `0.1.0`.
