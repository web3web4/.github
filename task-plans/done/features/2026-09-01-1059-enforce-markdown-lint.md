---
created: 2026-09-01 10:59
created_by: GitHub Copilot
---

## Context

Markdown lint is documented as the repository quality gate but does not run in CI. A full lint run is blocked by deliberate markup in the organization profile and one missing fence spacer in the prompt-authoring skill.

## Plan

- [x] Correct the substantive Markdown lint violation in the prompt-authoring skill.
- [x] Scope lint suppressions to the intentional profile markup rather than disabling rules repository-wide.
- [x] Add a Markdown lint job to the standards workflow.
- [x] Update repository guidance to describe the enforced lint baseline.
- [x] Verify all touched paths and run the full Markdown lint command.
- [x] Review the diff against `main` and record the outcome.
- [x] Add `actionlint` to the local and CI quality gates.
- [x] Propagate workflow linting and verified-artifact completion through the engineering standards.
- [x] Verify the full Markdown and GitHub Actions lint baselines, then move the artifact to `done`.

## Rationale

The profile uses its formatting intentionally, so inline file-level controls preserve lint coverage everywhere else. The missing blank around the skill's fenced YAML example has no intended semantic effect and is corrected as part of the substantive change that enables the lint gate.

---

## Outcome

Added repository-wide Markdown and GitHub Actions lint jobs to the standards workflow. Resolved the prompt-authoring skill's fence-spacing finding and scoped the profile's intentional `MD033` and `MD036` exceptions to that file. Updated the current repository, shared task-plan skill, templates, setup guide, and adoption prompt so verified artifacts move to `done` without waiting for a commit. Updated the documented quality gates and cleared the scratch list.

### Notes

`npx markdownlint-cli2 "**/*.md"` passed with 0 issues across 25 files. `actionlint` passed for every workflow after installing actionlint 1.7.12 locally. VS Code still cannot resolve the existing self-reference to `agents-md-check.yml`; the new lint job does not use that reference.

Known downstream consumers identified in organization code search are `awnun` and `saudi-realesate-tokenization-license` for the shared skills, and `analyzicai` and `realestate-tokenization` for the engineering standards. They receive the changes when they refresh the installed skill or adopt the updated standards.
