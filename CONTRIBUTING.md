# Contributing

Contributions should make this guide more correct, practical, and easy to verify.

## Content principles

- Optimize for correct, reviewable engineering outcomes—not novelty or prompt length.
- Prefer concrete workflows, examples, failure modes, and checks over generic advice.
- Keep one canonical explanation for each concept and link to it elsewhere.
- Separate cross-tool principles from product-specific behavior.
- Do not imply a model, agent, or automated review replaces human accountability.
- Avoid universal rules when the right process depends on risk and reversibility.

## Source policy

For facts that can change—models, commands, configuration keys, prices, quotas, plan behavior, preview status, data policy, or product capabilities:

1. Use a primary official source.
2. Verify the exact product surface, version, plan, and platform.
3. Add a `Last verified: YYYY-MM-DD` note to a product-specific guide.
4. Link the page that directly supports the claim.
5. Separate documented fact from inference.
6. Remove or qualify unsupported percentages and anecdotes.

Community posts can help discover a question but should not be the only support for a product fact when official documentation exists.

For secure-development advice, prefer established primary guidance such as OWASP, NIST, language/ecosystem security documentation, and official provider responsible-use material.

## Commands and examples

- Verify commands against the current tool or official reference.
- Use placeholders for model names when the example is about configuration structure rather than a specific recommendation.
- Do not include real secrets, personal data, production identifiers, or proprietary code.
- Make expected outputs and failure behavior explicit.
- Keep Markdown examples internally consistent and copy-pasteable where practical.
- Never recommend ignoring or deleting lockfiles as a token-saving technique.

## Adding a new guide

A focused guide should usually include:

- The problem and why it matters
- A recommended workflow
- Common failure modes
- A reusable prompt or checklist when useful
- Links to related guides and templates
- Primary sources for volatile or security-sensitive claims
- A short key takeaway

Add it to the appropriate `README.md` navigation section and check that existing pages do not already own the topic.

## Validation

Before submitting:

- Read the complete rendered diff.
- Run `git diff --check`.
- Confirm every relative Markdown link resolves.
- Check code fences and tables render correctly.
- Verify dated product claims against current official sources.
- Check that examples distinguish commands actually run from expected or illustrative output.
- Keep unrelated formatting changes out of the contribution.

## Benchmark contributions

Follow [the benchmark protocol](benchmarks/README.md). Freeze the repository commit, environment, task packet, and evaluator criteria; run repeated trials; and distinguish acceptance from verified correctness.
