# Codex Optimization

> Last verified: **2026-10-31** for plans, models, limits, and workflow guidance. Profile and configuration syntax below was last verified **2026-07-11**. Codex models, plans, limits, and configuration change frequently; check the linked official pages and your local `codex --help` before copying settings.

Optimize Codex for successful, reviewable tasks per unit of time and usage. The highest-leverage habits are clear acceptance criteria, focused context, matched model/reasoning, safe permissions, executable verification, and review of the actual diff.

## Understand the current meter

For ChatGPT plans, Codex usage depends on model, context, reasoning, and tools.

- On Plus and standard Business, local messages are counted per model over a five-hour window. OpenAI publishes ranges, such as 15–160 messages for GPT-6.1 Sol and 350–3,000 for GPT-6 Luna.
- Pro currently has no five-hour limit, and Pro tiers are $100, $200, and $500 per month. Weekly limits may still apply on any plan.
- Cloud tasks use more allowance than local messages.
- Plus and Pro users can buy credits after included limits are reached.
- API-key usage is usage-based rather than drawn from ChatGPT plan limits.
- Current limits are visible in the Codex usage dashboard and via `/status` in an active CLI session.

Use the live dashboard as the source of truth. Plan names, rates, and available models are volatile; see [Models and Plan Limits](models-and-plan-limits.md) for the current tables.

## Route routine and difficult work differently

Use the least costly available model that meets a measured quality bar for the task.

| Model | OpenAI positions it for |
|---|---|
| GPT-6 Luna | Focused, high-volume work: summarization, extraction, focused coding |
| GPT-6.1 Terra | Fast, efficient writing, visual understanding, and computer use |
| GPT-6.1 Sol | Repeated, long-running work across code, apps, and documents at lower cost than Astra |
| Astra | The hardest end-to-end work with sustained reasoning |

- Routine search, explanation, formatting, and local edits often fit Luna or a lower effort.
- Cross-cutting diagnosis, architecture, migrations, and subtle review may justify Sol, Astra, or higher effort.
- Effort runs from Light or Low through Medium, High, and Extra High, with Max on some models and Ultra, which uses subagents, on others. OpenAI says effort levels do not map exactly between model generations, so re-tune after a model change.
- Models retire on a schedule. GPT-5.5 left Codex with ChatGPT sign-in on 2026-10-14, so configs that name old models need updating.
- High-risk work also requires tests, scanners, and qualified human review; model strength is not a safety control.

Change one dimension deliberately. If an attempt fails because the requirement or evidence is incomplete, a more expensive model may repeat the same mistake.

## Prompt for current models

OpenAI's guidance for GPT-6 Astra suggests pruning habits built for older models. It was written for Astra, so re-test it on Luna, Terra, and Sol before deleting instructions they may still need:

- State goal, context, constraints, and when the task is done. Use `@` mentions for files.
- Drop generic "always run the tests" lines. Grant permission for specific safe checks, such as running the local suite and fixing failures caused by your change, and include that in the task definition.
- Drop long step-by-step recipes and strong "ask first" language. Describe the goal and boundaries, and keep strict limits only where real risk exists.
- Keep skill descriptions short, make the skill root a router to supporting docs, and review `AGENTS.md` and skills regularly to remove stale instructions.
- Use Plan mode (`/plan` or Shift+Tab) for ambiguous work, or ask Codex to interview you first.
- When Codex makes the same mistake twice, ask for a retrospective and update `AGENTS.md`.

See [Field Notes](field-notes.md) for how these compare with Anthropic's guidance.

## Use current profile files

Codex profiles are separate files in `CODEX_HOME` (normally `~/.codex`) and are selected explicitly.

Base defaults:

```toml
# ~/.codex/config.toml
model = "<routine-model>"
model_reasoning_effort = "low"
```

Higher-effort profile:

```toml
# ~/.codex/deep-review.config.toml
model = "<review-model>"
model_reasoning_effort = "high"
approval_policy = "on-request"
```

Run it with:

```bash
codex --profile deep-review
codex exec --profile deep-review "review this change"
```

Since Codex 0.134, legacy `[profiles.<name>]` tables and a top-level profile selector are no longer supported. Do not use old examples containing `default_profile` or `[profiles.*]`.

Supported reasoning values are model-dependent. The current config reference documents `minimal`, `low`, `medium`, `high`, and `xhigh`, with `xhigh` available only on some models.

## Keep `AGENTS.md` useful and scoped

Use `AGENTS.md` for durable repository facts:

- Canonical build, test, lint, and type-check commands
- Important domain and architecture invariants
- Generated-file rules
- Approval boundaries
- Verification expected before handoff

Codex discovers instructions from the project root down to the working directory. Nested guidance can specialize a subtree, and `AGENTS.override.md` takes precedence in its directory. Current Codex uses a combined default instruction limit of 32 KiB, configurable with `project_doc_max_bytes`.

Keep instructions concrete. Move long, occasional workflows into on-demand guides or skills, and let formatters and CI enforce mechanical rules.

See [Project Context Files](project-context-files.md).

## Manage context by task

- Start with outcome, acceptance criteria, constraints, entry points, and verification.
- Let Codex inspect callers and tests needed for correctness; avoid arbitrary file caps.
- Keep unrelated work in a fresh task.
- Use `/compact` at a coherent boundary when a long task must continue.
- After compaction, reopen critical files and re-run relevant checks because summaries are lossy.
- Filter large logs and tool results before they enter model context.
- Disable unused MCP servers; each server can add context and tool-selection work.

There is no currently documented `.codexignore` feature. Use repository structure, focused prompts, search filters, and client-supported configuration rather than relying on an invented ignore file.

## Configure permissions for the task

Codex separates approval policy from sandbox and network configuration. Prefer the narrowest combination that still lets the task finish.

- Use workspace-scoped writes for normal repository work.
- Keep network access disabled until a task requires it.
- Ask before installs, public API changes, destructive operations, deployments, messages, or external writes.
- Use project `.codex/config.toml` only for trusted repositories; project layers load only after trust is established.
- Keep production credentials out of the agent environment.

Never solve an approval inconvenience by broadly disabling the sandbox without understanding the environment.

## Make verification executable

Give Codex the repository's exact checks and require it to report results:

- Focused regression test
- Type, lint, and build checks
- Integration or manual flow when relevant
- Checks not run and why
- Remaining risk

Review the actual Git diff after the edit. Asking for a unified diff in the response is unnecessary when Codex already changed files; a concise handoff plus the real diff is clearer.

Codex's review workflow (`/review`) can inspect uncommitted changes, a base-branch diff, or a commit without modifying the worktree. Use a fresh review context for meaningful changes to reduce shared assumptions. Teams can point `AGENTS.md` at a `code_review.md` so review behavior stays consistent.

Manage chats by unit of work: `/resume` returns to a saved chat, `/fork` branches one while keeping the original, and `/compact` summarizes long context. Run parallel tasks in separate Git worktrees; OpenAI lists live tasks on the same files without worktrees as a common mistake.

## Use subagents for a reason

Subagents can isolate noisy research, specialize a review, or run independent work in parallel. They have their own context and usage.

- Prefer deterministic filtering before spawning a model only to summarize command output.
- Give workers bounded objectives and exclusive edit scopes.
- Use parallelism for wall-clock speed, not presumed total-token savings.
- Reconcile findings and run final verification centrally.

See [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Quick checklist

- [ ] Task has observable acceptance criteria
- [ ] Model and reasoning match difficulty and risk
- [ ] Current profile syntax is used
- [ ] `AGENTS.md` contains concise, scoped invariants and commands
- [ ] MCP and network access are limited to what the task needs
- [ ] Permissions require approval at meaningful boundaries
- [ ] Large tool output is filtered
- [ ] Exact checks and results are reported
- [ ] Actual diff is reviewed in fresh context for meaningful changes
- [ ] `/status` or the usage dashboard was checked when capacity matters

## Sources

- OpenAI — [Codex pricing and limits](https://learn.chatgpt.com/docs/pricing)
- OpenAI — [Codex models](https://learn.chatgpt.com/docs/models)
- OpenAI — [Codex best practices](https://learn.chatgpt.com/guides/best-practices)
- OpenAI — [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
- OpenAI — [Advanced configuration: profiles](https://learn.chatgpt.com/docs/config-file/config-advanced#profiles)
- OpenAI — [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- OpenAI — [Custom instructions with `AGENTS.md`](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- OpenAI — [Code review](https://learn.chatgpt.com/docs/code-review)
- OpenAI — [Sandboxing and approvals](https://learn.chatgpt.com/docs/sandboxing)

## Key takeaway

Codex is most efficient when the task is well-defined, access is bounded, context is focused, and completion is proven by tools and the real diff.
