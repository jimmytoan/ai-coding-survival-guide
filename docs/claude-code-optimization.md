# Claude Code Optimization (Claude Max)

> **TL;DR** On **Claude Max** (~$100 = "5×", ~$200 = "20×") you are not optimizing a bill — you are optimizing how much work fits inside your **5-hour window** and **weekly caps** before you get throttled. The highest-leverage moves: stay on **Sonnet** (it draws from a separate, larger weekly bucket than Opus), keep the cache **warm**, keep `CLAUDE.md` and MCP servers **lean**, and watch **`/usage`** and **`/context`** so you spend your window on real work.

This is the Claude Code companion to the cross-tool [Token Optimization Playbook](token-optimization-playbook.md). It collects the Claude-specific knobs; the *why* lives in the linked deep-dive docs.

## How Claude Max meters you

Two layers, and a detail that matters:

- **5-hour rolling window** — short-burst activity, resets ~5 hours after your first message.
- **Two weekly caps** — one across **all models**, and a separate, more generous one for **Sonnet only**. Staying on Sonnet literally draws from the bigger bucket.
- **Shared pool** — usage counts across **Claude Code, Claude.ai chat, and Cowork** together. Heavy chat use eats into your coding capacity.

When you exhaust included usage, **usage credits** let you continue at standard API rates (consumption-based) — useful as overflow, but that is the moment "free" becomes per-token, so the warm-cache discipline below starts mattering for dollars too.

## See where you stand

- **`/usage`** — token usage for the session plus, on paid plans, a breakdown of what counts against your limits, attributed to **skills, subagents, plugins, and individual MCP servers** (`d`/`w` toggles 24h vs 7d).
- **`/context`** — what is filling your window *right now*: system prompt, tool definitions, memory files, history. This is where you catch a bloated `CLAUDE.md` or an MCP server you forgot you connected.
- **Status line** — configure it to show context-window usage continuously so you compact/clear before you're forced to.
- **Settings → Usage** — progress bars for your 5-hour and weekly limits.

## Model & reasoning

- **Default to Sonnet.** It handles most coding work, costs less, and draws from the larger Sonnet-only weekly cap. Switch with `/model`; set a default in `/config`.
- **Reserve Opus** for architecture, multi-step reasoning, and hard debugging.
- **Right-size effort.** Extended thinking bills as output. Lower it with `/effort` (or in `/model`) for simple tasks; on fixed-budget models use `MAX_THINKING_TOKENS`. See [Model Selection](model-selection.md).
- **Use `model: haiku` for mechanical subagents** (running tests, renames). See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Context hygiene

- **`/clear` between unrelated tasks.** Stale context is re-sent every message. `/rename` before clearing so you can `/resume` later.
- **Compact early and warm.** `/compact` at a natural break (optionally `/compact Focus on the failing tests and changed files`). Doing it warm is cheap; doing it after a long idle gap reprocesses your whole history. See [Compaction and the Prompt Cache](compact-and-caching.md).
- **Keep the cache warm.** Set your model and connect MCP servers at the *start* of a session — switching model or MCP mid-task invalidates the cache. On Max you get the 1-hour TTL automatically.
- **Keep `CLAUDE.md` under ~200 lines.** It loads every session. Move specialized runbooks (migrations, releases) into **skills** that load on demand. See [Project Context Files](project-context-files.md).

## Tools & MCP

- **MCP tool definitions are deferred by default** — only tool names enter context until a tool is used. Still, connect only what you need; `/mcp` lists servers and lets you disable unused ones.
- **Prefer a CLI when one exists** (`gh`, `aws`, `gcloud`) — no per-tool listing overhead. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).
- **Code intelligence plugins** give precise symbol navigation instead of grep-then-read-many-files.

## Agents & verbose output

- **Plan mode (Shift+Tab)** before big changes — approving a plan is cheaper than unwinding a wrong path. Stop early with Escape; `/rewind` to a checkpoint instead of compounding a mistake.
- **Isolate verbose ops in subagents** so a 10,000-line test log stays in the subagent and only a summary returns.
- **Filter command output with a hook.** A `PreToolUse` hook can rewrite `npm test` / `pytest` to show only failures — turning tens of thousands of tokens into hundreds. See the [cost docs](https://code.claude.com/docs/en/costs) for a ready example.
- **Be wary of agent teams** — each teammate is a separate instance with its own window (~7× a normal session in plan mode). Keep teams small and shut them down when done.

## Write efficient prompts

- Specific beats broad: "add input validation to `login()` in `auth.ts`" reads a couple of files; "improve this codebase" triggers a broad, window-draining scan.
- Give verification targets (test cases, expected output) so Claude catches its own mistakes instead of you paying for fix-up rounds.
- Ask for diffs and terse output; cap with `CLAUDE_CODE_MAX_OUTPUT_TOKENS` if needed.

## Quick checklist

- [ ] On Sonnet unless this task truly needs Opus
- [ ] Effort matched to task difficulty
- [ ] `/context` checked — no surprise MCP server or fat `CLAUDE.md`
- [ ] One task this session; `/clear` since last unrelated one
- [ ] Compacted at the last natural break (warm)
- [ ] Verbose commands filtered to failures
- [ ] Plan mode used for the risky multi-file change

## Sources

- Claude Code Docs — *Manage costs effectively*: <https://code.claude.com/docs/en/costs>
- Claude Help Center — *Usage limit best practices*: <https://support.claude.com/en/articles/9797557-usage-limit-best-practices>
- Claude Help Center — *What is the Max plan?*: <https://support.claude.com/en/articles/11049741-what-is-the-max-plan>

*Limit specifics change frequently; check `/usage` and Settings → Usage for current numbers.*
