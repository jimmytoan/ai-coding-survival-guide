# Refactoring, Migrations, and Production Changes

AI can accelerate repetitive transformations, but broad changes amplify one wrong assumption across many files or records. Make invariants, stages, and recovery explicit.

## Separate three kinds of work

- **Mechanical change:** syntax, naming, imports, generated updates
- **Behavior-preserving refactor:** internal structure changes while contracts remain stable
- **Behavior or data migration:** callers, persisted data, deployment order, or operations change

Do not hide behavior changes inside a mechanical diff. Separate stages so each can be reviewed and verified with the right evidence.

## Define invariants before changing structure

Capture what must remain true:

- Public API, file format, event, and schema contracts
- Error and retry semantics
- Ordering, idempotency, and transaction behavior
- Performance and resource budgets
- Authorization and data-isolation boundaries
- Compatibility with older clients, services, or records
- Operational signals and alerts

Write characterization or contract tests for important behavior that is not already protected.

## Inventory the blast radius

Have the assistant find:

- Definitions and direct callers
- Dynamic or string-based references
- Tests, fixtures, examples, and documentation
- Configuration, build, CI, and deployment files
- Generated code and its source
- Stored data and migrations
- External consumers and version contracts
- Observability and runbooks

Ask it to cite evidence. Repository search can miss runtime reflection and external consumers; label those uncertainties.

## Use staged, reversible changes

For compatible migrations, prefer an expand-and-contract sequence:

1. **Expand:** add the new schema or interface without removing the old one.
2. **Bridge:** support both forms; dual-read or dual-write only with explicit consistency rules.
3. **Backfill:** migrate existing data in bounded, observable batches.
4. **Switch:** move traffic or callers using a controlled flag or release sequence.
5. **Observe:** verify correctness, performance, and error budgets.
6. **Contract:** remove old behavior only after compatibility is no longer required.

Each stage should be deployable, testable, and safe if the next stage is delayed.

## Make rollback concrete

“We can revert” is not a rollback plan when:

- Data was destructively transformed
- Old binaries cannot read new records
- External side effects already occurred
- A queue or cache contains the new format
- Credentials or infrastructure changed

State:

- Which code version can run against each schema state
- Whether data restoration is needed and from what backup
- How to stop or resume a backfill
- What happens to events produced during rollback
- The decision signal and owner for rollback

Test the recovery path when failure would be expensive.

## Use checkpoints for bulk edits

For a large mechanical migration:

1. Convert one representative slice.
2. Run focused and contract checks.
3. Review the pattern with a human.
4. Automate the proven transformation where possible.
5. Apply it in batches with exclusive file ownership.
6. Run repository-wide search for leftovers and malformed changes.
7. Run the broader verification suite.

Prefer an AST-aware codemod or official migration tool over hundreds of independent text edits when syntax matters.

## Design backfills for operations

A generated backfill should address:

- Idempotency and restartability
- Batching, throttling, and time bounds
- Transactions and partial failure
- Progress checkpoints and metrics
- Dead-letter or quarantine behavior
- Data validation before and after
- Dry-run or count-only mode
- Auditability and least-privilege credentials
- Safe cancellation

Test on representative data volume and edge cases without exposing production data to an unapproved AI service.

## Use flags deliberately

Feature flags help only when both states are maintained and tested.

- Define owner, default, rollout steps, and removal date.
- Test on/off and transition behavior.
- Avoid using a flag to mask an incompatible database deployment.
- Monitor results by cohort and version.
- Remove stale flags and dual paths after the migration is proven.

## Require production evidence

Pre-deploy checks are necessary but not sufficient. Define:

- Health, error, latency, saturation, and business metrics
- Data-integrity queries or reconciliation
- Logs that diagnose failure without exposing sensitive data
- Canary or staged rollout criteria
- Alert thresholds and an observation window
- Rollback trigger and responsible owner
- Post-deploy validation steps

An agent may draft or run approved checks, but production mutation and rollback remain explicit human-controlled actions unless a separately authorized automation owns them.

## Migration prompt

```text
Plan this migration before editing.

Current state:
[Interface, schema, callers, deployment]

Target state:
[Desired result]

Invariants:
[Behavior, compatibility, security, performance]

Return:
- Evidence-backed blast-radius inventory
- Staged expand/bridge/backfill/switch/contract plan
- Verification and observability for each stage
- Deployment ordering and compatibility matrix
- Rollback limits and recovery procedure
- Approval points and unresolved risks

Do not combine unrelated cleanup with the migration.
```

Template: [Migration Plan](../templates/migration-plan-prompt.md)

## Key takeaway

For broad changes, speed comes from a proven transformation and safe stages—not from asking the agent to edit everything at once.
