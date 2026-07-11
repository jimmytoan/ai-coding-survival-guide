# Cost and Outcome Observability

AI coding cost includes provider usage, latency, human review, rework, and risk. Measure the whole task from brief to accepted, verified result.

> A cheap wrong answer is expensive. An expensive call that prevents an incident can be efficient.

## Separate the meters

Record which constraint you are measuring:

- API spend
- Product credits
- Included subscription allowance
- Rate-limit or capacity exhaustion
- Wall-clock latency
- Developer interaction time
- Review and rework time
- Operational impact

Do not convert all of these into a fictional token price. Product plans account for usage differently.

## Record the task context

At minimum:

- Task category and risk level
- Repository commit and environment
- Tool, model, version, and reasoning setting
- Agent, subagent, and external-tool use
- Prompt/context policy
- Expected acceptance and verification criteria
- Whether this is a first attempt or follow-up

Without segmentation, a documentation edit and an authorization migration pollute the same average.

## Measure outcome quality

Useful metrics include:

### Correctness

- Acceptance criteria satisfied
- Public and hidden tests passed
- First-pass success rate
- Severity and count of review findings
- Escaped defects, incidents, and rollbacks

“Output accepted” is weaker than “output correct.” A developer may accept a result under time pressure or discover the defect later.

### Human effort

- Minutes preparing the task and context
- Minutes reviewing the diff
- Minutes correcting the result
- Number of prompt or implementation iterations
- Diff churn between first output and merge
- Time to understand and confidently own the change

### Delivery

- Lead time from task start to verified result
- Wall-clock agent time
- Queue or throttling delay
- Time waiting for CI or environments
- Deployment and post-deploy validation time

### Usage

- Input, cached input, and output where exposed
- Credits or included-limit consumption
- Tool calls and large-result volume
- Number of agents and total worker usage
- Direct API or product cost

### Maintainability and learning

- Complexity and code volume added
- New dependency count
- Documentation and tests added
- Follow-up work created
- Whether the author can explain and operate the change

## Calculate useful ratios carefully

Examples:

```text
verified_task_success_rate = tasks_passing_acceptance_and_checks / tasks_attempted
```

```text
first_pass_success_rate = tasks_passing_before_human_corrections / tasks_attempted
```

```text
total_human_minutes = brief + interaction + review + rework
```

```text
cost_per_verified_task = total_provider_cost / verified_tasks
```

```text
rework_ratio = lines_changed_after_first_candidate / final_lines_changed
```

Use ratios as signals, not performance targets. They can be gamed by choosing easy tasks, skipping tests, or accepting larger defects.

## Compare with a baseline

To know whether AI helped, compare with:

- The same developer or team doing matched tasks without AI
- A previous period with similar task mix
- Another model/tool using the same frozen task packet
- A deterministic script or IDE feature for mechanical work

Control repository commit, environment, inputs, and acceptance checks. Run repeated trials because model output varies.

See [Benchmarking AI Coding Tools](../benchmarks/README.md).

## Use provider diagnostics

Where available, inspect:

- Live context breakdown
- Session and plan usage
- Model and reasoning selection
- Agent and tool attribution
- Provider billing or credit dashboard
- Organization analytics and audit logs

Use the provider's authoritative dashboard for actual billing. Client estimates and token counters can still help find waste inside a workflow.

## Warning signs

- Accepted-output rate is high but escaped defects rise.
- Coding time falls while review time rises more.
- Large diffs need several AI cleanup rounds.
- Strong models are used for every task without better outcomes.
- Subagents reduce latency but total usage is unknown.
- Teams count prompts but not complete tasks.
- Developers omit failed or abandoned sessions.
- Security and production tasks are mixed with low-risk averages.
- No non-AI or alternative-tool baseline exists.

## Run a periodic review

By task type and risk, examine:

- Highest-value workflows
- Highest-rework workflows
- First-pass and verified success
- Review findings and escaped defects
- Model and reasoning distribution
- Agent and tool use
- Total human effort
- Capacity or spend pressure
- Incidents, near misses, and policy exceptions

Change one workflow variable at a time when possible. “Use less AI” and “use the biggest model” are both poor diagnoses.

Template: [AI Usage Retrospective](../templates/ai-usage-retro-template.md)

## Key takeaway

Measure verified outcomes, human effort, and risk alongside provider usage. Optimize the complete engineering system, not the prompt counter.
