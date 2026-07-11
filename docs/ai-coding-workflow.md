# A Reliable AI Coding Workflow

Good AI-assisted development is not one clever prompt. It is a controlled loop that turns an unclear request into a small, verified change.

Use this default:

> Frame → inspect → plan → implement → verify → review → hand off

Skip or compress steps when the risk is low. Add checkpoints when the risk is high.

## 1. Frame the task as a contract

Before asking for code, write down:

- **Goal:** the outcome, in user or system terms
- **Current behavior:** what happens now
- **Expected behavior:** what should happen instead
- **Scope:** files, services, interfaces, or workflows that may change
- **Constraints:** compatibility, performance, security, style, and dependency limits
- **Acceptance criteria:** observable conditions that make the task complete
- **Verification:** commands or manual checks that will prove it
- **Non-goals:** tempting adjacent work that is deliberately excluded

Example:

```text
Goal: Preserve the user's intended destination after sign-in.

Current behavior:
Unauthenticated requests to /billing are redirected to /login and then /home.

Expected behavior:
After successful sign-in, redirect back to /billing. Reject external redirect URLs.

Scope:
- src/auth/require-session.ts
- src/routes/login.ts
- related tests

Constraints:
- No new dependency
- Existing /login links must continue to work
- Redirect targets must remain same-origin

Acceptance criteria:
- A protected local path is restored after login
- Missing or unsafe targets fall back to /home
- Regression tests cover both cases

Non-goals:
- Redesigning the login page
- Changing session storage
```

If you cannot state the expected behavior, the assistant is being asked to invent product requirements.

Template: [Task Brief](../templates/task-brief-template.md)

## 2. Inspect before editing

Ask the assistant to find evidence in the repository:

- Applicable project instructions
- The entry point and call path
- Existing tests for the behavior
- Similar implementations elsewhere
- Public interfaces and downstream callers
- Relevant configuration, migrations, or generated files
- Recent local changes that must not be overwritten

Useful prompt:

```text
Inspect the repository for this task before editing.

Return:
- Applicable repository instructions
- Relevant files and why each matters
- Current behavior with code evidence
- Existing tests and gaps
- Risks or unanswered questions

Do not modify files yet.
```

The goal is not a complete repository tour. It is enough evidence to avoid guessing.

## 3. Match the plan to the risk

| Risk | Examples | Default control |
|---|---|---|
| Low | Docs, formatting, mechanical rename | Edit, run a focused check, review diff |
| Medium | Local bug fix, normal feature, dependency update | Inspect, short plan, implement, targeted tests |
| High | Auth, authorization, payments, migrations, infrastructure, privacy | Explicit plan approval, least privilege, staged changes, independent review |

A useful plan names:

- Files to change
- Behavior to preserve
- Implementation steps
- Verification for each step
- Rollback or migration concerns
- Unknowns that could change the approach

Planning should reduce uncertainty. A long plan for an obvious one-line fix is ceremony; no plan for an irreversible migration is recklessness.

## 4. Implement the smallest coherent change

Ask for one reviewable unit that fully handles its part of the behavior.

Good constraints include:

- Preserve unrelated behavior.
- Do not add dependencies without approval.
- Follow existing local patterns unless they are the cause of the bug.
- Do not edit generated files directly.
- Do not weaken, delete, or skip tests to make checks pass.
- Stop if the required scope expands materially.

Small does not mean incomplete. A correct change may include production code, a regression test, documentation, and a migration. It should not include opportunistic rewrites.

## 5. Verify with evidence

Require exact evidence, not “looks good”:

```text
After implementation, report:
- Commands run
- Result of each command
- Behavior checked manually
- Checks not run and why
- Remaining risks or assumptions
```

Use the narrowest useful check first, then expand based on risk. See [Verification and Testing](verification-and-testing.md).

## 6. Review the result, not the story

Review the actual diff and final files. Check:

- Does it meet every acceptance criterion?
- Is there a simpler correct change?
- Did scope expand?
- Did any public behavior change unintentionally?
- Are error, concurrency, and failure paths handled?
- Are new dependencies or permissions justified?
- Do tests prove behavior rather than mirror the implementation?
- Are logs, docs, migrations, and observability updated where needed?

The assistant's summary is a navigation aid, not proof. See [Reviewing AI-Generated Code](reviewing-ai-generated-code.md).

## 7. Hand off honestly

A useful completion note contains:

- What changed and why
- Files or components affected
- Verification performed
- User-visible or operational impact
- Known limitations and skipped checks
- Follow-up work that is genuinely required

Never hide a failed check in a long success summary. Never claim a check was run when it was inferred, unavailable, or skipped.

Template: [Change Handoff](../templates/handoff-template.md)

## Stop conditions

The assistant should pause rather than improvise when:

- Requirements conflict or acceptance criteria are unknowable
- A destructive or irreversible action is needed
- It needs secrets, production access, or broader permissions
- A new dependency or public API change is required but not approved
- The working tree contains overlapping user changes
- The planned scope is no longer sufficient
- Verification repeatedly fails for an unexplained reason

Stopping at the right boundary is a feature of a trustworthy agent.

## Definition of done

A task is done when:

- The requested behavior exists
- Relevant previous behavior still works
- The diff is scoped and understandable
- Appropriate checks pass
- Security and operational effects were considered
- Documentation or migrations are included when required
- Limitations are explicit
- A human can explain and own the result

“The agent finished” is not a definition of done.

## Key takeaway

Use AI to accelerate a disciplined engineering loop, not to replace one.
