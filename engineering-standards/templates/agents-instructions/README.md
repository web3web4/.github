# agents-instructions/

Static instruction files for AI agents working on this project. These are referenced from [`AGENTS.md`](../AGENTS.md) and read on-demand — not every session.

> `agents-instructions/` = **static how-to** (read-only reference).
> `execution-plans/` = **dynamic work output** (kanban-tracked tasks).

These files are **project-specific by design**. Copy them from the org templates as a starting point, then rewrite them for this project's language, stack, and scripts. Content that is identical across every project does not belong here — it belongs in a shared skill.

## Index

| File                               | Purpose                                       | When to read                                                     |
| ---------------------------------- | --------------------------------------------- | ---------------------------------------------------------------- |
| `architecture-reference.md`        | Tech stack, patterns, architectural decisions | Tasks involving stack choices, new dependencies, or architecture |
| `implementation-checklist.md`      | Quality gates for implementation plans        | Include in artifact plans for features, fixes, refactors         |
| `post-implementation-checklist.md` | Closing checklist after work is done          | After implementation is complete and code is committed           |

## Shared skills (not in this folder)

Project-agnostic guidance lives in `.agents/skills/`, installed from [`web3web4/.github`](https://github.com/web3web4/.github) with `npx skills`. Never edit an installed skill — updates overwrite it.

| Skill                      | Purpose                                                |
| -------------------------- | ------------------------------------------------------ |
| `execution-plans-workflow` | Folder lifecycle, naming convention, artifact template |
| `prompt-authoring-guide`   | How to write and review handoff prompts                |

## Adding a New Instruction File

Whether you're a human or an agent, follow these steps:

1. **Confirm the need.** Does the content belong here (static, reusable instructions for agents)? If it's task-specific or ephemeral, it belongs in `execution-plans/` instead.

2. **Name the file descriptively.** Use lowercase kebab-case. The name should be self-explanatory without needing to open the file:
   - Checklists: `[scope]-checklist.md` (e.g., `bugfix-checklist.md`, `planning-review-checklist.md`)
   - References: `[topic]-reference.md` (e.g., `architecture-reference.md`)
   - Guides: `[topic]-guide.md` (e.g., `scratch-guide.md`)

3. **Add a clear title and scope line.** First line = `# Title`. Second line or paragraph = when/why an agent should read this file.

4. **Register the file.** Update both:
   - This `README.md` — add a row to the Index table above.
   - [`AGENTS.md`](../AGENTS.md) — add a reference where agents will need it (e.g., in Development Rules, Artifact File Structure, or a new section as appropriate).

5. **Keep it focused.** One file = one concern. If a file grows to cover two distinct topics, split it.
