# Good vs Bad Prompts

## Example 1: Repo review

Bad:

```text
Review this whole repo and tell me what to improve.
```

Better:

```text
Review only src/auth/session.ts and src/api/login.ts.

Goal:
Find why users are logged out after token refresh.

Do not edit files yet.

Output:
- Likely causes
- Evidence
- Recommended next step
```

Why better:

- Smaller context
- Clear goal
- No unnecessary implementation
- Easier to review

## Example 2: Implementation

Bad:

```text
Build the whole billing alert feature.
```

Better:

```text
Create an implementation plan first.

Goal:
Add AI usage budget alerts.

Requirements:
- Warn at 70%
- Warn at 90%
- Block non-admin users at 100%
- Admins can override
- Usage events must be auditable

Do not write code yet.
```

Why better:

- Forces planning
- Reduces random code generation
- Makes trade-offs visible

## Example 3: Refactoring

Bad:

```text
Refactor this module and make it cleaner.
```

Better:

```text
Suggest cleanup for this module.

Rules:
- Do not change behavior.
- Do not introduce new abstractions unless necessary.
- Identify duplicated or redundant code.
- Suggest changes in small steps.

Do not write code yet.
```

Why better:

- Prevents over-engineering
- Keeps review manageable
- Avoids surprise behavior changes

## Example 4: Agent mode

Bad:

```text
Use agent mode and fix all failing tests.
```

Better:

```text
First inspect the failing test output.

Output:
- Which tests are failing
- Most likely root cause
- Files likely affected
- Minimal fix plan

Do not edit files yet.
```

Why better:

- Avoids blind edits
- Reduces unnecessary file changes
- Keeps the human in control

## Key takeaway

A good prompt narrows the job.

A bad prompt creates a larger mess at higher speed.
