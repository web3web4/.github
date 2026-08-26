# [Project Name] - Context for AI Agents

> This file is read by AI coding agents every session. Keep it lean — no examples, no placeholders.
> **Setup guide:** See [AGENTS-init.md](AGENTS-init.md) for instructions on filling this out.

## Project Overview

**[Project Name]** is ...

- **[app/package]** — ...

## Project Status

...

## Project Type

`[Development — TypeScript]` <!-- Options: Development — TypeScript | Development — [language] | Documentation | Research | ... -->

## Architecture & Stack

See [`agents-instructions/architecture-reference.md`](agents-instructions/architecture-reference.md) for tech stack, engineering patterns, and architectural decisions. Only read that file when the task involves stack choices, new dependencies, or architecture.

## Development Rules

<!-- This project's variations on the execution-plans-workflow skill's implementation quality bar. Keep them specific: language, validation library, test runner, and rules unique to this codebase. -->

- **Code Quality**: Strict TypeScript. No `any` types. Use `zod` for all runtime boundary validations (APIs, forms).
- **Shared types**: A type that crosses the client–server boundary is declared once in a shared package. Never redeclare it per app.
- **UI and accessibility**: Mobile-first and touch-accessible. No hover-only control and no essential data hidden in a tooltip. Interactive elements have large touch targets and a visible focus state.
- **Documentation Location**:
  - Persistent architecture/API docs → `docs/`
  - Ephemeral agent planning/logs → `execution-plans/` (do not gitignore)
  - All other `.md` files should only be created inside `execution-plans/` or `docs/` unless the task explicitly requires otherwise.
- **Testing**: ...
- **Specs vs. code**: If code deviates from a spec but the code is better, propose updating the spec — don't "fix" working code.
- **Test failures**: Investigate production code first before assuming the test is wrong.

## AI Agent Workflow (The "execution-plans" system)

Plans, task breakdowns, investigation notes, and progress logs are not chat output. They are Markdown artifacts under `execution-plans/[status]/[category]/`. Status is `todo`/`doing`/`done`. This project's categories are `features`, `fixes`, and `analysis`. <!-- Documentation projects: use drafting/revisions/analysis -->

- **Non-trivial work gets an artifact, and the skill tells you how to write it**: Multi-step tasks, bug investigations, and anything a future session needs as context get exactly one artifact. Skip it for trivial, single-step changes. The moment you decide an artifact is needed — or you are about to plan a task, write into an artifact, move one between status folders, or mark one done — read [`execution-plans-workflow`](.agents/skills/execution-plans-workflow/SKILL.md) in full. It owns the folder lifecycle, the naming convention, the required file structure, and the checklists. Do not work from memory and do not skim: if you have not opened the file this session, you do not know the rules. It is installed by `npx skills` and shared across repos — never edit it in place, and put project-specific rules in this file instead.
- **The plan goes in the artifact, not in the chat reply**: When the user asks for a plan, the deliverable is the artifact file. Reply with a link to it and a short summary.
- **Keep the artifact's status folder current**: Move the file when you start the work and again when the work is complete. The skill defines how.
- **Do not proactively scan `execution-plans/` for work** — not `todo/`, not `doing/`. The user attaches or names the artifact when one is relevant, including when resuming unfinished work from a previous session. `todo/` is a passive backlog — work on an item only when the user asks for that item.
- **Park stray findings in `scratch.md`**: Notice a bug, idea, reusable pattern, or architectural insight while doing unrelated work? Append one short entry to `execution-plans/todo/scratch.md` and continue the current task. Do not investigate it, and do not open an artifact for it.

## Reasoning & Pushback

- **Fact-based, not agreeable**: If a request relies on a flawed assumption or conflicts with project conventions/architecture, challenge it directly. Do not execute a bad plan just to be helpful.
- **Critical plan evaluation**: Plans, architecture docs, and prior agent artifacts are inputs, not gospel. Verify claims against actual code and data before executing. When you spot gaps, conflicts, or suboptimal choices — flag them before executing and propose an alternative. Do not over-flag cosmetic or subjective concerns — focus on issues that would cause bugs, data inconsistencies, or architectural drift.
- **No silent omissions**: If an edge case, requirement, or code block is being skipped, explicitly state what was omitted and why.

## Writing Style

Applies to everything you write — chat replies, docs, code comments, commit messages, and `execution-plans/` artifacts.

- **Plain English**: Write for non-native readers. One idea per sentence. Prefer the common word (`use` not `utilize`, `about` not `regarding`, `start` not `commence`). No idioms or metaphors. Keep technical and domain terms exact — never simplify `idempotent`, `nonce`, or `RSC` into an approximation.
- **Direct sentences**: Subject → verb → object, active voice. Avoid passive constructions and long clauses stacked before the subject.
- **Zero filler**: Skip formalities like pleasantries and apologies. Focus on actionable steps and code.
- **Copyable text as code blocks**: When asked to provide a piece of text for direct use (a template, message, example, form field value, very concise prompt, etc.) rather than to answer a question, wrap it in a fenced Markdown code block.
- **Files over chat for durable output**: If the output is meant to persist (docs, specs, plans, code, analysis), write it to a file and link to it — don't paste the full content in the chat reply. Keep chat replies to short explanations and summaries. This doesn't override "Copyable text as code blocks" above: that rule is for short-lived text the user will copy elsewhere, not content meant to live inside a file.

## Dev Scripts

Dev scripts live in `scripts/` with usage docs in each file's header comment.

## Commands

Quality gates. Run all of these from the repo root before completing any task, and fix every failure.

<!-- Example for a TypeScript/pnpm monorepo — replace with this project's actual stack and script names. -->

```sh
pnpm typecheck          # strict TS compilation
pnpm build              # full turbo build
pnpm lint:fix           # ESLint fix and report across all packages
pnpm test               # Vitest tests
pnpm format             # Prettier formatting that writes to files
```

For scoped work, narrow the run: `pnpm --filter @[org]/[package] typecheck build lint test`.

<!-- Example automation note — replace with what actually runs in this project, so the agent knows what it does not have to repeat. -->

> **Automation context:** Pre-commit hook (Husky + lint-staged) runs Prettier on staged files automatically. CI runs the full pipeline above on every push/PR to `main`/`develop`. Running these checks locally before pushing is recommended to avoid CI failures.

## Git

- **No git write ops without explicit per-operation approval**: `git commit`, `git push`, branch deletion, force-push, and amend each require their own explicit approval — even mid-workflow. Read-only git commands are fine.
- **Diff review** against the base branch: `git diff main...HEAD -- . ':!pnpm-lock.yaml'` <!-- Set the real base branch and exclude this project's lockfile and generated files. -->
- **Branch**: `type/issue-number-description` — e.g. `feat/42-wallet-siwe-binding`, `fix/87-auth-redirect`
- **Commit and PR title**: `type(scope): description` — e.g. `chore(agents): reorganize instruction files`
- **Types**: `feat`, `fix`, `chore`, `refactor`, `test`, `docs`

When the user asks for **New PR Creation Data**, give each field below under a bold label, each in its own Markdown code block, paste-ready for the GitHub form. Keep every field very concise.

- issue title
- issue description
- branch name
- commit message
- PR title
- PR description

...
