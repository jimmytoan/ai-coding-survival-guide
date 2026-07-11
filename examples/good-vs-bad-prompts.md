# Weak vs Useful Prompts

A useful prompt supplies an outcome, boundary, evidence, and expected response. It
does not need to be long.

## 1. Repository review

Weak:

```text
Review this whole repo and tell me what to improve.
```

Useful:

```text
Start with src/auth/session.ts and src/api/login.ts. Inspect direct callers and
tests needed to trace the behavior, but do not scan unrelated areas.

Goal: find why a successful token refresh still logs users out.
Do not edit files. Trace the failing path and return findings with file/line
evidence, the most likely cause, and the smallest next diagnostic step.
```

Why it works: it narrows context, defines the failure, and asks for evidence before
implementation.

## 2. Feature implementation

Weak:

```text
Build the billing alert feature.
```

Useful:

```text
Plan only; do not write code.

Goal: warn at 70% and 90% of workspace budget, then block non-admin generation at
100%. Admin overrides must be audited. Read-only chat must remain available.

Identify the existing usage and authorization paths, list assumptions, propose
small implementation checkpoints, and include tests, rollout, and rollback.
```

Why it works: observable behavior and non-goals make trade-offs reviewable before
the assistant creates a large diff.

## 3. Refactoring

Weak:

```text
Refactor this module and make it cleaner.
```

Useful:

```text
Suggest behavior-preserving cleanup for src/parser.ts. Do not edit it.

Use the existing tests as the behavior contract. Identify concrete duplication or
complexity, explain each proposed change, and skip abstractions that have only one
caller. Flag any behavior you cannot prove from tests or call sites.
```

Why it works: “cleaner” becomes a constrained review rather than permission to
redesign the module.

## 4. Debugging

Weak:

```text
The CI test is flaky. Fix it.
```

Useful:

```text
Investigate, but do not edit yet.

The checkout test failed 3 of 20 Linux CI runs with the attached timeout. It has
not failed locally. Inspect the test, its production path, and the smallest useful
log excerpt. Rank race, clock, network, and shared-state hypotheses by evidence.
Propose one experiment that can distinguish the top two causes.
```

Why it works: frequency and environment matter for intermittent failures, and the
requested experiment avoids a guess-based patch.

## 5. Test generation

Weak:

```text
Add lots of tests for this class.
```

Useful:

```text
Add focused tests for the public behavior changed in this diff.

Cover the success path, invalid input, downstream failure, and the reported
regression. Do not expose private methods or weaken assertions. If expected
behavior is unclear, stop and list the ambiguity before changing tests.
```

Why it works: tests protect behavior instead of implementation details or a coverage
number.

## 6. Verification

Weak:

```text
Looks good, right?
```

Useful:

```text
Verify without editing. Map each acceptance criterion to the diff and direct test
evidence. Run the focused checks, report exact commands and results, and mark
anything not demonstrated as not verified. Check for unrelated changes and likely
regressions before giving a verdict.
```

Why it works: it asks the assistant to challenge the result and prevents a summary
from being mistaken for proof.

## Key takeaway

State the outcome, scope, constraints, evidence, and desired response. Add detail
only when it changes the assistant's decision.
