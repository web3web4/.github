# AGENTS-init.md — Project Setup

> Use this file **once** when initializing a new project. Run through the setup actions and checklist, then delete this file.

---

## Preferred Stack

Use `pnpm` as the package manager for all projects. The default tech stack is:

- **Framework**: Next.js (App Router) with Tailwind CSS
- **Monorepo**: pnpm workspaces + Turborepo
- **Backend & Auth**: Supabase
- **Smart Contracts** (when needed): Hardhat
- **Deployment**: Vercel
- **Testing**: Vitest
- **Linting**: ESLint (shared config)
- **Validation**: Zod

Deviate only when the project has a specific reason to.

---

## Non-Development Projects

If this project is **not a software development repo** (e.g., documentation, research, regulatory submissions):

- Set `## Project Type` in `AGENTS.md` to the appropriate value.
- **Skip** _Set up agents-instructions/_, _Install dotenv-cli_, and the quality-gate items in the Checklist. Or replace them with an equivalent/similar step, if applicable.
- **Keep**: `AGENTS.md`, the shared skills, the CI checks, and the `task-plans/` scaffold. Set the categories in `AGENTS.md` to `drafting`/`revisions`/`analysis`.
- **Delete** `agents-instructions/architecture-reference.md` — or replace it with the equivalent file for this project. If nothing is left, delete the `agents-instructions/` folder.
- The implementation quality bar in the `task-plans` skill is written for code. Replace it for this project by putting the rules that actually apply — sourcing, accuracy, review — under `Development Rules` in `AGENTS.md`.

_Optional starter for regulatory, legal, or fact-sensitive documentation projects — adapt or drop what doesn't apply:_

- **No-Invention Rule (HARD RULE)**: Do not write any fact, number, date, name, citation, commitment, or timing claim that the user has not explicitly provided, that is not already in this repository, or that cannot be cited or verified. Replace an unverified claim with a `<!-- TODO (author): verify and cite source -->` placeholder instead of guessing.
- **Source-Comment Rule**: Every factual claim, statistic, date, or third-party statement gets an inline comment recording where it came from: `<!-- source: <URL or file path or "user (YYYY-MM-DD)">; verified YYYY-MM-DD by <agent> -->`.
- **Draft-status marker**: Documents are working drafts until marked `[FINAL]`.
- **Expert review required**: State the applicable professional review this project needs before external submission (legal, compliance, technical) and who must perform it.

If the workspace has a document-driven workflow (a form, a checklist, a build pipeline), add a short numbered `## How This Workspace Works` section to `AGENTS.md` describing the operating loop, not just the file list.

---

> **Do steps 1 and 2 first, before anything else.** They are ordered ahead of the writing steps on purpose: the `task-plans` skill governs how an agent plans work, and the `task-plans/` folder is where the plan artifact goes. An agent that starts at step 3 has neither, so it cannot follow the process it is about to be told to follow.

## 1. Install the shared skills

```bash
npx skills add web3web4/.github \
  --skill task-plans \
  --skill prompt-authoring-guide \
  --copy -a github-copilot -y
```

This writes `.agents/skills/<name>/SKILL.md` for each skill plus a `skills-lock.json` at the repo root. Commit all of it.

`.agents/skills/` is the project skill path shared by GitHub Copilot, Codex, Cursor, Gemini CLI, Antigravity, Cline, Zed, Amp, and OpenCode — one install covers all of them. Claude Code reads `.claude/skills/` instead; rather than duplicating the files, point `CLAUDE.md` at the `.agents/skills/` paths directly.

Refresh later with `npx skills update`. These skills are project-agnostic — **never edit an installed file**, updates overwrite it. Project-specific rules belong in `AGENTS.md`.

> Install from the GitHub source, not a local path. `skills-lock.json` records the source verbatim, and a local path is an absolute machine-specific path that breaks `npx skills update` for everyone else.

## 2. Scaffold task-plans

```bash
mkdir -p task-plans/{todo,doing,done}/{fixes,features,analysis}
mkdir -p task-plans/todo/deferred
mkdir -p task-plans/others/prompts
touch task-plans/todo/scratch.md task-plans/todo/backlog.md
```

No need to add `.gitkeep`. Adjust the category names if this project uses a different set — whatever you pick here must match the categories you write into `AGENTS.md` in the next step.

## 3. Fill in AGENTS.md

Copy `templates/AGENTS-template.md` to the repo root as `AGENTS.md`. It ships under a different name only so that it does not act as an instruction file inside the standards repo — the copy in your repo must be named `AGENTS.md`.

Then replace the `...` placeholders with your project's actual info:

- Project Overview (name, description, apps/packages)
- Project Status
- Development Rules (especially Testing strategy)
- Commands — the quality gates. The template ships an example for a TS/pnpm stack; replace it with this project's actual commands. The `task-plans` skill sends the agent here to find them, so a section left as the example (or emptied out) means the gates never run. If the project has non-quality-gate dev workflows worth documenting (cold-start scripts, environment resets, scoped per-app commands, single-test invocation), list them above the quality gates in their own subsection.
- Git — the base branch and the diff-review command, with this project's lockfile and generated paths excluded.

The skill owns the shared quality bar and the completion procedure. `AGENTS.md` supplies only what differs per project: category names, language and stack rules, commands, and git conventions. Anything you write in `Development Rules` becomes a checkbox the agent adds to its plan, so keep those rules specific and testable. If the project has 2+ recurring task types with different rules (e.g. drafting different document kinds, or working in different app layers), add a `### When doing X` subsection under `Development Rules` for each, instead of one long generic list.

Remove the `> **Setup guide:**` line from the top of `AGENTS.md` when done.

## 4. Set up agents-instructions/

Copy the `agents-instructions/` folder from the templates into your repo root. Then fill in:

- **`architecture-reference.md`**: List your actual tech stack, standard engineering patterns (copy from [org standards](https://github.com/web3web4/.github/blob/main/engineering-standards/)), and key architectural decisions. One line per constraint. Format: `**Topic**: Constraint.`

  Examples to inspire:

  ```markdown
  - **Auth**: Supabase SSR only. No client-side auth flows.
  - **Data Fetching**: RSC by default. `use client` only when interactive.
  - **State**: URL params for shareable, Zustand for complex local.
  - **API Design**: All endpoints validated with Zod. Return typed responses.
  ```

Keep this folder small. It holds deep reference material read on demand, not rules the agent needs every session — those go in `AGENTS.md`. Do not add implementation or completion checklists here; the `task-plans` skill owns both.

> The `task-plans/` workflow mechanics and the prompt authoring guide are **not** copied into `agents-instructions/`. They are project-agnostic and installed as shared skills in step 1.

## 5. Add the standards CI checks

Create `.github/workflows/standards.yml`:

```yaml
name: Standards

on:
  push:
    branches: [main]
  pull_request:

jobs:
  skills-drift:
    uses: web3web4/.github/.github/workflows/skills-drift.yml@main
  agents-md:
    uses: web3web4/.github/.github/workflows/agents-md-check.yml@main

  actionlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Download actionlint
        id: get_actionlint
        run: bash <(curl -fsSL https://raw.githubusercontent.com/rhysd/actionlint/v1.7.12/scripts/download-actionlint.bash)
        shell: bash
      - name: Lint GitHub Actions
        run: ${{ steps.get_actionlint.outputs.executable }} -color
        shell: bash
```

`skills-drift` fails if a vendored skill differs from upstream, or if `skills-lock.json` is missing or records a local install source.

`agents-md` fails if `AGENTS.md` is missing, still contains template placeholders, is missing a required section, still references this setup file, or lists a `pnpm <script>` under `## Commands` that does not exist in the root `package.json`. That last check is what stops the quality gates from rotting: the `task-plans` skill sends every agent to `## Commands`, so a stale script name there silently disables the gates.

`actionlint` validates GitHub Actions workflow syntax. The workflow downloads actionlint from its versioned upstream release, so it does not require a project dependency.

Both checks take inputs. Override them only when the project legitimately differs:

```yaml
  agents-md:
    uses: web3web4/.github/.github/workflows/agents-md-check.yml@main
    with:
      requiredSections: Project Overview,Project Status,Project Type,Development Rules,Commands
      verifyScripts: true # set false for non-pnpm or non-development repos
```

## 6. Install dotenv-cli

```bash
pnpm add -wD dotenv-cli
```

Adjust the relative path (`../../`) based on app depth. See [env-loading standard](https://github.com/web3web4/.github/blob/main/engineering-standards/env-loading.md).

## 7. Optional: README.md hints

[`templates/README.md`](README.md) has a short list of non-exclusive, optional sections (a pointer to `AGENTS.md`, a status badge, a quick-start link) worth adding to this project's existing `README.md` for human readers. Copy only what fits — it is not a drop-in replacement.

## Checklist

In order. The first two unblock every step after them.

- [ ] Installed the shared skills and committed `.agents/skills/` + `skills-lock.json`
- [ ] Scaffolded `task-plans/` directory
- [ ] Filled in AGENTS.md (Project Overview, Project Status, Project Type, Development Rules, Commands, Git)
- [ ] Every command listed under `## Commands` exists and was run once to confirm it passes
- [ ] _(dev only)_ Copied and filled in `agents-instructions/architecture-reference.md` (Tech Stack, Patterns, Arch Decisions)
- [ ] Added `.github/workflows/standards.yml` with `skills-drift`, `agents-md`, and `actionlint` jobs. And deleted, if previously existed, `skills-drift.yml` and `agents-md.yml`.
- [ ] _(Claude Code projects)_ Pointed `CLAUDE.md` at the `.agents/skills/` paths
- [ ] _(dev only)_ Installed `dotenv-cli` and updated app scripts
- [ ] _(optional)_ Applied relevant `templates/README.md` hints to this project's README.md
- [ ] _(non-dev)_ Deleted or replaced the dev-only `agents-instructions/` files, and rewrote `Development Rules` for this project's kind of work
- [ ] Removed the "Setup guide" line from the top of AGENTS.md
- [ ] Deleted this file
