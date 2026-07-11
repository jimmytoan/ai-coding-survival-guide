# Scoped Implementation Prompt

Use this after approving a plan. Authorize one reviewable checkpoint at a time.

```text
Implement only this approved step:
[Plan step and intended behavior]

Task brief:
[Paste or link]

Allowed scope:
- Files that may change: [List]
- Tests or commands that may run: [List]
- New dependencies: allowed / not allowed

Constraints:
- [Compatibility, security, performance, or style constraint]

Before editing:
- Inspect the named path and confirm the plan still matches the repository.
- Stop if a missing decision, unexpected sensitive area, or material scope increase
  would change the approach.

During implementation:
- Follow existing patterns unless there is a concrete reason not to.
- Do not refactor unrelated code or silently change the approved behavior.
- Add or update tests that demonstrate this step's observable behavior.

After implementation, return:
- Files changed and the behavior changed in each
- Exact checks run and their direct results
- Acceptance criteria demonstrated by this step
- Anything not verified, known risks, and the next safe checkpoint
```
