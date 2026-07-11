# Model Selection

Choose a model by the difficulty, risk, and evidence required—not by habit or brand hierarchy.

> Use the least expensive and fastest configuration that meets your quality target reliably.

That is an evaluation question, not a slogan. A cheap model with high rework is expensive; a strong model used for deterministic formatting is wasteful.

## Consider four dimensions

### Task difficulty

- Is the change mechanical or does it require discovering behavior?
- Is it local or spread across services and data flows?
- Are requirements complete or ambiguous?
- Does it require long-horizon planning, debugging, or trade-offs?

### Cost of error

- Can a mistake be caught by a fast deterministic check?
- Is the change reversible?
- Does it affect authentication, money, privacy, production data, or infrastructure?
- How large is the blast radius?

### Context and tools

- How much relevant context must the model connect?
- Does the task require vision, browser use, external documentation, or tool calls?
- Can retrieval and decomposition make the task simpler?

### Latency and budget

- Is interactive speed important?
- Are subscription limits, credits, or API cost constrained?
- Would parallel work save enough wall-clock time to justify extra use?

## A practical routing table

| Task | Start with | Escalate when |
|---|---|---|
| Formatting, comments, simple transforms | Fast / economical model, low reasoning | Output violates deterministic rules |
| Code explanation and repository search | Fast / balanced model | Call path is complex or evidence conflicts |
| Test scaffolding | Fast / balanced model | Domain behavior or concurrency is subtle |
| Small scoped implementation | Balanced coding model | Repeated failures, unclear interfaces, high risk |
| Bug investigation | Balanced / strong reasoning | Evidence spans components or hypotheses remain ambiguous |
| Architecture and migration planning | Strong reasoning | Add domain or security specialists; do not rely on model alone |
| Security-sensitive change | Strong capable model plus security tools | Always require qualified human review |
| Final mechanical cleanup | Fast model or deterministic tooling | Behavior may change |

Model labels and availability change. Benchmark the actual models available in your tool rather than copying a permanent “small vs. large” list.

## Adjust reasoning separately

Where supported, model capability and reasoning effort are separate controls.

Use lower effort for:

- Formatting and conversion
- Known, local edits
- Search, extraction, and summarization
- Applying an already approved mechanical plan

Use more effort for:

- Competing architectural constraints
- Root-cause analysis with conflicting evidence
- Concurrency, state machines, and distributed behavior
- Security and migration planning
- Reviews where subtle errors have high impact

More reasoning is not a substitute for missing context, executable checks, or a domain decision.

## Use an escalation ladder

Do not begin every task at the maximum setting. Also do not repeat a failing cheap attempt indefinitely.

1. Start with the configuration that usually passes this task class.
2. Give it a complete task packet and focused evidence.
3. Run the verification target.
4. If it fails, determine whether the cause is context, requirements, tools, or reasoning.
5. Improve the missing input or decompose the task.
6. Escalate model or effort when deeper reasoning is genuinely the bottleneck.
7. Stop and seek domain input when the model cannot resolve a product or policy question.

Repeatedly saying “try again” with the same inputs is not model routing.

## Decompose before upgrading

A large task may become reliable on a less expensive model when split into:

- Repository mapping
- Behavior diagnosis
- Plan
- One implementation step
- Test generation
- Verification
- Independent review

Decomposition improves observability and reviewability. It can also cost more if every step reloads large context, so group work into coherent units and reuse summaries deliberately.

## Match subagents to their jobs

If your tool supports model choice per subagent:

- Use fast models for search, log filtering, formatting, and mechanical edits.
- Use stronger models for orchestration, ambiguous diagnosis, and cross-cutting review.
- Do not send a low-capability worker to make high-risk decisions merely because it is cheap.
- Do not assume parallel agents save total usage; they usually trade more compute for speed.

See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Evaluate with real tasks

Build a small benchmark set from representative work:

- A local bug with a hidden regression check
- A multi-file feature with explicit acceptance criteria
- A code review with known seeded issues
- A repository-navigation task
- A security-sensitive change or threat-model exercise

Measure:

- Task success and hidden-check pass rate
- Number and severity of defects
- Human review and rework time
- Latency and total usage
- Scope violations and unsupported claims
- Variance across repeated runs

Use at least several runs per configuration; model output is variable. See [Benchmarking AI Coding Tools](../benchmarks/README.md).

## Warning signs

Your routing policy needs work when:

- The strongest model is used for every task.
- A weak model is kept on high-risk work despite poor results.
- Reasoning is always set to maximum.
- Developers optimize token price but ignore review and rework.
- Model names are hard-coded into policy long after the product changes.
- No one can say which tasks each configuration passes reliably.

## Key takeaway

Route by measured reliability and risk. Escalate deliberately, verify every configuration, and keep the human decision boundary where mistakes are expensive.
