# Compaction and Prompt Caching

> Claude-specific cache details last verified: **2026-07-11**.

Compaction and prompt caching solve different problems:

- **Prompt caching** reduces repeated processing of an unchanged request prefix.
- **Compaction** replaces detailed conversation history with a shorter summary to free context capacity.

Caching is an efficiency mechanism. Compaction is a lossy state transition. Use both deliberately.

## How prompt caching helps

Coding conversations repeatedly send a stable prefix: system instructions, project guidance, tool definitions, and earlier messages. A provider can reuse computation for that unchanged prefix instead of processing it from scratch.

Claude's current API pricing distinguishes:

- Base input
- Five-minute cache writes
- One-hour cache writes
- Cache hits
- Output

At the time this guide was verified, cache hits were priced at one-tenth of base input, while cache writes carried a premium. Check the current pricing for the model and provider you use.

On a subscription, these differences normally affect included usage rather than a direct per-request dollar bill. Once extra usage or API billing applies, they can affect spend as well.

## Cache lifetime depends on authentication

Claude Code currently selects cache behavior based on authentication and provider:

- Claude subscription authentication uses a one-hour cache duration by default.
- Direct API and supported third-party-provider setups commonly use a five-minute duration by default.
- API users can opt into a one-hour cache where supported, at the provider's documented write rate.

Every successful reuse refreshes the lifetime. Confirm current behavior in the official documentation; providers and integrations can differ.

## What compaction does

Compaction summarizes older conversation state and starts a shorter working context. It is useful when:

- A long task must continue past the comfortable context size.
- A coherent milestone has completed.
- Raw exploration or tool output no longer needs to remain verbatim.
- The next step depends on decisions and evidence, not the full transcript.

Compaction is not a substitute for starting fresh when the objective changes.

## Compact state that can survive

A useful summary preserves:

- Goal and acceptance criteria
- Branch, commit, and relevant workspace state
- Files changed and important code locations
- Confirmed facts and decisions with reasons
- Rejected approaches and why they failed
- Commands run and exact outcomes
- Remaining risks and assumptions
- Open questions
- The precise next step

Example: [Context Compaction](../examples/context-compaction-example.md)

Weak summaries such as “we discussed the auth bug” cause the next model turn to rediscover the work or, worse, invent missing state.

## Warm versus cold continuation

When a stable prefix is still cached, the provider can reuse it while producing the summary. After the cache expires, the next request must process the long history again before it can compact or continue.

The practical rule:

- Compact at a natural milestone while the task is active when you know it must continue.
- After a long break, decide whether the old task still needs its detailed history.
- If not, start a fresh task from a concise handoff rather than paying usage and attention to revive stale context.

Do not compare only a cache-hit input rate with a base-input rate and call that the total cost of compaction. Cache writes, output, subscription accounting, model choice, and provider behavior also matter.

## Cache-friendly changes

Stable prefixes cache better. Configure core model and tool choices deliberately near the start of a task.

Claude's current behavior has an important nuance:

- With deferred MCP tool loading—the default on supported models—tool changes can be appended without invalidating the existing cached prefix.
- When full tool schemas are loaded up front, changing those tools can invalidate the affected prefix.

Up-front loading can occur with unsupported models or providers, disabled tool search, or tools marked to always load. Do not apply a universal “any MCP change breaks the cache” rule.

## Re-establish evidence after compaction

Because summaries are lossy:

1. Reopen critical files before editing them.
2. Inspect the current diff and workspace state.
3. Re-run the focused test when the result is important to the next step.
4. Verify version-sensitive external facts again if time has passed.
5. Treat summary statements as leads when they conflict with current evidence.

Never claim a previous test still passes after later edits merely because the compacted summary says it passed earlier.

## When to start fresh instead

Start a new task when:

- The objective or risk profile changed.
- The prior approach is abandoned.
- Independent review is needed.
- Old logs and rejected hypotheses dominate.
- Only a small set of durable decisions remains relevant.

Carry forward a handoff, not the entire conversational history.

## Practical checklist

- [ ] Compact at a coherent milestone, not mid-edit
- [ ] Preserve facts, decisions, failures, checks, risks, and next step
- [ ] Start fresh for unrelated work
- [ ] Account for cache state without assuming universal pricing
- [ ] Reopen critical code and inspect the current diff after compaction
- [ ] Re-run checks whose result matters to the next step
- [ ] Verify MCP caching behavior for the active model/provider

## Sources

- Anthropic — [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching)
- Anthropic — [Manage costs effectively](https://code.claude.com/docs/en/costs)
- Anthropic API — [Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

## Key takeaway

Caching makes stable context cheaper to reuse. Compaction makes long context smaller by losing detail. Preserve the state that matters, then re-establish evidence before continuing.
