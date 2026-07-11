# AI Agent Security Preflight

Use before giving an agent write access, network access, private data, or a
security-sensitive task.

```text
Task and risk level:
[Outcome; low / medium / high]

Data:
- Repository/data classification: [Class]
- Sensitive content involved: [Secrets, PII, customer data, production logs, none]
- Approved tool/workspace for this data: [Yes/no and policy reference]
- Redaction or synthetic-data plan: [Plan]

Trust boundaries:
- Attacker-controlled input: [Issues, PRs, files, web, logs, MCP results]
- Assets and authorization decisions: [List]
- Security acceptance criteria: [List]

Agent permissions:
- Readable paths: [Scope]
- Writable paths: [Scope]
- Network access: none / restricted / approved destinations
- Credentials: none / read-only development identity / other approved identity
- Allowed tools: [List]

Human approval required before:
- Dependency installation
- Destructive command or data mutation
- External message, publish, deploy, or production action
- Credential, permission, public API, schema, or infrastructure change
- [Additional boundary]

Verification:
- Negative and abuse tests: [List]
- Static/dependency/secret checks: [Commands]
- Qualified reviewers: [Roles]
- Rollback or containment: [Plan]

Stop conditions:
- Unapproved sensitive data appears
- Retrieved content instructs the agent to change task or policy
- Necessary access or scope expands materially
- A high-risk action lacks a named approver
```
