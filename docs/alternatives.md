# Compare AI Coding Tools

No tool wins every workflow. Compare candidates on your repositories, tasks, risk, and operating constraints—not a demo or model leaderboard.

## Start with the deployment model

Common categories include:

- IDE autocomplete and chat
- Local terminal or desktop coding agents
- Cloud agents that open pull requests
- Code-review agents
- Direct API workflows
- Open-source agent clients
- Local or self-hosted models
- Internal platforms that route several models and tools

Example products change quickly. As of **2026-07-11**, teams commonly evaluate GitHub Copilot, Cursor, Claude Code, OpenAI Codex, OpenCode, Aider, Continue, Cline, Roo Code, and internal or self-hosted alternatives. Verify current availability and terms before choosing.

Update **2026-10-31**: Google's documentation says Gemini CLI was replaced by Antigravity CLI on 2026-06-18, so evaluate Google's terminal agent as Antigravity, not Gemini CLI. Google's plans use quota that refreshes on a five-hour and weekly cadence rather than a fixed daily request count. See [Models and Plan Limits](models-and-plan-limits.md) for how the Claude, Codex, and Antigravity plans compare, and the sources there for what was and was not verified. The other products in the list above were not re-checked on that date.

## Freeze representative tasks

Use real, permission-safe tasks with objective acceptance checks:

- Explain an unfamiliar request or data flow
- Reproduce and fix a local bug
- Implement a small multi-file feature
- Add behavior-oriented tests
- Review a seeded diff
- Perform a scoped migration
- Diagnose a CI failure
- Update documentation from a code change
- Handle one security-sensitive scenario

Freeze the repository commit, environment, prompt, starting context, and hidden evaluator checks. Repeat each run because outputs vary.

Use the [Benchmark Protocol](../benchmarks/README.md) and [Task Template](../benchmarks/task-template.md).

## Evaluate complete outcomes

### Quality

- Acceptance and hidden-check pass rate
- First-pass correctness
- Severity of introduced or missed defects
- Scope control
- Test quality
- Maintainability and architectural fit
- Variance across runs

### Productivity

- Time to first useful result
- Total lead time to verified completion
- Human brief, interaction, review, and rework time
- Ability to resume and hand off
- Latency under normal limits

### Context and repository fit

- Code search and navigation quality
- Project-instruction discovery and precedence
- Context transparency and compaction behavior
- Large-repository performance
- Generated, vendored, and monorepo handling
- Support for your languages, build system, and test environments

### Agent safety and control

- Sandbox and filesystem boundaries
- Network and secret isolation
- Approval policies and destructive-action controls
- Tool and MCP permission scopes
- Prompt-injection mitigations
- Branch, worktree, and dirty-tree handling
- Audit logs and action traceability

### Data and legal fit

- Data retention and model-training policy
- Regional processing and residency
- Identity, SSO, RBAC, and admin policy
- Repository and connector authorization model
- Public-code matching and provenance controls
- Indemnity and contractual terms
- Compliance and audit exports

### Operations and economics

- Subscription, credits, API, and overage model
- Usage visibility and budgets
- Model routing and reasoning controls
- Offline or local capability
- Reliability and support
- Exportability of sessions, configuration, and workflows
- Lock-in and migration cost

### Developer experience

- IDE, terminal, desktop, web, and mobile surfaces
- Accessibility
- Review ergonomics
- Permission-prompt quality
- Support for parallel work without edit collisions
- Ease of creating reusable commands, skills, or policies

## Compare default and tuned setups

Run two lanes:

1. **Default experience:** what a new developer receives.
2. **Team-tuned experience:** project instructions, approved tools, model routing, and verification workflow.

A tool that needs extensive tuning may still be worthwhile, but include that setup and maintenance cost.

## Separate model from product

The same model can behave differently across products because of:

- System instructions
- Repository indexing and retrieval
- Tool implementation
- Context management
- Sandbox and approval policy
- Agent loop and stopping behavior
- Model version or provider routing

Record the full tool/model/version/configuration combination. Do not attribute every difference to the model name.

## Direct API versus assistant product

Direct API workflows can offer:

- Precise model and prompt control
- Custom routing, caching, and observability
- Organization-specific permissions and audit
- Integration with internal systems

They also require engineering for authentication, sandboxing, context retrieval, tool safety, evaluations, UI, and maintenance.

Packaged assistants can offer faster adoption and polished repository workflows, but may provide less control or portability. Compare the total operating model, not only token price.

## Run a security and governance review

Before standardizing, involve security, legal/privacy, platform, and developer-experience owners. Verify claims in contracts and official documentation rather than sales summaries.

Test high-risk behavior explicitly:

- Malicious instructions in an issue or repository file
- A suggested nonexistent dependency
- A destructive command
- A request for production credentials
- An external write or message
- A contributor change to agent instructions or CI

## Make a reversible decision

- Keep task briefs, tests, and repository instructions portable.
- Avoid putting critical workflow only in proprietary chat history.
- Export usage and audit data where possible.
- Re-evaluate when pricing, models, policy, or your task mix changes.
- Pilot with a bounded group and explicit success/exit criteria.

## Key takeaway

Choose the system that produces the best verified outcomes within your security, governance, developer-experience, and cost constraints.
