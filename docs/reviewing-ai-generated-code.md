# Reviewing AI-Generated Code

AI-generated code deserves the same standards as human-written code and a few extra checks for model-specific failure modes.

The author remains accountable for the change. “The AI wrote it” is neither evidence nor an exception to review.

## Review inputs

Give the reviewer:

- The original requirement and acceptance criteria
- The actual diff
- Relevant surrounding code and tests
- Repository conventions and architectural constraints
- Verification results, including skipped checks
- Risk areas and unresolved assumptions

Do not give only the implementation chat or the assistant's summary. Those explain intent but can repeat the same mistaken assumptions as the code.

## Review in passes

### 1. Intent and scope

- Does the change solve the requested problem?
- Is every changed file necessary?
- Are non-goals still excluded?
- Is the behavior change documented where users or callers will notice it?

### 2. Correctness

- Trace important happy and failure paths.
- Check boundaries, empty states, invalid input, and partial failure.
- Check state transitions, ordering, retries, idempotency, and concurrency.
- Check compatibility with existing data, callers, and configurations.
- Verify error handling preserves useful context.

### 3. Security and privacy

- Authentication and authorization at every trust boundary
- Validation, encoding, and injection risks
- Secret and personal-data handling
- File, URL, network, and deserialization boundaries
- New dependencies, permissions, and external calls
- Logging, telemetry, and error disclosure

See [Security and Privacy](security-and-privacy.md).

### 4. Tests and evidence

- Does a regression test fail without the fix?
- Do tests assert behavior rather than implementation details?
- Were assertions weakened, snapshots blindly updated, or tests skipped?
- Are the exact commands and results available?
- Is a higher-level or manual check needed?

See [Verification and Testing](verification-and-testing.md).

### 5. Maintainability and operations

- Does it follow an existing pattern for a good reason?
- Is the simplest correct abstraction used?
- Are names and comments accurate?
- Are migrations, feature flags, metrics, logs, and rollback covered?
- Could the change be smaller without losing correctness?

## AI-specific failure modes

Look explicitly for:

- **Invented APIs:** methods, flags, configuration keys, or platform behavior that do not exist
- **Suspicious dependencies:** nonexistent, abandoned, malicious, mistyped, or unnecessary packages
- **Scope creep:** unrelated cleanup hidden inside a feature or bug fix
- **Pattern imitation:** code matches local style but violates the actual invariant
- **Test laundering:** skipped tests, weaker assertions, or mocks that manufacture a pass
- **Edge-case amnesia:** only the example in the prompt works
- **Compatibility gaps:** current behavior works but old data, callers, or rollback do not
- **Comment confidence:** comments make claims the code does not guarantee
- **Duplicate solutions:** a new helper or abstraction already exists elsewhere
- **Security theater:** a check exists in the UI or client but not at the trusted boundary

Verify all new library names, versions, and configuration keys against the registry or primary documentation.

## Ask for actionable findings

A useful review finding contains:

- Severity
- File and precise location
- The failing scenario
- Evidence from the code
- User or system impact
- A minimal correction or test

Example:

```text
High — src/routes/redirect.ts:42

The same-origin check compares the raw string with "/", but encoded values are
decoded later. A target such as `%2F%2Fevil.example` passes this check and becomes
a protocol-relative URL at redirect time.

Add validation after canonical parsing and a regression test for encoded and
backslash variants.
```

Avoid unactionable style preferences and hypothetical issues with no reachable scenario.

Template: [Code Review Prompt](../templates/code-review-prompt.md)

## Use independent review

For medium- and high-risk work, review in a fresh context or with a reviewer that did not generate the implementation. This reduces shared-assumption bias.

AI review is useful for:

- A fast pre-review before requesting a human
- Repeated checklist checks
- Finding affected call sites
- Generating adversarial cases
- Comparing the diff with explicit acceptance criteria

It does not replace:

- A domain owner for business rules
- A security reviewer for critical boundaries
- An operator for deployment and rollback risk
- A human who can take responsibility for the merge

GitHub's own responsible-use guidance warns that AI review can miss problems, report false positives, and suggest insecure or incorrect fixes. Treat review output as candidate findings, then validate each one.

## Control review size

Large diffs reduce both human and model review quality.

- Separate mechanical changes from behavioral ones.
- Review generated files separately from their source.
- Split migrations into safe stages.
- Ask focused passes instead of “find everything.”
- Re-run review on the final diff after fixes; earlier findings may be stale.

## Sources and further reading

- GitHub Docs — [Review AI-generated code](https://docs.github.com/en/copilot/tutorials/review-ai-generated-code)
- GitHub Docs — [Responsible use of GitHub Copilot agents](https://docs.github.com/en/copilot/responsible-use/agents)
- NIST — [Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)

## Key takeaway

Review the requirement, the diff, and the evidence independently. Plausible code is only a candidate for correctness.
