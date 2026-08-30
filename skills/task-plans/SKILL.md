---
name: task-plans
description: The process for non-trivial tasks - the todo/doing/done artifact lifecycle, the plan template, the implementation quality bar, and the completion procedure. Use when starting or resuming any non-trivial task, deciding whether a task needs a plan artifact, writing a plan, creating or moving an artifact, implementing tracked work, recording an outcome, or filing a note into the backlog.
---

# Task Plans

A non-trivial task gets **exactly one** Markdown artifact under `task-plans/` that moves through `todo` → `doing` → `done`. This skill is the unified process: routing the task, writing the artifact, the quality bar the work must meet, and how the task is closed out. Each project varies only the execution details — its category names, its concrete quality checks, and its commands.

Go straight to the procedure that matches what you are about to do:

- Starting or resuming any task → [Procedure 0 — Route](#procedure-0--route-the-task)
- Writing a plan, or starting a new artifact → [Procedure 1 — Create](#procedure-1--create-an-artifact)
- Changing an artifact's status → [Procedure 2 — Move](#procedure-2--move-an-artifact)
- Writing the code or content → [Procedure 3 — Implement](#procedure-3--implement)
- Finishing the work → [Procedure 4 — Complete](#procedure-4--complete)

## First: three things come from the project

This skill is shared across repositories and is deliberately project-agnostic. It defines the process. The project defines the flavor. Read these three from `AGENTS.md` — never assume them:

1. **The category names.** Code projects usually use `features` / `fixes` / `analysis`. Documentation projects usually use `drafting` / `revisions` / `analysis`.
2. **The project's own quality rules**, under `Development Rules` — its language, validation library, test runner, and any rule specific to this codebase. These apply on top of Procedure 3.
3. **The quality-gate commands and git conventions**, under `Commands` and `Git`. Used in Procedure 4.

**Never edit this skill file to record project details.** `npx skills` overwrites it wholesale on update, and CI fails on a local edit. Project configuration belongs in `AGENTS.md`.

## Procedure 0 — Route the task

Run this decision before any planning or implementation. Exactly one row applies:

| Situation                                                                        | Action                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trivial, single-step change                                                      | No artifact. Do the work directly.                                                                                                                                                                                                |
| The user attached or named an artifact                                           | Work from that artifact. Verify its plan still matches the code — if it looks outdated, say so and ask ([Procedure 2](#procedure-2--move-an-artifact)). Move it to `doing/` when you start.                                       |
| New non-trivial task, no artifact                                                | Create one now ([Procedure 1](#procedure-1--create-an-artifact)) before touching code or content.                                                                                                                                 |
| The task continues earlier work, and no artifact was attached, named, or already identified in this conversation | **Stop and ask the user to point to the artifact.** Do not scan `task-plans/` to find it. Do not create a new one — a duplicate breaks the one-artifact rule. If the user confirms no artifact exists, treat it as a new task. |

A task is **non-trivial** when any of these hold:

- It needs investigation before the approach is clear.
- It changes more than a couple of files, or takes more than a few steps.
- It spans sessions, or a future session will need it as context.
- The user asked for a plan.
- The deliverable is an assessment: a review, audit, critique, comparison, or gap analysis, even when no file changes come out of it.

An assessment is written down like any other work — into the artifact it assesses when one exists, otherwise a new one — and the chat reply is a link plus a short summary.

When in doubt, treat the task as non-trivial — an artifact is cheap, lost context is not. And `task-plans/` is never a source of work: do not scan `todo/` or `doing/` for something to pick up.

## Procedure 1 — Create an artifact

1. Choose the status folder: `todo/` if the work has not started, `doing/` if you are starting right now.
2. Choose the category folder from the list in `AGENTS.md`. Add a new category only when none of them fit.
3. Get the current timestamp. Run `date +%Y-%m-%d-%H%M`. Do not guess it.
4. Name the file `YYYY-MM-DD-HHmm-[short-title].md` — for example `2026-03-01-1430-fix-auth-redirect.md`.
5. Write the file using the [artifact file template](#artifact-file-template). Fill `Context` and `Plan` now.
6. If this is implementation work, add the [Procedure 3](#procedure-3--implement) checklist to the `Plan`.
7. Append one line for the new artifact to `task-plans/todo/backlog.md`: `` `filename.md` [category] [priority] [status] — description ``.

One file per task. Never split one task across two artifacts, and never open a second artifact for a task that already has one.

## Procedure 2 — Move an artifact

The status folders track the **work**, not the document.

| Folder             | Meaning                                                                          |
| ------------------ | -------------------------------------------------------------------------------- |
| `todo/[category]`  | Work has not started. A fully-planned item stays here until it does.              |
| `todo/deferred/`   | Deferred to a future project phase. No category subfolders. Do not pick these up. |
| `doing/[category]` | Work is happening right now — code being written, content being edited.          |
| `done/[category]`  | Work is implemented, verified, **and committed**.                                 |
| `todo/backlog.md`  | One-line index of every artifact currently in `todo/`, so anyone can see what's pending without opening or scanning each file. Kept current by the steps below and in Procedure 1. |

How to move it:

1. Use `mv` (or `git mv`). **Move the single file. Never copy it. Never leave the old file behind.**
2. Keep the original timestamp in the filename. It records when the task was opened, not when it moved.
3. Move it into `doing/` at the moment you start the work.
4. Move it into `done/` only after the work is verified and committed — not when the code is merely written.
5. Update `todo/backlog.md` in the same step: remove the artifact's line once it leaves `todo/`, and add it back if an artifact ever returns to `todo/`.

Move an artifact only because its own work changed status. If an artifact you were handed looks outdated, abandoned, or no longer matches the code, say so and ask what to do. Never reclassify or cancel one on your own.

## Procedure 3 — Implement

Copy this checklist into the artifact's `Plan` and adapt each line to the task. It is the shared quality bar for every project. Apply the project's own rules from `AGENTS.md` → `Development Rules` on top of it. Drop a category only when the task cannot touch it.

This checklist assumes code. If `AGENTS.md` → `Project Type` is not a software project (documentation, research, regulatory), do not force these six categories to fit — use the equivalent quality bar `AGENTS.md` → `Development Rules` defines for that kind of work instead (for example: sourcing, accuracy, review).

- [ ] **Boundary validation** — data entering the system (request payload, form submission, file, external response) is validated before domain logic sees it. Failures produce actionable errors, never a raw stack trace.
- [ ] **Type safety** — no escape hatches. Unknown data is narrowed, not asserted.
- [ ] **Failure handling** — anything that can fail reports failure explicitly. Nothing fails silently and nothing leaves partial output behind. An operation that crosses a network is safe to retry.
- [ ] **Interface quality** — the surface a person or caller touches (UI, CLI output, API response) gives clear feedback for success, loading, and failure, and stays usable on the smallest supported target.
- [ ] **Code organization** — domain logic stays separate from framework, transport, and I/O layers. No path or value hardcoded that belongs in configuration.
- [ ] **Comment hygiene** — a comment exists only when the WHY is non-obvious. No "removed X" markers, no "used by Y" hints, no restating what the next line does.
- [ ] **Testing** — pure logic and complex mappings have dedicated tests. A test that cannot run in the current environment is skipped explicitly, never quietly passed.

Keep the artifact current while you implement: check off `Plan` items as they finish, and record deviations from the plan the moment they happen — not retroactively at completion. The artifact must always describe what is actually being built.

## Procedure 4 — Complete

1. **Plan versus implementation.** Walk each `Plan` checkbox and confirm the work matches the intent. Mark finished items `[x]`. Keep any skipped item and record the reason under `Notes` — never delete it.
2. **Quality gates.** Run the project's quality-gate commands from `AGENTS.md`. Fix every failure before continuing. Skip this step only if the project defines no such commands.
3. **Diff review.** Read the full diff against the base branch and compare it with the `Plan`. Confirm there are no unintended changes, no debug output left behind, and no disabled lint rules or type escape hatches added without a stated reason.
4. **Deferred work.** Record skipped plan items and anything left undone in the artifact's `Notes`. Append ideas and findings worth keeping beyond this task to `task-plans/todo/scratch.md`. Do not create artifacts in `todo/` — the user decides what becomes tracked work.
5. **Knowledge.** If the work revealed a reusable pattern or an architectural decision, record it where `AGENTS.md` says documentation lives.
6. **Metadata.** Add `issue` and `pr` numbers to the frontmatter if the task has them. Append an `edits` entry if you are not the original author.
7. **Outcome.** Fill the `Outcome` section.
8. **Move.** Move the file to `done/[category]/` using [Procedure 2](#procedure-2--move-an-artifact).

> `done/` requires the work to be committed, and committing requires the user's explicit approval. Ask, then wait. `AGENTS.md` holds the project's git conventions.

## Artifact file template

Every artifact uses this layout:

```markdown
---
created: YYYY-MM-DD HH:mm
created_by: [LLM name and version]
issue: [number, only if one exists]
pr: [number, only if one exists]
edits:
  - date: YYYY-MM-DD HH:mm
    author: [LLM name and version]
---

## Context

[Concisely: why this task exists]

## Plan

- [ ] Step 1
- [ ] Step 2

## Rationale

_If needed — for reviewers_

[Why this approach over alternatives, key findings from investigation, design decisions, constraints, assumptions to verify]

---

## Outcome

[What was done and why — keep it concise]

### Notes

_If needed_

[Trade-offs, deferred work, follow-ups]
```

| Section     | Required? | Write it when                                                                                                                            |
| ----------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `Context`   | Yes       | Creating the file.                                                                                                                       |
| `Plan`      | Yes       | Creating the file. Check items off as you go.                                                                                            |
| `Rationale` | Optional  | Planning, when the approach is not self-evident or rests on deep investigation a reviewer would otherwise miss. Skip it for trivial work. |
| `Outcome`   | Yes       | Moving the file to `done/`.                                                                                                              |
| `Notes`     | Optional  | Moving the file to `done/`, for trade-offs, deferred work, and follow-ups.                                                               |

Dates inside the file use 24-hour format: `2026-03-24 14:30`.

## Project detail files

The project's `agents-instructions/` directory holds deeper reference material, such as an architecture reference. `AGENTS.md` says which files exist and when to read them. Nothing in this skill depends on them.

## Related skills

- **`prompt-authoring-guide`** — follow it when writing or reviewing prompts for major work. If it is not installed in this project, add it with `npx skills add web3web4/.github --skill prompt-authoring-guide --copy -a github-copilot -y`.
