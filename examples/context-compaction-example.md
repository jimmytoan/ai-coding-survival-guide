# Context Compaction Example

Long conversations accumulate outdated assumptions, failed approaches, and repeated
explanations. Compact them into a factual working state before continuing.

## Weak compaction

```text
We discussed the billing feature and made progress. Continue where we left off.
```

This does not say what changed, which decisions are current, or what remains.

## Useful compaction

```text
Current goal:
Add workspace AI-usage budget alerts without blocking read-only chat in V1.

Acceptance criteria:
- Warn workspace members at 70% and 90% monthly usage.
- Block non-admin generation requests at 100%.
- Allow an audited admin override.

Relevant files:
- src/usage/usage-tracker.ts
- src/billing/budget-service.ts
- src/routing/model-router.ts
- src/ui/budget-alert-banner.tsx

Confirmed decisions:
- Budget is per workspace and resets at the billing-cycle boundary.
- Existing usage_events remains the audit source.
- Enforcement occurs server-side; the banner is informational.
- No new billing provider or weekly email in V1.

Implemented:
- BudgetService now returns percentage used and threshold state.
- Unit tests cover 69%, 70%, 90%, and 100% boundaries.

Verification evidence:
- npm test -- budget-service.test.ts: 12 passed.
- Typecheck has not been run.
- UI and admin override are not implemented.

Rejected approach:
- Client-side blocking was rejected because it can be bypassed.

Known risk:
- Billing-cycle time-zone behavior is not yet confirmed.

Next step:
Confirm cycle-boundary semantics, then plan server-side enforcement in model-router.ts.

Workspace state:
- Branch: feature/budget-alerts
- Uncommitted changes: budget-service.ts and budget-service.test.ts
```

## What to preserve

- Goal and acceptance criteria
- Current files and workspace state
- Confirmed decisions and rejected approaches that prevent repeated debate
- Completed work and direct verification evidence
- Unknowns, risks, and the single next step

Remove speculation, obsolete plans, raw tool output, and conversational history that
does not change the next decision.
