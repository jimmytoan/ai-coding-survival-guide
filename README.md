# AI Coding Survival Guide

## The Max Power Way

Before talking about tokens, watch the spirit of the problem:

https://github.com/user-attachments/assets/21efcbfc-d374-4fb3-91c2-2f0eb92643f7

That is how many of us use AI coding tools today.

We open a giant repo, choose the strongest model, keep a 200-message chat alive forever, ask the assistant to “fix everything,” and then spend more prompts asking it to clean up the mess it just created.

Funny in a cartoon.

Expensive in a token meter.

## Stop using AI the Max Power way

AI coding assistants are not dead.

They are becoming normal engineering tools.

But after GitHub Copilot's move to usage-based billing with AI Credits, many developers suddenly felt the thing that cloud engineers learned years ago:

> If you cannot see the meter, the bill will teach you.

This repo is a practical guide for using AI coding assistants effectively without wasting tokens, credits, or engineering review time.

It is not anti-AI.

It is anti-waste.

It is also anti-this:

> “Here is my whole repo. Rewrite everything.  
> Now fix the duplicated code.  
> Now fix the tests.  
> Now remove the thing you added.  
> Now make it production-ready.”

That is not engineering acceleration.

That is generating faster chaos.

## Why this repo exists

The current debate around GitHub Copilot pricing is emotional because many developers were used to a simple subscription mental model:

> I pay monthly, so I can use it freely.

Usage-based billing changes the mental model:

> Every model call has a cost, especially when the assistant sends large context, uses expensive models, runs agentic workflows, or makes many hidden calls.

That does not mean usage-based pricing is evil.

A tiny autocomplete and a long-running coding agent are not the same workload.

But it does mean developers and teams need better habits:

- Better context control
- Better model selection
- Better task scoping
- Better review discipline
- Better cost observability
- Better team guidelines

Welcome to the beginning of **FinOps for AI coding agents**.

## Are you using AI the Max Power way?

You might be using AI the Max Power way if:

- You always pick the biggest model.
- You ask the assistant to “review the whole repo.”
- You keep using the same chat for five unrelated tasks.
- You use agent mode for tiny edits.
- You never check what context is being sent.
- You ask for implementation before asking for a plan.
- You accept a huge code diff, then ask AI to remove duplicate or redundant code.
- You use AI to generate more code than you are willing to review.
- You only look at token cost after the damage is done.

No judgment.

Most of us have done it.

The goal is to get better.

## The core idea

The best developers will not be the ones who ask AI to generate the most code.

They will be the ones who know:

- What context to give
- What context to remove
- Which model to use
- When to use agent mode
- When to ask for a plan
- What output to trust
- What output to review carefully
- When to stop and do the work manually

AI should reduce engineering waste, not generate faster waste.

## Start here

| Need | Read this |
|---|---|
| Learn how to avoid wasting context | [Context Discipline](docs/context-discipline.md) |
| Compact without surprise token costs | [Compaction and the Prompt Cache](docs/compact-and-caching.md) |
| Stop MCP servers from eating your context | [MCP and Tool Overhead](docs/mcp-and-tool-overhead.md) |
| Keep rules files from taxing every turn | [Project Context Files](docs/project-context-files.md) |
| Decide which model to use | [Model Selection](docs/model-selection.md) |
| Understand when to use agent mode | [Agent Mode](docs/agent-mode.md) |
| Know when subagents save vs. multiply cost | [Subagents and Agent Teams](docs/subagents-and-agent-teams.md) |
| Create lightweight team rules | [Team Guidelines](docs/team-guidelines.md) |
| Track AI coding cost and outcomes | [Cost Observability](docs/cost-observability.md) |
| Compare other tools fairly | [Alternatives](docs/alternatives.md) |
| Copy reusable prompts | [Prompt Patterns](docs/prompt-patterns.md) |
| See good vs bad examples | [Good vs Bad Prompts](examples/good-vs-bad-prompts.md) |
| Benchmark your tools | [Benchmark Template](benchmarks/benchmark-template.csv) |

## Practical tips

### 1. Start a new chat when the context is stale

Long conversations can become expensive and confusing.

Start a new chat when:

- The topic has changed
- The assistant keeps referencing old decisions
- The chat has become slow or unfocused
- You finished one feature and are starting another
- You only need a small answer but the previous context is huge

Simple rule:

> If the old context is no longer helping, it is probably costing you.

More: [Context Discipline](docs/context-discipline.md)

### 2. Compact the conversation

If your tool supports compaction or summarization, use it.

A useful compacted summary should include:

- Current goal
- Important decisions
- Relevant files
- Known constraints
- Open questions
- Next step

Bad summary:

```text
We discussed the project.
```

Good summary:

```text
We are implementing token budget alerts for the AI assistant platform.
Relevant files are usage-tracker.ts, billing-service.ts, and model-router.ts.
Warn users at 70%, warn again at 90%, block non-admin users at 100%.
Admins can override. Usage must be auditable.
Next step: propose the API and database changes before writing code.
```

Timing matters too: compact while the session is still active, not after a long idle break. In Claude Code the prompt cache only survives about an hour of inactivity on a subscription. Compacting a warm session is cheap; compacting a cold one reprocesses the whole history at ~10× the price.

More: [Compaction and the Prompt Cache](docs/compact-and-caching.md)

### 3. Use the right model for the task

Bigger is not always better.

Small and cheaper models are often good enough for:

- Summaries
- Simple explanations
- README drafts
- Basic unit tests
- Small code transformations
- First-pass code review
- Error explanation

Use stronger models for:

- Implementation planning
- Architecture decisions
- Security-sensitive code
- Complex debugging
- Multi-file refactoring
- Production incidents
- Final review before merge

Rule of thumb:

> Use the cheapest model that can reliably solve the task.

More: [Model Selection](docs/model-selection.md)

### 4. Do not dump everything into AI

Experienced developers still use tokens effectively.

They do not blindly send the whole repo and ask for a giant implementation.

A better workflow:

1. Understand the problem first.
2. Identify the relevant files.
3. Ask AI to explain or validate the approach.
4. Ask for an implementation plan before code.
5. Make smaller, reviewable changes.
6. Use AI to review the diff.
7. Keep the human responsible for architecture, trade-offs, and final quality.

A senior developer does not use AI as a code dumping ground.

A senior developer uses AI as a thinking partner, reviewer, and focused implementer.

### 5. Be specific about files

Bad:

```text
Review this repo and suggest improvements.
```

Better:

```text
Review only src/auth/session.ts and src/api/login.ts.

Goal:
Find why users are logged out after token refresh.

Do not edit files yet.
First explain the likely cause.
```

The second prompt gives the assistant a smaller, clearer job.

That usually means better output and lower token usage.

More examples: [Good vs Bad Prompts](examples/good-vs-bad-prompts.md)

### 6. Ask for a plan before implementation

Before letting AI write code, ask for a plan.

```text
Create an implementation plan before writing code.

Goal:
[Describe the goal]

Output:
- Files likely affected
- Data model changes
- API changes
- UI changes
- Test plan
- Risks
- Questions before implementation

Do not write code yet.
```

Template: [Implementation Plan Prompt](templates/implementation-plan-prompt.md)

### 7. Use agent mode carefully

Agent mode is powerful, but it can perform many hidden steps.

Use it when the task is worth delegation.

Avoid it for tiny tasks where normal chat is enough.

Agent mode should feel like delegation, not gambling.

The same caution applies when you spawn *multiple* agents: a subagent that absorbs noisy output (tests, logs) saves your main context, but running agents in parallel multiplies cost — each one carries its own full context window.

More: [Agent Mode](docs/agent-mode.md) · [Subagents and Agent Teams](docs/subagents-and-agent-teams.md)

### 8. Measure cost per useful outcome

Do not only ask:

> How much did this prompt cost?

Ask:

> Did this save meaningful time?
> Did the code pass review?
> Did it reduce bugs?
> Did it help me understand the system faster?

Sometimes a stronger model is worth it.

Sometimes a small model is enough.

Sometimes manual work is faster.

The goal is not minimum AI usage.

The goal is maximum useful output per token.

More: [Cost Observability](docs/cost-observability.md)

### 9. Mind the context you did not type

Not all context comes from your prompt. Some of it is loaded for you, on every turn:

- **MCP server tool definitions.** A few popular servers can eat 50K+ tokens before you type anything. Connect only what you need, prefer a CLI (`gh`, `aws`, `gcloud`) when one exists, and check what is loaded (`/context` in Claude Code).
- **Project rule files** (CLAUDE.md, `.cursorrules`, `AGENTS.md`). Loaded at session start and carried every turn. Keep them to genuine essentials and move occasional runbooks into on-demand skills or commands.

Anything loaded automatically is paid for repeatedly.

More: [MCP and Tool Overhead](docs/mcp-and-tool-overhead.md) · [Project Context Files](docs/project-context-files.md)

## Recommended team message

You can send this to your engineering team:

```text
Everyone, please be mindful of token usage when using AI coding assistants.

These tools are useful and we should continue using them, but as more providers move toward usage-based pricing, we need to be more disciplined.

A few practical habits:

- Start a new chat when the conversation becomes too large or stale.
- Compact/summarize the conversation when the tool supports it.
- Use the right model for the task.
- Try smaller/cheaper models for simple tasks like summaries, explanations, docs, basic tests, or small code transformations.
- Use stronger models for complex or higher-risk work, such as implementation planning, architecture decisions, security-sensitive logic, or production changes.
- Avoid dumping the whole repo into AI and asking it to generate a large amount of code.
- Ask for a plan first, then implement in smaller reviewable steps.

The goal is not to use less AI.

The goal is to use AI better.
```

More: [Team Guidelines](docs/team-guidelines.md)

## Templates

Reusable prompts:

- [Implementation Plan Prompt](templates/implementation-plan-prompt.md)
- [Code Review Prompt](templates/code-review-prompt.md)
- [Debugging Prompt](templates/debugging-prompt.md)
- [PR Review Checklist](templates/pr-review-checklist.md)
- [AI Coding Policy Template](templates/ai-coding-policy-template.md)
- [AI Usage Retro Template](templates/ai-usage-retro-template.md)

## Benchmark your AI coding tools

Use the benchmark template to compare tools by actual workflow, not vibes:

- Tool
- Model
- Task type
- Context size
- Agent mode
- Cost estimate
- Time saved
- Output accepted
- Review effort
- Notes

Start here: [Benchmark Template](benchmarks/benchmark-template.csv)

## Final thought

The future is not “use less AI.”

The future is:

> Use AI better.

No more Max Power way.

Well…

Maybe sometimes.

But only when the token budget can survive it.
