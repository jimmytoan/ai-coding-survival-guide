# FAQ

## Is this repo anti-GitHub Copilot?

No.

This repo is pro-effective AI coding.

The GitHub Copilot pricing debate is the trigger, but the lesson applies to many AI coding tools.

## Is usage-based billing bad?

Not necessarily.

A short autocomplete and a long agentic coding session do not have the same cost.

Usage-based billing can be reasonable if users have good visibility, prediction, budgets, and control.

## Should I stop using AI coding assistants?

No.

Use them better.

AI coding tools can save real time when used well.

## Should I always use the cheapest model?

No.

Use the cheapest model that can reliably solve the task.

Use stronger models when mistakes are expensive.

## Should I avoid agent mode?

No.

Agent mode is useful for clear, scoped tasks.

Avoid using it for tiny tasks or vague “fix everything” requests.

## What is context discipline?

Context discipline means giving the assistant enough information to solve the task, but not dumping unnecessary context into the model.

## Do MCP servers cost tokens even when I do not use them?

They can. Each connected server's tool definitions may be loaded into context — historically on every request — and a few popular servers can total tens of thousands of tokens. Newer clients defer tool definitions (load names only until a tool is used), which helps a lot, but connecting only what you need still matters. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).

## Does my CLAUDE.md / rules file cost tokens?

Yes. Project rule files are loaded at session start and carried on every turn, so a long one is a tax on every message — even unrelated ones. Keep it to genuine essentials and move occasional runbooks into on-demand skills or commands. See [Project Context Files](project-context-files.md).

## Do parallel agents save money?

No — they multiply it. Each subagent has its own context window, so running several in parallel costs more in total even though it finishes faster. Subagents *do* save tokens when used to isolate noisy output (tests, logs) from your main conversation. See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## What is the Max Power way?

It is a joke from The Simpsons:

> “Isn't that the wrong way?”  
> “Yeah, but faster!”

In AI coding, it means using powerful tools carelessly to create faster waste.

## What is the main takeaway?

The future is not “use less AI.”

The future is:

> Use AI better.
