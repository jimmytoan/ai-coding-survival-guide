# Subagents and Agent Teams: When Delegation Saves Tokens and When It Multiplies Them

> **TL;DR** Subagents have two very different cost profiles. Used to **isolate verbose work** (running tests, reading logs, fetching docs), a subagent *saves* your main context — the noise stays in the subagent and only a summary comes back. Used to **run many agents in parallel** ("agent teams"), they *multiply* cost — each agent carries its own full context window, so a team can use several times the tokens of a single session (Anthropic reports roughly **7×** when teammates run in plan mode). Same feature, opposite economics. Know which one you are doing.

This is the parallel-and-delegated extension of [Agent Mode](agent-mode.md). Agent mode is about delegating *one* job carefully; this is about what happens when you spawn *more agents*.

## The two faces of subagents

### Face 1 — Isolation: subagents that save context

Some work produces a lot of tokens you do not want in your main conversation: a 10,000-line test run, a noisy log file, a docs crawl. If the main agent does that work, all that output lands in your context and rides along on every later turn.

Delegate it to a subagent instead. The verbose output stays in the **subagent's** context, and only a short summary returns to your main conversation. Your main window stays small and cheap.

Good candidates for isolation:

- Running a test suite and reporting only failures
- Searching or summarizing large log files
- Fetching and condensing external documentation
- Broad codebase exploration where you only need the conclusion

This is a net **win** for token discipline. It is the same instinct as [context discipline](context-discipline.md) — keep the expensive noise out of the conversation you keep paying for.

### Face 2 — Parallelism: agent teams that multiply cost

Running several agents at once is the opposite trade. Each subagent has its **own context window**, and when it starts it loads its own context: instructions, project files (CLAUDE.md), MCP servers, skills, and whatever you put in the spawn prompt. That load costs tokens **every time a new agent spins up.**

So four agents in parallel do not cost the same as one agent — they cost more in total, even though they finish sooner. Anthropic's docs note agent teams use **~7× more tokens** than a standard session when teammates run in plan mode, because each teammate is a separate instance maintaining its own window.

Parallelism buys **wall-clock speed**, not token savings. That can absolutely be worth it — but it is a deliberate trade, not a free one.

## When each is worth it

| Situation | Reach for |
| --- | --- |
| Verbose op pollutes main context | **Isolation subagent** — saves tokens |
| Several genuinely independent tasks, speed matters | **Parallel agents** — costs more, finishes faster |
| Tasks depend on each other's output | **Sequential** — parallelism wastes spawns on stale context |
| Tiny one-off task | **Neither** — normal chat or a single agent |

## Keeping agent costs sane

- **Match the model to the subagent.** A subagent doing mechanical work (running tests, renaming, mechanical edits) does not need a frontier model. Assign a cheaper, faster model (e.g. Haiku) to it and save the strong model for the orchestrator. See [Model Selection](model-selection.md).
- **Keep spawn prompts focused.** Everything in the spawn prompt is context the subagent carries from token zero — and it *also* auto-loads CLAUDE.md, MCP servers, and skills. Pass only what the subagent needs to do its job.
- **Keep teams small.** Token usage is roughly proportional to the number of active teammates. Two focused agents beat six vague ones.
- **Shut teammates down when done.** An idle-but-alive teammate keeps consuming tokens until it exits or the session ends.
- **Don't parallelize dependent work.** If task B needs task A's output, running them at once just means B started with the wrong context and has to be redone.
- **Attribute the cost.** In Claude Code, `/usage` breaks recent usage down by subagents (and skills, plugins, MCP servers), so you can see whether a team was worth it.

## A quick decision check

Before spawning agents, ask:

1. Am I isolating noise (saves context) or chasing speed (multiplies cost)?
2. Are these tasks actually independent?
3. Can a cheaper model do the subagent's job?
4. Is the time saved worth several times the tokens?

If you cannot answer 1 clearly, you are probably about to pay agent-team prices for a job a single focused agent would do.

## Key takeaway

Subagents that absorb noise save you money.

Subagents that run in parallel spend it faster to buy speed.

Both are fine — as long as you know which one you just launched.

## Sources

- Claude Code Docs — *Manage costs effectively* (isolate high-volume operations; agent team ~7× token cost; subagent model selection): <https://code.claude.com/docs/en/costs>
- Claude Code Docs — *Subagents*: <https://code.claude.com/docs/en/sub-agents>
- Claude Code Docs — *Agent teams*: <https://code.claude.com/docs/en/agent-teams>
