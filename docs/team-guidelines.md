# Team Guidelines for AI Coding Assistants

AI coding assistants are engineering tools. Teams should make their safe path clear, their high-risk boundaries explicit, and their outcomes observable.

> The developer who submits a change remains accountable for understanding, verifying, and operating it.

Use the [AI Coding Policy Template](../templates/ai-coding-policy-template.md) to turn this guide into an organization-specific policy.

## Define approved use

Document:

- Approved tools, providers, models, and surfaces
- Accounts and workspace types developers must use
- Allowed repository and data classifications
- Retention, training, regional, and contractual requirements
- Whether local, cloud, connector, or API access is permitted
- Support and exception contacts

Do not make developers infer policy from procurement status or a chat message.

## Classify data before sharing it

State how the team handles:

- Credentials, tokens, keys, and certificates
- Customer and employee personal data
- Production logs, traces, screenshots, and database records
- Confidential source and unreleased product information
- Regulated or contract-restricted data
- Third-party code and licensed material

Prefer synthetic, minimal examples. Redaction must cover files and tool output as well as prompts. Exposed secrets must be rotated and reported; deleting the conversation is not enough.

## Use risk-based controls

| Risk | Examples | Minimum controls |
|---|---|---|
| Low | Docs, formatting, mechanical rename | Normal review and focused check |
| Medium | Bug fix, feature, dependency update | Acceptance criteria, scoped diff, tests, human review |
| High | Auth, payments, privacy, migrations, infrastructure | Threat/risk review, least privilege, staged plan, rollback, specialist approval |

Adjust the classification to the product. A “small” authorization diff can be high risk.

## Set autonomy and approval boundaries

Define what agents may do without additional approval and what always requires it.

Common approval gates:

- New production dependencies
- Public API or schema changes
- Network access or external data retrieval
- Destructive Git or filesystem operations
- Database mutation or backfill
- Infrastructure, deployment, or credential changes
- Messages, issue updates, pull requests, or publication
- Access to secrets or production systems

Use sandboxes, limited writable paths, read-only credentials, and separate environments. Prompt instructions do not replace access controls.

## Require an engineering task contract

Meaningful tasks should include:

- Goal and expected behavior
- Acceptance criteria
- Constraints and non-goals
- Relevant entry points or reproduction
- Verification expectations
- Risk and data classification

Use the [Task Brief](../templates/task-brief-template.md).

## Require evidence at handoff

Every AI-assisted change should report:

- What changed and why
- Exact commands run and results
- Manual or specialized checks
- Checks skipped and why
- New dependencies, permissions, migrations, or external effects
- Known limitations and residual risk

Do not accept “the agent says tests pass” without reproducible command evidence.

## Review generated code for AI-specific risks

In addition to normal review, check for:

- Invented APIs, flags, and configuration keys
- Hallucinated or lookalike packages
- Weakened, skipped, circular, or deleted tests
- Unrelated scope expansion
- Secrets or sensitive data in code and logs
- Changes to lockfiles, CI, build, hooks, containers, and deployment files
- Public-code or license concerns
- Client-side-only security checks
- Comments that promise behavior the code does not enforce

For high-risk code, use a fresh independent review plus the appropriate specialist. See [Reviewing AI-Generated Code](reviewing-ai-generated-code.md).

## Keep verification layered

The team should publish canonical commands for:

- Formatting, linting, types, and build
- Focused and full tests
- Security, secret, and dependency scans
- Contract and integration checks
- Migration validation
- Accessibility, visual, performance, or operational checks where relevant

CI should enforce mechanical policy. Humans should focus on intent, architecture, domain behavior, and risk.

## Govern dependencies and provenance

Before adding a suggested package, verify its exact registry identity, ownership, maintenance, license, compatibility, advisories, transitive risk, and lockfile changes.

Define policy for public-code matching, attribution, software bills of materials, and generated content. See [Dependencies and API Research](dependencies-and-api-research.md).

## Manage persistent agent configuration

Treat project instructions, skills, hooks, workflows, and tool configuration as security-relevant code:

- Review changes through normal ownership rules.
- Keep organization policy outside contributor-controlled instructions.
- Avoid secrets and volatile product facts.
- Verify precedence and what actually loads.
- Enforce critical rules through permissions and CI.

## Measure outcomes, not adoption theater

Track by task type and risk:

- Verified and first-pass success
- Review and rework minutes
- Lead time
- Escaped defects and rollbacks
- Total credits, quota, or API spend
- Scope violations and policy exceptions
- Incidents and near misses

Do not reward raw lines generated, prompt count, or percentage of developers using a tool.

See [Cost and Outcome Observability](cost-observability.md).

## Create an incident and exception path

Document how to report:

- Secret or sensitive-data exposure
- Unsafe or unauthorized agent action
- Malicious package suggestion or installation
- Prompt-injection or compromised tool behavior
- License or provenance concern
- AI-assisted defect or near miss
- Needed policy exception

Favor learning over concealment. Update automation, tests, documentation, and permissions when the same failure could recur.

## Suggested team message

```text
Use AI coding assistants where they improve delivery, but keep the same ownership
and quality bar as any other change.

For meaningful work:
- Define expected behavior and acceptance criteria.
- Share only data approved for the tool and workspace.
- Keep agent permissions and scope as narrow as practical.
- Verify packages, APIs, tests, and tool claims independently.
- Run the repository's required checks and report exact results.
- Review every changed file, including tests, lockfiles, CI, and configuration.
- Get independent or specialist review for high-risk changes.

If an action affects production, data, credentials, dependencies, external
systems, or public interfaces, follow the documented approval path.

AI can accelerate the work. The submitting developer still owns the result.
```

## Key takeaway

A useful policy makes ordinary AI-assisted work easy, sensitive actions explicit, and human accountability non-negotiable.
