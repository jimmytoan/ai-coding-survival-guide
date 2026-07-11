# Codex Optimization

> Last verified: **2026-07-11**. Codex models, plans, limits, and configuration change frequently; check the linked official pages and your local `codex --help` before copying settings.

Optimize Codex for successful, reviewable tasks per unit of time and usage. The highest-leverage habits are clear acceptance criteria, focused context, matched model/reasoning, safe permissions, executable verification, and review of the actual diff.

## Understand the current meter

For ChatGPT plans, Codex usage depends on model, context, reasoning, and tools.

- Local messages and cloud tasks share a five-hour window.
- Additional weekly limits may apply; do not assume a universal reset schedule.
- Plus and Pro tiers have different included capacity; current Pro offerings include 5× and 20× tiers.
- Plus and Pro users can use purchased credits after included limits are reached.
- API-key usage is usage-based rather than drawn from ChatGPT plan limits.
- Current limits are visible in the Codex usage dashboard and via `/status` in an active CLI session.

Use the live dashboard as the source of truth. Plan names, rates, and available models are volatile.

## Route routine and difficult work differently

Use the least costly available model that meets a measured quality bar for the task.

- Routine search, explanation, formatting, and local edits often fit an economical model and lower reasoning.
- Cross-cutting diagnosis, architecture, migrations, and subtle review may justify a more capable model or higher effort.
- High-risk work also requires tests, scanners, and qualified human review; model strength is not a safety control.

Change one dimension deliberately. If an attempt fails because the requirement or evidence is incomplete, a more expensive model may repeat the same mistake.

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

Codex's review workflow can inspect uncommitted changes, a base-branch diff, or a commit without modifying the worktree. Use a fresh review context for meaningful changes to reduce shared assumptions.

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
- OpenAI — [Advanced configuration: profiles](https://learn.chatgpt.com/docs/config-file/config-advanced#profiles)
- OpenAI — [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- OpenAI — [Custom instructions with `AGENTS.md`](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- OpenAI — [Code review](https://learn.chatgpt.com/docs/code-review)
- OpenAI — [Sandboxing and approvals](https://learn.chatgpt.com/docs/sandboxing)

## Key takeaway

Codex is most efficient when the task is well-defined, access is bounded, context is focused, and completion is proven by tools and the real diff.
