# [Project Name] - Context for AI Agents

> This file is read by AI coding agents every session. Keep it lean — no examples, no placeholders.
> **Setup guide:** See [AGENTS-init.md](AGENTS-init.md) for instructions on filling this out.

## Project Overview

**[Project Name]** is ...

- **[app/package]** — ...

<!-- Optional: for repos with many files, non-obvious naming, or a structure that matters for the workflow (e.g. doc build order), add a directory tree here. Skip it for small or self-explanatory repos. -->

## Project Status

...

## Project Type

`[Development — TypeScript]` <!-- Options: Development — TypeScript | Development — [language] | Documentation | Research | ... -->

## Architecture & Stack

See [`agents-instructions/architecture-reference.md`](agents-instructions/architecture-reference.md) for tech stack, engineering patterns, and architectural decisions. Only read that file when the task involves stack choices, new dependencies, or architecture.

## Development Rules

<!-- This project's variations on the task-plans skill's implementation quality bar. Keep them specific: language, validation library, test runner, and rules unique to this codebase. -->

- **Code Quality**: Strict TypeScript. No `any` types. Use `zod` for all runtime boundary validations (APIs, forms).
- **Shared types**: A type that crosses the client–server boundary is declared once in a shared package. Never redeclare it per app.
- **UI and accessibility**: Mobile-first and touch-accessible. No hover-only control and no essential data hidden in a tooltip. Interactive elements have large touch targets and a visible focus state.
- **Comment hygiene**: No comments unless the WHY is non-obvious. No "removed X" markers, no "used by Y" hints, no restating what the code does.
- **No backward compatibility** _(state this only if true)_: This project has no external consumers or published interface yet. Never add shims, compat layers, aliasing, or transitional adapters — break interfaces freely and always implement the cleanest design over preserving the existing one.
- **No legacy context in docs**: Documentation should reflect only the current state of the system. Do not reference deprecated features, past architectures, or how things "used to work." Write for someone brand new to the codebase.
- **Documentation Location**:
  - Persistent architecture/API docs → `docs/`
  - Static agent guidance → `agents-instructions/`
  - Ephemeral agent planning/logs → `task-plans/` (do not gitignore)
  - All other `.md` files should only be created inside `task-plans/` or `docs/` unless the task explicitly requires otherwise.
- **Testing**: ...
- **Spec** _(if this project has a formal spec)_: Check `[path]` for the source of truth on requirements.
- **Specs vs. code**: If code deviates from a spec but the code is better, propose updating the spec — don't "fix" working code.
- **Test failures**: Investigate production code first before assuming the test is wrong.
- **Template/asset parity** _(if this repo ships assets or templates consumed by other repos)_: Name the consuming repos and their manifests/config here, and verify any change to a shared asset against each of them before merging.

### Non-negotiables

<!-- Reserve this subsection for the 2-5 rules where a violation causes a production bug, an outage, or silent data loss — not general style preferences. Everything else stays in Development Rules above. -->

## AI Agent Workflow (The "task-plans" system)

Plans, task breakdowns, investigation notes, and progress logs are not chat output. They are Markdown artifacts under `task-plans/[status]/[category]/`. Status is `todo`/`doing`/`done`. This project's categories are `features`, `fixes`, and `analysis`. <!-- Documentation projects: use drafting/revisions/analysis -->

- **The skill owns the process — read it before you act on it**: Before planning a non-trivial task, creating, moving, or completing an artifact, or resuming earlier work, read [`task-plans`](.agents/skills/task-plans/SKILL.md) in full. Its Procedure 0 decides whether the task needs an artifact. Do not work from memory and do not skim: if you have not opened the file this session, you do not know the rules. It is installed by `npx skills` and shared across repos — never edit it in place; project-specific rules belong in this file.
- **The deliverable is the artifact, not the chat reply**: When the user asks for a plan, review, audit, or analysis, the deliverable is the artifact file. Reply with a link to it and a short summary.
- **Never scan `task-plans/` for work** — not `todo/`, not `doing/`. The user attaches or names the artifact when one is relevant. If the task continues earlier work and no artifact was attached, named, or already identified in this conversation, ask the user to point to it — do not search for it, and do not create a duplicate.
- **Park stray findings in `scratch.md`**: Notice a bug, idea, reusable pattern, or architectural insight while doing unrelated work? Append one short entry to `task-plans/todo/scratch.md` and continue the current task. Do not investigate it, and do not open an artifact for it.
- **Complete verified work**: Move an artifact to `done/` as soon as its plan, validation, and outcome are complete. Do not wait for a commit; Git approval is a separate rule.

## Reasoning & Pushback

- **Fact-based, not agreeable**: If a request relies on a flawed assumption or conflicts with project conventions/architecture, challenge it directly. Do not execute a bad plan just to be helpful.
- **Critical plan evaluation**: Plans, architecture docs, and prior agent artifacts are inputs, not gospel. Verify claims against actual code and data before executing. When you spot gaps, conflicts, or suboptimal choices — flag them before executing and propose an alternative. Do not over-flag cosmetic or subjective concerns — focus on issues that would cause bugs, data inconsistencies, or architectural drift.
- **No silent omissions**: If an edge case, requirement, or code block is being skipped, explicitly state what was omitted and why.

## Writing Style

Applies to everything you write — chat replies, docs, code comments, commit messages, and `task-plans/` artifacts.

- **Plain English**: Write for non-native readers. One idea per sentence. Prefer the common word (`use` not `utilize`, `about` not `regarding`, `start` not `commence`). No idioms or metaphors. Keep technical and domain terms exact — never simplify `idempotent`, `nonce`, or `RSC` into an approximation.
- **Direct sentences**: Subject → verb → object, active voice. Avoid passive constructions and long clauses stacked before the subject.
- **Zero filler**: Skip formalities like pleasantries and apologies. Focus on actionable steps and code.
- **Copyable text as code blocks**: When asked to provide a piece of text for direct use (a template, message, example, form field value, very concise prompt, etc.) rather than to answer a question, wrap it in a fenced Markdown code block.
- **Files over chat for durable output**: Anything meant to persist — docs, specs, plans, code, analysis — lives in a file you link to, never pasted whole into a reply. Before you send a reply, look at what you drafted: a Markdown heading, a table, a list of findings, or more than about 10 lines means write it to a file instead. Reply with the link, at most five lines of summary, and any question you need answered. Short answers, status lines, confirmations, and clarifying questions stay in chat. This does not override "Copyable text as code blocks" above, which covers short text the user will paste elsewhere.
- **Analysis is a deliverable**: A review, audit, verdict, critique, comparison, gap analysis, root-cause finding, or post-implementation summary is durable output. Write it into the artifact it assesses, or a new one under `task-plans/[status]/analysis/`, then summarise it in chat. A question asked in chat still gets a written answer when the answer is long-form. No finding may exist only in the conversation.

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
actionlint              # GitHub Actions workflow validation
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
