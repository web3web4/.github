# Adoption Prompts

Copy-paste prompts for handing standards adoption to an LLM agent. Three situations, three prompts. Pick one, attach the listed files, paste into a **fresh** agent session.

These prompts deliberately do **not** restate the setup procedure. [`templates/AGENTS-init.md`](templates/AGENTS-init.md) is the procedure; the prompts only tell the agent to follow it and add the constraints an agent will otherwise get wrong.

## How to use

1. Pick the matching prompt below.
2. Attach the files listed above it. Attach — do not paste contents.
3. Paste the prompt into a new session in the **target** repo.
4. Approve the plan the agent produces before it writes anything beyond the scaffold.

If [`web3web4/.github`](https://github.com/web3web4/.github) is open as a sibling folder in the same workspace, say so in the prompt and the agent can read the templates directly instead of you attaching them. If you don't have it cloned yet:

```bash
git clone https://github.com/web3web4/.github.git ~/repos/web3web4/.github
```

Then add it to the current VS Code workspace. `.github` is a dotfolder, so the "Add Folder to Workspace" picker hides it by default — don't browse for it, type the path directly:

1. Command Palette → `Workspaces: Add Folder to Workspace...`
2. In the file picker, type or paste the full path (e.g. `~/repos/web3web4/.github`) into the path/filename field and confirm, instead of navigating the folder tree.
3. Command Palette → `Workspaces: Save Workspace As...` if you want the added folder to persist across restarts.

From a terminal, with the target repo's window already open, `code --add ~/repos/web3web4/.github` does the same in one step.

---

## 1. New project

Use when the repo has no `AGENTS.md`, or only the unfilled template.

**Attach:** `templates/AGENTS-init.md`, `templates/AGENTS-template.md`, `templates/agents-instructions/architecture-reference.md`, `templates/agents-instructions/README.md`, `templates/README.md`, `env-loading.md`

```text
Set this repository up against the web3web4 engineering standards.

The attached templates/AGENTS-init.md is the authoritative procedure. Follow it in
order, from step 1 to the end, and finish by working its Checklist. Do not improvise
around it and do not reorder it — steps 1 and 2 exist first for a reason stated in the
file.

After step 2, read .agents/skills/task-plans/SKILL.md in full, write a plan artifact
covering the remaining steps, and wait for my approval before continuing.

Constraints the procedure assumes but does not spell out:
- Decide the Project Type before writing anything. If this is not a software repo,
  apply the "Non-Development Projects" section instead of the dev-only steps.
- Do not invent architecture. If you cannot determine the stack, auth model, or data
  layer by reading the code, ask me. A plausible guess in AGENTS.md is worse than a
  gap, because every future session will treat it as fact.
- Every command you write under `## Commands` must exist and must pass. Run each one
  once. Do not copy the template's example commands.
- Ask for explicit approval before any git write operation.

Report the Checklist at the end, item by item, as pass or fail with evidence.
```

---

## 2. Existing project

Use when the repo already has working conventions. The risk here is an agent flattening a deliberate choice into the template default, so the prompt forces an audit before any write.

**Attach:** the same files as above, plus the repo's current `AGENTS.md` and `CLAUDE.md` if they exist.

```text
Retrofit the web3web4 engineering standards onto this existing repository.

The attached templates/AGENTS-init.md describes the target end state. It is written for
greenfield repos, so adapt it — do not run it top-down over working conventions.

Phase 1, read-only. Produce a gap table: for each AGENTS-init step and Checklist item,
record present / partial / missing, and what this repo does today instead. Include the
real build, test, lint, and typecheck commands, verified by reading package.json and
running each one. Check `.github/workflows/` specifically for a possible standalone
`skills-drift.yml` or `agents-md.yml` predating combining them in `standards.yml`. Write no files in
this phase.

Phase 2. Do AGENTS-init steps 1 and 2 only (shared skills, task-plans scaffold) if they
are missing. Read .agents/skills/task-plans/SKILL.md in full. Put the gap table and the
migration steps into a plan artifact. Stop and wait for my approval.

Phase 3, after approval only. Implement.

Constraints:
- Existing conventions win over the template when they are deliberate and working.
  Flag each divergence with a recommendation. Do not silently normalise.
- Keep the AGENTS.md content that is still true. Reorganise into the template's section
  order. Do not delete a project rule you have not verified is stale.
- A command that fails today gets reported, not deleted, and not worked around by
  changing unrelated code.
- A pre-existing standalone `skills-drift.yml` or `agents-md.yml` is not "already
  compliant" — the current standard is the combined `standards.yml`. Flag it for
  consolidation and deletion of the old files, per the AGENTS-init Checklist.
- Ask for explicit approval before any git write operation.
```

---

## 3. Refresh after upstream changes

Use periodically, or when CI reports drift. Attach nothing — the agent fetches current upstream itself.

```text
Re-align this repo with the current web3web4 engineering standards.

Run `npx skills update`. Then diff this repo's AGENTS.md and agents-instructions/
against the current templates in https://github.com/web3web4/.github, and re-run the
AGENTS-init Checklist against the repo as it stands now.

Report only substantive drift: a section the template now requires that we lack, a rule
whose meaning changed, a command under `## Commands` that no longer runs, or a standalone
`skills-drift.yml`/`agents-md.yml` that should now be one `standards.yml` calling both
reusable jobs. Ignore wording differences and intentional project-specific content.

Propose the edits and wait for my approval before applying them.
```

---

## Rules that apply to all three

- **Never edit `.agents/skills/`.** The prompts say it because agents do it anyway. `skills-drift.yml` catches it in CI.
- **One session per repo.** These prompts assume the agent's working directory is the target repo.
- **Where prompts live.** These three live here because they cannot work anywhere else — they drive the templates in the next folder. A one-off task handoff prompt for a single repo belongs in that repo's `task-plans/others/prompts/`; see the `prompt-authoring-guide` skill. A prompt that shapes an answer and depends on nothing goes in the [prompt library](../prompt-library/) at the repo root.

For a slash command available in any workspace, including repos that have no standards files yet, save prompt 1 as `adopt-standards.prompt.md` in your editor's user prompts folder:

```text
---
description: Set up this repo against the web3web4 engineering standards
---

[paste the "New project" prompt body here]
```
