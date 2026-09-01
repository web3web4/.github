# Direct Answer

For one focused question where an unsupported assumption would cost you: a version constraint, a semantics question, "does X actually do Y".

The default answer opens by restating the question, hedges in both directions, and quietly fills any gap in the context with something plausible. This prompt forces the answer into the first line, separates fact from assumption, and makes "not enough information" a legitimate result.

```text
Answer the question below.

Question: [Paste the question here]
Context: [Paste only what is needed to answer it, or write "None"]

Rules:
- Direct to the answer. No preamble, no restatement of the question, no
  closing summary.
- After it, do not follow up with anything except if there something important that affects what are to do next.
- Put anything you are not certain of on its own line, starting with
  "Assuming:". Do not fold assumptions into the answer.
- If the context is not enough to answer, say so in the first line and list what
  you would need. Do not guess, and do not answer a nearby question instead.
- If the honest answer is "it depends", name the things it depends on and
  give the answer for each case.
- Name the version, spec, or API you are relying on whenever the answer turns on
  it.
```

**Filling it in:** keep the context minimal and exact. Pasting a whole file instead of the twenty relevant lines is what makes the model answer a nearby question convincingly.
