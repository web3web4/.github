---
created: 2026-07-26 00:13
created_by: GitHub Copilot (Claude Opus 5)
edits: []
---

## Context

Follow-up to `2026-07-25-2255-execution-plans-workflow-skill.md`, which flagged the remaining `agents-instructions/` companion docs as untriaged duplication. Three questions:

1. Should `implementation-checklist.md`, `post-implementation-checklist.md`, and `prompt-authoring-guide.md` join the same skill (marked dev-only), become a new skill, or stay as they are?
2. `md-doc-forge` and `saudi-realesate-tokenization-license` have no `agents-instructions/README.md` — add one.
3. Add a CI check for `skills-lock.json`, provided it does not constrain how skills are customized.

## Plan

### 1. Measure before deciding

- [x] Hash-compare all copies of the three files across the four repos and the `.github` template.
- [x] Diff the non-identical ones to tell deliberate per-project adaptation apart from drift.

**Result:**

| File                               | Copies | Finding                                                          |
| ---------------------------------- | ------ | ---------------------------------------------------------------- |
| `prompt-authoring-guide.md`        | 5      | **Byte-identical everywhere** (`194df346…`). Zero customization. |
| `implementation-checklist.md`      | 4      | All differ. Deliberate rewrites.                                 |
| `post-implementation-checklist.md` | 4      | All differ. Deliberate rewrites.                                 |

The checklists are not drifted — they are rewritten per project. `md-doc-forge` checks manifest JSON validation, `subprocess(check=True)`, `pandoc`/`typst` binary availability, `pytest -v`, and stray `print()`. `realestate-tokenization` checks Zod DTO boundaries, `any` types, `pnpm --filter @realestate-tokenization/…`, Hardhat tests, and `backlog.md` bookkeeping that exists in no other repo. Both are dev projects, so the proposed "dev-only" flag would not have separated them.

### 2. Act on the measurement

- [x] `prompt-authoring-guide` → **new skill** at `skills/prompt-authoring-guide/SKILL.md`. Its own skill, not a supporting file of `execution-plans-workflow`, so its `description` triggers it independently when a prompt is being written or reviewed.
- [x] Both checklists → **stay in `agents-instructions/`**. Templates reworded from "can be used as-is or adapted" to "starting points, not drop-ins".
- [x] `execution-plans-workflow` skill updated: the companion section now separates project-specific checklists from the sibling skill.
- [x] Delete the four vendored `agents-instructions/prompt-authoring-guide.md` copies and the `.github` template copy.
- [x] `saudi-realesate-tokenization-license/agents-instructions/` became empty as a result — folder deleted.

### 3. Add the missing README

- [x] `md-doc-forge/agents-instructions/README.md` — created, with an index of its three project-specific files and a table pointing at the shared skills.
- [x] `saudi-realesate-tokenization-license` — **not applicable.** Its `agents-instructions/` folder no longer exists (see above), so a README there would describe an empty directory.
- [x] Rewrote the two existing READMEs (`AnalyzicAI.ai`, `realestate-tokenization`) to match: project-specific index + shared-skills table + a "do not restore toward the template" warning.

### 4. Add the CI check

- [x] Reusable workflow `.github/workflows/skills-drift.yml` in `web3web4/.github`, called by a 14-line file in each of the four repos.
- [x] It fails on three conditions: a vendored skill differs from upstream, `skills-lock.json` is missing, or the lockfile records `"sourceType": "local"`.
- [x] Verified locally against live upstream — all four repos pass.
- [x] Negative-tested: appending one line to an installed `SKILL.md` is caught.

### 5. Verify

- [x] All 5 copies of each skill byte-identical (`077a5e72…` workflow, `43b08ded…` prompt guide).
- [x] No stale references to `agents-instructions/prompt-authoring-guide` or `agents-instructions/execution-plans-workflow` anywhere in the workspace.
- [x] Workflow YAML parses.

## Rationale

- **Why measure first**: the question offered "dev-only" as the splitting axis. Hashing showed the real axis is _whether the content is project-specific at all_, and that axis cuts straight through the dev projects. Deciding on the stated axis would have skillified two files that cannot survive `npx skills update`.
- **Why the checklists must not become skills**: skills are overwritten wholesale on update. A file whose whole value is per-project wording is the worst possible skill candidate. This is the same reasoning that removed the `CUSTOM` block in the parent task, applied to a second case.
- **Why a separate skill rather than a supporting file**: bundling `prompt-authoring-guide` inside `execution-plans-workflow` would make it discoverable only when the workflow skill loads. As its own skill it has an independent `description` and triggers on prompt-writing requests directly.
- **Why the CI check compares against upstream instead of the lockfile hash**: the CLI's `computedHash` is not a plain sha256 of `SKILL.md` (verified — `e90925bc…` in the lockfile vs `e25b6cb4…` from `shasum -a 256`). Reimplementing an undocumented hash would be fragile. Fetching the upstream file and diffing is exact, needs no auth (the repo is public), and produces a readable diff in the CI log.
- **Why the check does not constrain customization**: it guards only skills that are project-agnostic by construction and carry an explicit "never edit in place" rule. The `skills` input is an explicit allowlist, so a future customizable skill is simply left off it. Everything that legitimately varies per project already lives in `AGENTS.md` or `agents-instructions/`, neither of which the check touches.
- **Why fail rather than warn when upstream moves ahead**: chosen by the user. A plain diff cannot distinguish "edited locally" from "upstream advanced", and failing keeps every repo in lockstep — the whole point of centralizing. The fix is one command.

---

## Outcome

Two shared skills now exist, both installed in all four repos and guarded by CI.

**Pushed to `web3web4/.github`** (`ddbbda4`):

- `skills/prompt-authoring-guide/SKILL.md` — new.
- `skills/execution-plans-workflow/SKILL.md` — companion section reworked.
- `.github/workflows/skills-drift.yml` — new reusable workflow.
- `engineering-standards/README.md` — skills table + reusable-workflows section.
- `engineering-standards/templates/AGENTS-init.md` — installs both skills, new CI step, renumbered to 6 steps.
- `engineering-standards/templates/agents-instructions/README.md` — project-specific framing + shared-skills table.
- `engineering-standards/templates/agents-instructions/prompt-authoring-guide.md` — deleted.

**Left unstaged in the four consuming repos:**

- `.agents/skills/prompt-authoring-guide/` — new; `.agents/skills/execution-plans-workflow/` — refreshed; `skills-lock.json` — both skills recorded.
- `.github/workflows/skills-drift.yml` — new caller. First workflow ever in `AnalyzicAI.ai`.
- `agents-instructions/prompt-authoring-guide.md` — deleted in all four.
- `agents-instructions/README.md` — new in `md-doc-forge`, rewritten in `AnalyzicAI.ai` and `realestate-tokenization`.
- `saudi-realesate-tokenization-license/agents-instructions/` — removed entirely.
- `realestate-tokenization/CLAUDE.md` — both skills listed.

Net: the three code repos now hold an identical _set_ of `agents-instructions/` files whose _contents_ are deliberately project-specific, and two byte-identical skills that CI keeps in lockstep.

### Notes

- **`marriage/awnun` and `marriage/awnun copy`** sit in the same parent directory and use the same `agents-instructions/` system, including a copy of `prompt-authoring-guide.md`. They are not workspace folders and were left untouched. They are candidates for the same rollout.
- **`realestate-tokenization` is on branch `poc-v0.2`.** The caller workflow triggers on `push` to `main` and on `pull_request`, so it will first run on that branch's PR.
- The `.github` repo now has its own `.github/workflows/` directory. The `skills-drift.yml` there is `workflow_call`-only and never runs on its own.
