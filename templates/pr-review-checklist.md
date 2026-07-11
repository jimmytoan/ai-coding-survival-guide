# PR Review Checklist for AI-Assisted Code

Use the relevant sections; a documentation-only change does not need a database
migration review. AI-assisted code receives the same scrutiny as any other code.

## Intent and scope

- [ ] The problem and user impact are clear.
- [ ] Acceptance criteria are observable and satisfied.
- [ ] The diff matches the stated scope and leaves unrelated files untouched.
- [ ] Assumptions and non-goals are documented.
- [ ] The author can explain the change without relying on “AI wrote it.”

## Correctness

- [ ] Happy paths, boundary cases, and invalid inputs behave correctly.
- [ ] Errors are handled without hiding useful failure information.
- [ ] State transitions, retries, timeouts, and partial failures are safe.
- [ ] Concurrency, ordering, idempotency, and time-zone concerns were considered.
- [ ] Existing callers and behavior remain compatible where required.

## Design and maintainability

- [ ] The change follows existing repository patterns where appropriate.
- [ ] Names and control flow make the behavior easy to understand.
- [ ] There is no speculative abstraction, duplicated logic, or dead code.
- [ ] Comments explain non-obvious reasons rather than restating the code.
- [ ] Generated code, dependencies, and licenses are acceptable and necessary.

## Tests and evidence

- [ ] Tests cover the important success, failure, and regression cases.
- [ ] Assertions verify behavior rather than merely increasing coverage.
- [ ] Tests were not changed only to bless incorrect output.
- [ ] Exact commands and results are recorded.
- [ ] Skipped checks and remaining manual verification are explicit.

## Security and privacy

- [ ] Inputs are validated at the correct trust boundary.
- [ ] Authentication and authorization are enforced server-side where required.
- [ ] Secrets and sensitive data are not exposed in code, logs, prompts, or errors.
- [ ] Injection, unsafe parsing, path handling, and command execution were reviewed.
- [ ] New permissions, integrations, and dependencies use least privilege.
- [ ] Abuse cases, rate limits, and denial-of-service risks were considered.

## Data, APIs, and operations

- [ ] Schema or data changes are compatible, reversible, and safe at expected scale.
- [ ] Public contracts and generated clients are updated or intentionally unchanged.
- [ ] Rollout, rollback, feature flags, and mixed-version behavior are addressed.
- [ ] Logs, metrics, and alerts make failures diagnosable without leaking data.
- [ ] User-facing, operational, and architecture documentation is current.

## Final check

- [ ] The complete diff was reviewed, including tests, lockfiles, CI, hooks,
  containers, configuration, deployment, and generated files.
- [ ] The evidence supports the PR description; there are no fabricated claims.
- [ ] Known risks and follow-up work have owners.
- [ ] I would be comfortable owning and operating this change.
