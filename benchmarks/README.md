# Benchmarks

Use this folder to compare AI coding workflows on representative work. Benchmark
accepted outcomes and total human effort, not demo quality or vibes.

Start with:

- [Task template](task-template.md) for a reproducible task packet and rubric
- [Results template](benchmark-template.csv) for one row per run

The CSV includes one clearly labeled illustrative row; delete or replace it before
recording real results.

## 1. Choose representative tasks

Use several tasks from the work your team actually performs:

1. Explain a bug in a small function.
2. Add tests for an existing behavior contract.
3. Debug an intermittent or cross-file failure.
4. Implement a small feature from acceptance criteria.
5. Review a pull request with seeded defects.
6. Refactor a two-file module without behavior changes.
7. Plan a migration with compatibility and rollback constraints.
8. Update documentation from a code diff.

Include low-, medium-, and high-complexity work. Do not generalize from one task.

## 2. Freeze the task packet

For every tool, keep these inputs stable:

- Repository revision and environment
- Prompt, visible acceptance criteria, and supplied artifacts
- Time limit, allowed tools, permissions, and network access
- Starting session state and context strategy
- Model settings as closely as the products allow

Keep evaluator-only tests or seeded defects separate from the prompt. Start each
run from a clean checkout and fresh conversation.

## 3. Repeat fairly

- Run each tool-model-task combination at least three times.
- Randomize run order to reduce learning and fatigue effects.
- Do not rescue one tool with extra hints unless that becomes a separately named
  workflow.
- Record timeouts, refusals, broken environments, and abandoned runs instead of
  deleting them.
- When possible, have an evaluator review the output without knowing the tool.

Model and product versions change. Record the date and exact model label so results
remain interpretable.

## 4. Score direct evidence

First record objective outcomes:

- Acceptance criteria met
- Automated and manual checks passed
- Defects, regressions, and scope violations
- First-pass acceptance
- Exact commands or evaluator tests used

Then assign a `quality_score_0_10` with this common rubric:

| Area | Points | Question |
|---|---:|---|
| Functional correctness | 0–4 | Does it satisfy the criteria and evaluator tests? |
| Scope discipline | 0–2 | Is the diff focused and free of unrelated changes? |
| Maintainability | 0–2 | Is it clear, consistent, and no more complex than needed? |
| Verification | 0–1 | Are important success and failure paths demonstrated? |
| Handoff quality | 0–1 | Are decisions, evidence, and limitations reported accurately? |

A severe security, privacy, or data-integrity defect should fail the run regardless
of the numerical score.

## 5. Count the whole workflow

Track separately:

- Wall-clock duration
- Human prompting and steering time
- Human review time
- Human rework time
- Input, cached-input, and output tokens when exposed
- Credits, quota, or other product usage units
- Direct estimated or billed cost

If a comparable human-only baseline exists:

```text
time saved = baseline human time - (interaction + review + rework time)
```

Report the median, range, and failure rate across repeated runs. A fast first draft
that creates long review or rework is not a productivity win.

## Interpret carefully

Benchmark results describe a specific workflow, model version, repository, and
date. They do not prove that one product is universally better. Re-run the suite
after meaningful model, tool, prompt, or repository changes.
