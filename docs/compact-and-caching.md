# Compaction and the Prompt Cache: Avoid Surprise Token Costs

> **TL;DR** Claude Code keeps your conversation in a server-side cache so it does not reprocess your whole history on every turn. While you are actively working, that cache is "warm" and compacting is cheap. But the cache only survives a limited gap of inactivity (**one hour** on a Claude subscription). If you leave a conversation idle for too long, then come back and compact, the cache has expired — so the entire conversation gets reprocessed at the **full input rate, roughly 10× the cached price**. Compact while the session is still warm. Be deliberate about compacting after a long break.

This is a deeper dive into the "compact long conversations" advice in [Context Discipline](context-discipline.md).

## Why caching exists

The model has no memory between requests. Every time you send a message, Claude Code re-sends the **entire context** — the system prompt, your project context (CLAUDE.md, memory), every prior message and tool result, and your new message.

Prompt caching stops you from paying full price for that repeated content on every turn. The API matches the **start of each request** (the "prefix") against content it recently processed. On a normal turn, almost the whole request is identical to the last one and only your latest exchange is new — so the unchanged prefix is served from cache, and only the new part is processed.

Two prices matter here:

- **Base input tokens** — full price for processing content the first time (a "cache miss" or "cache write").
- **Cache hits** — content served from cache, billed at roughly **10% of the base input rate**.

That 10× gap is the whole story.

## The pricing, in numbers

Using Claude Opus 4.8 as the example (rates per million tokens, "MTok"):

| Token type | Rate | Relative to base input |
| --- | --- | --- |
| **Base input** (cache miss / full reprocess) | **$5.00 / MTok** | 1× |
| 5-minute cache write | $6.25 / MTok | 1.25× |
| 1-hour cache write | $10.00 / MTok | 2× |
| **Cache hit & refresh** (warm read) | **$0.50 / MTok** | **0.1× (10× cheaper)** |
| Output | $25.00 / MTok | — |

The key comparison: **$0.50/MTok when the cache is warm vs. $5.00/MTok when it has expired.** Same content, 10× the cost.

## What `/compact` actually does

Compaction replaces your long message history with a shorter **summary**, freeing up context space. It happens two ways:

- **Auto-compaction** — Claude Code triggers it automatically as you approach the context-window limit.
- **Manual** — you run `/compact` yourself, optionally with instructions (e.g. `/compact Focus on the failing tests and the files we changed`).

To build that summary, Claude Code sends a **one-off summarization request** containing your existing system prompt, tools, and full history, plus a "summarize this" instruction at the end. **That request shares your prefix** — so whether it is cheap or expensive depends entirely on whether your cache is still warm.

## The cache window: one hour (on a subscription)

A cached prefix expires after a period of **inactivity**. Every request that hits the cache **resets the timer**, so the cache stays warm as long as you keep working. The length of gap the cache can survive is the TTL ("time to live"), and Claude Code picks it for you based on how you authenticate:

- **On a Claude subscription** (Pro / Max / Team / Enterprise): Claude Code automatically requests the **1-hour TTL**. Because usage is included in your plan rather than billed per token, the longer window costs nothing extra — it just keeps your cache warm through longer breaks. (If you go over your plan limit and start drawing on usage credits, it drops to the 5-minute TTL.)
- **On an API key or third-party provider** (Bedrock, Vertex, Foundry): you pay per token, so the default is the cheaper **5-minute TTL**. You can opt into the 1-hour window with `ENABLE_PROMPT_CACHING_1H=1`.

For most people on a subscription, **one hour is the number to remember.**

## The cheap case: compacting a warm session

You have been actively working — sending messages every few minutes. Your cache is warm.

When you compact (auto or manual), the summarization request **reads your existing history from cache** instead of reprocessing it. Most of the time and cost goes into *generating* the summary, not re-reading the conversation. The turn right after compaction then rebuilds the cache for only the much shorter summary.

**Net effect:** compacting mid-flow is inexpensive. This is exactly when you *want* to do it — at a natural break between tasks.

## The expensive case: compacting after a long break

You leave the conversation for **more than an hour** (lunch, a meeting, end of day, the weekend). When you come back to a long conversation, Claude Code may notice the large context and **suggest compacting it.**

Here is the trap. While you were away, the cache **expired.** So the first request you send back into that session — *including a compaction summarization request* — is a **cache miss**. The API has to recompute the **entire conversation history from scratch at the full base input rate**, then re-establish the cache.

If that long history is, say, 150,000 tokens:

| Scenario | What is billed | Cost (Opus 4.8) |
| --- | --- | --- |
| Compact while **warm** | 150K tokens at the cache-hit rate ($0.50/MTok) | **~$0.08** |
| Compact while **cold** (>1h idle) | 150K tokens at the base input rate ($5.00/MTok) + re-cache | **~$0.75+** |

Same conversation, **~10× the cost**, just because the cache lapsed before you acted.

> **Important nuance:** the expensive part is not `/compact` itself — it is the **cache having expired.** That first turn back into a long, cold session is costly *whether or not you compact*. But compaction is the moment it bites hardest, because it forces a full read of your entire (long) history to produce the summary. The real warning: **be careful with the first action you take when returning to a long, idle conversation.**

## Practical rules of thumb

**Do**

- Compact at **natural breaks between tasks while you are still actively working** — the cache is warm and it is cheap.
- Use `/clear` to start fresh when switching to unrelated work, rather than carrying a huge history forward.
- Set your model and connect any MCP servers **at the start** of a session, and avoid changing them mid-task (a model switch or MCP connect/disconnect also invalidates the cache).
- Watch your cache health: a status-line script can show `cache_read_input_tokens` (warm reads) vs. `cache_creation_input_tokens` (writes/misses). A high read-to-write ratio means caching is working.

**Be careful**

- Returning to a conversation that has been **idle for more than an hour** — the cache is gone, and the first turn (especially a compact on a long history) is your most expensive request.
- If you have been away and just want to abandon a path rather than continue, consider `/rewind` to an earlier point instead of compacting — rewinding truncates back to a prefix that was already cached.
- If the context is large but stale and you do not need it, `/clear` and a fresh start is often cheaper and cleaner than reprocessing 150K tokens just to summarize them.

## FAQ

**Does just sending a normal message after a long break also cost more?** Yes. Any first request after the cache expires reprocesses the full history at the base input rate. Compaction simply makes it most visible because it reads the entire long history to summarize it.

**Is compaction bad, then?** No — compaction is good and necessary for long sessions. It keeps your context small so later turns stay cheap. The only thing to manage is *when* the one-time reprocessing cost lands. Do it warm.

**How do I know if I am on the 1-hour or 5-minute window?** Subscription plans get the 1-hour TTL automatically. API-key / Bedrock / Vertex / Foundry users get 5 minutes unless they set `ENABLE_PROMPT_CACHING_1H=1`. You can force 5 minutes anywhere with `FORCE_PROMPT_CACHING_5M=1` (useful for debugging).

## Key takeaway

Compaction is not the cost. A cold cache is.

Compact while the session is warm.

## Sources

- Claude Code Docs — *How Claude Code uses prompt caching*: <https://code.claude.com/docs/en/prompt-caching>
- Claude Code Docs — *Manage costs effectively*: <https://code.claude.com/docs/en/costs>
- Claude API Docs — *Prompt caching* (TTLs and pricing): <https://platform.claude.com/docs/en/build-with-claude/prompt-caching>

*Pricing figures above use Claude Opus 4.8 list rates and are illustrative; check the current model pricing page for the model your team actually uses.*
