# Learn While Coding with AI

AI can shorten the path to an answer while quietly removing the struggle that builds a durable mental model. Use it to increase feedback, not outsource understanding.

## Never merge code you cannot explain

Before owning a generated change, be able to answer:

- What behavior changed?
- Why is this the right layer for the change?
- What assumptions does it make?
- What happens on failure?
- Which test would catch a regression?
- What security or performance boundary is involved?
- How would you debug it in production?

If the explanation is unclear, ask the assistant to trace the code with concrete inputs or reduce the change until it is understandable.

## Predict before asking

For learning tasks:

1. Write your current hypothesis.
2. Ask the assistant for an explanation or counterexample.
3. Compare the difference.
4. Verify against code, execution, or primary documentation.
5. Explain the result back in your own words.

Prediction makes misunderstandings visible. Passive reading often produces only a feeling of familiarity.

## Ask for traces and contrasts

Useful learning prompts include:

```text
Trace this function with one normal input and one boundary input. Show each
state change and cite the line that causes it.
```

```text
Compare these two approaches. Give the invariant each one protects, one case
where it fails, and the evidence in this repository that favors either one.
```

```text
Ask me three questions that would reveal whether I understand this module.
Do not give the answers until I respond.
```

The aim is active recall and causal understanding, not a longer summary.

## Use primary sources for unstable facts

Models can confidently describe an API, command, or configuration option from an older version—or invent one entirely.

For libraries and tools that change:

- Check the installed version.
- Read the repository's lockfile or manifest.
- Prefer official documentation and release notes.
- Ask the assistant to link the exact page supporting a claim.
- Run the smallest example or inspect the local type/API definition.

Treat “I know this framework” as a search starting point, not current evidence.

## Keep a decision log

For non-obvious work, record:

- Decision
- Alternatives considered
- Evidence and constraints
- Consequences and follow-up

This prevents the next session from reopening settled questions and gives future developers context that is more reliable than a compacted chat.

## Practice without the assistant sometimes

Use AI heavily for feedback and unfamiliar navigation, but periodically:

- Reproduce a small bug unaided.
- Write a test before requesting suggestions.
- Review a diff before asking for an AI review.
- Sketch the data flow from memory, then check it.
- Reimplement a small concept in a disposable exercise.

The goal is not purity. It is preserving the ability to notice when the assistant is wrong.

## Go further

- [Engineering Fundamentals in the AI Era](engineering-fundamentals.md): what to study, in what order, with a weekly routine and 90-day plan.
- [Software Development Tips](software-development-tips.md): day-to-day habits for debugging, testing, review, and operations.

## Key takeaway

Use AI to get more deliberate practice per hour, not fewer opportunities to think.
