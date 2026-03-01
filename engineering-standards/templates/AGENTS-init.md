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

## 1. Fill in AGENTS.md

Replace the `...` placeholders in `AGENTS.md` with your project's actual info:

- Project Overview (name, description, apps/packages)
- Development Rules (especially Testing strategy)

## 2. Fill in AGENTS-reference.md

This is the persistent reference file for tech stack, patterns, and architectural decisions:

- **Tech Stack**: List your actual stack, one item per line
- **Standard Engineering Patterns**: Copy from [org standards](https://github.com/web3web4/.github/blob/main/engineering-standards/). Env-loading is pre-filled.
- **Key Architectural Decisions**: One line per constraint. Format: `**Topic**: Constraint.`

Examples to inspire:

```markdown
- **Auth**: Supabase SSR only. No client-side auth flows.
- **Data Fetching**: RSC by default. `use client` only when interactive.
- **State**: URL params for shareable, Zustand for complex local.
- **API Design**: All endpoints validated with Zod. Return typed responses.
```

## 3. Install dotenv-cli

```bash
pnpm add -wD dotenv-cli
```

Adjust the relative path (`../../`) based on app depth. See [env-loading standard](https://github.com/web3web4/.github/blob/main/engineering-standards/env-loading.md).

## 4. Scaffold agents-artifacts

```bash
mkdir -p agents-artifacts/{todo,doing,done}/{bugs,features,analysis}
touch agents-artifacts/todo/backlog.md
```

## No need to add `.gitkeep`.

## Checklist

- [ ] Filled in AGENTS.md (Project Overview, Development Rules)
- [ ] Filled in AGENTS-reference.md (Tech Stack, Patterns, Arch Decisions)
- [ ] Installed `dotenv-cli` and updated app scripts
- [ ] Scaffolded `agents-artifacts/` directory
- [ ] Removed the "Setup guide" link from the top of AGENTS.md
- [ ] Deleted this file
