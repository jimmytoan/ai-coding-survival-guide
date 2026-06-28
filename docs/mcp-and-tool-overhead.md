# MCP Servers and Tool Overhead: The Context You Pay For Every Turn

> **TL;DR** Every MCP server you connect injects its tool definitions — names, descriptions, and parameter schemas — into the context. In the worst case this is loaded on **every single request**, before you type anything. A handful of popular servers can eat **50K+ tokens** of a 200K window. Modern clients fix this by *deferring* tool definitions (loading only tool names until a tool is actually used), but the habit still matters: connect only the servers you need, prefer a CLI when one exists, and check what is actually in your context.

This extends [Context Discipline](context-discipline.md) to the context you do not type: tool definitions, project files, and other "invisible" tokens that ride along on every turn.

## Why tool definitions cost tokens

The model cannot call a tool it has not been told about. So for the model to use an MCP server, the client has to describe each tool — its name, what it does, and every parameter — in the request. Historically that full schema was sent **on every message in the session**, whether or not you ever used the tool.

That overhead is real and often invisible. Reported figures for popular servers:

| MCP server | Tools | Approx. tokens |
| --- | --- | --- |
| GitHub (official) | ~35 | ~26K |
| Slack | ~11 | ~21K |
| Sentry | ~5 | ~3K |
| Grafana | ~5 | ~3K |
| Splunk | ~2 | ~2K |
| **Five-server setup** | **~58** | **~55K** |

A 200K-token context window sounds huge until ~25% of it is gone before the conversation starts. GitHub's server alone is roughly 13% of the window. Connect ten servers and you can burn 75K+ tokens on definitions you may never call — tokens that also push your real work closer to auto-compaction.

## The fix: deferred tool loading (Tool Search)

The newer approach is to **not** load every definition upfront. Instead the client loads only tool *names*, and the model searches for and pulls in a tool's full schema **on demand** when it actually needs it.

- On the Claude Developer Platform this is the **Tool Search Tool** (mark tools with `defer_loading: true`).
- In Claude Code, **MCP tool definitions are deferred by default** — only tool names enter context until Claude uses a specific tool.

The savings are large: Anthropic reports the Tool Search approach preserves ~191,300 tokens of context vs. ~122,800 with everything loaded upfront — about an **85% reduction** in tool-definition overhead, and up to ~95% for setups with many servers.

This is great, but it is not a reason to connect everything. Deferred loading lowers the *idle* cost; it does not make a sprawling, half-used set of servers free, and not every tool or client defers. The discipline still applies.

## Practical rules

**Connect intentionally**

- Only enable the MCP servers you actually use in a given project. More servers = more names, more tool-search round-trips, more surface area.
- Disable servers you are not actively using. In Claude Code, `/mcp` lists configured servers and lets you turn them off.

**Prefer a CLI when one exists**

- Tools like `gh`, `aws`, `gcloud`, and `sentry-cli` are often **more context-efficient than an MCP server** because they add no per-tool listing at all — the agent just runs a shell command. If you already have the CLI authenticated, you may not need the equivalent MCP server.

**Look at what is actually loaded**

- In Claude Code, `/context` shows what is consuming your window (tool definitions, system prompt, files, history). `/usage` attributes recent usage to skills, subagents, plugins, and individual MCP servers. If a server you never call is showing up large, drop it.

**Watch for the same pattern outside MCP**

- Project rule files, large system prompts, and long tool lists all behave the same way: they are sent repeatedly. The MCP lesson generalizes — anything that rides on every request deserves the same scrutiny as the prompt you type. See [Project Context Files](project-context-files.md).

## Key takeaway

Tool definitions are context you pay for whether or not you use them.

Connect the servers you need, prefer a CLI when there is one, and check `/context` instead of guessing.

## Sources

- Anthropic Engineering — *Introducing advanced tool use* (Tool Search Tool, token figures): <https://www.anthropic.com/engineering/advanced-tool-use>
- Claude Code Docs — *Manage costs effectively* (reduce MCP overhead, deferred by default, `/context`, `/mcp`, prefer CLI tools): <https://code.claude.com/docs/en/costs>
- Claude Code Docs — *Scale with MCP tool search*: <https://code.claude.com/docs/en/mcp>

*Token figures above are reported/illustrative and vary by server version and client. Use `/context` to measure your own setup.*
