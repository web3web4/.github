# Actionable Review

For a draft, design, proposal, or decision you want stress-tested before committing to it.

The default review opens with praise, then lists a broken assumption and a stray comma at the same weight, then hands back a full rewrite that quietly drops your intent. This prompt anchors the review to a stated goal, sorts findings by impact, makes each finding carry its own fix, and bans the rewrite.

```text
Review the item below against its goal.

Goal: [What the item must achieve, stated so it can be judged met or not met]
Reader or user: [Who the item is for, or "None"]
Out of scope: [What not to comment on, or "None"]

<item>
[Paste the item here]
</item>

Rules:
- Report only what materially affects the goal. No praise, no style preference,
  no restating what the item already gets right.
- Order findings by impact, highest first, and number them.
- Label each finding either "Fails the goal" or "Weakens the goal". Nothing
  else.
- Give each finding three lines: what is wrong, why that matters for the goal,
  and the smallest change that fixes it. Quote the exact text you mean.
- Do not rewrite the item. A fix is described, or given as a one-line
  replacement for the quoted text.
- Say so when a finding rests on an assumption about context I did not give you.
- If nothing material is wrong, reply "No material issues found" and stop.
```

**Filling it in:** a vague goal produces a vague review. "Is this good?" gets you style notes. "A reviewer who has not seen this code can approve it without asking a question" gets you the gaps.
