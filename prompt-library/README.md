# Prompt Library

A collection of prompts to take and use. Each one shapes an LLM's **response** — it sets an output contract, what to produce, in what order, and what to leave out, and blocks the specific way models get that kind of request wrong. None of them prescribe how to do the underlying work.

Use one as it stands, or edit it. Nothing here is a rule, and no repository is out of compliance for ignoring it. They are collected so that a prompt somebody already got right does not have to be rewritten from scratch by the next person.

These are not task-handoff prompts. A prompt that asks an agent to plan or implement repository work belongs in that repo's `task-plans/others/prompts/`, per the [`prompt-authoring-guide`](../skills/prompt-authoring-guide/SKILL.md) skill. The line is simple: **these produce an answer, those produce a plan artifact.**

## The prompts

- [English Review](english-review.md) - For text you wrote in English and want corrected without losing your voice. Stops the model rewriting you in its own register, or "fixing" identifiers, product names, and quoted output.
- [Direct Answer](direct-answer.md) - For one focused question where a wrong answer is expensive. Stops preamble, two-sided hedging, and gaps in the context being filled with something plausible.
- [Options and Recommendation](options-and-recommendation.md) - For a bounded decision that needs a call, not a survey. Stops options you ruled out reappearing, tables full of rating words, and the refusal to pick one.
- [Actionable Goal-based Review](actionable-goal-based-review.md) - For a draft, design, or decision that needs critique you can act on. Stops the opening praise, nitpicks weighted like real problems, and the unasked-for full rewrite.

## Use

1. Copy the fenced prompt block. The prose around it is for you, not for the model.
2. Replace **every** bracketed placeholder. A placeholder left in place is the most common reason the output comes back generic.
3. Paste it into a fresh chat. These prompts assume no prior conversation — in a long thread, earlier turns will override the rules.
4. If the answer still comes back the wrong shape, add the missing rule to your copy of the prompt rather than arguing with the model turn by turn.

## Writing your own

They share a shape worth reusing:

- **State the output contract, in order.** "Be concise" changes nothing. "The first line is the answer, then at most three bullets" does.
- **Name the failure mode and forbid it.** Every kind of request has a default way of going wrong. Write the rule against that one, not a general plea for quality.
- **Give an escape hatch.** Say what to do when the input is insufficient, contradictory, or ambiguous. Without one, the model invents its way past the gap.
- **Define "nothing to report".** Otherwise you will always get findings, whether or not any exist.
- **Fence the scope.** Say what must not be touched, added, or rewritten. Models expand scope by default.
- **Keep every rule checkable.** You should be able to read the answer and say whether each rule was followed.
