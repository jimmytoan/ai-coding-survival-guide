# Debugging with AI

AI is useful for generating hypotheses and navigating unfamiliar code. It is less reliable when asked to guess a root cause from one error message and immediately rewrite several files.

Use an evidence-driven loop:

> Reproduce → observe → hypothesize → discriminate → fix → regress

## Start with a reproducible problem

Capture:

- Exact expected and actual behavior
- Minimal reproduction steps or input
- Environment, version, and configuration
- Error message and the relevant part of the stack trace
- Frequency and timing
- Last known good version or recent changes
- What has already been tried

Redact secrets and personal data before sharing logs.

Bad:

```text
Login is broken. Fix it.
```

Better:

```text
On app 2.7.1 in staging, a user with an expired access token and valid
refresh token receives 401 from GET /account. The refresh request succeeds,
but the retried request still sends the old Authorization header.

Reproduction:
1. Sign in.
2. Expire only the access token.
3. Open /account.

Expected: one refresh, then a successful retry.
Actual: refresh succeeds, retry returns 401.

First inspect src/http/client.ts and its tests. Do not edit yet.
```

## Separate facts from hypotheses

Ask for a small evidence table:

| Item | Status | Evidence |
|---|---|---|
| Refresh endpoint succeeds | Fact | 200 response in trace |
| Retry uses stale closure | Hypothesis | Header created before refresh |
| Server rejects new token | Hypothesis | Not yet tested |

This prevents a plausible theory from quietly becoming an assumed fact.

## Rank hypotheses and choose discriminating tests

A useful hypothesis includes:

- Why it fits the evidence
- What evidence contradicts it
- One cheap observation that would distinguish it from alternatives

Prompt:

```text
List at most five root-cause hypotheses, ranked by likelihood.
For each, cite the evidence, note contradictory evidence, and propose the
smallest experiment that would confirm or reject it. Do not change code yet.
```

Prefer an experiment that splits the search space over more logging “just in case.”

## Trace the real execution path

Have the assistant identify:

- Entry point
- State transitions
- Callers and callees
- Data transformations
- Async boundaries, caches, and retries
- Error handling and fallback paths
- Configuration or feature flags

Ask it to cite files and symbols. Verify those references before acting; models can invent a call path that sounds coherent.

## Change one variable at a time

Avoid shotgun debugging: three speculative edits, a dependency upgrade, and a refactor in one attempt.

Instead:

1. Add the smallest observation or failing test.
2. Run it.
3. Update the hypothesis ranking.
4. Make one causal change.
5. Run the reproduction again.

This produces information even when the first hypothesis is wrong.

## Fix the cause, not the visible symptom

Be suspicious of changes that:

- Add arbitrary retries or delays
- Catch and ignore an exception
- Disable validation
- Increase a timeout without evidence
- Duplicate state to “keep it in sync”
- Add a null check far from where invalid state begins
- Replace a deterministic failure with silent fallback behavior

Ask: what invariant was violated, where should it have been enforced, and why did existing tests miss it?

## Turn the diagnosis into a regression test

A good debugging outcome includes:

- A test or deterministic reproduction that failed before the fix
- The smallest change that addresses the cause
- Nearby cases showing preserved behavior
- Removal of temporary logs, flags, and debug code
- An explanation of why the fix works

For intermittent issues, preserve useful observability rather than a flaky test that sleeps and hopes.

## Know when to reset

Pause and reconsider when:

- Several fixes fail without changing the evidence
- The assistant repeats the same theory in different words
- The reproduction is not stable
- Logs come from a different build or environment
- A dependency, generated artifact, or configuration is being assumed rather than checked
- The proposed change keeps expanding in scope

At that point, start a clean investigation summary containing facts, rejected hypotheses, open questions, and the next experiment.

## Reusable debugging prompt

```text
Diagnose before editing.

Expected behavior:
[What should happen]

Actual behavior:
[What happens]

Reproduction:
[Minimal steps or input]

Environment:
[Version, configuration, recent changes]

Evidence:
[Relevant error, stack trace, or logs]

Return:
1. Confirmed facts
2. Ranked hypotheses
3. Evidence for and against each
4. The smallest next experiment
5. Files likely involved and why

Do not propose a fix until the evidence supports a root cause.
```

## Key takeaway

Use AI to make debugging more scientific, not merely more verbose.
