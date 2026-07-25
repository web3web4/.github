---
created: 2026-07-25 22:55
created_by: GitHub Copilot (Claude Sonnet 4.5)
edits:
  - date: 2026-07-25 23:13
    author: GitHub Copilot (Claude Opus 5)
    note: Fresh-eyes review. Dropped the CUSTOM-block design and its post-update reconciliation step; resolved all open pre-check questions; added missing reference sites; listed the concrete drift to reconcile.
  - date: 2026-07-25 23:30
    author: GitHub Copilot (Claude Opus 5)
    note: Implemented. Recorded outcome.
---

## Context

`agents-instructions/execution-plans-workflow.md` is duplicated across four repos (`AnalyzicAI.ai`, `realestate-tokenization`, `saudi-realesate-tokenization-license`, `md-doc-forge`) and has drifted. `.github/engineering-standards/templates/AGENTS.md` also inlines its own copy of the same text, so there are two sources of truth already.

Package the workflow as an installable **skill** (via the [`skills` CLI](https://github.com/vercel-labs/skills)), sourced from this `.github` repo, vendored into each consuming repo with `--copy` so the content is real committed content rather than a symlink pointing outside the repo.

### Pre-checks — resolved

| Check                                          | Result                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `web3web4/.github` reachable as a skill source | **Public repo** (`gh repo view` → `"visibility":"PUBLIC"`). No auth friction. Anything under `skills/` is public — the workflow text contains nothing sensitive.                                                                                                                                                                                         |
| Node/`npx` available                           | `node v20.18.3`, `npx 10.8.2`. Available for `md-doc-forge` (Python) and `saudi-realesate-tokenization-license` (docs) too. Nothing is added to those repos except the vendored `SKILL.md` and `skills-lock.json`.                                                                                                                                       |
| Where GitHub Copilot reads project skills      | **`.agents/skills/`** — confirmed by the CLI's agent table _and_ empirically: this session had `~/.agents/skills/find-skills/SKILL.md` loaded. The earlier confusion was a global-vs-project path mix-up; `~/.copilot/skills/` is the _global_ path, `~/.agents/skills/` is the global path for the Amp/Cline/Zed family that Copilot also honours here. |
| Where the CLI finds skills in a source repo    | Repo root (if it holds `SKILL.md`), then `skills/`, `skills/.curated/`, `skills/.experimental/`, `skills/.system/`, `.agents/skills/`, `.claude/skills/`, … Walked one level deep for `skills/<name>/SKILL.md`. So `skills/execution-plans-workflow/SKILL.md` at the repo root is a documented path, not a fallback.                                     |

### Correction to the original plan — the `CUSTOM` block was dropped

The original plan put a `<!-- CUSTOM:START -->` block inside the skill holding each project's category names, `doing/` terminology, and companion-doc list — then added a whole plan step (step 5) telling the agent to restore that block from git history after every `npx skills update`, because the CLI overwrites installed files wholesale.

That is a manual merge step dressed up as automation, and it is unnecessary:

1. **The config already lives in `AGENTS.md`.** Every one of the four repos already states its categories there (`features`/`fixes`/`analysis`, or `drafting`/`revisions`/`analysis` for the Saudi repo). A `CUSTOM` block would create a _second_ source of truth per repo — the exact problem this task exists to remove.
2. **The variance can be expressed generically.** Comparing the four copies line by line, the only project-specific parts are the category names, the "writing code" vs "editing content" wording, and which companion checklists exist. Categories → point at `AGENTS.md`. Terminology → "doing the work (writing code, editing content)". Companion docs → "apply only if the file exists in this project". No customization needed.
3. **A file that must never be hand-edited cannot be silently corrupted.** With no `CUSTOM` block, `npx skills update` overwriting is a feature, not a hazard.

Net effect: step 5 deleted, the `CUSTOM` block deleted, and the skill is 100% project-agnostic and safe to update blindly.

### Drift found (what the canonical version reconciles)

Measured with `diff`, not assumed:

1. **Kanban intro line** — `realestate-tokenization` and `AnalyzicAI.ai` have "The directory is structured around a kanban-style lifecycle…"; `md-doc-forge` and `saudi` do not. **Kept.**
2. **`todo/deferred` rule** — `md-doc-forge` had the fullest wording; `AnalyzicAI.ai` dropped the last sentence; `saudi` dropped both. **Kept the fullest.**
3. **`## Rationale`** — missing from `AnalyzicAI.ai`, both in the artifact template and in the "Fill `Rationale` when…" bullet. **Kept.**
4. **Naming-convention example** — `saudi` lacked the `(e.g. 2026-03-24 14:30)` clarifier. **Kept.**
5. **Companion-doc bullets** — `saudi` legitimately omitted the checklist bullets because those files do not exist there. **Made conditional** rather than dropped or hardcoded.

## Plan

### 1. Author the skill in this repo

- [x] Create `skills/execution-plans-workflow/SKILL.md` at the **root** of the `.github` repo.
- [x] YAML frontmatter: `name`, plus a `description` written to actually trigger — naming the concrete moments (starting a non-trivial task, moving an artifact between `todo`/`doing`/`done`, recording an outcome, checking `doing/` at session start).
- [x] Body = the reconciled canonical text from the drift list above, with the three variance points expressed generically.
- [x] Include an explicit "do not edit this file" note pointing project config at `AGENTS.md`.

### 2. Pilot in `md-doc-forge`

- [x] Install and verify on disk — real file, not a symlink.
- [x] **Decision checkpoint hit and resolved.** The CLI writes a `skills-lock.json` at the repo root. A local-path install records `"source": "/Users/funcy/repos/web3web4/.github"`, `"sourceType": "local"` — a machine-specific absolute path that would break `npx skills update` for anyone else. The pilot install was reverted and the real installs were sourced from `web3web4/.github`, which records `"source": "web3web4/.github"`, `"sourceType": "github"`. Portable. The skill mechanism itself passed; no fallback needed.

### 3. Roll out to all four repos

- [x] `md-doc-forge`
- [x] `AnalyzicAI.ai` — also fixed the missing-`Rationale` drift as a side effect.
- [x] `realestate-tokenization` — **plan revised during execution.** The plan proposed a `.claude/skills/` relative symlink for Claude Code. The user rejected it as duplication; instead `CLAUDE.md` now carries a `## Skills` section pointing Claude Code directly at `.agents/skills/execution-plans-workflow/SKILL.md`. Simpler, one real file, no symlink for the CLI to trip over.
- [x] `saudi-realesate-tokenization-license` — the conditional companion-doc wording resolves correctly there (only `prompt-authoring-guide.md` exists).

### 4. Fix every reference site

- [x] `AnalyzicAI.ai/agents-instructions/README.md` — table row removed, replaced with a pointer paragraph. **(missed by the original plan)**
- [x] `realestate-tokenization/agents-instructions/README.md` — same. **(missed)**
- [x] `realestate-tokenization/execution-plans/todo/scratch.md` line 11. **(missed)**
- [x] All four `AGENTS.md` files.
- [x] `realestate-tokenization/CLAUDE.md`.

### 5. Fix the upstream templates

- [x] `engineering-standards/templates/AGENTS.md` — inlined mechanics replaced with a pointer. Session-level rules (session-start check, proactive filing, `todo/` is passive) kept inline; they belong in `AGENTS.md`, not the skill.
- [x] `engineering-standards/templates/AGENTS-init.md` — new step 3 (install the skill, with the Claude Code note), later steps renumbered, checklist updated, non-dev section updated.
- [x] `engineering-standards/README.md` — new `## Skills` section with the install command.

### 6. Verify

- [x] `npx skills list` in each repo shows the skill, source `web3web4/.github`, agents _Antigravity, Cursor, Gemini CLI, GitHub Copilot_.
- [x] All five copies byte-identical (`md5` → `3410f7f33826ed56e090928213431b9a`), lockfile `computedHash` identical across all four.
- [x] `grep -rn "agents-instructions/execution-plans-workflow"` returns only this artifact.
- [x] `git check-ignore` confirms no repo gitignores `.agents/` or `skills-lock.json`.
- [ ] Confirm in a **fresh session** that the skill appears in the agent's skill list. Cannot be self-verified inside the installing session — skills are enumerated at session start.

## Rationale

- **Why `.github` as the source repo**: it already holds the org's shared `agents-instructions/` templates and the `AGENTS.md` / `AGENTS-init.md` templates. Existing convention, not a new concept.
- **Why a skill over a symlink or `git subtree`**: `npx skills` is purpose-built for this, works across Claude Code / Copilot / Cursor / Codex and ~70 other agents, and gives an explicit reviewable per-repo update step instead of silent propagation or subtree ceremony.
- **Why `--copy` and not the default symlink method**: the default symlinks to a canonical copy _outside_ the repo. Only `--copy` produces real, git-committed, team-portable content.
- **Why `skills/` at repo root**: it is one of the CLI's documented container directories, walked one level deep for the flat `skills/<name>/SKILL.md` layout. Nesting under `engineering-standards/` would rely on the undocumented recursive fallback.
- **Why `-a github-copilot` only**: its project path is `.agents/skills/`, which is also the project path for Codex, Cursor, Gemini CLI, Amp, Cline, Zed, Antigravity, and OpenCode. One install covers the broadest set with one file.
- **Why the skill still needs the `AGENTS.md` pointer**: skills load by description matching, which is probabilistic. The explicit `AGENTS.md` line makes the read deterministic. Both mechanisms, not one.
- **Why install from GitHub and not a local path**: `skills-lock.json` records the source verbatim. A local path is machine-specific and non-portable.

---

## Outcome

The `execution-plans/` workflow now has one source of truth: `skills/execution-plans-workflow/SKILL.md` in `web3web4/.github`, vendored into all four consuming repos as an installed skill.

**Committed and pushed** (`.github` only, commit `0293809`):

- `skills/execution-plans-workflow/SKILL.md` — new, project-agnostic.
- `engineering-standards/templates/AGENTS.md` — ~80 lines of inlined mechanics replaced with a 3-line pointer.
- `engineering-standards/templates/AGENTS-init.md` — new install step + checklist items.
- `engineering-standards/README.md` — new `## Skills` section.

**Left unstaged for review** in the four consuming repos:

- `.agents/skills/execution-plans-workflow/SKILL.md` + `skills-lock.json` — new, both need committing.
- `agents-instructions/execution-plans-workflow.md` — deleted.
- `AGENTS.md` — pointer repointed at the skill.
- `agents-instructions/README.md` (AnalyzicAI.ai, realestate-tokenization) — index row replaced with a pointer paragraph.
- `execution-plans/todo/scratch.md`, `CLAUDE.md` (realestate-tokenization).

Duplication went from 5 divergent copies to 5 byte-identical copies with a machine-checkable upstream. Refresh any repo with `npx skills update execution-plans-workflow`.

### Notes

- **The one open item** is the fresh-session check in step 6. Skills are enumerated when a session starts, so the install cannot be confirmed from inside the session that performed it. Verify at the start of the next session.
- **`md-doc-forge` had pre-existing staged changes** (`M AGENTS.md`, `A agents-instructions/execution-plans-workflow.md`) that were already in the index before this task. They were left untouched. Note that the staged `A` entry now refers to a deleted working file — unstage it before committing.
- Out of scope, flagged for later: `implementation-checklist.md`, `post-implementation-checklist.md`, and `prompt-authoring-guide.md` are duplicated across three repos too and have not been checked for the same drift. Natural candidates for the same treatment now that this has proven out.
- The CLI sends anonymous install telemetry (auto-disabled in CI). `DISABLE_TELEMETRY=1` / `DO_NOT_TRACK=1` are available.
