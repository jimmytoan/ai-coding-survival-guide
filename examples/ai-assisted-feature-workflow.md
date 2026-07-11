# Example: A Reviewable AI-Assisted Feature Workflow

This example shows how to keep one feature grounded from request to handoff. Use it
with the [task brief](../templates/task-brief-template.md),
[implementation plan](../templates/implementation-plan-prompt.md),
[scoped implementation](../templates/scoped-implementation-prompt.md),
[verification](../templates/verification-prompt.md), and
[handoff](../templates/handoff-template.md) templates. The feature is a
failed-login lockout for an existing application.

## 1. Brief the outcome

```text
Task:
Temporarily lock an account after repeated failed login attempts.

Acceptance criteria:
- Five failed attempts within 15 minutes lock the account for 30 minutes.
- A successful login clears the failure counter.
- The response must not reveal whether an account exists.
- Lock and unlock events are auditable without logging credentials.

In scope:
- Existing password-login path and its tests

Out of scope:
- MFA, administrator unlock UI, and a new notification service

Constraints:
- Reuse the existing cache and audit-event abstractions.
- Do not add a dependency or change the public error response.

Verification:
- Unit tests with a controllable clock
- Existing authentication integration suite
- Manual check that logs contain no password or raw token
```

The acceptance criteria describe behavior. They do not prematurely prescribe a
database table, cache key, or class hierarchy.

## 2. Ask for a plan, then inspect it

```text
Create a plan only. Trace the current login and audit paths first.

For each step, name the likely files, behavior change, failure modes, and tests.
Call out race conditions, cache outages, account-enumeration risks, and clock
handling. Prefer existing abstractions and list any assumption you cannot confirm.
```

Before implementation, resolve material assumptions such as whether failed
attempts are counted per account, per IP address, or both.

## 3. Implement one checkpoint at a time

```text
Implement only the failure-counter behavior and unit tests from the approved plan.
Do not add the lockout response or audit event yet. Modify only the named files,
run the focused tests, and report the exact command and result.
```

Review the diff before authorizing the next step. A checkpoint makes a wrong
assumption cheap to undo.

## 4. Verify independently

After implementation, start from the original brief rather than the assistant's
summary.

```text
Do not edit files. Map every acceptance criterion to code and test evidence.
Run the focused and integration checks. Try boundary cases at 4 and 5 failures,
expiry at exactly 30 minutes, a successful login, concurrent failures, and a cache
error. Mark any claim not directly demonstrated as not verified.
```

## 5. Hand off facts, not the transcript

```text
Outcome:
Lockout behavior is implemented behind the existing auth feature flag.

Verification performed:
- npm test -- auth-lockout.test.ts: 14 passed
- npm run test:integration -- auth: 38 passed

Not verified:
- Production cache failover; staging access was unavailable.

Known risk:
- Simultaneous failures rely on the cache increment being atomic. Review the
  adapter implementation in src/cache before enabling the flag.

Remaining work:
1. Verify cache failover in staging.
2. Enable the feature for internal accounts and monitor lockout/error metrics.
```

The handoff preserves decisions, evidence, risks, and the next action while
discarding conversational noise.
