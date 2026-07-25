---
created: 2026-07-25 22:55
created_by: GitHub Copilot (Claude Sonnet 4.5)
edits:
  - date: 2026-07-25 23:13
    author: GitHub Copilot (Claude Opus 5)
    note: Fresh-eyes review. Dropped the CUSTOM-block design and its post-update reconciliation step; resolved all open pre-check questions; added missing reference sites; listed the concrete drift to reconcile.
---

## Context

`agents-instructions/execution-plans-workflow.md` is duplicated across four repos (`AnalyzicAI.ai`, `realestate-tokenization`, `saudi-realesate-tokenization-license`, `md-doc-forge`) and has drifted. `.github/engineering-standards/templates/AGENTS.md` also inlines its own copy of the same text, so there are two sources of truth already.

Package the workflow as an installable **skill** (via the [`skills` CLI](https://github.com/vercel-labs/skills)), sourced from this `.github` repo, vendored into each consuming repo with `--copy` so the content is real committed content rather than a symlink pointing outside the repo.

### Pre-checks — resolved

| Check | Result |
| --- | --- |
| `web3web4/.github` reachable as a skill source | **Public repo** (`gh repo view` → `"visibility":"PUBLIC"`). No auth friction. Anything under `skills/` is public — the workflow text contains nothing sensitive. |
| Node/`npx` available | `node v20.18.3`, `npx 10.8.2`. Available for `md-doc-forge` (Python) and `saudi-realesate-tokenization-license` (docs) too. Nothing is added to those repos except the vendored `SKILL.md`. |
| Where GitHub Copilot reads project skills | **`.agents/skills/`** — confirmed by the CLI's agent table *and* empirically: this very session has `~/.agents/skills/find-skills/SKILL.md` loaded. The earlier confusion was a global-vs-project path mix-up; `~/.copilot/skills/` is the *global* path, `~/.agents/skills/` is the global path for the Amp/Cline/Zed family that Copilot also honours here. |
| Where the CLI finds skills in a source repo | Repo root (if it holds `SKILL.md`), then `skills/`, `skills/.curated/`, `skills/.experimental/`, `skills/.system/`, `.agents/skills/`, `.claude/skills/`, … Walked one level deep for `skills/<name>/SKILL.md`. So `skills/execution-plans-workflow/SKILL.md` at the repo root is a documented path, not a fallback. |

### Correction to the original plan — the `CUSTOM` block is dropped

The original plan put a `<!-- CUSTOM:START -->` block inside the skill holding each project's category names, `doing/` terminology, and companion-doc list — then added a whole plan step (step 5) telling the agent to restore that block from git history after every `npx skills update`, because the CLI overwrites installed files wholesale.

That is a manual merge step dressed up as automation, and it is unnecessary:

1. **The config already lives in `AGENTS.md`.** Every one of the four repos already states its categories there (`features`/`fixes`/`analysis`, or `drafting`/`revisions`/`analysis` for the Saudi repo). A `CUSTOM` block would create a *second* source of truth per repo — the exact problem this task exists to remove.
2. **The variance can be expressed generically.** Comparing the four copies line by line, the only project-specific parts are the category names, the "writing code" vs "editing content" wording, and which companion checklists exist. Categories → point at `AGENTS.md`. Terminology → "doing the work (writing code, editing content)". Companion docs → "apply only if the file exists in this project". No customization needed.
3. **A file that must never be hand-edited cannot be silently corrupted.** With no `CUSTOM` block, `npx skills update` overwriting is a feature, not a hazard.

Net effect: step 5 is deleted, the `CUSTOM` block is deleted, and the skill becomes 100% project-agnostic and safe to update blindly.

### Drift found (what the canonical version must reconcile)

Measured with `diff`, not assumed:

1. **Kanban intro line** — `realestate-tokenization` and `AnalyzicAI.ai` have "The directory is structured around a kanban-style lifecycle, categorized by the type of work:"; `md-doc-forge` and `saudi` do not. **Keep it.**
2. **`todo/deferred` rule** — `md-doc-forge` has the fullest wording ("Not picked up until the project advances to that phase. Files here are not categorized into subfolders."); `AnalyzicAI.ai` drops the last sentence; `saudi` drops both. **Keep the fullest.**
3. **`## Rationale`** — missing from `AnalyzicAI.ai`, both in the artifact template and in the "Fill `Rationale` when…" bullet. **Keep it** (this was the drift the original plan spotted).
4. **Naming-convention example** — `saudi` lacks the `(e.g. 2026-03-24 14:30)` clarifier. **Keep it.**
5. **Companion-doc bullets** — `saudi` legitimately omits the implementation/post-implementation checklist bullets because those files do not exist there. **Make the bullets conditional** rather than dropping or hardcoding them.

## Plan

### 1. Author the skill in this repo

- [ ] Create `skills/execution-plans-workflow/SKILL.md` at the **root** of the `.github` repo.
- [ ] YAML frontmatter: `name`, plus a `description` written to actually trigger — it must name the concrete moments (starting a non-trivial task, moving an artifact between `todo`/`doing`/`done`, recording an outcome, checking `doing/` at session start).
- [ ] Body = the reconciled canonical text from the drift list above, with the three variance points expressed generically.
- [ ] Include an explicit "do not edit this file" note pointing project config at `AGENTS.md`, since `npx skills update` overwrites.

### 2. Pilot in `md-doc-forge`

Smallest repo, standard categories.

- [ ] `npx skills add web3web4/.github --skill execution-plans-workflow --copy -a github-copilot -y`
- [ ] Verify on disk: `.agents/skills/execution-plans-workflow/SKILL.md` is a real file, not a symlink; check whether the CLI wrote any lockfile/metadata alongside it and whether that should be committed or ignored.
- [ ] Update `md-doc-forge/AGENTS.md` — repoint the "Before you create, move, or complete an artifact" line at the installed skill path.
- [ ] Delete `md-doc-forge/agents-instructions/execution-plans-workflow.md`.
- [ ] **Decision checkpoint**: if the install produces broken or non-portable content, or the CLI writes machine-specific state into the repo, stop and fall back to a plain vendored copy — do not force the skill mechanism past this point.

### 3. Roll out to the remaining three repos

Same sub-steps as the pilot, minus the checkpoint.

- [ ] `AnalyzicAI.ai` — also fixes the missing-`Rationale` drift as a side effect.
- [ ] `realestate-tokenization` — this is the only repo with a `CLAUDE.md`. Claude Code reads `.claude/skills/`, not `.agents/skills/`. Rather than a second `--copy` (which would put two identical files in one repo), add a committed **repo-relative symlink** `.claude/skills/execution-plans-workflow → ../../.agents/skills/execution-plans-workflow`. Relative symlinks are stored by git and stay valid for anyone who clones. Verify the CLI's `list`/`update` do not choke on it.
- [ ] `saudi-realesate-tokenization-license` — categories `drafting`/`revisions`/`analysis` (stays in its `AGENTS.md`, not in the skill); only `prompt-authoring-guide.md` exists there, so the conditional companion-doc wording must resolve correctly.

### 4. Fix every reference site

The original plan missed three of these.

- [ ] `AnalyzicAI.ai/agents-instructions/README.md` — table row for `execution-plans-workflow.md`. **(missed)**
- [ ] `realestate-tokenization/agents-instructions/README.md` — same table row. **(missed)**
- [ ] `realestate-tokenization/execution-plans/todo/scratch.md` line 11 — inline path reference. **(missed)**
- [ ] All four `AGENTS.md` files (covered by steps 2–3).

### 5. Fix the upstream templates

- [ ] `engineering-standards/templates/AGENTS.md` — replace the inlined `### Directory Structure & Lifecycle` / `### Stale Work` / `### Naming Convention` / `### Artifact File Structure` subsections with a short pointer to the skill. Keep the session-level rules (session-start check, proactive filing, `todo/` is passive) inline — those belong in `AGENTS.md`, not the skill.
- [ ] `engineering-standards/templates/AGENTS-init.md` — step 2 and the checklist: install the skill instead of copying a workflow file.
- [ ] `engineering-standards/README.md` — add a `## Skills` entry.

### 6. Verify

- [ ] `npx skills list` in each repo shows the skill.
- [ ] Byte-compare the four installed copies — they must be identical.
- [ ] `grep -rn "agents-instructions/execution-plans-workflow"` across the workspace returns nothing.
- [ ] Confirm in a **fresh session** that the skill appears in the agent's skill list. (This cannot be self-verified inside the session that installs it — skills are enumerated at session start.)

## Rationale

- **Why `.github` as the source repo**: it already holds the org's shared `agents-instructions/` templates and the `AGENTS.md` / `AGENTS-init.md` templates. Existing convention, not a new concept.
- **Why a skill over a symlink or `git subtree`**: `npx skills` is purpose-built for this, works across Claude Code / Copilot / Cursor / Codex and ~70 other agents, and gives an explicit reviewable per-repo update step instead of silent propagation or subtree ceremony.
- **Why `--copy` and not the default symlink method**: the default symlinks to a canonical copy *outside* the repo. Only `--copy` produces real, git-committed, team-portable content.
- **Why `skills/` at repo root**: it is one of the CLI's documented container directories, walked one level deep for the flat `skills/<name>/SKILL.md` layout. Nesting under `engineering-standards/` would rely on the undocumented recursive fallback.
- **Why `-a github-copilot` only (plus one symlink)**: `github-copilot`'s project path is `.agents/skills/`, which is also the project path for Codex, Cursor, Gemini CLI, Amp, Cline, Zed, and OpenCode. One install covers the broadest set. Only `realestate-tokenization` has a `CLAUDE.md`, and a relative symlink serves it without a duplicate file.
- **Why the skill still needs the `AGENTS.md` pointer**: skills load by description matching, which is probabilistic. The explicit `AGENTS.md` line makes the read deterministic. Both mechanisms, not one.

---

## Outcome

_In progress._

### Notes

- Out of scope, flagged for later: `implementation-checklist.md`, `post-implementation-checklist.md`, and `prompt-authoring-guide.md` are duplicated across three repos too and have not been checked for the same drift. Natural candidates for the same treatment once this proves out.
- The CLI sends anonymous install telemetry (auto-disabled in CI). `DISABLE_TELEMETRY=1` / `DO_NOT_TRACK=1` are available.
