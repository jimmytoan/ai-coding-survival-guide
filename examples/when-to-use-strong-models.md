# When to Use Strong Models

Strong models are valuable when the task requires deeper reasoning or when mistakes are expensive.

## Use strong models for

- Architecture trade-offs
- Implementation planning
- Security review
- Authentication/authorization logic
- Production incident analysis
- Complex debugging
- Data migration
- Large refactoring plans
- Final review before merge

## Example: implementation planning

```text
Create an implementation plan before writing code.

Goal:
Migrate the user permission model from role-based access to policy-based access.

Output:
- Current assumptions
- Risk areas
- Data model changes
- API changes
- Migration plan
- Test plan
- Rollback plan
- Questions before implementation

Do not write code yet.
```

## Example: security review

```text
Review this authentication change.

Focus on:
- Token validation
- Session expiry
- Replay risks
- Permission bypass
- Logging of sensitive data
- Edge cases

Return critical issues first.
```

## Key takeaway

Use strong models where they reduce expensive mistakes.
