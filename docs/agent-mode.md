# Agent Mode

Agent mode is powerful.

It can inspect files, make changes, run commands, and iterate.

It can also burn more tokens because one user request may trigger many internal steps.

Use it like delegation, not gambling.

## When agent mode is useful

Use agent mode for:

- Multi-file changes
- Repetitive edits
- Test generation across modules
- Clear scoped implementation tasks
- Simple migrations
- Documentation updates across many files
- Refactors with a clear plan
- Tasks where saved time is worth the cost

Good prompt:

```text
Implement the approved plan.

Scope:
- Only update files listed in the plan.
- Do not introduce new dependencies.
- Keep changes small and reviewable.
- Run tests if available.

Stop and ask if you need to touch additional files.
```

## When agent mode is overkill

Avoid agent mode for:

- Simple explanations
- One-line changes
- Small questions
- README edits
- Basic error explanation
- “What does this function do?”
- “Suggest a better variable name”
- “Write a small helper function”

Normal chat is often enough.

## Ask for a plan first

Before agent mode writes code, ask for a plan.

```text
Create a plan first.

Do not edit files yet.

Output:
- Files likely affected
- Proposed changes
- Risks
- Test plan
- Questions
```

Only approve implementation after the plan makes sense.

## Limit scope

Bad:

```text
Fix the whole payment module.
```

Better:

```text
Implement only the validation cleanup in:
- src/payment/validate.ts
- src/payment/errors.ts

Do not modify API behavior.
Do not update unrelated files.
Stop if you find a larger design issue.
```

## Use checkpoints

For larger tasks, ask the agent to pause:

```text
After each file change, summarize:
- What changed
- Why it changed
- What tests should cover it

Do not continue to the next file until I approve.
```

## Review the diff

Agent mode does not remove your responsibility.

After agent work, review:

- Files touched
- Unexpected changes
- New dependencies
- Duplicated logic
- Deleted behavior
- Tests
- Security impact
- Error handling
- Observability/logging

Template: [PR Review Checklist](../templates/pr-review-checklist.md)

## Use plan mode before expensive changes

Many tools have a dedicated plan mode (in Claude Code, Shift+Tab) where the agent explores the codebase and proposes an approach for your approval **before** it edits anything.

This is a token saver, not just a safety feature: approving a plan is far cheaper than letting the agent take an expensive wrong path and then paying again to unwind it.

If the agent starts heading the wrong direction, stop it early (Escape) and use `/rewind` or a checkpoint rather than letting the mistake compound.

## Spawning multiple agents

One agent doing a delegated job is the simple case. Spawning *several* agents is a different cost trade:

- A subagent that **absorbs noisy output** (running tests, reading logs, fetching docs) keeps that noise out of your main context — it saves tokens.
- Running agents **in parallel** multiplies cost — each one carries its own full context window — but buys wall-clock speed.

Know which one you are doing before you launch it. See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Key takeaway

Agent mode should feel like controlled delegation.

Not “please go wild in my repo.”
