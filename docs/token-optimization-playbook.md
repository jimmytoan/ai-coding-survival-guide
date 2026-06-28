# Token Optimization Playbook (Subscription Edition)

> **TL;DR** If you are on a subscription — **Claude Max** (~$100 / ~$200 a month) or **ChatGPT / Codex Pro** (~$100 / ~$200 a month) — your bill is fixed. The thing you are actually optimizing is **not dollars, it is throttling.** Both tools meter you with a **5-hour rolling window** *and* a **weekly cap**. Waste does not show up as a charge; it shows up as "you've hit your limit, come back later." Every habit in this guide buys you the same thing: **more useful work before the window resets.**

This is the hub. The durable, cross-tool principles live here. The exact commands and config knobs live in the tool-specific guides:

- **[Claude Code Optimization](claude-code-optimization.md)**
- **[Codex Optimization](codex-optimization.md)**

## The meter you can't see

On a usage-based plan the meter is the price page. On a subscription the meter is your **rate limit**, and it has two layers on both tools:

| Layer | What it governs | Roughly |
| --- | --- | --- |
| **5-hour rolling window** | Short bursts of activity | Resets ~5 hours after your first message in the window |
| **Weekly cap** | Sustained compute over the week | Resets on a rolling 7-day basis |

You can hit *either* and get throttled. The tiers are parallel across both products: the **~$100 tier is "5×"** baseline usage and the **~$200 tier is "20×"**. Same shape, same lesson — burning the window on low-value work means you are locked out (or pushed onto pay-as-you-go credits) when you need it for real work.

> Limits and tiers change often and the exact numbers are not published precisely. Treat the structure as stable and **check the live usage screen in your tool**, not a blog number.

## What actually drains the bucket

Usage scales with **how much the model has to process and produce**, not how many messages you send. The big drivers, in rough order:

1. **Model capability.** A frontier model (Opus, GPT-5-class) spends your allowance far faster than a mid model (Sonnet, GPT-mini-class). On Claude Max the weekly cap is even split — there's an *all-model* limit and a separate, more generous *Sonnet-only* limit — so staying on Sonnet literally draws from a different, larger bucket.
2. **Reasoning / thinking effort.** Extended thinking bills as output. High effort on a trivial task is pure drain.
3. **Output length.** Long reports, full-file rewrites, and "review your own work again" loops are expensive. Ask for diffs and terse answers.
4. **Context re-sent every turn.** The whole conversation, your rules file, and tool definitions ride along on *every* message. A bloated context taxes every turn until you compact or clear.
5. **Agent / parallel multiplication.** Spawning multiple agents multiplies the work — each carries its own context window. Great for speed, expensive for your window.

## The cross-tool playbook

These apply to **both** Claude Code and Codex. The tool-specific guides show how to do each one.

**1. Default to the mid model; opt *into* the frontier model.**
Sonnet / GPT-mini-class handles ~80% of coding work and draws from a bigger bucket. Reserve the top model + high reasoning effort for architecture, security, and hard debugging. See [Model Selection](model-selection.md).

**2. Right-size reasoning effort, not just the model.**
Low effort for formatting, lint, simple reviews. High effort only when reasoning changes the answer. Thinking tokens count.

**3. Stop feeding the model noise.**
The classic drain is a thousand-line test/build log sent so the model can find one failing line. Filter command output down to failures (a hook in Claude Code, a wrapper or `AGENTS.md` rule in Codex). Engineers report **60–90%** reductions on dev-loop output.

**4. One task per session; clear between unrelated work.**
Give each session a single clear objective and start fresh afterward. Stale context is re-sent — and re-charged against your window — on every later message. See [Context Discipline](context-discipline.md).

**5. Compact early, while the session is warm.**
Compact at a natural break around ~60% context rather than waiting for forced auto-compaction. On a warm cache this is cheap; after a long idle gap it reprocesses your whole history. See [Compaction and the Prompt Cache](compact-and-caching.md).

**6. Keep the always-on context lean.**
Your rules file (`CLAUDE.md` / `AGENTS.md`) and connected MCP servers are sent every turn. Keep rules to essentials, move occasional runbooks to on-demand skills/commands, and connect only the MCP servers you need. See [Project Context Files](project-context-files.md) and [MCP and Tool Overhead](mcp-and-tool-overhead.md).

**7. Point the agent at the right files.**
"Dump five files because nobody knew where the function lived" is waste. Name the files, or use a repo-map / code-graph / LSP tool so the agent does targeted lookups instead of blind reads.

**8. Constrain output.**
Ask for unified diffs, no preamble, no trailing summary. Cap output length. A frontier model writing an essay when you wanted a three-line fix is a window you don't get back.

**9. Plan intensive work around your windows.**
Know when your 5-hour window and weekly cap reset, and line up the heavy, frontier-model work for the start of a fresh window — not the moment before a reset.

**10. Watch the live usage screen.**
Both tools show you where you stand. Look *before* you guess, and especially before kicking off an expensive agent run.

## The 80/20

If a teammate reads nothing else:

1. Stay on the mid model + low effort by default; opt into the big one deliberately.
2. Filter noisy command output — the single biggest win.
3. One task per session, clear between tasks, compact early.
4. Keep `CLAUDE.md` / `AGENTS.md` and MCP servers lean.

## Key takeaway

You already paid for the month. What you are protecting now is **access** — staying inside your window so the tool is there when the work is real.

Waste doesn't cost you money on a subscription. It costs you the tool, right when you need it.

## Sources

- Claude Help Center — *Usage limit best practices*: <https://support.claude.com/en/articles/9797557-usage-limit-best-practices>
- Claude Help Center — *What is the Max plan?*: <https://support.claude.com/en/articles/11049741-what-is-the-max-plan>
- Claude Code Docs — *Manage costs effectively*: <https://code.claude.com/docs/en/costs>
- OpenAI Developers — *Codex pricing*: <https://developers.openai.com/codex/pricing>
- OpenAI Developer Community — *Understanding the new Codex limit system*: <https://community.openai.com/t/understanding-the-new-codex-limit-system-after-the-april-9-update/1378768>

*Tier and limit specifics change frequently and vary by plan; the structure (5-hour window + weekly cap, 5× vs 20× tiers) is the durable part. Check your tool's live usage screen for current numbers.*
