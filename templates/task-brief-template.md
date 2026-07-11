# AI Coding Task Brief

Use this before planning or implementation. A good brief gives the assistant a
testable destination and a clear boundary without dumping the whole repository.

```text
Task:
[One-sentence outcome]

User or system impact:
[Who benefits and what changes for them]

Current behavior:
[What happens today, with a minimal example if useful]

Acceptance criteria:
- Given [starting state], when [action], then [observable result].
- [Performance, compatibility, accessibility, or reliability requirement].

In scope:
- [Behavior, component, or file]

Out of scope:
- [Explicit non-goal]

Relevant context:
- Files or components: [List]
- Issue, design, or API contract: [Reference]
- Existing pattern to follow: [Reference]

Constraints:
- [Language/framework/version]
- [No new dependencies, public API stability, deadline, etc.]

Risk and permissions:
- Risk level: low / medium / high
- Sensitive areas: [Security, privacy, data, infrastructure, or none]
- Allowed actions: [Read, edit, run tests, use network, etc.]
- Actions requiring approval: [List]

Verification:
- Automated checks: [Commands or suites]
- Manual checks: [Scenarios]
- Completion evidence: [What must be reported]

Open questions:
- [Question that could materially change the approach]
```

Delete empty fields only when they truly do not apply. Unknown information should
be marked `unknown`, not guessed.
