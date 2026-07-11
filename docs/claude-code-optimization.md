# Claude Code Optimization

> Last verified: **2026-07-11**. Plan limits, model names, and commands can change; confirm them in Anthropic's current documentation and your local client.

Optimize Claude Code for useful, verified work within your plan or API budget. Focus first on task clarity, context quality, verification, and permissions; token savings that increase rework are false economy.

## Understand the current limits

Claude subscription usage is affected by model, message length, files and conversation history, tool use, and the work Claude performs.

Current Anthropic guidance describes:

- A session limit that resets after a usage window
- Weekly limits with separate displays for **Opus-only** use and **all other models**
- Shared usage across Claude surfaces associated with the account
- Extra usage, where enabled, after included plan limits are exhausted

Reset times and allowances vary by plan and account. Use the live usage view rather than a copied estimate.

## Inspect usage and context

- Use `/usage` or the plan's usage page to see current allowance and reset information.
- Use `/context` to inspect what occupies the current context window.
- Watch for large conversation history, project instructions, tool definitions, and raw command output.
- Treat local dollar estimates as estimates; the provider console is authoritative for API spend.

## Match model and effort to the task

- Use a balanced model for normal repository work.
- Reserve the highest-capability model and higher effort for problems where deeper reasoning changes the result.
- Use a fast model for focused mechanical subagents when it meets the quality bar.
- Escalate after identifying a reasoning bottleneck; missing requirements and missing evidence need better input, not only a stronger model.

Security-sensitive and production work still requires deterministic checks and qualified human review.

## Keep context coherent

- One objective per session is a useful default.
- Start fresh when switching to unrelated work.
- Give a task brief with expected behavior, acceptance criteria, constraints, and verification.
- Filter large logs and test output before model ingestion.
- Compact at a coherent milestone when the task must continue in the same session.
- Treat compaction summaries as lossy: preserve rejected approaches, reopen critical files, and rerun relevant checks.

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

## Sources

- Anthropic — [Usage limit best practices](https://support.claude.com/en/articles/9797557-usage-limit-best-practices)
- Anthropic — [Manage costs effectively](https://code.claude.com/docs/en/costs)
- Anthropic — [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- Anthropic — [MCP tool search](https://code.claude.com/docs/en/mcp)
- Anthropic — [Claude Code memory](https://code.claude.com/docs/en/memory)

## Key takeaway

Claude Code is efficient when context stays relevant, tools and permissions stay narrow, and every change ends with reproducible evidence.
