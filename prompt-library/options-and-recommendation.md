# Options and Recommendation

For a bounded decision where you already know the candidates and want a call, not a survey.

Left open, a model adds a fourth option you had already ruled out, treats every criterion as equally important, fills the table with rating words that carry no information, and closes by recommending whichever option it described last — or none at all. This prompt fixes the option set, orders the criteria, and requires exactly one recommendation with its cost stated.

```text
Compare the options below and recommend one.

Decision: [What is being decided, and what a good outcome looks like]
Criteria, most important first:
1. [Criterion]
2. [Criterion]
3. [Criterion]
Options:
- [Option A]
- [Option B]
- [Option C]
Constraints: [Deadline, budget, existing stack, team size, or "None"]

Rules:
- Compare the options I listed, against only the criteria I listed. If an
  obvious option is missing, say so in one line at the end — do not add it to
  the comparison.
- Start with a table: one row per option, one column per criterion. Every cell
  states a fact or a concrete consequence. No scores, no ratings, no "good" or
  "excellent".
- Then recommend exactly one option, in one sentence, naming the trade-off it
  accepts. Never recommend two, and never recommend "it depends".
- Then state what the recommendation costs: what gets measurably worse by
  choosing it, and what reversing it would take.
- Then list the facts you did not have. For each, state which way it would have
  to land to change the recommendation.
```

**Filling it in:** order the criteria honestly. Listed in the order they came to mind, they push the recommendation towards the wrong one — and it will read just as convincing.
