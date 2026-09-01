---
created: 2026-09-01 09:17
created_by: GitHub Copilot
edits:
  - date: 2026-09-01 09:33
    author: GitHub Copilot (Claude Opus 5)
---

# Document Prompt Examples

## Context

The engineering standards need a small set of reusable prompts for shaping an
LLM's response, kept clearly separate from the task-handoff prompts governed by
the `prompt-authoring-guide` skill, with navigation from the standards README.

The first pass produced files that listed output requirements without saying
what each prompt is for or which failure mode it guards against, and one file
whose name (`prompt-light-review.md`) did not match its content. The whole
folder was rewritten.

## Plan

- [x] Review the existing prompt example against the prompt-authoring guidance.
- [x] Keep the examples in `engineering-standards/prompts-examples/` and explain
  the distinction from task-handoff prompts.
- [x] Add varied reusable response-prompt examples and a folder README.
- [x] Link the examples from the engineering standards README.
- [x] Rewrite every file in the folder to the house style used by
  `adoption-prompts.md`: state the use case, name the failure mode the prompt
  blocks, then give the prompt.
- [x] Rename `prompt-light-review.md` to `prompt-english-review.md` so the
  filename matches the content.
- [x] Give the folder README a "Writing your own" section so the collection
  teaches the pattern instead of only listing instances.
- [x] Run Markdown lint and review the resulting diff.

## Rationale

`adoption-prompts.md` is the closest sibling and the quality reference: it says
why each prompt exists and what an agent gets wrong without it. The first pass
of the examples folder carried none of that context, which made the prompts hard
to choose between and impossible to adapt.

Each prompt now closes the four gaps that made the first pass weak: an explicit
first-line and ordering contract, a rule aimed at that request type's default
failure, an escape hatch for insufficient or ambiguous input, and a defined
"nothing to report" result.

Prose is left unwrapped to match `adoption-prompts.md` and the standards README.
Text inside the fenced prompt blocks wraps at roughly 80 characters, since that
text gets pasted into a chat.

---

## Outcome

Rewrote all six files in `engineering-standards/prompts-examples/`. The set is
English Review, Direct Answer, Audience Summary, Options and Recommendation, and
Actionable Review, plus a README that indexes them, explains how to use them,
and documents the shared pattern for writing new ones. The standards README
entry was rewritten and unwrapped to match the surrounding style.

Markdown lint reports only MD013 line-length findings, which match the existing
unwrapped style throughout `engineering-standards/`; the repo has no
markdownlint config and no lint CI job. This artifact stays in `doing/` until
the work is committed with user approval.

### Notes

- `prompt-light-review.md` was deleted and replaced by
  `prompt-english-review.md`. Both appear in `git status` until the change is
  staged.
- The folder name `prompts-examples` reads awkwardly next to `adoption-prompts`.
  Renaming it to `response-prompts` would also let the redundant `prompt-`
  filename prefix go. Not done here, since it touches the standards README and
  is worth deciding separately.
