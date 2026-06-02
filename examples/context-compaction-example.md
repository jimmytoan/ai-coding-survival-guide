# Context Compaction Example

Long conversations can become stale, noisy, and expensive.

Instead of keeping everything, compact the conversation into a useful working summary.

## Bad compaction

```text
We discussed the billing feature and some ideas.
```

This is too vague.

## Good compaction

```text
Current goal:
Implement AI usage budget alerts in the internal assistant platform.

Relevant files:
- usage-tracker.ts
- billing-service.ts
- model-router.ts
- budget-alert-banner.tsx

Decisions:
- Warn at 70% budget usage.
- Warn again at 90%.
- Block non-admin users at 100%.
- Admin users can override.
- Every budget event must be auditable.
- Do not block read-only AI chat during the first rollout.

Constraints:
- Do not introduce a new billing provider yet.
- Use existing usage_events table if possible.
- Keep UI minimal for V1.

Open questions:
- Should budget be per user, team, or workspace?
- Should admins receive weekly usage email?

Next step:
Design API and database changes before writing code.
```

## Why this works

It keeps:

- Goal
- Relevant files
- Decisions
- Constraints
- Open questions
- Next step

It removes:

- Old debate
- Failed ideas
- Repeated explanations
- Irrelevant context
