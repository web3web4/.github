# Post-Implementation Checklist

> **Scope:** Software development projects only. Delete, adapt or replace this file for documentation, research, or non-code repos. The Quality Gates section (step 3) is TypeScript/pnpm-specific — adapt commands if using a different stack.

Run this checklist after implementation is complete and code is committed.

## 1. Metadata

- [ ] Frontmatter `created` / `created_by` fields are set (or add an `edits` entry if you're not the original author)
- [ ] Add `issue` and/or `pr` number to frontmatter if applicable:
  ```yaml
  ---
  created: 2026-04-04 14:30
  created_by: Claude Opus 4
  issue: 12
  pr: 19
  edits:
    - date: 2026-04-05 09:00
      author: GPT-4o
  ---
  ```

## 2. Plan vs Implementation Review

- [ ] Walk each plan checkbox — confirm the code matches intent
- [ ] Mark completed steps `[x]`, note any skipped steps with reason (capture follow-ups in step 5)

## 3. Quality Gates

Run from the repo root. Fix any failures before proceeding.

```sh
pnpm typecheck          # strict TS compilation
pnpm build              # full turbo build
pnpm lint:fix           # ESLint fix and report across all packages
pnpm test               # Vitest tests
pnpm format             # Prettier formatting that writes to files
```

For scoped work, use filters: `pnpm --filter @[org]/[package] typecheck build lint test`

> **Automation context:** Pre-commit hook (Husky + lint-staged) runs Prettier on staged files automatically. CI runs the full pipeline above on every push/PR to `main`/`develop`. Running these checks locally before pushing is recommended to avoid CI failures.

## 4. Final Diff Review

- [ ] Review the full git diff against the plan: `git diff main...HEAD -- . ':!pnpm-lock.yaml'`
- [ ] No unintended changes, no leftover debug code, no `console.log`
- [ ] No `any` types introduced, no disabled lint rules without justification

## 5. Deferred Work

- [ ] File skipped plan items or new ideas to `todo/[category]/` or `todo/deferred/` (follow [Proactive Filing](../AGENTS.md#proactive-filing))

## 6. Artifact Lifecycle

- [ ] Fill `## Outcome` section in the artifact
- [ ] Move artifact from `doing/[category]/` → `done/[category]/`

## 7. Knowledge Extraction

- [ ] If the work reveals a reusable architectural pattern → update `docs/` or `AGENTS.md`
- [ ] Record lessons learned in repo/user memory if applicable

## 8. Git Operations — Explicit Approval Required

- [ ] When the user asks for a **New PR Creation Data** provide each of the following fields under a bold label with a separate MarkDown code block — paste-ready for GitHub forms fields:
  - issue title,
  - issue description
  - branch name
  - commit message
  - PR title
  - PR description
- [ ] **Stop. Wait for explicit approval before running any git write operation** (commit, push, branch create/delete, force-push, amend).

**Note:** Be very concise.

**Git Conventions:**

- **Branch:** `type/issue-number-description` — e.g. `feat/42-wallet-siwe-binding`, `fix/87-auth-redirect`
- **Commit & PR title:** `type(scope): description` — e.g. `chore(agents): reorganize instruction files`
- **Issue title/description:** Plain English

Types: `feat`, `fix`, `chore`, `refactor`, `test`, `docs`
