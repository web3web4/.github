# Engineering Standards

This directory contains company-wide engineering standards and patterns. These standards apply across all repositories and teams unless explicitly stated otherwise.

## Active Standards

- [Environment Variables](env-loading.md) - Standard for loading environment variables in monorepos using `dotenv-cli`.

## Templates

- [AGENTS.md Template](templates/AGENTS.md) - Boilerplate for giving AI coding agents instant context on a repo's architecture, rules, and workflows.
- [AGENTS-init.md](templates/AGENTS-init.md) - One-time setup guide: fill in AGENTS.md, scaffold `execution-plans/`, and configure tooling. Delete after setup.
- [agents-instructions/](templates/agents-instructions/) - Companion instruction files referenced by AGENTS.md: architecture reference, implementation checklist, post-implementation checklist. These are **starting points to rewrite per project**, not drop-ins.

## Skills

Project-agnostic agent guidance, shared across repos via the [`skills` CLI](https://github.com/vercel-labs/skills).

| Skill                                                                     | Purpose                                                                                                           |
| ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [`execution-plans-workflow`](../skills/execution-plans-workflow/SKILL.md) | Mechanics of the `execution-plans/` task-artifact system: folder lifecycle, naming convention, artifact template. |
| [`prompt-authoring-guide`](../skills/prompt-authoring-guide/SKILL.md)     | How to write and review handoff prompts that instruct a fresh agent session to produce a plan.                    |

```bash
npx skills add web3web4/.github \
  --skill execution-plans-workflow \
  --skill prompt-authoring-guide \
  --copy -a github-copilot -y
```

Installs to `.agents/skills/` (shared by GitHub Copilot, Codex, Cursor, Gemini CLI, Antigravity, Cline, Zed, Amp, OpenCode). Refresh with `npx skills update`. Commit `.agents/skills/` and `skills-lock.json`.

**Skills carry no project-specific content.** Updates overwrite them wholesale, so never edit an installed copy — per-project rules belong in that repo's `AGENTS.md`. Anything that legitimately differs per project (quality-gate commands, language-specific checks) stays in `agents-instructions/` instead.

Install from the GitHub source, never a local path: `skills-lock.json` records the source verbatim, and a local path is machine-specific.

## Reusable Workflows

- [`skills-drift.yml`](../.github/workflows/skills-drift.yml) - Fails CI when a vendored skill differs from upstream, or when `skills-lock.json` is missing or records a local install source. Call it from a consuming repo:

  ```yaml
  jobs:
    skills-drift:
      uses: web3web4/.github/.github/workflows/skills-drift.yml@main
  ```

## Principles

1. **Follow Industry Standards**: Always prefer common, well-established community conventions over custom in-house solutions. Don't reinvent the wheel.
2. **Be Pragmatic**: Adopt tools that solve actual problems with the lowest maintenance burden.
3. **Standardize Where Valuable**: If a problem is solved three times in three ways, it belongs here.
4. **Keep it Concise**: Standards should be quick to read, focused on implementation over theory, and easy to copy-paste.
