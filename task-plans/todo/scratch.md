# Scratch

Stray findings noticed during unrelated work. One short entry each. Not tracked
work — the user decides what graduates into an artifact.

- 2026-09-01 — `skills/prompt-authoring-guide/SKILL.md` line 44 fails
  `MD031/blanks-around-fences`. Left alone on purpose: a whitespace-only edit to
  `skills/` fails `skills-drift` in every consumer repo until they update. Fix it
  alongside the next substantive change to that skill.
- 2026-09-01 — CI does not run Markdown lint. `.markdownlint.jsonc` exists and
  the gate is documented in `AGENTS.md`, but nothing enforces it. Adding a lint
  job to `.github/workflows/standards.yml` would need the known findings in
  `profile/README.md` and `skills/` cleared first.
