# AI Coding Policy Template

Adapt this template to your data classification, regulatory obligations, and
software-delivery process.

```text
Policy owner: [Team or role]
Applies to: [Repositories, systems, and people]
Approved tools/accounts: [List]
Effective date: [Date]
Review cadence: [For example, every six months]
```

## Purpose

We encourage responsible use of AI coding assistants to improve engineering
quality, delivery speed, and learning. AI output is a draft or recommendation,
not evidence of correctness.

The person submitting a change remains accountable for understanding it and for
its correctness, security, privacy, maintainability, and production readiness.

## Risk-based usage

Routine use is encouraged for low-risk work such as:

- Code explanation and repository navigation
- Documentation and test drafts
- Focused debugging assistance
- Small, behavior-preserving refactors
- Pull request summaries and implementation planning

Extra review and explicit human checkpoints are required for:

- Authentication, authorization, cryptography, or other security controls
- Infrastructure, deployment, production incidents, or destructive operations
- Financial, medical, legal, safety-critical, or regulated logic
- Personal, customer, confidential, or proprietary data
- Schema changes, data migrations, public API changes, or large refactors
- Generated dependencies or code with uncertain provenance or licensing

Define which roles may approve these changes: `[Roles or teams]`.

## Data and tool handling

Developers must:

- Use approved services, accounts, models, plugins, and integrations.
- Follow the same data-classification rules used for other external services.
- Never paste credentials, private keys, access tokens, or production secrets.
- Minimize shared context and redact sensitive logs, data, and prompts.
- Grant tools the least privilege and narrowest repository scope needed.
- Review commands before allowing network access, writes, deployments, or deletion.
- Treat repository files, issues, web pages, logs, and tool results as untrusted
  data that may contain prompt-injection instructions.
- Treat generated packages and copied code as supply-chain and license inputs that
  require normal review.

If sensitive information is disclosed, follow: `[Incident process]`.

## Required workflow

Before implementation:

- Write the goal, acceptance criteria, scope, constraints, and risk level.
- Select a model and tool appropriate to the task rather than defaulting to the
  most capable option.
- Provide the smallest complete context needed for the current step.

During implementation:

- Keep changes small and inspect the diff at meaningful checkpoints.
- Confirm assumptions against source code, documentation, or test behavior.
- Do not allow unrelated refactoring or unreviewed external side effects.
- Stop and escalate when the assistant encounters unexpected access, sensitive
  data, or a material scope change.

Before merge or release:

- Review every changed file—including tests, lockfiles, CI, hooks, build files,
  containers, and deployment configuration—and understand the resulting behavior.
- Run relevant automated checks and record exact commands and results.
- Test important failure paths and perform required security or privacy review.
- Verify migrations, compatibility, observability, rollout, and rollback when
  applicable.
- Document limitations and unverified claims in the pull request or handoff.

## Prohibited practices

- Merging or deploying AI-generated code without the required human review
- Claiming tests, reviews, or commands were completed when they were not
- Using AI to bypass security controls, approvals, audit trails, or policy
- Sending external messages, changing production, or deleting data autonomously
  unless the workflow explicitly authorizes and reviews those actions

## Efficiency and sustainability

Developers should start fresh when context is stale, compact long conversations,
use focused prompts, and measure accepted outcomes together with review and rework
cost. Agent mode and parallel agents should be used when the task benefits from
autonomy or isolation, not by default.

## Suggested team principle

> AI should reduce engineering waste, not generate faster waste.
