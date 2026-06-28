# Codex Optimization (ChatGPT / Codex Pro)

> **TL;DR** On **ChatGPT / Codex Pro** (~$100 = "5×", ~$200 = "20×") your bill is fixed — what you are protecting is your **5-hour message window** and **weekly cap**. The highest-leverage Codex moves: make a **cheaper model the default profile** and opt into the strong one explicitly, tune **`model_reasoning_effort`**, keep junk out of context with **`.codexignore`**, keep **`AGENTS.md`** small and nested, and **`/compact`** before the window fills.

This is the Codex companion to the cross-tool [Token Optimization Playbook](token-optimization-playbook.md). The durable *why* lives in the playbook; this collects Codex-specific knobs.

> Codex moved to API-token-aligned usage in 2026, but for Pro subscribers the day-to-day meter is still the **5-hour rolling window** (counted in messages/compute) plus a **weekly cap**. Check **`/status`** for where you stand.

## How Codex Pro meters you

- **5-hour rolling window** — local messages and cloud tasks share it; resets ~5 hours after your first message.
- **Weekly cap** — stacks on top, resets on a rolling 7-day basis from your first message of the week.
- **Tiers** — Plus is the entry tier; **Pro ~$100 is 5×**, **Pro ~$200 is 20×**.
- **Overflow** — credits let you continue at API rates after you hit the cap; eligible plans also have rate-limit reset *banking* (a small number of resets you can hold for ~30 days).

## Default to a cheaper model with named profiles

The single most effective Codex habit: make the cheap model the **default** and require an explicit opt-in for the expensive one. Set this in `~/.codex/config.toml`:

```toml
[profiles.explore]            # the default — cheap, for search/edits/triage
model = "<cheap-model>"
model_reasoning_effort = "low"
max_tokens_per_session = 100000

[profiles.commit]             # opt into this for real implementation
model = "<strong-model>"
model_reasoning_effort = "high"
max_tokens_per_session = 300000

default_profile = "explore"
```

Then escalate only when the task earns it:

```bash
codex --profile commit "implement the auth flow"
```

The reported cost ratio between mini-class and full-class models is roughly **5×** — the most important number in Codex usage management. Most search, formatting, and single-file work does not need the big model. See [Model Selection](model-selection.md).

## Tune reasoning effort

`model_reasoning_effort` controls how many reasoning tokens Codex spends:

- `"low"` — formatting, deterministic transforms, simple reviews, triage.
- `"high"` — security audits, novel algorithms, complex multi-file diagnosis.

Match reasoning depth to task complexity rather than running one setting all session. A common pattern is two profiles (`cheap` vs `frontier`) selected with `--profile`.

## Keep junk out of context: `.codexignore`

A maintained `.codexignore` keeps the agent from pulling build artifacts and vendored code into context:

```
node_modules/
dist/
build/
*.lock
__pycache__/
.git/
```

On large repos this is reported to cut input tokens **40–60%** — context you were re-sending every turn for no benefit.

## Keep `AGENTS.md` small and nested

`AGENTS.md` is loaded into context like Claude's `CLAUDE.md`, so the same rule applies: essentials only.

- Prefer **nested `AGENTS.md`** in subdirectories (targeted context) over one monolithic project-wide file. Keep each small (a couple of KiB).
- Put output discipline here so it applies every turn:

  ```
  ## Output style
  - Reply in unified diff form. No full-file rewrites unless asked.
  - No preamble, no trailing summary.
  ```

- Add a rule to prefer quiet output for noisy commands (filter `test`/`build`/`status` down to failures) — engineers report **60–90%** reductions on dev-loop output. A wrapper tool or a `prefer <command>` instruction in `AGENTS.md` both work. See [Project Context Files](project-context-files.md).

## Manage the session

- **`/compact`** replaces detailed history with a summary (~30–50% context reduction). Run it around **~60% full** rather than waiting for forced compaction, which loses more detail.
- **Work iteratively on the same files** so prompt caching keeps hitting; jumping between unrelated areas of the repo breaks the cached prefix.
- **`/status`** — check cumulative usage during a session so you don't blow the window unaware.
- **Cap the session** with `max_tokens_per_session` so a runaway debugging loop on a big legacy repo can't quietly drain your window in one sitting.

## Delegate verbose operations to subagents

File-heavy or noisy operations (test runs, log scans) belong in a subagent so the output stays in the subagent's context and only a summary returns to the main session — keeping the orchestrator's window small. See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Quick checklist

- [ ] `default_profile` points at the cheap model
- [ ] Reasoning effort matched to the task
- [ ] `.codexignore` covers build artifacts and vendored code
- [ ] `AGENTS.md` small, nested, with output-style rules
- [ ] Noisy commands filtered to failures
- [ ] `/compact` run before the context fills; iterating on the same files
- [ ] `/status` checked for window/weekly headroom

## Sources

- OpenAI Developers — *Codex pricing*: <https://developers.openai.com/codex/pricing>
- OpenAI Developer Community — *Understanding the new Codex limit system*: <https://community.openai.com/t/understanding-the-new-codex-limit-system-after-the-april-9-update/1378768>
- Codex Knowledge Base — *Codex CLI cost management: token strategy, model routing and quota control*: <https://codex.danielvaughan.com/2026/03/28/codex-cli-cost-management-token-strategy/>
- AutoScout24 Engineering — *3 techniques to reduce token consumption (Claude Code & Codex)*: <https://tech.autoscout24.com/blog/posts/3-techniques-to-reduce-token-consumption-claude-code-codex/>

*Model names, tiers, and limits change frequently; the config knobs (profiles, reasoning effort, `.codexignore`, `AGENTS.md`) are the durable part. Check `/status` for current usage.*
