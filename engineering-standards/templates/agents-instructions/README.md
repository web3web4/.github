# agents-instructions/

Static instruction files for AI agents working on this project. These are referenced from [`AGENTS.md`](../AGENTS.md) and read on-demand — not every session.

> `agents-instructions/` = **static how-to** (read-only reference).
> `task-plans/` = **dynamic work output** (kanban-tracked tasks).

These files are **project-specific by design**. Copy them from the org templates as a starting point, then rewrite them for this project's language, stack, and scripts. Content that is identical across every project does not belong here — it belongs in a shared skill.

## Index

| File                        | Purpose                                       | When to read                                                     |
| --------------------------- | --------------------------------------------- | ---------------------------------------------------------------- |
| `architecture-reference.md` | Tech stack, patterns, architectural decisions | Tasks involving stack choices, new dependencies, or architecture |

## Shared skills (not in this folder)

Project-agnostic guidance lives in `.agents/skills/`, installed from [`web3web4/.github`](https://github.com/web3web4/.github) with `npx skills`. Never edit an installed skill — updates overwrite it.

| Skill                      | Purpose                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------ |
| `task-plans` | The unified task process: plan, folder lifecycle, implementation bar, completion     |
| `prompt-authoring-guide`   | How to write and review handoff prompts                                              |

The implementation and completion checklists live in `task-plans`, not here. This project's variations on them belong in [`AGENTS.md`](../AGENTS.md) under `Development Rules`, `Commands`, and `Git`.

## Adding a New Instruction File

Whether you're a human or an agent, follow these steps:

1. **Confirm the need.** Does the content belong here (static, reusable reference an agent reads on demand)? If it's task-specific or ephemeral, it belongs in `task-plans/` instead. If it's a rule the agent must follow every session, it belongs in [`AGENTS.md`](../AGENTS.md). If it's an implementation or completion checklist, it belongs in the `task-plans` skill — not here.

2. **Name the file descriptively.** Use lowercase kebab-case. The name should be self-explanatory without needing to open the file:
   - References: `[topic]-reference.md` (e.g., `architecture-reference.md`)
   - Guides: `[topic]-guide.md` (e.g., `scratch-guide.md`)

3. **Add a clear title and scope line.** First line = `# Title`. Second line or paragraph = when/why an agent should read this file.

4. **Register the file.** Update both:
   - This `README.md` — add a row to the Index table above.
   - [`AGENTS.md`](../AGENTS.md) — add a reference where agents will need it, saying when to read the file.

5. **Keep it focused.** One file = one concern. If a file grows to cover two distinct topics, split it.
