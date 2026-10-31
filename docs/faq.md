# FAQ

## Is this guide anti-AI coding?

No. It is pro-useful AI and anti-unverified output, uncontrolled scope, needless usage, and avoidable review burden.

## What is the default workflow?

Frame the task, inspect evidence, plan in proportion to risk, implement a small coherent change, verify it, review the actual diff, and hand off limitations honestly.

See [A Reliable AI Coding Workflow](ai-coding-workflow.md).

## Should I always ask for a plan?

No.

- For a clear, local, reversible edit: implement and verify directly.
- For an ambiguous or multi-component change: inspect, then make a short evidence-backed plan.
- For a migration, destructive action, external side effect, or high-risk boundary: plan stages, rollback, approvals, and verification explicitly.

## Should I always use the cheapest model?

Use the least costly configuration that meets a measured quality target reliably. Escalate when deeper reasoning is the bottleneck, not when requirements or evidence are missing.

High-risk changes need tests, tools, and qualified human review regardless of model. See [Model Selection](model-selection.md).

## Should I avoid agent mode?

No. Agent mode is valuable when it can inspect, edit, and verify a well-scoped task. Match permissions and checkpoints to reversibility and blast radius.

See [Agent Mode](agent-mode.md).

## Does more context always improve the answer?

No. Necessary evidence helps; stale and irrelevant material competes with it. Start with a complete task packet and retrieve outward through entry points, tests, callers, and contracts.

See [Context Discipline](context-discipline.md).

## Do project instruction files consume tokens?

They consume context capacity and can increase usage. Exact accounting depends on the client, caching, and plan. Keep durable commands and invariants concise, scope specialized guidance close to the work, and enforce mechanical rules in tools.

See [Project Context Files](project-context-files.md).

## Do MCP servers consume tokens when unused?

They can. Some clients load schemas up front; others defer full definitions until selected. Names, discovery, tool calls, and results still have overhead, and permissions remain important.

Measure your client, enable only needed tools, and filter results before model ingestion. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).

## Do subagents save tokens?

They can protect the parent conversation from noisy context and reduce later parent usage, but the worker performs additional model work. Total usage often rises. Parallel workers primarily buy speed; independent workers can buy review diversity.

See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## How do I know generated code is correct?

You do not know from the response alone. Establish a baseline, run deterministic checks, add behavior-oriented tests, inspect every changed file, validate manual or operational behavior where relevant, and report skipped checks.

See [Verification and Testing](verification-and-testing.md).

## Can AI review its own code?

It can find some issues, but an authoring context shares assumptions with its implementation. For meaningful work, use a fresh review context with the original requirement and actual diff. Critical code also needs the appropriate human specialist.

See [Reviewing AI-Generated Code](reviewing-ai-generated-code.md).

## What should I never share with an AI coding tool?

Follow your organization's approved-tool and data-classification policy. Do not paste live secrets. Be careful with personal, regulated, customer, production, confidential, and contract-restricted data. Redact attachments and tool output as well as prompts.

See [Security and Privacy](security-and-privacy.md).

## Can repository text attack an agent?

Yes. Issues, docs, comments, logs, web pages, images, and tool results can contain instructions aimed at the model. Treat retrieved content as untrusted data, use least privilege, and require approval for sensitive actions.

## How do I avoid hallucinated packages and APIs?

Inspect installed versions, verify exact names and signatures in primary sources, review lockfile changes, and run a minimal compile or behavior check. Never install a plausible package name just to see whether it exists.

See [Dependencies and API Research](dependencies-and-api-research.md).

## I have a flat subscription. Why optimize usage?

Subscriptions can still have session, weekly, fair-use, or feature limits. Waste may appear as throttling or reduced access rather than an invoice. Model, context, tools, and output also affect latency and review burden.

See the [Token Optimization Playbook](token-optimization-playbook.md), [Claude Code](claude-code-optimization.md), and [Codex](codex-optimization.md) guides.

## Which plan and model should I pick?

Start with the mid-tier model your tool offers and the plan that covers a normal week of your real tasks. Move up a model when an evaluation at higher effort still fails, and move up a plan when you repeatedly hit the weekly limit doing valuable work. The current Claude, Codex, and Antigravity models and plan rules are in [Models and Plan Limits](models-and-plan-limits.md).

## Why did my limit run out faster than usual?

Common causes are a long session that rereads its full history every turn, an expired prompt cache, a top-tier model, background loops or subagents, and many parallel sessions. Check the usage view, start a fresh session for unrelated work, and see [Field Notes](field-notes.md) and the [Token Optimization Playbook](token-optimization-playbook.md).

## Is usage-based billing bad?

Not inherently. A short completion and an autonomous multi-step task use different resources. What matters is visibility, budgets, controls, data terms, and value per verified outcome.

## How should a team measure success?

Measure verified and first-pass success, human review and rework, lead time, escaped defects, total provider usage, and policy exceptions by task type and risk. Do not optimize prompt count or generated lines.

See [Cost and Outcome Observability](cost-observability.md).

## What should I learn as an engineer now?

Read code fast, debug from evidence, design checks that prove behavior, understand data and production systems, and think about security and failure. Syntax recall matters less than it did. See [Engineering Fundamentals](engineering-fundamentals.md) and [Software Development Tips](software-development-tips.md).

## What is the Max Power way?

It is a joke from *The Simpsons*: doing something the wrong way, but faster. In AI coding it means using powerful automation to create unscoped, unverified changes faster.

## What is the main takeaway?

Use AI to accelerate disciplined engineering. Define success, preserve human ownership, and require evidence before trust.
