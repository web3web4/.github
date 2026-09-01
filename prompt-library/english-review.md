# English Review

For text you wrote yourself and want corrected without being taken over: a PR description, a commit message, a doc paragraph, a message to a colleague.

Asked plainly, a model returns a single rewrite in its own voice, softens or strengthens technical claims on the way, and "corrects" identifiers, product names, and pasted output. This prompt returns three graded versions so you choose how far to go, and ring-fences everything that must not change.

```text
Review the English in the text below. Return three versions. Each is a complete
replacement for the original, not a diff.

<text>
[Paste the text here]
</text>

1. Corrected — fix only what is wrong: grammar, spelling, punctuation, and
   wording that is not idiomatic English. Keep my sentence structure, my
   vocabulary, and my register, including where they are plain or blunt.
2. Improved — additionally fix clarity and flow. Split or merge sentences where
   that helps. Keep my meaning, my ordering, and my level of formality.
3. Polished — the strongest version that still says exactly what I said, to the
   same reader and around the same length.

Rules:
- Add no fact, example, caveat, hedge, or conclusion that is not in my text, and
  drop none that is.
- Do not change code, identifiers, file paths, commands, URLs, quoted output,
  product names, or Markdown and list structure.
- Keep my terminology. If a term looks wrong for what I mean, leave it in place
  and raise it separately.
- Where a sentence is ambiguous, keep the ambiguity rather than picking a
  reading, and raise it separately.

Output the three labelled versions, then a "Check" list of the terminology and
ambiguity points you raised. Omit "Check" when it is empty. Do not explain the
edits.
```

**Filling it in:** paste the text exactly as written, Markdown included. If it is a reply or a message, add one line above the block saying who the reader is — register is the main thing the polished version gets wrong without it.
