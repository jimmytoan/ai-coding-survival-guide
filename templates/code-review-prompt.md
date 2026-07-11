# Code Review Prompt

Use this to review a diff or a small set of files. Include the task brief, diff, and
test results when available.

```text
Review this change without editing it.

Goal:
[What the change should accomplish]

Acceptance criteria:
- [Observable requirement]

Scope:
- Diff or files: [List]
- Base revision: [Commit or branch, if relevant]
- Out of scope: [Behavior that must not change]

Evidence already available:
- Tests run: [Commands and results]
- Known risks or open questions: [List]

Review for:
- Incorrect behavior and unmet acceptance criteria
- Edge cases, failure modes, and concurrency issues
- Security, privacy, and data-integrity risks
- Backward-compatibility or migration problems
- Invented APIs, suspicious dependencies, and unsafe configuration
- Missing or misleading tests
- Unnecessary complexity and unrelated changes

For each finding, return:
- Severity: critical, high, medium, or low
- Location: file and line
- Evidence: the relevant execution path or concrete scenario
- Impact: what can fail and for whom
- Smallest reasonable fix

Then return:
- Verification gaps
- Assumptions or questions
- A short summary if no actionable findings exist

Rules:
- Prioritize correctness over style preferences.
- Do not report speculative issues without a plausible failure path.
- Do not rewrite the change unless asked.
```
