# web3web4/.github

Shared assets for the organization. Nothing here runs. Everything here is consumed by other repositories or pasted into a chat.

| Folder | What it is |
| --- | --- |
| [engineering-standards/](engineering-standards/) | The standards, the `AGENTS.md` and setup templates other repos copy, and the prompts that hand adoption to an agent. |
| [skills/](skills/) | Agent skills published through the [`skills` CLI](https://github.com/vercel-labs/skills). Consuming repos vendor them into `.agents/skills/`. |
| [prompt-library/](prompt-library/) | Prompts for shaping an LLM's answer, to take and use as they are or edit. Coupled to no repository, stack, or organization. |
| [.github/workflows/](.github/workflows/) | Reusable workflows consuming repos call: `skills-drift.yml` and `agents-md-check.yml`. |
| [profile/](profile/) | The organization profile page GitHub renders at [github.com/web3web4](https://github.com/web3web4). |

Adopting the standards in a repository: [engineering-standards/adoption-prompts.md](engineering-standards/adoption-prompts.md).

Working in this repository with an agent: [AGENTS.md](AGENTS.md).
