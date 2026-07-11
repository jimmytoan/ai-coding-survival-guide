# AI-Assisted Change Handoff

Use this when work moves to another developer, reviewer, or a fresh AI session.
Keep it factual and link to durable artifacts instead of pasting a long transcript.

```text
Outcome:
[What now works, or why the task is incomplete]

Task and acceptance criteria:
- [Link to brief or summarize]

Changed:
- [File/component]: [Behavioral change and reason]

Key decisions:
- [Decision]: [Why, including rejected alternative if important]

Verification performed:
- [Exact command/check]: [Pass/fail and relevant result]
- Acceptance criteria demonstrated: [List]

Not verified:
- [Check or environment]: [Reason]

Known risks or limitations:
- [Risk, affected users, and mitigation]

Remaining work:
1. [Concrete next step]

Review starting points:
- [Important file, function, test, log, or diff section]

Workspace state:
- Branch/revision: [Reference]
- Uncommitted or generated files: [List]
- Required setup: [Command or documentation]
```

Do not claim a command passed unless its result was observed. Separate unfinished
work from optional follow-ups so the next person knows what blocks completion.
