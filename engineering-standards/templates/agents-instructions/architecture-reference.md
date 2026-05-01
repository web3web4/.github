# Architecture & Stack Reference

> Only read this file when the task involves tech stack choices, new dependencies, or architectural decisions. Do NOT parse this every session.

## Tech Stack

- **Framework**: ...
- **Backend**: ...
- **Language**: TypeScript (strict)
- **Monorepo**: pnpm workspaces + Turborepo
- **Database & Auth**: ...
- **Testing**: Vitest
- **Deployment**: ...
- **Linting**: ESLint (shared config)
- **Validation**: Zod

## Standard Engineering Patterns

- **Env Variables**: Handled via `dotenv-cli` at the workspace root. Do NOT use framework-specific env loaders. See [env-loading standard](https://github.com/web3web4/.github/blob/main/engineering-standards/env-loading.md).

## Key Architectural Decisions

- ...

## Pre-commit & CI

- ...
