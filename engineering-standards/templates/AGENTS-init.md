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
- **Skip** steps 2 (architecture-reference), 3 (dotenv-cli), and the quality-gate items in the Checklist. Or replace them with an equivalent/similar step(s), if applicable.
- **Keep**: `AGENTS.md`, `execution-plans/` scaffold, and `agents-instructions/prompt-authoring-guide.md`.
- **Delete** `agents-instructions/implementation-checklist.md`, `agents-instructions/post-implementation-checklist.md`, and `agents-instructions/architecture-reference.md` — or replace with the equivalent/similar file(s), if applicable.

---

## 1. Fill in AGENTS.md

Replace the `...` placeholders in `AGENTS.md` with your project's actual info:

- Project Overview (name, description, apps/packages)
- Project Status
- Development Rules (especially Testing strategy)

Remove the `> **Setup guide:**` line from the top of `AGENTS.md` when done.

## 2. Set up agents-instructions/

Copy the `agents-instructions/` folder from the templates into your repo root. Then fill in:

- **`architecture-reference.md`**: List your actual tech stack, standard engineering patterns (copy from [org standards](https://github.com/web3web4/.github/blob/main/engineering-standards/)), and key architectural decisions. One line per constraint. Format: `**Topic**: Constraint.`

  Examples to inspire:

  ```markdown
  - **Auth**: Supabase SSR only. No client-side auth flows.
  - **Data Fetching**: RSC by default. `use client` only when interactive.
  - **State**: URL params for shareable, Zustand for complex local.
  - **API Design**: All endpoints validated with Zod. Return typed responses.
  ```

- The other files (`implementation-checklist.md`, `post-implementation-checklist.md`, `prompt-authoring-guide.md`) can be used as-is or adapted to your project. Update the `post-implementation-checklist.md` quality-gate commands to match your project's script names.

## 3. Install dotenv-cli

```bash
pnpm add -wD dotenv-cli
```

Adjust the relative path (`../../`) based on app depth. See [env-loading standard](https://github.com/web3web4/.github/blob/main/engineering-standards/env-loading.md).

## 4. Scaffold execution-plans

```bash
mkdir -p execution-plans/{todo,doing,done}/{fixes,features,analysis}
mkdir -p execution-plans/todo/deferred
mkdir -p execution-plans/others/prompts
touch execution-plans/todo/scratch.md
```

No need to add `.gitkeep`.

## Checklist

- [ ] Filled in AGENTS.md (Project Overview, Project Status, Project Type, Development Rules)
- [ ] _(dev only)_ Copied and filled in `agents-instructions/architecture-reference.md` (Tech Stack, Patterns, Arch Decisions)
- [ ] _(dev only)_ Adapted `agents-instructions/post-implementation-checklist.md` quality-gate commands
- [ ] _(dev only)_ Installed `dotenv-cli` and updated app scripts
- [ ] _(non-dev)_ Deleted, edit and/or replace dev-only `agents-instructions/` files (architecture-reference, implementation-checklist, post-implementation-checklist)
- [ ] Scaffolded `execution-plans/` directory
- [ ] Removed the "Setup guide" line from the top of AGENTS.md
- [ ] Deleted this file
