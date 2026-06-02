# AI Coding Policy Template

## Purpose

We encourage responsible use of AI coding assistants to improve engineering productivity, code quality, and learning.

The goal is not to reduce AI usage.

The goal is to use AI effectively, safely, and sustainably.

## Recommended usage

AI can be used for:

- Code explanation
- Documentation drafts
- Test generation
- Debugging assistance
- Refactoring suggestions
- Pull request summaries
- Implementation planning
- Learning unfamiliar code

## Careful usage required

Extra review is required for:

- Security-sensitive code
- Authentication
- Authorization
- Infrastructure
- Financial logic
- Production incident changes
- Data migrations
- Privacy-sensitive workflows
- Large multi-file refactoring

## Token discipline

Developers should:

- Use the right model for the task.
- Try smaller models for simple work.
- Use stronger models for complex or risky work.
- Avoid unnecessary large context.
- Start new chats when context becomes stale.
- Compact long conversations when possible.
- Ask for a plan before implementation.
- Keep generated changes small and reviewable.
- Avoid using agent mode for tiny tasks.

## Review expectation

AI-generated code must be reviewed like human-written code.

The author remains responsible for correctness, security, maintainability, and production readiness.

## Suggested team principle

> AI should reduce engineering waste, not generate faster waste.
