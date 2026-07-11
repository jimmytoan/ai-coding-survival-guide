# Small Model Use Cases

Small and cheaper models are often enough when the task is narrow, the evidence is
local, and mistakes are easy to detect.

## Good small-model tasks

- Explain or summarize one file or function
- Draft documentation from supplied facts
- Generate table-driven tests for a clear contract
- Rename variables or perform a mechanical transformation
- Classify logs, issues, or review comments
- Create a checklist or pull request summary from a diff
- Find obvious duplication in a small snippet
- Convert structured data between simple formats

## Example prompts

```text
Explain this function's inputs, outputs, side effects, and error behavior.
Use only the supplied code and do not suggest refactoring.
```

```text
Draft release notes from this diff. Mention only user-visible behavior that the
diff demonstrates. Put uncertain items under “Needs confirmation.”
```

```text
Generate table-driven unit tests for this pure helper. Cover documented boundaries
and invalid input. Do not change production code or invent undocumented behavior.
```

```text
Classify these CI failures by likely component. Preserve each test name and error
message, and mark ambiguous cases unknown.
```

## Escalate when the task changes

Use a stronger model, a specialist review, or smaller investigation steps when the
task requires:

- Architecture or cross-system trade-offs
- Security, privacy, financial, or destructive decisions
- Reasoning across many unfamiliar files
- Ambiguous requirements or conflicting evidence
- Complex debugging, concurrency, migration, or incident analysis
- A final review where a missed defect would be expensive

Model size does not replace verification. Review generated output and run the same
checks regardless of which model produced it.
