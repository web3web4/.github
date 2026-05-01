# Engineering Standards

This directory contains company-wide engineering standards and patterns. These standards apply across all repositories and teams unless explicitly stated otherwise.

## Active Standards

- [Environment Variables](env-loading.md) - Standard for loading environment variables in monorepos using `dotenv-cli`.

## Templates

- [AGENTS.md Template](templates/AGENTS.md) - Boilerplate for giving AI coding agents instant context on a repo's architecture, rules, and workflows.
- [AGENTS-init.md](templates/AGENTS-init.md) - One-time setup guide: fill in AGENTS.md, scaffold `execution-plans/`, and configure tooling. Delete after setup.
- [agents-instructions/](templates/agents-instructions/) - Companion instruction files referenced by AGENTS.md: architecture reference, implementation checklist, post-implementation checklist, and prompt authoring guide.

## Principles

1. **Follow Industry Standards**: Always prefer common, well-established community conventions over custom in-house solutions. Don't reinvent the wheel.
2. **Be Pragmatic**: Adopt tools that solve actual problems with the lowest maintenance burden.
3. **Standardize Where Valuable**: If a problem is solved three times in three ways, it belongs here.
4. **Keep it Concise**: Standards should be quick to read, focused on implementation over theory, and easy to copy-paste.
