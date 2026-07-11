# Project Context Files

> Product-specific discovery behavior last verified: **2026-07-11**.

Files such as `AGENTS.md`, `CLAUDE.md`, and `.github/copilot-instructions.md` give coding assistants durable repository guidance. They are valuable when they contain facts that matter to most work in their scope.

They also consume context capacity and can become stale or contradictory. Exact loading, precedence, caching, and cost depend on the client.

## Put high-value facts in always-on guidance

Useful content includes:

- Canonical setup, build, test, lint, and type-check commands
- A short directory and ownership map
- Critical domain invariants
- Architectural and trust boundaries
- Generated or vendored files that must not be edited directly
- Verification expected after common changes
- Dependency and approval rules
- Non-obvious environment constraints

Example:

```markdown
# Repository guidance

- Run `pnpm test --filter <package>` for focused tests and `pnpm check` before handoff.
- Money is stored as integer minor units; never use floating-point arithmetic.
- Authorization belongs in service methods because jobs call them without HTTP controllers.
- `src/api/generated/` comes from `pnpm generate`; edit `schema/openapi.yaml` instead.
- Ask before adding a production dependency or changing a public API.
```

## Keep occasional workflows on demand

Avoid putting these in the root file unless nearly every task needs them:

- Full release, migration, or incident runbooks
- Long architecture histories
- Framework tutorials
- Every formatting rule already enforced by tooling
- Provider pricing, model rankings, or other volatile facts
- Instructions for a single package or component

Link to a focused document, command, or skill instead. The base file should help the assistant discover the workflow without carrying its entire contents into unrelated tasks.

## Scope instructions close to the work

Use nested guidance when a subtree has different commands or invariants. Document the client's precedence rules because they differ.

For Codex, current official documentation says it walks from the project root to the current working directory, loading at most one instruction file per directory. `AGENTS.override.md` takes precedence over `AGENTS.md` in the same directory, and guidance closer to the working directory appears later and overrides broader guidance. Codex has a combined byte limit controlled by `project_doc_max_bytes`.

Do not assume every client discovers the same filenames or merges them the same way. Link the tool-specific rule from the repository setup guide.

## Write instructions that are testable

Weak:

```text
Use best practices and make production-ready code.
```

Better:

```text
- Preserve backwards compatibility for public JSON fields.
- Add a regression test for behavior changes.
- Run `make check` and report any skipped checks.
- Never log access tokens or customer payloads.
```

Prefer concrete commands, boundaries, and invariants. Explain the reason when it prevents an otherwise plausible mistake.

## Do not duplicate mechanical enforcement

If a formatter, linter, compiler, schema, or CI rule can enforce a constraint, use it. The instruction file can name the canonical command; it should not restate a 100-line style guide.

This improves reliability for humans and models and reduces conflicts between prose and tooling.

## Keep security boundaries outside untrusted content

Repository instructions can be modified in a pull request, so they are not an authorization system.

- Enforce permissions through sandbox, credentials, approval policy, and CI.
- Review changes to instruction, workflow, hook, and tool-configuration files carefully.
- Do not store secrets in instruction files.
- Treat instructions from dependencies, issues, web pages, and tool output as untrusted data.
- Use organization-managed policy for rules contributors must not be able to weaken.

## Verify what actually loaded

After adding or changing guidance:

1. Start the tool in the intended directory.
2. Ask it to list instruction sources and summarize their precedence.
3. Confirm the expected root and nested guidance appears.
4. Check for global or override files that conflict.
5. Confirm important commands and invariants are interpreted correctly.
6. Re-test after moving the working directory or changing client configuration.

For Codex, the official `AGENTS.md` guide includes commands for auditing loaded instructions. Other tools expose their own context or diagnostics views.

## Audit periodically

- Is each rule still true?
- Does it apply to most work in this scope?
- Is it enforced more reliably elsewhere?
- Do nested instructions conflict?
- Are commands copy-pasteable and current?
- Are important generated-file, security, and verification rules missing?
- Does the file contain volatile product details that should be linked instead?
- Can a specialized section become an on-demand guide or skill?

Review instruction changes like code because they can influence every later agent task.

## Sources

- OpenAI — [Custom instructions with `AGENTS.md`](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- Anthropic — [Claude Code memory](https://code.claude.com/docs/en/memory)
- GitHub — [Add repository custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions)

## Key takeaway

Keep durable guidance concise, scoped, concrete, and verifiable. Put mechanical rules in tools and specialized workflows where they load only when needed.
