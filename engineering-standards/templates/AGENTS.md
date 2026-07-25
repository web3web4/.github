# [Project Name] - Context for AI Agents

> This file is read by AI coding agents every session. Keep it lean — no examples, no placeholders.
> **Setup guide:** See [AGENTS-init.md](AGENTS-init.md) for instructions on filling this out.

## Project Overview

**[Project Name]** is ...

- **[app/package]** — ...

## Project Status

...

## Project Type

`[Development — TypeScript]` <!-- Options: Development — TypeScript | Development — [language] | Documentation | Research | ... -->

## Architecture & Stack

See [`agents-instructions/architecture-reference.md`](agents-instructions/architecture-reference.md) for tech stack, engineering patterns, and architectural decisions. Only read that file when the task involves stack choices, new dependencies, or architecture.

## Development Rules

- **Code Quality**: Strict TypeScript. No `any` types. Use `zod` for all runtime boundary validations (APIs, forms).
- **Documentation Location**:
  - Persistent architecture/API docs → `docs/`
  - Ephemeral agent planning/logs → `execution-plans/` (do not gitignore)
  - All other `.md` files should only be created inside `execution-plans/` or `docs/` unless the task explicitly requires otherwise.
- **Testing**: ...
- **Knowledge capture**: When you discover a reusable pattern or architectural insight during a task, append it to `execution-plans/todo/scratch.md` — don't break focus to write to `docs/` mid-task.
- **No git write ops without explicit per-operation approval**: `git commit`, `git push`, branch deletion, force-push, and amend each require their own explicit approval — even mid-workflow. Read-only git commands are fine.
- **Specs vs. code**: If code deviates from a spec but the code is better, propose updating the spec — don't "fix" working code.
- **Test failures**: Investigate production code first before assuming the test is wrong.

## AI Agent Workflow (The "execution-plans" system)

> **First thing every session:** Check `execution-plans/doing/` for unfinished work from a previous session before starting anything new.

When planning complex tasks, investigating bugs, or leaving context for future sessions, use the `execution-plans/` directory. **Skip this for trivial, single-step changes** — not every task needs an artifact.

Artifacts live at `execution-plans/[status]/[category]/`, where status is `todo`/`doing`/`done` and category is `features`/`fixes`/`analysis`. <!-- Documentation projects: use drafting/revisions/analysis -->

**Before you create, move, or complete an artifact, read the [`execution-plans-workflow` skill](.agents/skills/execution-plans-workflow/SKILL.md).** It defines the folder lifecycle, the naming convention, the required file structure, and the checklists to include. The skill is shared across repos and installed with `npx skills` — do not edit it in place; project-specific rules belong in this file.

### Proactive Filing

If you notice anything worth noting during unrelated work (bugs, ideas, patterns, insights), append your thoughts to `execution-plans/todo/scratch.md` and move on. Don't create artifacts or backlog entries mid-task — stay focused.

> `todo/` is a passive backlog. **Never** autonomously pick up items from it — only work on them when the user explicitly asks.

## Communication & Reasoning Standards

- **Fact-based, not agreeable**: If a request relies on a flawed assumption or conflicts with project conventions/architecture, challenge it directly. Do not execute a bad plan just to be helpful.
- **Critical plan evaluation**: Plans, architecture docs, and prior agent artifacts are inputs, not gospel. Verify claims against actual code and data before executing. When you spot gaps, conflicts, or suboptimal choices — flag them before executing and propose an alternative. Do not over-flag cosmetic or subjective concerns — focus on issues that would cause bugs, data inconsistencies, or architectural drift.
- **No silent omissions**: If an edge case, requirement, or code block is being skipped, explicitly state what was omitted and why.
- **Zero filler**: Skip formalities like pleasantries and apologies. Focus on actionable steps and code.
- **Copyable text as code blocks**: When asked to provide a piece of text for direct use (a template, message, example, form field value, very concise prompt, etc.) rather than to answer a question, wrap it in a fenced Markdown code block.

## Dev Scripts

Dev scripts live in `scripts/` with usage docs in each file's header comment.

...
