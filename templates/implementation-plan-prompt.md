# Implementation Plan Prompt

Use this after a task brief when the work is ambiguous, cross-cutting, high-risk,
or easier to verify in stages. Obvious reversible edits may not need a separate plan.

```text
Create an implementation plan. Do not write code yet.

Goal and user impact:
[Describe the outcome, not just the requested code]

Current behavior:
[Describe what happens today]

Acceptance criteria:
- [Observable requirement]
- [Observable requirement]

Scope:
- Relevant files or components: [List if known]
- In scope: [List]
- Out of scope: [List]

Constraints:
- Compatibility: [Versions, APIs, or clients]
- Security and privacy: [Requirements]
- Performance and reliability: [Requirements]
- Dependencies: [Allowed or prohibited changes]
- Delivery: [Migration, rollout, or deadline constraints]

Return:
1. Assumptions and blocking questions
2. Existing patterns or code paths to reuse
3. Ordered implementation steps, with likely files and behavior changes
4. API, data-model, migration, and compatibility effects
5. Test matrix and exact verification approach
6. Logging, observability, rollout, and rollback needs
7. Risks, trade-offs, and alternatives rejected

Rules:
- Distinguish facts found in the repository from assumptions.
- Prefer the smallest change that meets the acceptance criteria.
- Do not include unrelated refactoring.
- Identify a safe checkpoint after each meaningful step.
```
