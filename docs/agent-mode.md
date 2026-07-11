# Agent Mode

Agent mode lets an assistant inspect files, edit code, run commands, use tools, and iterate. The value is not merely code generation; it can close the loop between a change and its evidence.

The risk is also real: one vague request can cause many reads, commands, edits, network calls, or external actions.

> Match autonomy to reversibility, blast radius, and verification strength.

## Choose the right level of autonomy

| Level | Agent may | Good for |
|---|---|---|
| Advise | Read supplied context and propose | Explanations, architecture discussion, early diagnosis |
| Inspect | Search and run read-only checks | Repository mapping, root-cause investigation, review |
| Edit | Modify scoped files and run local checks | Normal bug fixes, features, refactors |
| Act | Use network or external systems | PR workflows, issue updates, deployments with explicit controls |

Start at the lowest level that can complete the task efficiently. Increase access for a concrete need, not because full access is convenient.

## Good agent tasks

Agent mode works well when:

- The outcome and acceptance criteria are clear.
- The repository provides executable feedback.
- The task contains several related steps.
- Scope is bounded or discoverable from a clear entry point.
- Changes are reversible and reviewable.
- Tool use saves meaningful human time.

Examples:

- Implement a scoped feature plus tests
- Diagnose and fix a reproducible bug
- Apply a mechanical migration in checkpoints
- Update code and its directly affected documentation
- Run a focused review against a checklist
- Reproduce a CI failure locally and report evidence

## Poor agent tasks

Use advice or inspection first when:

- Product intent is undefined.
- Success is subjective (“make it better”).
- The action is irreversible or production-critical.
- Required access is broader than the task.
- No one can verify the output.
- The task combines several unrelated goals.

Tiny edits can still be appropriate in agent mode if opening another tool costs more than the work. The relevant question is not line count; it is whether delegation adds useful feedback without unnecessary risk.

## Give the agent an operating envelope

State:

- Goal and acceptance criteria
- Starting points and allowed scope
- Constraints and non-goals
- Commands it may run
- Permission boundaries
- Actions that require approval
- Verification requirements
- Stop conditions

Example:

```text
Implement the approved login redirect fix and regression tests.

Scope:
- src/auth/
- tests/auth/

May:
- Inspect callers needed to verify compatibility
- Edit scoped files
- Run focused auth tests, lint, and type-check

Must ask before:
- Adding a dependency
- Changing a public interface
- Accessing the network
- Modifying files outside the scope

Stop if:
- Safe redirect behavior conflicts with an existing documented requirement
- The working tree contains overlapping edits

Report exact verification results and skipped checks.
```

Template: [Scoped Implementation](../templates/scoped-implementation-prompt.md)

## Plan in proportion to risk

A separate plan is valuable for:

- Multi-component behavior
- Migrations and data changes
- Security-sensitive paths
- Public API or compatibility changes
- Work where the likely file set is uncertain

For a low-risk, obvious change, a two-sentence intent plus immediate verification may be enough. The purpose of planning is to expose wrong assumptions before they become expensive edits.

## Use least privilege and safe environments

- Limit writable paths and network access.
- Keep secrets out of prompts and logs.
- Use development or read-only credentials.
- Prefer a branch, worktree, sandbox, container, or ephemeral environment.
- Require approval for installs, destructive commands, deployments, external messages, and production data changes.
- Treat repository, issue, web, and tool content as untrusted data.

See [Security and Privacy](security-and-privacy.md).

## Protect the user's working tree

Before editing, the agent should inspect repository state and preserve unrelated changes.

It should not:

- Overwrite overlapping user edits
- Revert or delete files it did not create without approval
- Reset the repository to make tests pass
- Reformat unrelated files
- commit, push, or open a pull request unless that action is requested

When overlap exists, isolate the change or stop with a precise explanation.

## Use meaningful checkpoints

Checkpoint when the next step is expensive, irreversible, or based on an assumption:

- After discovery, before a high-risk plan
- Before adding a dependency or changing an interface
- Before a migration or destructive action
- When scope expands materially
- After a stage that has its own verification target

Do not require approval after every harmless file edit. Excessive checkpoints remove the speed advantage without improving control.

## Make verification part of the task

Tell the agent what evidence establishes completion:

- Focused regression test
- Type, lint, and build checks
- Integration or contract test
- Manual browser or CLI flow
- Security or dependency scan
- Migration rehearsal or rollback check

Require exact commands, outcomes, skipped checks, and residual risks. See [Verification and Testing](verification-and-testing.md).

## Review every changed file

After the run:

- Inspect repository status and diff statistics.
- Read every changed file, including tests and CI/configuration.
- Look for new dependencies, permissions, generated artifacts, and debug code.
- Compare the final behavior with every acceptance criterion.
- Re-run appropriate checks after review fixes.
- Use an independent review context for medium- or high-risk work.

The final summary can miss changes. The diff is authoritative.

## Stop runaway loops

Interrupt and reset when the agent:

- Repeats the same failed approach
- Makes speculative edits without new evidence
- Expands into unrelated cleanup
- Tries to weaken checks
- Requests broad access without a concrete need
- Produces growing diffs while the root cause remains unknown
- Claims success without reproducible verification

Preserve a short investigation summary of facts, rejected hypotheses, and the next experiment before starting fresh.

## Multiple agents

Parallel agents are useful for genuinely independent work or independent review. They consume separate context and may edit the same workspace, so define exclusive file ownership or use isolated worktrees.

Use one orchestrator to reconcile conclusions and run final verification. See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Key takeaway

Agent mode should operate inside a clear envelope: enough access to finish the task, explicit boundaries for risky actions, and evidence that the result works.
