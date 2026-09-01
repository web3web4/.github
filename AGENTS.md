# web3web4/.github - Context for AI Agents

## Project Overview

**web3web4/.github** is the organization's shared standards repository. Nothing here runs; everything here is consumed by other repositories.

- **engineering-standards/** - The standards themselves, the `AGENTS.md` and setup templates other repos copy, and the adoption prompts that hand setup to an agent.
- **skills/** - Agent skills published through the [`skills` CLI](https://github.com/vercel-labs/skills): `task-plans` and `prompt-authoring-guide`. Consuming repos vendor them into `.agents/skills/`.
- **prompt-library/** - Prompts for shaping an LLM's answer, to take and use as they are or edit. Coupled to nothing in this repo, which is why they sit outside `engineering-standards/`.
- **.github/workflows/** - Reusable workflows called by consuming repos: `skills-drift.yml` and `agents-md-check.yml`.
- **profile/README.md** - The organization profile page GitHub renders at `github.com/web3web4`.
- **task-plans/** - This repository's own work artifacts.

## Project Status

Active. The standards are in use across several organization repositories, which pin the reusable workflows at `@main`.

## Project Type

`Documentation and shared engineering assets`

## Development Rules

Every file under `engineering-standards/`, `skills/`, and `.github/workflows/` is a shipped asset. A change here changes other repositories the next time they run `npx skills update`, copy a template, or trigger CI. Treat edits accordingly.

- **Skills carry no project-specific content**: A skill defines a process; the consuming project defines the details. Never name a repository, stack, command, or file path that only exists in one project. Anything that legitimately varies per project belongs in that project's `AGENTS.md` under `Development Rules`, `Commands`, or `Git`.
- **Skill edits cause drift downstream**: `skills-drift.yml` compares a consumer's vendored copy byte-for-byte against this repo. Cosmetic edits to `skills/` — reflowing a line, fixing whitespace — fail CI in every consumer until they update. Change a skill for substance, not for tidiness.
- **The template and its checker move together**: `agents-md-check.yml` greps `AGENTS.md` for literal placeholder strings and required section headings shipped by `templates/AGENTS-template.md`. Renaming a section or rewording a placeholder in the template without updating the workflow lets unfilled repos pass, and renaming one in the workflow without the template fails healthy repos. Change both, in the same commit.
- **The adoption path is three files, not one**: `templates/AGENTS-template.md`, `templates/AGENTS-init.md`, and `adoption-prompts.md` describe the same procedure to three audiences. A change to any one of them needs the other two checked.
- **Reusable workflows are a public interface**: Consumers pin `@main`. Adding an input with a default is safe; renaming or removing one breaks their CI on the next push. Keep defaults backward compatible and say so in the commit message when they change.
- **Template naming**: Template files keep the filename they will have at their destination, so the copy step is obvious. `AGENTS.md` is the single exception and ships as `AGENTS-template.md`, because agent tools load `AGENTS.md` by name from the nearest directory upward — a file with that name under `templates/` would silently govern any agent editing this repo.
- **Prompts have three homes, and the difference is coupling**: `engineering-standards/adoption-prompts.md` drives an agent through this repo's templates and cannot work without them. `prompt-library/` is coupled to nothing and sits at the root for that reason. A prompt that hands one task to an agent lives in that repo's `task-plans/others/prompts/` and follows the `prompt-authoring-guide` skill. Do not mix them, and do not move one closer to another because the names rhyme.
- **Markdown style**: Prose is not hard-wrapped; one paragraph is one line. Text inside a fenced block that a person will copy into a chat wraps at roughly 80 characters. Dash bullets, asterisk emphasis. No Prettier — it rewrites emphasis markers and re-pads tables.
- **No legacy context**: Documentation describes the current standard only. Do not explain what a template used to contain or how a workflow used to behave. Write for someone who arrived today.
- **Accuracy over completeness**: Do not document a command, input, or path you have not verified exists. In a standards repo, an invented rule propagates.

### Non-negotiables

- Never edit a file under `skills/` to fix formatting alone. It breaks `skills-drift` for every consumer.
- Never change a placeholder string or required section in `templates/AGENTS-template.md` without making the matching change in `.github/workflows/agents-md-check.yml`.
- Never remove or rename a `workflow_call` input in a reusable workflow without a stated migration.
- Never perform a git write operation without explicit, per-operation approval.

## AI Agent Workflow (The "task-plans" system)

Plans, task breakdowns, investigation notes, and progress logs are Markdown artifacts under `task-plans/[status]/[category]/`. Status is `todo`/`doing`/`done`. This project's categories are `features`, `fixes`, and `analysis`.

- **The skill owns the process — read it before you act on it**: Before planning a non-trivial task, creating, moving, or completing an artifact, or resuming earlier work, read [`skills/task-plans/SKILL.md`](skills/task-plans/SKILL.md) in full. This repository is the skill's source, so it is read from `skills/` here rather than `.agents/skills/`. Its Procedure 0 decides whether a task needs an artifact.
- **The deliverable is the artifact, not the chat reply**: When the user asks for a plan, review, audit, or analysis, the deliverable is the artifact file. Reply with a link and a short summary.
- **Never scan `task-plans/` for work** — not `todo/`, not `doing/`. The user attaches or names the relevant artifact. If a task continues earlier work and no artifact was named in the conversation, ask which one; do not search, and do not open a duplicate.
- **Park stray findings in `scratch.md`**: Append one short entry to [`task-plans/todo/scratch.md`](task-plans/todo/scratch.md) and continue the current task.

The implementation quality bar in the `task-plans` skill is written for code. This repository ships documents and CI configuration, so replace it in each artifact's plan with the rules under `Development Rules` above, plus: every path, command, and link is verified to exist, and every downstream consumer of a changed asset is named.

## Reasoning & Pushback

- **Fact-based, not agreeable**: If a request conflicts with a standard published here, say so and explain the downstream effect before acting.
- **Critical plan evaluation**: Prior artifacts and templates are inputs, not gospel. Verify a claim against the files before building on it. Flag gaps and conflicts before implementing, and propose an alternative.
- **No silent omissions**: If a referenced file, consumer, or check is being skipped, say what was skipped and why.

## Writing Style

Applies to everything: chat replies, standards, templates, skills, and `task-plans/` artifacts.

- **Plain English**: Write for non-native readers. One idea per sentence. Prefer the common word. No idioms. Keep technical terms exact.
- **Direct sentences**: Subject, verb, object. Active voice.
- **Zero filler**: No pleasantries, no apologies, no restating the request.
- **Say why a rule exists**: A standard nobody understands gets worked around. Every non-obvious rule in this repository names the failure it prevents. Match that.
- **Copyable text as code blocks**: A prompt, template, message, or config value the user will paste elsewhere goes in a fenced block.
- **Files over chat for durable output**: Anything meant to persist lives in a file you link to. If a draft reply contains a heading, a table, a list of findings, or more than about ten lines, write it to a file instead and reply with the link plus a short summary. Short answers, status lines, and clarifying questions stay in chat.
- **Analysis is a deliverable**: A review, audit, critique, comparison, or gap analysis is durable output. Write it into the artifact it assesses, or a new one under `task-plans/[status]/analysis/`, then summarize it in chat. No finding may exist only in the conversation.

## Commands

This repository has no package manifest, so there is no build, test, or typecheck. Markdown lint and GitHub Actions lint are the automated gates and run in CI.

```sh
npx markdownlint-cli2 "<the files you changed>"
actionlint
```

Lint the Markdown files you changed, not the whole repository. `actionlint` checks every workflow in `.github/workflows/`. [`.markdownlint.jsonc`](.markdownlint.jsonc) disables the rules this repository breaks by design, with the reason recorded in the file. `profile/README.md` suppresses its intentional inline HTML and emphasis-as-heading rules locally. Never reformat `skills/` to satisfy a lint rule outside a substantive change to that skill.

Before finishing a change to a shipped asset, verify by hand:

- Every relative link in a file you touched resolves.
- No file still references a path you renamed: `grep -rn "<old path>" --include='*.md' --include='*.yml' .`
- A change to `templates/AGENTS-template.md` is reflected in `.github/workflows/agents-md-check.yml`, `templates/AGENTS-init.md`, and `engineering-standards/adoption-prompts.md`.

## Git

- **No git write ops without explicit per-operation approval**: `git commit`, `git push`, `git add`, branch changes, resets, and amends each require their own approval, even mid-workflow. Read-only git commands are fine.
- **Base branch**: `main`.
- **Diff review**: `git diff main...HEAD -- .`
- **Branch**: `type/short-description` — for example `docs/agents-md-template-rename`.
- **Commit and PR title**: `type(scope): description` — for example `docs(templates): rename AGENTS.md to AGENTS-template.md`.
- **Types**: `feat`, `fix`, `chore`, `refactor`, `docs`.

A commit that changes `skills/` or a reusable workflow states the downstream effect in its body, because consumers pick it up automatically.
