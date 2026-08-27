# Engineering Standards

This directory contains company-wide engineering standards and patterns. These standards apply across all repositories and teams unless explicitly stated otherwise.

## Active Standards

- [Environment Variables](env-loading.md) - Standard for loading environment variables in monorepos using `dotenv-cli`.

## Templates

- [AGENTS.md Template](templates/AGENTS.md) - Boilerplate for giving AI coding agents instant context on a repo's architecture, rules, and workflows.
- [AGENTS-init.md](templates/AGENTS-init.md) - One-time setup guide: fill in AGENTS.md, scaffold `task-plans/`, and configure tooling. Delete after setup.
- [agents-instructions/](templates/agents-instructions/) - Companion reference files reads on demand by AGENTS.md, currently the architecture reference. These are **starting points to rewrite per project**, not drop-ins.
- [README.md hints](templates/README.md) - Non-exclusive, optional sections to add to a project's existing README.md for human readers. Not a drop-in replacement — copy only what fits.

## Skills

Project-agnostic agent guidance, shared across repos via the [`skills` CLI](https://github.com/vercel-labs/skills).

| Skill                                                                     | Purpose                                                                                                           |
| ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [`task-plans`](../skills/task-plans/SKILL.md)                            | The unified process for non-trivial tasks: plan, folder lifecycle, implementation quality bar, completion.       |
| [`prompt-authoring-guide`](../skills/prompt-authoring-guide/SKILL.md)     | How to write and review handoff prompts that instruct a fresh agent session to produce a plan.                    |

```bash
npx skills add web3web4/.github \
  --skill task-plans \
  --skill prompt-authoring-guide \
  --copy -a github-copilot -y
```

Installs to `.agents/skills/` (shared by GitHub Copilot, Codex, Cursor, Gemini CLI, Antigravity, Cline, Zed, Amp, OpenCode). Refresh with `npx skills update`. Commit `.agents/skills/` and `skills-lock.json`.

**Skills carry no project-specific content.** Updates overwrite them wholesale, so never edit an installed copy. A skill defines the process; the project defines the details. Anything that legitimately differs per project — quality-gate commands, language-specific checks, git conventions — goes in that repo's `AGENTS.md` under `Development Rules`, `Commands`, and `Git`.

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
