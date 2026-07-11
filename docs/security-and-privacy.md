# Security and Privacy

AI coding tools combine three risky capabilities: they can read valuable data, follow untrusted text, and sometimes act with developer permissions. Treat security as part of the workflow, not a final prompt asking “is this secure?”

## Know what you are allowed to share

Before adding code, logs, tickets, screenshots, or production data to a tool, check:

- Organization policy and approved providers
- Data retention and model-training settings
- Workspace, regional, and contractual requirements
- Repository classification and customer agreements
- Whether secrets or personal, regulated, or export-controlled data are present

Prefer minimal synthetic examples. Redaction must remove values from attachments, terminal output, screenshots, traces, and conversation history—not just the typed prompt.

Never paste live credentials. If a secret is exposed, treat it as compromised: revoke or rotate it and follow the incident process. Deleting the chat is not remediation.

## Use least privilege

Give an agent only the access needed for the current task:

- Limit writable paths.
- Keep network access off unless the task needs it.
- Use read-only credentials for investigation.
- Separate development, staging, and production identities.
- Require approval for dependency installation, external messages, deployments, data changes, and destructive commands.
- Prefer a sandbox, disposable branch, worktree, container, or ephemeral environment.

Do not let convenience turn a coding assistant into an unreviewed production operator.

Template: [AI Agent Security Preflight](../templates/security-preflight-template.md)

## Treat retrieved content as untrusted

Repository files, issues, web pages, package metadata, logs, tool output, images, and documents can contain instructions aimed at the model. That is an indirect prompt-injection risk.

Practical defenses:

- State that external content is data, not authority.
- Keep system, team, and task instructions separate from retrieved text.
- Do not follow commands embedded in source files or web content unless the user explicitly authorized them.
- Restrict tools and credentials even if the model appears well-instructed.
- Require human approval for privileged or irreversible actions.
- Review tool calls and outputs, not only the final prose.
- Test workflows with adversarial content before automating them.
- Treat changes to `AGENTS.md`, `CLAUDE.md`, hooks, CI workflows, and tool configuration as security-sensitive persistence.

Prompt wording helps, but it is not a security boundary. OWASP recommends least privilege, clear separation of untrusted content, and human approval for high-risk actions.

## Verify every new dependency

Models can invent package names or suggest a malicious lookalike. Before adding a dependency:

1. Confirm the exact name in the authoritative registry.
2. Confirm the package is maintained and appropriate for the ecosystem.
3. Review ownership, release history, source repository, license, and advisories.
4. Check whether the standard library or an existing dependency already solves the need.
5. Pin or lock the version according to repository policy.
6. Inspect install scripts and transitive risk when the change is sensitive.
7. Run the repository's dependency and vulnerability checks.

Do not install a package merely to discover whether the assistant guessed correctly.

## Review high-risk code explicitly

Strong models still produce insecure code. Apply focused review to:

- Authentication, session, and account recovery flows
- Authorization and multi-tenant boundaries
- SQL, command, template, LDAP, and other injection surfaces
- URL fetching, redirects, SSRF, and webhook validation
- File paths, uploads, archives, and traversal
- Parsing, deserialization, and unsafe reflection
- Cryptography, tokens, randomness, and key management
- Secrets, logs, analytics, and personal data
- Financial calculations and transaction boundaries
- Concurrency, replay, idempotency, and race conditions
- Infrastructure, CI/CD, permissions, and supply chain changes

Ask for a threat model or abuse cases before implementation when the boundary is important. Then validate findings with code paths and tests; a generic security checklist is not proof.

## Keep trust decisions server-side

Watch for generated code that:

- Enforces authorization only in the client or UI
- Trusts hidden fields, headers, filenames, MIME types, or redirect parameters
- Accepts model output as validated structured data
- Builds commands or queries through string concatenation
- Logs tokens, passwords, session IDs, or sensitive payloads
- Uses custom cryptography instead of a reviewed library
- Broadens CORS, IAM, firewall, or sandbox rules to make a test pass
- Silently falls back when a security check fails

Validate at the boundary that owns the resource, using canonicalized data and deny-by-default behavior.

## Protect provenance and licensing

Generated code can resemble public code or reproduce licensed material.

- Use provider controls for public-code matching where available.
- Investigate unusually polished or distinctive blocks.
- Preserve required notices when intentionally adapting code.
- Do not ask a model to reproduce proprietary source you are not authorized to use.
- Follow the organization's policy for attribution, generated content, and software bills of materials.

## Security checklist for agent tasks

Before:

- [ ] Provider and data are approved
- [ ] Secrets and personal data are excluded or safely transformed
- [ ] Agent permissions and network access are minimal
- [ ] High-risk actions require approval
- [ ] Threat boundaries and security acceptance criteria are stated

After:

- [ ] Diff contains no secrets, debug bypasses, or unexpected files
- [ ] Every changed file was reviewed, including lockfiles, CI, hooks, containers, and deployment configuration
- [ ] New packages and configuration keys were verified independently
- [ ] Auth and validation occur at trusted boundaries
- [ ] Negative and abuse cases are tested
- [ ] Static, dependency, and secret checks ran where available
- [ ] Logs and errors do not leak sensitive data
- [ ] A qualified human reviewed high-risk changes

## Sources and further reading

- OWASP Cheat Sheet Series — [Secure Coding with AI](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Coding_with_AI_Cheat_Sheet.html)
- OWASP GenAI Security Project — [Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
- NIST — [Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)
- GitHub Docs — [Risks and mitigations for GitHub Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/risks-and-mitigations)
- GitHub Docs — [Review AI-generated code](https://docs.github.com/en/copilot/tutorials/review-ai-generated-code)

## Key takeaway

Assume the model can be wrong, the content it reads can be hostile, and the permissions it holds are real.
