# Prompt Patterns

Good coding prompts do not need magic words. They need a clear task, the right evidence, explicit boundaries, and a verifiable finish.

Use this shape:

```text
Goal:
[Observable outcome]

Current and expected behavior:
[What happens now; what should happen]

Scope and context:
[Entry points, files, issue, or reproduction]

Constraints and non-goals:
[Compatibility, dependencies, risk boundaries]

Acceptance criteria:
[Conditions that establish completion]

Verification:
[Commands or manual checks]

Action:
[Explain, diagnose, plan, implement, or review]
```

Not every prompt needs every heading. The fields matter more than the format.

## 1. Inspect and explain

```text
Inspect this code path before suggesting changes.

Goal:
Understand how a request becomes a persisted order.

Start at:
- src/routes/orders.ts:createOrder

Return:
- Call path with files and symbols
- Inputs, outputs, and state changes
- Transaction and error boundaries
- Important invariants
- Existing tests
- Uncertainties that require more evidence

Cite the code for each important claim. Do not edit files.
```

## 2. Plan a change

```text
Create an implementation plan. Do not edit yet.

Goal:
[Outcome]

Acceptance criteria:
[Observable conditions]

Constraints:
[Compatibility, security, performance, dependencies]

Return:
1. Relevant files and current behavior
2. Proposed steps and why
3. Behavior that must remain unchanged
4. Test and verification plan
5. Migration, rollout, or rollback concerns
6. Risks and unanswered questions

Keep the plan proportional to the task.
```

Template: [Implementation Plan](../templates/implementation-plan-prompt.md)

## 3. Implement a scoped step

```text
Implement the approved step below.

Step:
[One coherent change]

Allowed scope:
[Files or component]

Constraints:
- Preserve unrelated behavior.
- Do not add dependencies or change a public interface without approval.
- Do not weaken, delete, or skip tests.
- Follow existing repository instructions.
- Stop if the necessary scope expands materially.

Verification:
[Focused commands, then broader checks based on risk]

At the end, report changed files, exact checks and results, skipped checks,
and remaining assumptions.
```

Template: [Scoped Implementation](../templates/scoped-implementation-prompt.md)

## 4. Diagnose a bug

```text
Diagnose before editing.

Expected behavior:
[Expected]

Actual behavior:
[Actual]

Reproduction and environment:
[Minimal steps, version, configuration]

Evidence:
[Error, focused logs, stack trace]

Return:
- Confirmed facts
- Up to five hypotheses ranked by likelihood
- Evidence for and against each
- One discriminating experiment per hypothesis
- The cheapest next experiment

Do not propose a fix until evidence supports a root cause.
```

Template: [Debugging Prompt](../templates/debugging-prompt.md)

## 5. Generate a regression test

```text
Add a regression test for this behavior before changing production code.

Bug:
[Externally visible behavior]

Test requirements:
- Exercise the public path, not a new internal helper.
- Fail against the current bug for the expected reason.
- Assert the intended outcome and important side effects.
- Include the nearest preserved-behavior case.
- Do not over-mock the behavior under test.

Run the focused test and report its exact failure.
```

## 6. Verify a change

```text
Verify the current change independently of its implementation summary.

Acceptance criteria:
[Criteria]

Use the verification ladder appropriate to the risk:
- Inspect diff
- Compile/type-check/lint
- Targeted regression tests
- Integration or broader tests
- Manual or specialized checks

Return each command and result, checks not run and why, acceptance criteria
not established, and remaining risk. Do not infer a pass from reading code.
```

Template: [Verification Prompt](../templates/verification-prompt.md)

## 7. Review a diff

```text
Review the current diff against the requirement below.

Requirement and acceptance criteria:
[Paste or link]

Review for:
- Wrong or incomplete behavior
- Regressions and compatibility
- Error, concurrency, and data-integrity paths
- Security and privacy boundaries
- Suspicious dependencies or invented APIs
- Weakened tests or missing verification
- Scope creep and unnecessary abstraction

For each finding include severity, file and line, reachable scenario, impact,
evidence, and a minimal correction. Omit style-only preferences.
```

Template: [Code Review Prompt](../templates/code-review-prompt.md)

## 8. Research an unstable API or tool

```text
Research this question using current primary documentation.

Question:
[Specific API, setting, limit, or behavior]

Environment:
[Installed version, plan, platform]

Requirements:
- Prefer official docs, source, schema, or release notes.
- Separate documented facts from inference.
- Cite the exact page for material claims.
- Note version, date, preview status, and uncertainty.
- Do not use a community post when primary documentation resolves the claim.

Return the smallest recommendation that fits this environment.
```

## 9. Evaluate a dependency

```text
Evaluate whether we should add [package] for [need]. Do not install it yet.

Check:
- Exact package and authoritative registry entry
- Existing standard-library or repository alternative
- Maintenance and ownership signals
- License and known advisories
- Transitive or install-script risk
- Bundle/runtime impact
- Removal and migration cost

Recommend add, reuse existing, implement locally, or investigate further.
Cite current primary sources for package facts.
```

Template: [Dependency Evaluation](../templates/dependency-evaluation-prompt.md)

## 10. Plan a migration or refactor

```text
Plan a behavior-preserving migration from [current] to [target].

Return:
- Inventory of callers, data, and compatibility constraints
- Invariants to protect
- Staged sequence with independently verifiable checkpoints
- Dual-read/write or compatibility period if needed
- Backfill and rollback strategy
- Tests and observability for each stage
- Cleanup conditions

Do not combine mechanical migration, behavior changes, and unrelated cleanup
unless they cannot be separated safely.
```

Template: [Migration Plan](../templates/migration-plan-prompt.md)

## 11. Threat-model a change

```text
Threat-model this feature before implementation.

Feature and data flow:
[Description]

Identify:
- Assets and trust boundaries
- Actors and attacker-controlled inputs
- Authentication and authorization decisions
- Abuse cases and failure modes
- Required mitigations and security acceptance criteria
- Tests and monitoring that would detect failure

Prioritize reachable risks. Do not return only a generic checklist.
```

Template: [AI Agent Security Preflight](../templates/security-preflight-template.md)

## 12. Create a handoff

```text
Prepare a factual handoff for the current task.

Include:
- Goal and outcome
- Changed files and behavior
- Important decisions and why
- Exact verification performed and results
- Checks not run and why
- Known limitations or risks
- Required next action, if any

Separate confirmed facts from assumptions. Do not claim completion when an
acceptance criterion remains unverified.
```

Template: [Handoff](../templates/handoff-template.md)

## Patterns to avoid

| Weak prompt | Why it fails | Better constraint |
|---|---|---|
| “Fix everything” | No finish line or review boundary | One outcome with acceptance criteria |
| “Make it production-ready” | Undefined quality target | Name reliability, security, and operational requirements |
| “Use best practices” | Invites generic or fashionable changes | Cite repository constraints and desired properties |
| “Do not ask questions” | Encourages invented requirements | Ask only when an unknown materially changes the result |
| “Only read this file” | Can hide required callers and contracts | Start here; justify material expansion |
| “Make tests pass” | Can reward weakening tests | Preserve assertions; fix root cause; report baseline |
| “Review your own work” | Shares the implementation's assumptions | Use fresh context and the original requirement |

## Key takeaway

A strong prompt defines the outcome, gives a trustworthy starting point, constrains risky behavior, and demands reproducible evidence.
