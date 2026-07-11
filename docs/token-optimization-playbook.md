# Token Optimization Playbook

Token efficiency is not the goal by itself. The goal is more correct, useful, reviewable work within a budget, credit pool, or subscription limit.

> Optimize cost per verified outcome, not tokens per prompt.

This page covers durable cross-tool principles. Product details live in the dated guides:

- [Claude Code Optimization](claude-code-optimization.md)
- [Codex Optimization](codex-optimization.md)

## Know which meter you are using

Different products expose different constraints:

- **API usage:** input, cached input, output, and tool charges
- **Credits:** provider-specific units derived from model and feature use
- **Subscription allowance:** rolling session windows, weekly or other limits, and fair-use policies
- **Latency:** time spent waiting for long reasoning, tools, or parallel agents
- **Human cost:** prompt preparation, review, rework, and incident risk

A subscription may fix the monthly bill while still limiting access. Credits may let you continue after included use. Exact schedules and accounting change, so use the provider's live usage dashboard rather than an old blog number.

## What commonly drives usage

- Model capability and reasoning effort
- Conversation history and attached files
- Always-on instructions and tool definitions
- Large command, log, search, and tool results
- Output length
- Agent iterations and retries
- Subagents and parallel workers
- Repeated discovery caused by weak task handoffs
- Rework after an incorrect broad implementation

Prompt caching and deferred tool loading can reduce repeated processing, but they do not make irrelevant context or noisy output harmless.

## The high-leverage playbook

### 1. Define success before spending context

State goal, expected behavior, acceptance criteria, constraints, non-goals, and verification. A focused task packet prevents expensive fix-up loops.

Template: [Task Brief](../templates/task-brief-template.md)

### 2. Retrieve context progressively

Start with an entry point and let the assistant inspect required callers, tests, contracts, and configuration. Do not dump the whole repository, but do not hide required context behind arbitrary file limits.

See [Context Discipline](context-discipline.md).

### 3. Route by measured difficulty and risk

Use a fast, economical model and lower effort for tasks it solves reliably. Escalate for genuinely difficult reasoning, not because the first prompt omitted requirements.

High-risk work needs layered verification and human review regardless of model.

See [Model Selection](model-selection.md).

### 4. Keep sessions coherent

Use one objective per task. Start fresh when old decisions and logs are no longer useful. Compact at a coherent milestone when continuity matters, and preserve exact state because summaries are lossy.

### 5. Filter tool output early

Choose focused tests, fields, date ranges, and failure summaries before output reaches the model. Preserve the command, exit status, and enough surrounding context to diagnose the failure.

Deterministic filtering is usually cheaper than asking another model to read thousands of irrelevant lines.

### 6. Keep always-on context valuable

Project instruction files should contain durable commands, invariants, boundaries, and verification expectations. Put specialized runbooks in on-demand guides or skills. Disable unused tool servers.

See [Project Context Files](project-context-files.md) and [MCP and Tool Overhead](mcp-and-tool-overhead.md).

### 7. Plan in proportion to risk

- Clear, local, reversible: implement and verify directly.
- Ambiguous or multi-component: inspect and produce a short evidence-backed plan.
- Destructive, external, security-sensitive, or migratory: plan stages, rollback, approvals, and verification explicitly.

A plan is valuable when it prevents wrong work. It is overhead when it merely restates an obvious edit.

### 8. Make one small coherent change

Avoid huge “improve everything” diffs. Separate mechanical edits, behavior changes, and migrations. Small changes reduce context, review time, and rework while making tests more diagnostic.

### 9. Ask for concise handoffs, not duplicate artifacts

When an agent edits files, review the real diff. Ask the final response for:

- What changed and why
- Exact verification results
- Skipped checks
- Remaining risks

Do not pay for a full-file rewrite or a second unified diff when the workspace already contains the authoritative change.

### 10. Use subagents deliberately

Subagents can protect parent context, specialize a pass, provide independent review, or reduce wall-clock time. They perform additional model work.

Measure total use, assign bounded scopes, and avoid parallelizing dependent tasks.

### 11. Stop failing loops early

After a failed attempt, identify whether the issue is:

- Missing or stale context
- Ambiguous requirements
- Insufficient reasoning
- Missing tool or environment
- Wrong hypothesis
- A product decision the model cannot make

Change the input or approach. Repeating “try again” burns usage without adding evidence.

### 12. Verify before declaring efficiency

A short answer that creates a defect is not efficient. Include compilation, focused tests, static checks, broader behavior checks, and independent review based on risk.

See [Verification and Testing](verification-and-testing.md).

## A useful optimization order

When a workflow uses too much capacity:

1. Remove rework by improving the task contract and acceptance criteria.
2. Filter large tool output.
3. Remove stale conversation context.
4. Reduce always-on instructions and unused tools.
5. Right-size model and reasoning.
6. Constrain output length.
7. Reconsider parallel agents and repeated review loops.
8. Automate deterministic recurring work outside the model.

This order protects quality while attacking common waste.

## Measure the whole outcome

Track:

- Correctness and hidden-check pass rate
- Human review and rework time
- Wall-clock lead time
- Total usage or credits across all agents
- Escaped defects and rollback
- Diff size and scope violations
- Variance across repeated runs

See [Cost Observability](cost-observability.md) and [Benchmarking AI Coding Tools](../benchmarks/README.md).

## Key takeaway

The best token optimization is avoiding wrong work. Give the model a verifiable task, keep context and tools focused, and stop when new evidence—not more generation—is needed.
