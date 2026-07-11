# When to Use Strong Models

Strong models earn their cost when a task needs deeper synthesis or when an error
would create expensive rework or risk.

## Good uses

- Architecture and implementation trade-offs
- Security and privacy reviews
- Authentication and authorization logic
- Complex, intermittent, or cross-service debugging
- Production incident analysis
- Data and API migrations
- Large refactor planning and dependency sequencing
- Final review of high-risk changes

## Give the model a decision-ready context packet

More context is not automatically better. Supply:

- Goal, current behavior, acceptance criteria, and non-goals
- The relevant code paths and contracts
- Constraints and known risks
- Evidence such as errors, traces, diffs, and test results
- The decision or output format you need

## Example: migration planning

```text
Plan only. Migrate authorization from fixed roles to policies while old and new
application versions may run together.

Return confirmed repository facts separately from assumptions, then cover schema
and API compatibility, backfill and cutover, observability, failure recovery,
rollback, security review, and a test matrix. Compare at least two viable rollout
strategies and recommend one using the stated constraints.
```

## Example: security review

```text
Review this authentication diff without editing it.

Trace untrusted input through token validation, session creation, authorization,
logging, and error responses. Report only findings with a plausible attack path.
For each finding include severity, file/line evidence, impact, preconditions, and
the smallest mitigation. List verification gaps separately.
```

## Keep human checkpoints

A stronger model can still make confident mistakes. Use it for the hard reasoning,
then inspect the diff, verify claims with direct evidence, run relevant checks, and
require specialist review for high-risk domains.
