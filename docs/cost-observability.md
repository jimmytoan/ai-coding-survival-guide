# Cost Observability for AI Coding

AI coding cost should be measured by useful outcome, not only by prompt count.

A cheap prompt that creates bad code is expensive.

An expensive model call that prevents a production incident may be cheap.

## What to measure

Track:

- Tool used
- Model used
- Task type
- Context size
- Agent mode on/off
- Estimated cost
- Time saved
- Output accepted
- Review effort
- Bugs introduced
- Rework required
- Notes

Use: [Benchmark Template](../benchmarks/benchmark-template.csv)

## See it before you guess

Measuring outcomes is the goal, but most tools also expose the raw numbers — look at them instead of guessing:

- **Session usage** — many CLIs show token counts and an estimated cost for the current session (in Claude Code, `/usage`). Treat the dollar figure as an estimate computed locally, not a bill.
- **What is in your context right now** — a context breakdown (`/context` in Claude Code) shows how much of the window is going to tool definitions, project files, history, and so on. This is where you catch a bloated MCP server or rules file. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).
- **Per-feature attribution** — newer tooling attributes usage to skills, subagents, plugins, and individual MCP servers, so you can tell whether an agent team or a connected server was actually worth it.
- **Authoritative billing** — for real spend on usage-based plans, check the provider's console/usage page, not the local estimate.

For teams: provider consoles support workspace spend limits and per-developer cost reporting. If you route through an LLM gateway, it sees every request and is a natural place to track spend centrally.

## Useful metrics

### Cost per accepted task

```text
total_ai_cost / accepted_outputs
```

This is better than cost per prompt.

### Time saved per dollar

```text
estimated_minutes_saved / ai_cost
```

Useful, but subjective.

### Review burden

Track review effort:

- Low
- Medium
- High

If AI saves coding time but creates high review burden, the net benefit may be low.

### Rework rate

Track how often AI output requires major changes.

High rework rate may mean:

- Wrong model
- Too much context
- Too little context
- Poor task definition
- Overuse of agent mode
- Asking for code before plan

## Warning signs

You may have poor cost observability if:

- Nobody knows which model they are using.
- Developers cannot see approximate usage.
- Agent mode is used without clear scope.
- Large context scans happen silently.
- Teams only discuss cost after budget is exhausted.
- AI-generated diffs are large and difficult to review.
- The same issue requires many “fix this” follow-up prompts.

## Better questions

Instead of asking:

> Which AI tool is cheapest?

Ask:

- Which tool solves our tasks with the least rework?
- Which model gives the best cost/quality balance?
- Which workflows should use small models?
- Which workflows justify strong models?
- Where do we need human approval?
- Which agent tasks are worth the cost?

## Suggested weekly review

For teams adopting AI coding tools, review weekly:

- Top AI use cases
- Most expensive workflows
- Most useful workflows
- Tasks with high rework
- Model usage distribution
- Agent mode usage
- Incidents or near misses
- Policy changes needed

## Key takeaway

AI coding cost is not just a finance problem.

It is an engineering productivity signal.
