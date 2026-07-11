# Verification and Testing

AI can produce plausible code and plausible explanations at the same time. Verification is what separates a convincing answer from a working change.

> Trust evidence you can reproduce, not confidence in the response.

## Establish a baseline

Before changing code, determine what is already true:

- Reproduce the bug or capture the missing behavior.
- Run the nearest existing test when practical.
- Record relevant failures already present.
- Confirm the working tree and environment are the ones you intend to change.

For a bug fix, the best starting point is often a regression test that fails for the right reason. Without a baseline, you may “fix” behavior that was never reproduced or blame the change for an existing failure.

## Use a verification ladder

Start narrow for fast feedback, then climb as risk and blast radius require.

1. **Inspect the diff:** unintended files, deleted checks, debug code, secrets, generated artifacts.
2. **Parse or compile:** syntax, type checking, build errors.
3. **Static checks:** formatter, linter, security and dependency scans.
4. **Targeted tests:** the changed unit and a regression test for the requirement.
5. **Integration or contract tests:** boundaries such as database, API, queue, filesystem, or external service.
6. **Broader suite:** affected package, service, or repository.
7. **Behavioral validation:** browser, CLI, mobile, accessibility, or manual workflow.
8. **Specialized checks:** performance, load, migration rehearsal, security, fault injection, or rollback.

Not every change needs every rung. A payment migration needs more than a README correction.
For user interfaces, render required states and viewports and exercise real interactions;
see [Frontend and Visual Validation](frontend-and-visual-validation.md).

## Make the assistant show its work

Require a verification report with exact commands and outcomes:

```text
Verification report:
- PASS — npm test -- auth/session.test.ts (18 tests)
- PASS — npm run typecheck
- NOT RUN — end-to-end login flow; browser environment unavailable
- MANUAL — inspected redirect validation for encoded external URLs
- RISK — behavior behind the legacy SSO flag is not covered
```

Avoid vague claims such as:

- “All tests should pass.”
- “The implementation is production-ready.”
- “This handles all edge cases.”

Those are predictions unless backed by evidence.

Template: [Verification Prompt](../templates/verification-prompt.md)

## Generate tests from behavior, not implementation

Weak generated tests restate the code and pass even when the requirement is wrong.

Start from externally visible behavior:

- Inputs, outputs, and side effects
- Boundaries and equivalence classes
- Invalid, missing, malformed, and adversarial inputs
- Failure and retry behavior
- Permissions and identity boundaries
- Ordering, concurrency, and idempotency
- Compatibility with existing callers or data
- Invariants that should always hold

Ask what bug each test would catch. If the answer is “it covers this line,” the test may be too coupled to the implementation.

## Prove a regression test is meaningful

For a bug fix:

1. Demonstrate the test fails against the buggy behavior.
2. Implement the smallest fix.
3. Demonstrate the test passes.
4. Run nearby tests for preserved behavior.

If practical, temporarily reverse or remove the fix and confirm the regression test fails. This catches tests that never exercise the intended path.

## Watch for false-green tests

AI-generated changes sometimes make a suite green without making the software correct. Look for:

- Deleted, skipped, or weakened assertions
- Broad snapshots updated without inspection
- Mocks that bypass the behavior under test
- Tests that call a new helper but not the public path
- Expected values copied directly from the implementation
- Exceptions swallowed by test utilities
- Flaky retries added instead of fixing the race
- Coverage increased with assertions that prove nothing
- A test command that silently selects zero tests

Always inspect test changes with the same care as production code.

## Use independent oracles where possible

The implementation and the test can share the same mistaken assumption, especially when one model generated both.

Use an independent source of truth:

- Product acceptance criteria
- A protocol or standards document
- Existing contract tests
- A known-good implementation
- Golden fixtures reviewed by a domain expert
- Invariants or property-based tests
- Differential tests against the previous version
- A second reviewer who did not inherit the implementation chat

For critical logic, consider mutation testing or deliberate fault injection to see whether tests detect meaningful errors.

## Verify non-code effects

Tests may pass while the change still fails operationally. Check relevant effects:

- Database migrations are forward-compatible and rehearsed on realistic data.
- Rollback behavior is defined where rollback is possible.
- Configuration defaults are safe.
- Feature flags work in both states.
- Logs contain useful context without secrets or personal data.
- Metrics and alerts cover the new failure mode.
- Documentation and examples match the shipped interface.
- Performance and resource use remain within budgets.

## When checks cannot run

Do not substitute confidence for a missing environment.

The handoff should say:

- Which checks were not run
- Why they were unavailable
- What partial evidence exists
- The exact command or manual workflow the next person should run
- What risk remains until that happens

An honest “not verified” is more useful than a fictional pass.

## Key takeaway

AI writes candidates. Tests, tools, and observable behavior establish correctness.
