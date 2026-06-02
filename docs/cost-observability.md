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
