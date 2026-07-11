# Dependency Evaluation Prompt

Use this before installing an AI-suggested package.

```text
Evaluate whether this dependency should be added. Do not install or edit yet.

Need:
[Behavior the project requires]

Suggested package:
[Exact name and proposed version]

Environment:
- Ecosystem/runtime: [Version]
- Existing manifest and lockfile: [Paths]
- Constraints: [License, bundle size, support, security, etc.]

Inspect the repository first, then verify current package facts using the
authoritative registry, source repository, advisories, and official documentation.

Return:
1. Whether the exact package and version exist
2. Owner, maintenance, release, and deprecation signals
3. License, provenance, advisories, install scripts, and transitive risk
4. Runtime/framework compatibility and operational cost
5. Standard-library or existing-repository alternatives
6. Exit or removal cost
7. Recommendation: add / reuse existing / implement locally / investigate
8. Primary-source links and verification date

Separate documented facts from inference. Never install a plausible package name
only to discover whether it exists.
```
