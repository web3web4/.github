---
created: 2026-09-01 09:59
created_by: GitHub Copilot (Claude Opus 5)
---

# Repo AGENTS.md and Template Rename

## Context

Two related gaps in this repository:

1. `engineering-standards/templates/AGENTS.md` is a template, but agent tools
   discover `AGENTS.md` by filename from the nearest directory upward. Any agent
   editing a file under `engineering-standards/templates/` picks it up as the
   governing instruction file and inherits rules that do not apply here: strict
   TypeScript, `zod` at boundaries, `pnpm typecheck`, a `pnpm-lock.yaml`
   exclusion. In a repository whose product is agent instructions, that is the
   worst possible place for a decoy.
2. The repository has no root `AGENTS.md` and no `standards.yml`, so it
   publishes `agents-md-check.yml` without running it on itself. It cannot fail
   its own standard.

## Plan

- [x] Rename `engineering-standards/templates/AGENTS.md` to `AGENTS-template.md`.
- [x] Update every reference: the standards README, the adoption-prompts attach
  list, the error URL in `agents-md-check.yml`, and step 3 of `AGENTS-init.md`.
- [x] State the naming rule in the templates section of the standards README, so
  the exception does not look like an accident.
- [x] Write a root `AGENTS.md` for this repository, covering the rules that are
  specific to publishing skills, templates, and reusable workflows.
- [x] Complete the `task-plans/` scaffold this repository is missing:
  `todo/deferred`, `others/prompts`, `backlog.md`, `scratch.md`, and the `fixes`
  and `analysis` categories.
- [x] Add `.github/workflows/standards.yml` calling `agents-md-check.yml` with
  `verifyScripts: false`.
- [x] Verify the new `AGENTS.md` against every check in `agents-md-check.yml`.

Quality bar for this task, in place of the code checklist in the `task-plans`
skill:

- [x] **Accuracy** — every command, path, and link in the new `AGENTS.md` refers
  to something that exists in this repository.
- [x] **No leftover references** — no file still points at
  `templates/AGENTS.md`.
- [x] **Self-check** — the root `AGENTS.md` passes `agents-md-check.yml`'s
  placeholder, required-section, and setup-pointer checks.
- [x] **Style** — unwrapped prose, matching the rest of `engineering-standards/`.

## Rationale

`AGENTS-init.md`, `templates/README.md`, and `templates/agents-instructions/*`
all keep their destination filenames, and that is worth preserving — it makes the
copy step obvious. The rename is a deliberate single exception, so the rule gets
written down: **template files keep their destination filename, except
`AGENTS.md`, because agent tooling auto-loads it by name.** A hyphen matches the
existing `AGENTS-init.md`.

Categories stay `features` / `fixes` / `analysis` rather than moving to the
documentation set (`drafting` / `revisions` / `analysis`). Two artifacts already
live in `done/features/`, and the work in this repository is building shipped
assets — templates, workflows, skills — not drafting prose.

`verifyScripts` is `false` because there is no `package.json`. The script check
would otherwise skip silently and print a nag line on every run.

---

## Outcome

The template is now `engineering-standards/templates/AGENTS-template.md`. Four
files referenced the old path and were updated: the standards README entry, the
attach list in `adoption-prompts.md`, the error URL in `agents-md-check.yml`, and
step 3 of `AGENTS-init.md`, which now says to copy the file to the repo root as
`AGENTS.md` and explains why it ships under a different name. The naming rule and
its exception are stated in the standards README. References inside
`task-plans/done/` were left alone; they are historical records.

The repository now has a root `AGENTS.md` and a `.github/workflows/standards.yml`
that calls its own `agents-md-check.yml` with `verifyScripts: false`. The new
`AGENTS.md` was checked locally against every rule that workflow applies and
passes. It is not a filled-in copy of the template: the rules that matter here
are about publishing to other repositories, so it covers skill drift, template
and checker coupling, reusable-workflow input compatibility, and the two homes
for prompts.

Two things came up during the work and were handled:

- The `task-plans/` scaffold was incomplete. Added `fixes` and `analysis` under
  each status, `todo/deferred`, `others/prompts`, `backlog.md`, and `scratch.md`.
  Categories stay `features` / `fixes` / `analysis`.
- `## Commands` had nothing true to put in it. A bare `markdownlint-cli2` run
  failed every file in the repository on `MD013` alone, which is exactly the rot
  the repo warns about. Added `.markdownlint.jsonc` turning off `MD013`, `MD060`,
  and `MD041` — all three are broken here by design, and the file records why.
  That reduced a repo-wide run from 26 findings to 6. Four unlabelled fenced
  blocks in `adoption-prompts.md` were labelled `text`.

### Notes

- Six lint findings remain, all deliberate and recorded in `AGENTS.md`:
  emphasis-as-heading and an inline `div` in `profile/README.md`, and `MD031` in
  `skills/prompt-authoring-guide/SKILL.md`. The skill one is left on purpose — a
  whitespace-only edit to `skills/` fails `skills-drift` in every consumer repo
  until they update. It is filed in `scratch.md` to fix alongside the next
  substantive change to that skill.
- Nothing enforces Markdown lint in CI. Adding a lint job to `standards.yml`
  needs the `profile/` findings cleared first. Filed in `scratch.md`.
- The rename changes a path that may already be pasted into a saved
  `adopt-standards.prompt.md` slash command or an attach list. Cheap to fix, but
  it will bite once.
