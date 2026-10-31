# Claude Code Optimization

> Last verified: **2026-10-31** for plans, models, defaults, and workflow guidance. Plan limits, model names, and commands can change; confirm them in Anthropic's current documentation and your local client.

Optimize Claude Code for useful, verified work within your plan or API budget. Focus first on task clarity, context quality, verification, and permissions; token savings that increase rework are false economy.

## Understand the current limits

Claude subscription usage is affected by model, message length, files and conversation history, tool use, and the work Claude performs.

Current Anthropic guidance describes:

- A five-hour session limit on Pro and Max, and a weekly limit on every plan; verified Ultra accounts have no five-hour limit
- A Fable allowance: on Max and Ultra, Fable models count toward your weekly limit up to 50% of it, shown as its own meter; on Pro, Fable runs on usage credits only
- Shared usage across Claude web, desktop, mobile, and Claude Code
- Usage credits at API rates after included limits are exhausted, where enabled; monthly API credits bundled with Max, Ultra, and Team cannot be spent in Claude Code

Anthropic publishes multipliers relative to Pro (Max 5x, Ultra 10x weekly) but not token counts. Use `/usage` rather than a copied estimate. See [Models and Plan Limits](models-and-plan-limits.md) for the current tables.

## Inspect usage and context

- Use `/usage` or the plan's usage page to see current allowance and reset information. On subscription plans it also breaks usage down by skill, subagent, plugin, and MCP server, and flags long context and frequent cache misses.
- Use `/context` to inspect what occupies the current context window.
- Watch for large conversation history, project instructions, tool definitions, and raw command output.
- Remember that background work keeps spending while you are away: a `/loop`, a goal check after each turn, and subagents.
- Treat local dollar estimates as estimates; the provider console is authoritative for API spend.

## Match model and effort to the task

- A new session runs Opus 5.5 at medium effort on most plans and providers. That default handles most coding well.
- Use Sonnet 5.5 for routine work where it meets your quality bar, and Haiku 5.5 for simple subagent jobs.
- Select Fable 5.1 explicitly with `/model fable` when evals on Opus 5.5 at higher effort still fall short. It spends weekly allowance fastest, and on Pro it bills usage credits.
- Change effort with `/effort`. Use `high` or `xhigh` when correctness depends on easy-to-miss cases, `low` for fast back-and-forth where you review every step, and `max` only for hard problems you leave unattended, because it tends to overthink ordinary tasks.
- Put `ultrathink` in a single prompt when only that turn needs deeper reasoning. Phrases such as `think hard` are ordinary text and do not raise effort.
- Escalate after identifying a reasoning bottleneck; missing requirements and missing evidence need better input, not only a stronger model.

Security-sensitive and production work still requires deterministic checks and qualified human review.

## Keep context coherent

- One objective per session is a useful default.
- Start fresh when switching to unrelated work. `/clear` is free and stops the next task paying to reread the last one.
- Give a task brief with expected behavior, acceptance criteria, constraints, and verification. For vague goals, ask Claude to interview you and write a `SPEC.md`, then build from it in a fresh session.
- Filter large logs and test output before model ingestion.
- Compact at a coherent milestone when the task must continue in the same session, with a focus such as `/compact keep the API changes and the test commands`.
- After two failed corrections, rewind (`/rewind` or `Esc` twice) or `/clear` and write one better prompt instead of a third correction.
- Treat compaction summaries as lossy: preserve rejected approaches, reopen critical files, and rerun relevant checks.

With 1M-token windows and automatic compaction, running out of context is now rare. Focus matters for quality and cost rather than survival.

Claude Code uses prompt caching to reduce repeated processing of stable prefixes. Subscription authentication currently receives a one-hour cache duration by default, while API and third-party provider behavior can differ. See [Compaction and Prompt Caching](compact-and-caching.md).

## Keep `CLAUDE.md` focused

Anthropic recommends keeping `CLAUDE.md` concise—roughly under 200 lines—and moving specialized workflows into skills that load on demand.

High-value content:

- Build, test, lint, and type-check commands
- Critical domain and architecture invariants
- Generated-file rules
- Verification and approval expectations

Move long release, migration, and incident runbooks out of always-on context. Use deterministic tooling for formatting and other mechanical rules.

See [Project Context Files](project-context-files.md).

## Enforce what cannot have exceptions

`CLAUDE.md`, prompts, and skills are advice the model can decline. Hooks, deny rules, the sandbox, managed settings, and CI are enforced by the harness or your pipeline.

- If you keep adding emphasis to a `CLAUDE.md` line and it is still missed, move it to a hook or deny rule. If you have hooks for mere preferences, delete them.
- Use `permissions.deny` for secrets such as `.env`, and the sandbox for OS-level file and network limits. Auto mode is a classifier, so keep "never" rules in deny rules or hooks.
- Use a `PostToolUse` hook for formatting and a `Stop` hook that exits with code 2 to keep Claude working until the typecheck passes.
- Use `.claude/rules/` with a `paths:` filter for rules that matter in one part of the repository, and skills for team procedures. Add `disable-model-invocation: true` to a skill with side effects, such as a release.
- Run `/doctor prompt-audit` to find instructions written for older models, dead file references, and contradictions.
- Keep the enforced set small; each hook will occasionally fire when it should not.

## Use MCP and tools deliberately

Claude Code defers MCP tool definitions by default on supported models. With deferred loading, connecting or disconnecting a server can append tool information without invalidating the existing cached prefix.

Full schema changes can still disturb caching when tools are loaded up front—for example with an unsupported model/provider, disabled tool search, or an `alwaysLoad` configuration.

Regardless of cache behavior:

- Enable only the servers needed for the task.
- Prefer least-privilege, read-only scopes for investigation.
- Filter tool results before they enter context.
- Choose CLI, MCP, or connector based on permissions, auditability, structure, and measured overhead.
- Treat returned content as untrusted input.

See [MCP and Tool Overhead](mcp-and-tool-overhead.md).

## Use agent mode with boundaries

- Use plan mode for ambiguous, cross-cutting, destructive, or high-risk changes—not as ceremony for every edit.
- Define what the agent may edit, run, and access.
- Require approval for installs, external actions, destructive commands, production data, and broad permission changes.
- Use subagents for context isolation, specialization, independent review, or parallel speed.
- Count total worker use; protecting the parent context does not automatically reduce total tokens.
- Give parallel editors exclusive file ownership or isolated worktrees.

See [Agent Mode](agent-mode.md) and [Subagents and Agent Teams](subagents-and-agent-teams.md).

## Filter noisy commands

Hooks, wrapper scripts, focused test selection, and structured command options can keep thousands of irrelevant lines out of context.

Preserve:

- The exact command and exit status
- Failing test names or diagnostics
- Enough surrounding lines to understand the cause
- A path to the complete artifact when needed

Do not rely on universal percentage-savings claims; measure your own workflow.

## Verify and review

Give Claude executable completion criteria:

- Focused regression test
- Build, type, lint, and static checks
- Integration or manual flow based on risk
- Exact command results and skipped checks
- Review of every changed file, including tests and configuration

For meaningful changes, use a fresh review context with the original requirement and actual diff. The authoring session's summary can share the implementation's blind spots.

Options, from advisory to enforced:

- Ask in the prompt for the command it ran and its output, not "all tests pass."
- `/goal <condition>` has a separate model check the condition after every turn, which helps long background runs that end early with open items.
- A `Stop` hook runs your script when Claude tries to finish and sends it back to work on failure.
- `/code-review` reviews the diff in a fresh subagent. Ask any review to `skip style` so it reports correctness gaps rather than inventing work.
- `/verify` has Claude build and run the app, which catches problems that tests and typechecks miss.

For unattended runs, answer five questions first: what does done look like, how does it check itself, what may it touch, what stops it (`--max-turns`, `--max-budget-usd`), and who approves the result. Use `claude -w <name>` or a worktree per parallel session so edits cannot collide.

## Quick checklist

- [ ] Goal and acceptance criteria are explicit
- [ ] Model and effort match the task
- [ ] `/context` shows no surprising history, instructions, or tools
- [ ] `CLAUDE.md` is concise and durable
- [ ] Unrelated work starts fresh
- [ ] Compaction state preserves facts, failures, checks, and next step
- [ ] MCP and permissions are least-privilege
- [ ] Noisy output is filtered without losing diagnostic context
- [ ] Exact verification evidence is reported
- [ ] Meaningful changes receive independent diff review
- [ ] Rules that cannot bend are hooks or deny rules, not emphasized prose
- [ ] Unattended runs have a done condition, a self-check, and a turn or spend cap

## Sources

- Anthropic — [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- Anthropic — [Use Claude Code with your Pro, Max, or Ultra plan](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan)
- Anthropic — [Claude Fable models on your plan](https://support.claude.com/en/articles/15424964-claude-fable-models-on-your-plan)
- Anthropic — [Usage limit best practices](https://support.claude.com/en/articles/9797557-usage-limit-best-practices)
- Anthropic — [Manage costs effectively](https://code.claude.com/docs/en/costs)
- Anthropic — [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- Anthropic — [MCP tool search](https://code.claude.com/docs/en/mcp)
- Anthropic — [Claude Code memory](https://code.claude.com/docs/en/memory)

## Key takeaway

Claude Code is efficient when context stays relevant, tools and permissions stay narrow, and every change ends with reproducible evidence.
