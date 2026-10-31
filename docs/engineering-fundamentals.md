# Engineering Fundamentals in the AI Era

Agents write plausible code in seconds. What stays scarce is deciding what to build, judging whether the result is right, and keeping it running. If you cannot read the output, you cannot correct it.

DORA's research on AI-assisted development frames AI as an amplifier of what a team already does well or badly. The same holds for one person: fundamentals decide whether the assistant multiplies your judgment or your mistakes.

This page is a learning map, not a product guide, so it does not go stale when models change. The prioritization is judgment; the linked references are stable primary material.

## What matters more, what matters less

| Gaining value | Losing value |
|---|---|
| Reading code and diffs quickly and accurately | Memorizing syntax and standard-library signatures |
| Framing a problem with testable acceptance criteria | Typing boilerplate fast |
| Designing checks that prove behavior | Writing first-draft code by hand |
| Debugging with evidence | Knowing one framework's API by heart |
| Operating systems: observability, rollback, incident response | Treating "it merged" as done |
| Security and failure-mode thinking | Assuming the generated path is the safe path |
| Writing: specs, decisions, and review comments | Producing code nobody else can explain |

## The skill stack

| Layer | Be able to | Why it matters with agents | Practice |
|---|---|---|---|
| 1. Read and judge code | Trace control flow, state, errors, complexity, and invariants in an unfamiliar file | You are the reviewer for every change | Predict what a function does, then run it |
| 2. Debug and test | Reproduce, minimize, bisect, read logs, design tests for behavior and edge cases | An agent's guess is only as good as the evidence you give it | Fix one bug a week without help |
| 3. Version control | Read history, bisect, rebase cleanly, keep commits atomic | Agents produce large diffs; you must split and audit them | Rewrite one messy branch into a reviewable series |
| 4. Systems basics | HTTP, DNS and TLS, processes and threads, memory, filesystems, latency, concurrency | Generated code often works on the happy path and fails under load or races | Explain one request from browser to database |
| 5. Data | SQL, indexes, transactions and isolation, migrations, caching, consistency | Data bugs are slow, silent, and hard to roll back | Run `EXPLAIN` on three real queries |
| 6. Design and APIs | Decompose, set boundaries, version contracts, model state, choose idempotency | Models imitate local patterns; they do not own architecture | Write a one-page design doc before coding |
| 7. Production | CI/CD, deploy and rollback, logs, metrics, traces, SLOs, incident response | "Works locally" is not a deployment | Deploy something small and watch its dashboards |
| 8. Security | Authn and authz, secrets, injection, dependency risk, threat modeling | Agents read untrusted text and run tools | Threat-model one feature before asking for it |
| 9. Working with agents | Frame tasks, curate context, build verification harnesses, track cost, evaluate models | Setup quality decides whether leverage or rework compounds | Benchmark two setups on a frozen task |
| 10. Product and communication | Clarify requirements, write trade-offs, review kindly and precisely, estimate as ranges | Ambiguity is the most expensive input | Write a decision record for your next choice |

## Priorities by career stage

- **Early career:** Build the mental model by hand first. Use AI to explain, quiz, and review, and attempt learning tasks before you ask. Layers 1 to 3 and 5 compound fastest.
- **Mid-level:** Own a service end to end. Layers 4 to 8 turn a feature builder into someone who can run what they ship.
- **Senior and staff:** Judgment, risk, and decomposition. Split work so humans and agents can each verify it, set the checks that gate merges, and mentor others to explain every change.
- **Leads and managers:** Policy, measurement, and guardrails. See [Team Guidelines](team-guidelines.md) and [Cost and Outcome Observability](cost-observability.md).

## A weekly routine

- **One unaided exercise.** Reproduce and fix a small bug, or write a test before asking for suggestions.
- **Review before reviewing.** Read a diff yourself, write down your concerns, then compare with an AI review.
- **One postmortem.** Read a public incident write-up and ask what check would have caught it.
- **One explanation.** Trace a system you use daily, from memory, then verify against the code or docs.
- **One decision record.** Capture decision, alternatives, evidence, and consequences. See [Learn While Coding with AI](learning-with-ai.md).

## A 90-day plan

| Days | Focus | Deliverable |
|---|---|---|
| 1–30 | Read code, tests, and history in a codebase you did not write | A written walkthrough of one feature, verified by running it |
| 31–60 | Data and production: queries, migrations, logs, metrics, rollback | A small service with a migration, a dashboard, and a rollback note |
| 61–90 | Security, design, and agents: threat model, design doc, verification harness | A feature built with an agent, plus the spec, checks, review notes, and a retro using the [AI Usage Retrospective](../templates/ai-usage-retro-template.md) |

## Warning signs you are outsourcing understanding

- You merge changes you cannot explain without rereading the chat.
- You cannot predict a test's result before running it.
- You accept an error fix without knowing why the error occurred.
- You cannot name the failure mode of the code you just shipped.
- You skip reading generated tests because they pass.

Respond by shrinking the task, writing the test yourself, or asking the assistant to quiz you instead of answer.

## References

These are free or widely available, vendor-neutral, and slow to age.

- Google — [Engineering Practices: code review](https://google.github.io/eng-practices/review/), including small changes and the standard of code health
- Winters, Manshreck, Wright — [Software Engineering at Google](https://abseil.io/resources/swe-book)
- Kleppmann — [Designing Data-Intensive Applications](https://dataintensive.net/)
- Google — [Site Reliability Engineering books](https://sre.google/books/)
- OWASP — [Top Ten](https://owasp.org/www-project-top-ten/) and [Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [The Twelve-Factor App](https://12factor.net/)
- [Trunk Based Development](https://trunkbaseddevelopment.com/)
- Willison — [Agentic Engineering Patterns](https://simonwillison.net/guides/agentic-engineering-patterns/)
- DORA — [State of AI-assisted Software Development](https://dora.dev/research/ai)

For day-to-day habits see [Software Development Tips](software-development-tips.md). For secure-development guidance in this guide see [Security and Privacy](security-and-privacy.md).

## Key takeaway

Use AI to get more deliberate practice per hour. The engineers who gain most are the ones who can read the output, prove it works, and operate it afterward.
