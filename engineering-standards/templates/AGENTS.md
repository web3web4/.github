# [Project Name] - Context for AI Agents

> This file is read by AI coding agents every session. Keep it lean — no examples, no placeholders.
> **Setup guide:** See [AGENTS-init.md](AGENTS-init.md) for instructions on filling this out.

## Project Overview

**[Project Name]** is ...

- **[app/package]** — ...

## Architecture & Stack

See [AGENTS-reference.md](AGENTS-reference.md) for tech stack, engineering patterns, and architectural decisions. Only read that file when the task involves stack choices, new dependencies, or architecture.

## Development Rules

- **Code Quality**: Strict TypeScript. No `any` types. Use `zod` for all runtime boundary validations (APIs, forms).
- **Documentation Location**:
  - Persistent architecture/API docs → `docs/`
  - Ephemeral agent planning/logs → `agents-artifacts/` (do not gitignore)
  - All other `.md` files should only be created inside `agents-artifacts/` or `docs/` unless the task explicitly requires otherwise.
- **Testing**: ...

## AI Agent Workflow (The "agents-artifacts" system)

> **First thing every session:** Check `agents-artifacts/doing/` for unfinished work from a previous session before starting anything new.

When planning complex tasks, investigating bugs, or leaving context for future sessions, use the `agents-artifacts/` directory. **Skip this for trivial, single-step changes** — not every task needs an artifact.

### Directory Structure & Lifecycle

The directory is structured around a kanban-style lifecycle, categorized by the type of work:

`agents-artifacts/[status]/[category]/`

- **[status]**: `todo/`, `doing/`, `done/`
- **[category]**: `bugs/`, `features/`, `analysis/` (or other sensible grouping)

**Workflow Rules:**

1. **New Request**: Create a markdown file describing the task in `todo/[category]/`.
2. **Starting Work / AI Planning**:
   - Move the file to `doing/[category]/`.
   - **For AI Agents:** This is where you should write your implementation plans, checklists, and scratchpad notes before modifying code. E.g., read the request, map out the files to change directly in the markdown file, and update your checklist as you go.
3. **Completion**: Once verified and committed, move the file to `done/[category]/`.
   - _Crucial step_: If the artifact reveals a permanent architectural pattern, extract that knowledge to `docs/` or update this `AGENTS.md` file _before_ marking it done.

### Proactive Filing

If you notice a bug, issue, or idea during unrelated work:

1. Create a new artifact file in `todo/[category]/` with at least the `## Context` section filled in.
2. Add a corresponding entry to `agents-artifacts/todo/backlog.md`:

```markdown
- [ ] `YYYY-MM-DD-HHmm-title.md` [category] [priority (if available): high/medium/low] — One-line summary and optional notes.
```

The backlog is the **dashboard** — use it to track priority and add deferral notes (e.g., "Defer until auth refactor is done"). The artifact file holds the full context.

**Sync rules:** When an artifact moves to `doing/` or `done/`, check off its backlog entry (`- [x]`).

> `todo/` is a passive backlog. **Never** autonomously pick up items from it — only work on them when the user explicitly asks.

### Stale Work

If you find items in `doing/` that appear outdated or abandoned, move them back to `todo/` (preserving all notes) or forward to `done/` with an `## Outcome: Canceled — [reason]` note. Ask the user if unsure.

### Naming Convention

Use this format: `YYYY-MM-DD-HHmm-[title].md`, e.g. `2026-03-01-1430-fix-auth-redirect.md`.

One file per task. Plans, progress, and outcomes all live inside the same file using the structure below.

### Artifact File Structure

Every artifact file must follow this layout:

```markdown
## Context

[Concisely: why this task exists]

## Plan

- [ ] Step 1
- [ ] Step 2

...

---

## Outcome

[What was done and why — keep it concise]

### Notes

_If needed_

[Trade-offs, deferred work, follow-ups]
```

- Fill `Context` and `Plan` when creating/moving to `doing/`.
- Fill `Outcome` when moving to `done/`. Check off plan items as you go.
