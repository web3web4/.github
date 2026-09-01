---
name: prompt-authoring-guide
description: How to write and review handoff prompts that instruct a fresh agent session to produce a plan artifact. Use when asked to create, draft, refine, or review a prompt for a task - especially prompts saved under task-plans/others/prompts/.
---

# Prompt Authoring Guide

When the user asks to **create or review a prompt**, follow this guide. Unless stated otherwise, the prompt being created is a **prompt-to-create-a-plan** — a file that will be fed to a new agent session whose first job is to produce a detailed plan artifact before any implementation begins.

This skill is shared across repositories and carries no project-specific content. It is installed by `npx skills` and is overwritten wholesale on update — **do not edit the installed copy.** Project-specific rules belong in the project's `AGENTS.md`.

## How it works

1. **User asks for a prompt** — e.g., "write a prompt for migrating auth to SIWE" or "let's create a prompt for the smart contract review."
2. **Create the prompt file immediately** — create the `.md` file in `task-plans/others/prompts/` as soon as the conversation about a prompt starts. The file will be refined iteratively as scope is discussed.
3. **Refine** — update the file as the user and agent discuss scope, boundaries, and constraints.
4. **Review** — user reviews the prompt (optionally with agent-assisted review).
5. **Handoff** — user pastes the prompt into a fresh agent session. That agent reads it, creates a plan artifact in `task-plans/`, and waits for approval before writing any code.

## Writing the prompt

The prompt scopes the _what_ and _why_. The plan (created by the receiving agent) handles the _how_.

**Keep it concise:**

- Draw boundaries: what's in, what's out, what must not be touched.
- Reference files by path — don't inline contents.
- State constraints, not implementation steps.
- The receiving agent will explore the codebase and figure out the approach — that's what the plan is for.

**Every prompt must end by instructing the receiving agent to:**

1. Create a plan artifact in `task-plans/` before writing any code.
2. Wait for user approval before starting implementation.

**Length check:** If a prompt exceeds ~80–100 lines, it's likely doing the planning job. Flag it and discuss with the user before trimming.

## File conventions

- **Location:** `task-plans/others/prompts/`
- **Naming:** `YYYY-MM-DD-PROMPT_[TITLE].md` (e.g., `2026-04-05-PROMPT_WALLET_BINDING_AUDIT.md`)
- **One prompt per file.** Multi-phase work gets one prompt per phase, not one mega-prompt.
- **Frontmatter:** Include the standard frontmatter when creating the file. Add an `edits` entry when modifying an existing prompt.

  ```yaml
  ---
  created: YYYY-MM-DD HH:mm
  created_by: [LLM name and version]
  edits:
    - date: YYYY-MM-DD HH:mm
      author: [LLM name and version]
  ---
  ```

## Review checklist

When reviewing a prompt (self-review or agent-assisted), check:

- [ ] **Goal is falsifiable.** Could someone determine whether the goal was achieved?
- [ ] **Scope is bounded.** In-scope and out-of-scope are both stated.
- [ ] **No implementation leakage.** If it reads like a technical design doc, it's too detailed — that belongs in the plan.
- [ ] **Referenced paths exist** and are up to date.
- [ ] **Constraints are explicit.** Anything painful to fix if done wrong is called out.
- [ ] **Ends with plan request.** The prompt instructs the receiving agent to create a plan and wait for approval.

### Agent-assisted review

When the user asks to review a prompt:

1. Read the prompt file.
2. Check for ambiguities, stale file references, scope conflicts with existing code, implicit assumptions, and over-specification.
3. Propose concrete edits — don't just flag concerns.

## Related

The plan artifact the receiving agent produces is governed by the `task-plans` skill — folder lifecycle, naming convention, and required file structure.
