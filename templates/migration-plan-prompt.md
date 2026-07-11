# Migration Plan Prompt

Use for schema, API, data, dependency, infrastructure, or broad code migrations.

```text
Plan only. Do not edit files or mutate data.

Current state:
[Schema/interface/deployment/callers]

Target state:
[Observable end state]

Invariants:
- [Compatibility, data integrity, security, performance, availability]

Constraints:
- Versions that may run together: [List]
- Data volume and time budget: [Details]
- Deployment and rollback limitations: [Details]
- Actions requiring approval: [List]

Inspect the repository and return:
1. Evidence-backed blast-radius inventory
2. Compatibility matrix for callers, versions, schemas, and events
3. Staged expand / bridge / backfill / switch / observe / contract sequence
4. Verification and observability for every stage
5. Backfill idempotency, batching, restart, failure, and audit behavior
6. Deployment ordering, feature flags, canary, and success thresholds
7. Concrete rollback and data-recovery limits
8. Human approval checkpoints, owners, and unresolved risks

Keep mechanical edits, behavior changes, and unrelated cleanup separate. Every
stage should be safe if the next stage is delayed.
```
