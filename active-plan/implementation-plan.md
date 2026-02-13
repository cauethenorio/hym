# Implementation Plan: Rename artifacts and archive instead of delete
RFC: active-plan/rfc.md
Date: 2026-02-13

## Step 1: Rename skill directories

- Action: `git mv skills/write-task-rfc skills/write-task-blueprint` and `git mv skills/write-implementation-plan skills/write-task-recipe`
- Tests: Verify directories exist at new paths, old paths don't exist
- Notes: Must happen first — all subsequent steps reference the new paths

## Step 2: Update `write-task-blueprint` skill (formerly `write-task-rfc`)

- Files: `skills/write-task-blueprint/SKILL.md`
- Changes:
  - Rename frontmatter `name: write-task-rfc` → `name: write-task-blueprint`
  - Update description to reference "blueprint" instead of "RFC"
  - Change save path from `active-plan/rfc.md` to `tasks/current/blueprint.md`
  - Update document format template (header: `# Blueprint:` instead of `# RFC:`)
  - Add auto-create: "If `tasks/current/` does not exist, create it and add `tasks/current/` to the project's `.gitignore`"
  - Update all internal references (`rfc.md` → `blueprint.md`, `active-plan/` → `tasks/current/`)
  - Update lifecycle section to reference archiving instead of removal
  - Update next step to suggest `hym:write-task-recipe`
- Tests: Read the file, verify no references to `rfc`, `active-plan`, or `write-implementation-plan`

## Step 3: Update `write-task-recipe` skill (formerly `write-implementation-plan`)

- Files: `skills/write-task-recipe/SKILL.md`
- Changes:
  - Rename frontmatter `name: write-implementation-plan` → `name: write-task-recipe`
  - Update description to reference "recipe" instead of "implementation plan"
  - Change read path from `active-plan/rfc.md` to `tasks/current/blueprint.md`
  - Change save path from `active-plan/implementation-plan.md` to `tasks/current/recipe.md`
  - Update plan format template header
  - Add auto-create: same as Step 2
  - Update next step to suggest `hym:follow-recipe`
- Tests: Read the file, verify no references to `rfc.md`, `implementation-plan`, or `active-plan`

## Step 4: Update `wrap-up` skill with archive logic

- Files: `skills/wrap-up/SKILL.md`
- Changes:
  - Replace the "Clean up artifacts" section with archive process:
    1. Generate `summary.md` from blueprint header + one-paragraph distillation
    2. Move files from `tasks/current/` to `tasks/archive/YYYY-MM/YYYY-MM-DD-TICKET-short-name/`
    3. Commit the archived directory
    4. Clean `tasks/current/`
  - Define `summary.md` template:
    ```markdown
    # [ticket] — [task title]
    Date: [YYYY-MM-DD]
    Ticket: [link]
    PR: [link]

    ## Summary
    [One paragraph: what was done and why]

    ## Artifacts
    - blueprint.md — Design document
    - recipe.md — Implementation steps
    - qa-steps.md — QA testing report
    ```
  - Update all path references (`active-plan/` → `tasks/current/`, artifact names)
  - Update key principles to mention archiving
- Tests: Read the file, verify archive process is described, summary.md template exists, no references to `active-plan/` or old artifact names

## Step 5: Update `set-up-environment` skill

- Files: `skills/set-up-environment/SKILL.md`
- Changes:
  - Add a new step in the process (after "Check prerequisites", before "Guide through setup") or as part of setup:
    - Create `tasks/current/` and `tasks/archive/` directories
    - Add `tasks/current/` to the project's `.gitignore`
  - Keep it lightweight — just a bullet point, not a full section
- Tests: Read the file, verify the new step is present

## Step 6: Update all other skill files with path and name changes

- Files (8 skill files):
  - `skills/follow-recipe/SKILL.md`
  - `skills/verify-work/SKILL.md`
  - `skills/open-pr/SKILL.md`
  - `skills/generate-qa-steps/SKILL.md`
  - `skills/address-pr-feedback/SKILL.md`
  - `skills/start-work/SKILL.md`
  - `skills/develop-tdd/SKILL.md`
  - `skills/commit/SKILL.md` (check if it references any paths)
- Files (3 subagent prompts):
  - `skills/follow-recipe/implementer-prompt.md`
  - `skills/follow-recipe/spec-reviewer-prompt.md`
  - `skills/follow-recipe/code-quality-reviewer-prompt.md`
- Changes (applied to each file):
  - `active-plan/rfc.md` → `tasks/current/blueprint.md`
  - `active-plan/implementation-plan.md` → `tasks/current/recipe.md`
  - `active-plan/qa-steps.md` → `tasks/current/qa-steps.md`
  - `active-plan/` → `tasks/current/` (any remaining references)
  - `rfc.md` → `blueprint.md` (in prose, not just paths)
  - `implementation-plan.md` → `recipe.md` (in prose)
  - `write-task-rfc` → `write-task-blueprint` (skill name references)
  - `write-implementation-plan` → `write-task-recipe` (skill name references)
  - `hym:write-task-rfc` → `hym:write-task-blueprint`
  - `hym:write-implementation-plan` → `hym:write-task-recipe`
- Notes: `commit/SKILL.md` may not reference any of these — check and skip if clean
- Tests: Grep the entire `skills/` directory for any remaining references to old names/paths

## Step 7: Update load skill map

- Files: `skills/load/SKILL.md`
- Changes:
  - Workflow chain: update `write-task-rfc` → `write-task-blueprint`, `write-implementation-plan` → `write-task-recipe`
  - Workflow chain: update paths (`active-plan/rfc.md` → `tasks/current/blueprint.md`, etc.)
  - Artifact flow section: update all artifact names and paths
  - Update the "All artifacts live in" line to reference `tasks/current/` and explain archive
- Tests: Read the file, verify no old references remain

## Step 8: Update README

- Files: `README.md`
- Changes:
  - Living Documents section: update artifact names, paths, produced/consumed by references
  - Full Chain diagram: update skill names and artifact paths
  - Skills Reference tables: rename `write-task-rfc` → `write-task-blueprint`, `write-implementation-plan` → `write-task-recipe`, update descriptions
  - Architecture directory structure: `active-plan/` → `tasks/` with `current/` and `archive/`
  - Configuration section: update any references to `active-plan/`
  - Hym vs Superpowers section: update artifact references if present
  - Update all prose references to RFC/implementation plan
- Tests: Grep README for any remaining old references

## Step 9: Update plugin.json

- Files: `.claude-plugin/plugin.json`
- Changes:
  - Update keywords: remove `"rfc"`, `"implementation"`, add `"blueprint"`, `"recipe"`, `"tasks"`
- Tests: Read plugin.json, verify updated keywords

## Step 10: Update `docs/hym-design.md`

- Files: `docs/hym-design.md`
- Changes:
  - Living documents concept: update artifact names and paths
  - Directory structure: `active-plan/` → `tasks/` with `current/` and `archive/`
  - `hym:start-work` section: `active-plan/rfc.md` → `tasks/current/blueprint.md`
  - `hym:write-task-rfc` section → rename to `hym:write-task-blueprint`, update all paths, document format template, lifecycle
  - `hym:write-implementation-plan` section → rename to `hym:write-task-recipe`, update all paths
  - `hym:follow-recipe` section: update artifact paths
  - `hym:open-pr` section: update artifact paths
  - `hym:generate-qa-steps` section: update artifact paths
  - `hym:verify-work` section: update artifact paths
  - `hym:wrap-up` section: replace delete with archive process
  - `hym:set-up-environment` section: add tasks/ setup mention
  - "Adding Hym to a New Project" section: `active-plan` → `tasks/current` and `tasks/archive`
  - Roadmap: update skill names
  - All prose references to RFC → blueprint, implementation plan → recipe
- Tests: Grep the file for any remaining old references
