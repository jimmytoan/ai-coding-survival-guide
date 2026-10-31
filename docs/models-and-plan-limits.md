# Models and Plan Limits Snapshot

> Last verified: **2026-10-31**, from the official pages listed under [Sources](#sources). Models, prices, multipliers, and limits change often and vendors publish few exact numbers. Treat this page as a map, then confirm in the live product (`/model`, `/usage`, `/status`, or the account dashboard) before you commit to a plan.

Use this page to answer three questions: which models exist, what each subscription plan gives you, and how the limit meters behave. For routing strategy see [Model Selection](model-selection.md); for spending less see the [Token Optimization Playbook](token-optimization-playbook.md).

## Claude (Anthropic)

### Models

| Model | API ID | Anthropic positions it for | Price per 1M tokens (in / out) | Default effort |
|---|---|---|---|---|
| Claude Fable 5.1 | `claude-fable-5-1` | Demanding reasoning and long-horizon agentic work | $10 / $50 | `high` |
| Claude Opus 5.5 | `claude-opus-5-5` | Long-running agentic coding and knowledge work; the "start here" model | $4 / $20 | `medium` |
| Claude Sonnet 5.5 | `claude-sonnet-5-5` | Best mix of speed and intelligence | $2 / $10 | `high` |
| Claude Haiku 5.5 | `claude-haiku-5-5` | High-volume, latency-sensitive work such as classification and routing | from $0.10 / $0.50 | `medium` |

All four list a 1M-token context window, 128K max output, and a June 2026 knowledge cutoff. Earlier models (Fable 5, Opus 5 through 4.5, Sonnet 5 and 4.6, Haiku 4.5) remain available as legacy. Prices above are API rates; subscriptions meter usage differently.

In Claude Code, a new session runs Opus 5.5 at medium effort on most plans and providers. Select Fable explicitly with `/model fable`. Change effort with `/effort` (`low`, `medium`, `high`, `xhigh`, `max`).

### Plans

| Plan | Price | Usage vs. Pro | Limit shape | Fable 5 / 5.1 |
|---|---|---|---|---|
| Free | $0 | Limited | No Claude Code | Not included |
| Pro | $20/month ($17 billed annually) | 1x | Five-hour session limit plus weekly limit | Pay-as-you-go usage credits only |
| Max | $100/month | 5x | Five-hour session limit plus weekly limit | Included; capped at 50% of your weekly limit |
| Ultra | $200/month | 10x weekly | Weekly limit; no five-hour limit for verified accounts | Included; capped at 50% of your weekly limit |

How the meters behave, per Anthropic's support pages:

- Limits are **shared** between Claude (web, desktop, mobile) and Claude Code, including IDE use.
- Anthropic does not publish token counts or message counts. The multipliers are relative to Pro.
- Fable draws from the same weekly pool as other models, and faster. The 50% cap is a share of that pool, not extra capacity, although the Claude Code usage view shows it as its own Fable meter.
- When a limit hits mid-response, Claude Code may finish the current step ("wrapping up"). That work counts toward the weekly limit. Pro gets it once per weekly period; Max can get it at each five-hour limit.
- Beyond the plan, usage credits bill at standard API rates. Monthly API credits bundled with Max, Ultra, and Team **cannot** be spent in Claude Code or the Claude apps.
- The prompt cache lasts about an hour on a subscription and five minutes on an API key, cloud provider, or usage credits, so long idle sessions cost more to resume on the shorter cache.

## Codex (OpenAI)

### Models

| Model | Config name | OpenAI positions it for |
|---|---|---|
| Astra | `gpt-6-astra` | Hardest end-to-end work needing sustained reasoning and judgment |
| GPT-6.1 Sol | `gpt-6.1-sol` | Repeated, long-running work at lower cost than Astra (500K default context in Codex) |
| GPT-6.1 Terra | `gpt-6.1-terra` | Fast, efficient work: writing, visual understanding, computer use (needs Codex CLI 0.159.0 or later) |
| GPT-6 Luna | `gpt-6-luna` | Focused, high-volume tasks: summarization, extraction, focused coding |

Reasoning effort is Light/Low, Medium, High, Extra High, Max (supported by Luna), and Ultra (uses subagents; not supported by Luna). OpenAI notes that efforts do not map exactly between model generations, so re-tune after switching models.

Retirements in Codex with ChatGPT sign-in: `gpt-5.4` and `gpt-5.4-mini` on 2026-08-31, GPT-5.3-Codex-Spark on 2026-09-14, and GPT-5.5 on 2026-10-14. Old configs that name them need a new model.

### Plans

| Plan | Price | Codex access |
|---|---|---|
| Free / Go | $0 / $8 per month | Luna only, subject to rollout |
| Plus | $20/month | Codex on web, CLI, IDE, and iOS; the plan names Sol and Luna, and Terra and Astra also appear in the published limits |
| Pro | From $100/month ($100, $200, or $500 tiers) | No five-hour limit currently; weekly limits may apply; Ultrafast mode on the $500 tier |
| Business | $20/user/month annual, $25 monthly | Plus-level limits per user, workspace credits, SSO and MFA |
| API key | Standard API pricing | Model access follows the key; not drawn from plan limits |

Local messages per five hours on Plus and standard Business, as published:

| Model | Messages per five hours |
|---|---|
| Astra | 5–45 |
| GPT-6.1 Sol | 15–160 |
| GPT-6.1 Terra | 35–285 |
| GPT-6 Luna | 350–3,000 |

Cloud tasks use more allowance than local messages. Plus and Pro can buy credits after the included limits; for example Sol is listed at 50 / 2.5 / 250 credits per 1M input / cached / output tokens, Luna at 2.5 / 0.25 / 12.5. The wide ranges mean task size dominates: a long agentic run can burn a window in a handful of messages.

## Google (Gemini and Antigravity)

### Models

| Model | API ID | Status and role |
|---|---|---|
| Gemini 4 Argon | `gemini-4-argon` | Stable flagship reasoning model, 1M context |
| Gemini 3.8 Flash | `gemini-3.8-flash` | Stable; most capable Flash model, aimed at long-horizon software engineering and agents |
| Gemini 3.6 Flash | `gemini-3.6-flash` | Stable; previous-generation Flash |
| Antigravity Agent | `antigravity-preview-09-2026` | Preview; managed agent in a sandbox |

### Tools and plans

- **Gemini CLI was replaced by Antigravity CLI on 2026-06-18.** The Gemini CLI documentation says it is not optimized for Gemini 4 Argon and points users to Antigravity. Its per-day request table (Google AI Pro 1,500, Ultra 2,000, free Google login 1,000, free API key 250 on Flash only) is dated 2026-06-18 and should be read as legacy.
- Antigravity uses a **quota** rather than a request count. Ultra's quota refreshes every five hours with the highest weekly limits. Pro's refreshes every five hours until a weekly limit is reached. Other plans refresh weekly. Complex tasks consume more quota than simple ones.
- Pro and Ultra can use purchased AI credits beyond baseline quota. The *AI Credit Overages* setting is either *Never* (wait for refresh) or *Always* (spend credits automatically), so set it deliberately.
- Google's plan page lists Ultra in 5x and 20x tiers relative to Pro and gives Pro and Ultra expanded access to Gemini 4 Argon in Antigravity. The Antigravity plans page names Gemini 3.8 Flash as the core agent model and does not mention Argon, so check which model your tier actually runs.
- Neither Google page lists dollar prices or numeric quotas, and Antigravity's page says limits are "subject to modification."

## Side by side: how the meters work

| | Claude Code | Codex | Antigravity |
|---|---|---|---|
| Short window | Five hours (not on verified Ultra) | Five hours on Plus and Business; none on Pro currently | Five-hour refresh on Pro and Ultra |
| Weekly cap | Yes, all plans | May apply | Yes, all plans |
| Top-model rule | Fable capped at half the weekly pool on Max and Ultra; credits only on Pro | Astra has the lowest message range | Check your tier's model |
| Overflow | Usage credits at API rates | Purchased credits | Purchased AI credits |
| Live view | `/usage`, `/status` | Usage dashboard, `/status` | Plan and credit settings |

## Practical guidance

These points are judgment drawn from the facts above, not vendor claims.

- **Start on the mid-tier model and escalate on evidence.** Fable, Astra, and Argon cost the most allowance (Fable is credits-only on Pro). Move up when an eval on the mid-tier model at higher effort still fails, not by default.
- **Match the window to the work.** Short windows punish long autonomous runs. Weekly caps punish many parallel sessions. Set budgets and stop conditions before you leave a run unattended.
- **Spread workloads deliberately.** Routine edits, search, and extraction fit Haiku, Sonnet, Luna, or Flash-class models. Keep the top model for design, hard debugging, and review.
- **Read the meter, not the marketing.** A "5x" plan is five times Pro's allowance, which Anthropic does not quantify. Measure your own tasks per window for a week before you upgrade.
- **Re-check after every model launch.** New models change effort defaults, cost per task, and sometimes which plan includes them. Retired models also break pinned configs.

## Reported but not confirmed in official pages

- Secondary sources reported that Anthropic doubled Claude Code's five-hour limits in May 2026 and removed a peak-hours reduction for Pro and Max.
- Secondary sources reported that OpenAI temporarily removed the five-hour Codex limit in July 2026. The current official page says only that Pro has no five-hour limit.
- The Codex pricing page still mentions "GPT-5.6 Sol" promotional pricing through at least 2026-11-21 and lists a "GPT-6 Sol" row beside GPT-6.1 Sol. It is unclear whether those are stale lines, so verify the model name before relying on them.
- Third-party posts quote Antigravity Ultra prices and old multipliers that disagree with each other. Use Google's own plan page.

## Verify in five minutes

1. Claude Code: run `/model` and `/usage`; read the plan table on [claude.com/pricing](https://claude.com/pricing).
2. Codex: run `/status` in the CLI and open the usage dashboard; read the [pricing page](https://learn.chatgpt.com/docs/pricing).
3. Antigravity: check plan, quota, and credit-overage settings in the product.
4. API users: list models through each provider's models endpoint instead of copying IDs from articles.
5. Record the plan, model, effort, version, and date next to any benchmark or budget you share.

## Sources

- Anthropic — [Use Claude Code with your Pro, Max, or Ultra plan](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan)
- Anthropic — [Claude Fable models on your plan](https://support.claude.com/en/articles/15424964-claude-fable-models-on-your-plan)
- Anthropic — [Plans and pricing](https://claude.com/pricing)
- Anthropic — [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- Anthropic — [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- OpenAI — [Codex pricing and limits](https://learn.chatgpt.com/docs/pricing)
- OpenAI — [Codex models](https://learn.chatgpt.com/docs/models)
- Google — [Gemini models](https://ai.google.dev/gemini-api/docs/models)
- Google — [Gemini CLI quotas and pricing](https://geminicli.com/docs/resources/quota-and-pricing/)
- Google — [Antigravity plans](https://antigravity.google/docs/plans)
- Google — [Google AI plans](https://one.google.com/intl/en/about/google-ai-plans/)

## Key takeaway

Plans buy relative allowance, not a fixed number of tokens, and the top model in each family spends that allowance fastest. Escalate on evidence, watch the live meter, and re-verify after every model or plan change.
