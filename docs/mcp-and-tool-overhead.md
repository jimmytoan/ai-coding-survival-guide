# MCP and Tool Overhead

> Product-specific behavior last verified: **2026-07-11**.

Coding agents need tool descriptions to know what they can call. Those descriptions, tool results, and follow-up reasoning consume context capacity and usage—even when your typed prompt is short.

The practical goal is not “avoid tools.” It is to expose the smallest safe, useful tool surface for the task.

## Where overhead comes from

A tool-enabled request may include:

- Tool names, descriptions, and parameter schemas
- Authentication and policy metadata
- Tool-selection or search steps
- Returned files, records, logs, and errors
- Model reasoning and follow-up calls

Some clients load every schema up front. Others defer full definitions until the model searches for or selects a tool. Prompt caching can also reduce repeated processing. Exact behavior depends on the client, model, server, and cache state.

## Deferred loading helps

Modern tool-search designs can keep large schemas out of the initial context and load them only when needed. Anthropic's Tool Search documentation reports large savings in its example configurations, and Claude Code defers MCP tools on supported models by default.

Deferred loading reduces idle overhead; it does not make every connection free:

- Names and discovery information may still be present.
- Selecting a tool loads its definition.
- Results can be much larger than the schema.
- Too many similar tools make selection less reliable.
- Permissions and prompt-injection risks remain.

Measure your actual client rather than applying one token estimate to every setup.

## Choose CLI, MCP, or connector deliberately

| Option | Strengths | Trade-offs |
|---|---|---|
| CLI | Composable, familiar, often no large schema catalog | Can inherit broad ambient credentials and emit noisy unstructured output |
| MCP server | Typed tools, structured results, explicit capabilities | Adds discovery/schema surface and another trust boundary |
| Product connector/app | Managed auth and rich private context | Data flow, scopes, auditability, and availability are provider-specific |
| Direct API or script | Precise, automatable, filter before model input | Engineering and credential-management cost |

“Prefer the CLI” is not universal advice. A read-only, narrowly scoped connector may be safer and more efficient than a shell with administrator credentials. A small CLI query may be cheaper than loading a broad tool suite.

Evaluate:

- Least-privilege scopes
- Structured and filterable output
- Audit logs and approval behavior
- Credential isolation
- Context and round-trip cost
- Reliability and error semantics
- Whether a maintained purpose-built integration already exists

## Filter before model ingestion

The largest cost is often tool output, not tool definitions.

Prefer:

- Focused test selection before a full suite
- Failure summaries plus a small surrounding excerpt
- Server-side fields, date ranges, limits, and filters
- `git diff --stat` before a full diff
- Search results with file paths and line numbers
- Structured JSON reduced to required fields

Keep enough context to diagnose the problem. A filtered log without the command, timestamp, or surrounding event can be confidently misread.

## Apply least privilege

- Enable only tools needed for the current repository or task.
- Prefer read-only scopes for investigation and review.
- Separate development and production identities.
- Require approval for external messages, writes, deployments, billing actions, and destructive operations.
- Disable or disconnect unused servers.
- Review server provenance, updates, and transport security.
- Do not expose secrets as model-visible tool arguments or results when a broker can handle them.

Tools turn model output into real actions. Their authorization boundary matters more than the wording of the prompt.

## Treat tool results as untrusted data

Issues, documents, web pages, code comments, logs, and remote records can contain instructions designed to redirect an agent.

- Delimit retrieved content as data.
- Do not execute commands embedded in results without task-level authorization.
- Keep sensitive actions behind deterministic policy and human approval.
- Validate identifiers and arguments before write operations.
- Inspect the proposed action and target, not only the final response.
- Red-team automated workflows with malicious content.

See [Security and Privacy](security-and-privacy.md).

## Audit the tool surface

Periodically ask:

- Which tools and servers are enabled?
- Which were actually used?
- How much context and output did they add?
- Are two tools overlapping?
- Are scopes broader than the task?
- Can filtering happen before data reaches the model?
- Is a rarely used workflow better as an on-demand skill, command, or server?
- Does the client support deferred loading or tool search?

Use the client's context and usage diagnostics when available.

## Sources

- Anthropic — [Introducing advanced tool use](https://www.anthropic.com/engineering/advanced-tool-use)
- Anthropic — [Scale with MCP tool search](https://code.claude.com/docs/en/mcp#scale-with-mcp-tool-search)
- OpenAI — [Model Context Protocol](https://learn.chatgpt.com/docs/extend/mcp)
- OWASP — [Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)

## Key takeaway

Select tools by capability, permissions, output quality, and measured overhead. Reduce the surface, filter results early, and treat everything retrieved as untrusted data.
