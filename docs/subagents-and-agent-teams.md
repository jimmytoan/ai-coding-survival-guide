# Subagents and Agent Teams

Subagents can isolate context, specialize work, or run independent tasks in parallel. They usually trade more total model work for a cleaner orchestrator context, better specialization, or lower wall-clock time.

> Context isolation is not the same as total token savings.

## Four useful patterns

### 1. Context isolation

A worker reads a large log, test run, documentation set, or repository area and returns a concise evidence-backed summary.

This protects the main conversation from noisy output and may reduce **future orchestrator** context. The worker still has its own input and output cost. If a deterministic filter can reduce the data before any model reads it, that is often cheaper.

Use isolation when the summary will be reused or the raw material would materially degrade the main task.

### 2. Specialization

Assign a bounded role with a distinct threat model or skill:

- Security review
- Database migration analysis
- Accessibility or visual QA
- Test and failure triage
- Current primary-source research

Specialization helps when the worker receives the right evidence and rubric. A role name alone does not create expertise.

### 3. Parallel execution

Run independent work at the same time:

- Separate modules with exclusive ownership
- Independent research questions
- Tests on independent platforms
- Documentation and code changes that do not overlap

Parallelism buys wall-clock speed. Each worker carries its own instructions and context, so total usage generally increases.

### 4. Independent review

Give a fresh worker the original requirement and final diff, without the implementation conversation. This reduces shared-assumption bias and is often more valuable than asking the authoring agent to approve itself.

The reviewer should return evidence-backed candidate findings, not a blanket “looks good.”

## When not to spawn a worker

Keep work in one agent when:

- The task is small and coherent.
- Each step depends on the previous result.
- The spawn context is as large as the work itself.
- Multiple workers would edit the same files.
- A shell filter, parser, search, or test command can do the job deterministically.
- No one will reconcile conflicting conclusions.

Spawning an agent to run one short command and repeat its output is overhead, not delegation.

## Design a good delegation packet

Give each worker:

- One concrete objective
- Relevant inputs and trusted sources
- Exact scope and file ownership
- Expected output format
- Verification or evidence requirements
- Permission boundaries
- Stop conditions
- What not to do

Example:

```text
Review the current auth diff for authorization regressions.

Inputs:
- Original acceptance criteria below
- Current Git diff
- src/auth/policy.ts and direct callers as needed

Return only validated candidate findings with severity, file and line, reachable
scenario, impact, and evidence. Do not edit files. Ignore style-only feedback.
```

Do not copy the entire main conversation by default. Pass the smallest complete task packet.

## Prevent edit collisions

Agents sharing a workspace can overwrite or invalidate one another's work.

Use one of these strategies:

- Assign mutually exclusive files or directories.
- Give only one agent edit rights; others inspect and report.
- Use isolated worktrees or branches, then integrate deliberately.
- Sequence dependent changes.
- Name an orchestrator that owns final reconciliation and verification.

Every worker should preserve pre-existing user changes and avoid repository-wide formatting.

## Choose models by role

- Fast models can handle focused search, classification, formatting, and mechanical work.
- Strong reasoning can help with orchestration, ambiguous diagnosis, and subtle review.
- High-risk decisions still require deterministic checks and qualified human review.
- A cheaper worker that repeatedly fails may cost more than one reliable pass.

Benchmark recurring worker roles just like the main workflow.

## Control fan-out

Before parallelizing, ask:

1. Are the tasks genuinely independent?
2. Is the expected time saving useful?
3. Will workers duplicate repository discovery?
4. Are edit scopes exclusive?
5. Can deterministic tooling replace any worker?
6. Who reconciles results and resolves disagreement?
7. What is the total usage budget?

Start with the smallest team. Add a worker for a clear missing lane, not vague “more coverage.”

## Reconcile and verify centrally

The orchestrator should:

- Check that every assigned task completed.
- Validate material claims and citations.
- Resolve contradictory recommendations.
- Inspect the combined diff for collisions and scope creep.
- Run final checks after all edits are integrated.
- Report skipped verification and residual risk.

Worker summaries are inputs to the final decision, not independent definitions of done.

## Cost language to use accurately

- **Protects main context:** raw worker output does not remain in the orchestrator thread.
- **May lower future orchestrator usage:** later turns carry a short summary.
- **Usually increases total compute:** another model instance did work.
- **Reduces wall-clock time:** independent tasks ran concurrently.
- **Can improve quality:** an independent or specialized pass found issues.

Avoid claiming that subagents inherently save money or tokens without measuring the entire run.

## Key takeaway

Use subagents for isolation, specialization, parallel speed, or independent review. Define ownership, count total work, and verify the integrated result once.
