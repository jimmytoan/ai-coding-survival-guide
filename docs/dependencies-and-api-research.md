# Dependencies and API Research

AI assistants are good at recalling common APIs and bad at knowing whether that memory matches the version, plan, platform, or package in front of you.

Treat library names, methods, flags, configuration keys, limits, and pricing as claims to verify.

## Inspect the local environment first

Before searching the web, establish:

- Language and runtime version
- Installed package and tool versions
- Manifest and lockfile state
- Framework adapters or wrappers used by the repository
- Provider, plan, platform, and feature flags
- Existing implementation patterns and compatibility constraints

The repository may intentionally use an older interface. “Current latest” is not automatically the correct target.

## Use a source hierarchy

Prefer sources in this order:

1. Installed types, help output, source, or generated API schema
2. Official version-specific documentation
3. Official release notes, migration guides, and package registry
4. Maintainer repository issues or discussions
5. Reputable secondary explanations

Use community answers to discover terminology, then verify material claims against a primary source or executable behavior.

## Freeze volatile facts

For changing product behavior, record:

- Exact product and surface
- Version or plan
- Platform or provider
- Date verified
- Direct primary-source link
- Preview, beta, or deprecation status
- What remains uncertain

Avoid presenting a current price, quota, model, command, or preview feature as a timeless best practice.

## Verify an API claim

If an assistant proposes a method or setting:

1. Check whether it exists in the installed version.
2. Confirm its exact signature, types, defaults, and failure behavior.
3. Check whether it is deprecated or surface-specific.
4. Read the surrounding official guide, not only a search snippet.
5. Build a minimal compile or runtime probe when ambiguity remains.
6. Add a test that protects the behavior the application relies on.

Never add compatibility shims around an invented API before confirming the API exists.

## Evaluate a new dependency

Ask whether the dependency is necessary before asking which one to add.

Check:

- Exact name and owner in the authoritative registry
- Source repository and release provenance
- Maintenance, release cadence, and deprecation status
- Compatibility with the project's runtime and framework
- License and organizational policy
- Known vulnerabilities and malicious-package reports
- Transitive dependency and install-script surface
- Runtime, bundle, build, and operational cost
- Existing repository or standard-library alternatives
- Lockfile and reproducible-install impact
- Exit cost if the package becomes unsuitable

For a tiny, stable operation, a local implementation may be safer. For security primitives, parsers, protocols, and complex standards, a mature reviewed library is usually safer than generated code.

## Defend against package hallucination

Models may suggest plausible names that do not exist. Attackers can publish those names later or use lookalikes.

- Never install first and investigate second.
- Copy package names from the registry, not from prose.
- Check spelling, namespace, owner, and repository link.
- Preserve and review lockfile changes.
- Treat unexpected install scripts and large transitive trees as risk signals.
- Use the ecosystem's advisory and provenance tooling.

## Research efficiently

Give the assistant a bounded question:

```text
Determine whether version 4.2 of [library] supports [behavior] in [runtime].

Inspect the local manifest and lockfile first. Then use official version-specific
documentation and release notes. Cite the exact source for the API signature,
defaults, and migration concern. Separate documented facts from inference.
Do not edit or install anything.
```

Search for the smallest fact that unlocks the task. A broad “research this framework” request produces noise and dates poorly.

Template: [Dependency Evaluation](../templates/dependency-evaluation-prompt.md)

## Preserve research in the repository

If a verified fact is load-bearing, encode it where future work will find it:

- A version constraint and lockfile
- A compatibility or contract test
- A short comment linking a surprising external rule
- An architecture decision record
- A migration note with the verification date
- A project instruction only if it affects most tasks

Do not rely on an old chat as the only record of why a dependency or workaround exists.

## Research report checklist

- [ ] Local version and surface identified
- [ ] Primary source used for material claims
- [ ] Exact API, default, and limitations verified
- [ ] Source date/version and preview status recorded
- [ ] Package identity, license, and advisories checked
- [ ] Lockfile and transitive changes reviewed
- [ ] Claims separated from inference
- [ ] Minimal executable check added where useful

## Key takeaway

Model memory is a lead. Installed behavior, primary documentation, and executable checks are evidence.
