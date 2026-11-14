# CsFox agent instructions

These are the repository-wide instructions for designing, implementing, debugging, testing, reviewing, and documenting CsFox.
They apply whether or not an agent uses Spec Kit and do not require personal or global instruction files.
Read the [README](README.md) for the project overview and the [constitution](.specify/memory/constitution.md) for project principles and constraints.
Use the files in the current checkout, including local changes.
Report missing or conflicting guidance rather than silently assuming its contents.

## Project context and development commands

CsFox is an experimental C# and .NET interpreter for DOS FoxPro source programs with text-based screens.
The execution goal is Windows, macOS, and Linux, without an original FoxPro installation.
Implement a useful subset through small examples rather than attempt complete compatibility immediately.

`csfox.slnx` contains `src/csfox`, a .NET 10 console application, and `tests/csfox.tests`, the single MSTest project.
Keep solution/project filenames and project directories lowercase; retain conventional C# namespace, type, and source-file casing.
The root namespaces are `CsFox` and `CsFox.Tests`.
Keep engine and host code in `csfox` until a concrete need justifies a separate library.
Run `dotnet build` and `dotnet test` from the repository root; `global.json` selects the .NET 10 SDK family.
The console entry point is empty, the test project has no tests yet, and `samples` holds only documentation for the planned samples.
Keep the README's project layout, prerequisites, and commands current as functionality is added.
Keep local development usable without requiring a particular IDE.

Spec Kit skills live under `.github/skills/`; shared templates and PowerShell scripts live under `.specify/`.
The installed scripts require PowerShell (`pwsh`), not Windows-only Windows PowerShell.
With the Specify CLI installed, these commands inspect the development tooling:

```text
specify version
specify check
```

`git diff --check` checks changed tracked files for whitespace errors; it is not an application test.
None of these commands demonstrates interpreter functionality or cross-platform execution.

## Design and change scope

- Build the smallest correct solution for the present requirement, prioritizing readability and long-term maintainability.
- Read relevant code and tests and look for existing helpers before introducing new logic.
- Avoid speculative interfaces, wrappers, frameworks, configuration, factories, dependencies, and project boundaries.
  Abstractions at real boundaries or that clarify the design are useful; abstractions solely for one test are not.
- Prefer cohesive helpers and clear ownership over mechanical DRY.
  Small duplication is preferable to indirection that hides behavior.
- Keep control flow understandable and separate logical steps with blank lines.
  Simplify confusing code before adding comments to explain it.
- Fix the root cause and directly coupled issues, without unrelated cleanup or formatting churn.
  Preserve unrelated behavior and user-authored changes, and update directly affected documentation.
- Explain meaningful tradeoffs and distinguish facts, assumptions, and recommendations.
  Ask when an unresolved choice materially affects correctness; otherwise state reasonable assumptions and proceed.

## C# conventions

Use the [.NET C# coding conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
as the baseline and follow repository EditorConfig and analyzer settings when present.
The preferences below are CsFox choices, not claims about universal .NET rules.
Do not import another project's corporate style or copyright headers.

- Use file-scoped namespaces for new C# files.
- Use an explicit `Main` entry point rather than top-level statements.
- Prefer block-bodied methods, including `Main`, over expression-bodied methods.
- Always use braces for branches, loops, and other control-flow statements.
- Use meaningful semantic names and established project terminology.
  Name new handwritten test doubles `Test*`.
- Target roughly 120 characters per line, allowing up to 160 when clearer.
  Keep short signatures, calls, throws, and assertions on one line; wrap naturally rather than mechanically one argument per line.
- Put functional constructor dependencies first, followed by configuration and support dependencies where sensible.
  Keep metrics near the end and the logger last among required parameters; optional parameters must still follow required parameters.
- Match injected field declarations and independent constructor assignments to parameter order where practical.
  Keep constants, static members, and runtime state separate, and preserve guard order, initialization dependencies, and side effects.

Apply these conventions to new or relevant changed code, not as a reason to reformat unrelated code.

## Testing

For new behavior, write an observable failing test first, implement the smallest correct solution, then refactor with the tests green.
For bug fixes, first reproduce the failure with a regression test that fails for the intended reason.
If test-first development is impractical, explain why and provide another reproducible validation path.
Documentation and empty scaffolding do not need artificial behavior tests.

Use classical, state-based unit tests through public contracts, with small handwritten fakes, stubs, or recording spies when needed.
Do not add mocking frameworks or expose implementation details solely for tests, including through `InternalsVisibleTo`.
Interaction assertions are appropriate when the interaction itself is the contract.
Keep tests deterministic, independent, and nonduplicative.
Avoid tests of third-party library behavior, trivial accessors, or implementation details that do not protect an observable contract.
Never weaken assertions or skip failures merely to obtain green results.

Use synthetic fixtures and disposable working copies for tests and sample runs that modify data.
Do not modify committed sample data during normal execution.
Do not use test doubles where they would hide the file-format, interpreter, or terminal behavior being tested.

## Documentation and writing

Document public production types and members with meaningful XML documentation, including parameters, type parameters, and return values where applicable.
Use `inheritdoc` only when the inherited contract actually supplies the required documentation.
Explain the purpose and important ownership or lifecycle invariants of major internal components.
Ordinary test classes and methods do not require API documentation.

Use plain language, active voice, and focused paragraphs.
Comments explain non-obvious intent, invariants, or tradeoffs rather than restate identifiers or syntax.
Avoid boilerplate, unsupported claims, em dashes, and inflated language.
For maintained Markdown, prefer one sentence per physical line, wrapping unusually long sentences near 160 characters.
Use repository-relative links for local documents and canonical URLs for external references, not contributor-specific machine paths.

## Scripts, portability, and errors

Use portable .NET APIs and dependencies that support the constitution's execution targets.
Account for host path separators, filesystem case sensitivity, text encodings, line endings, and terminal or keyboard differences.
Do not assume DOS application semantics and host operating-system behavior are interchangeable.
Keep unavoidable platform-specific behavior isolated and explicit, without adding a portability framework ahead of a concrete need.

Keep handwritten PowerShell scripts simple, using standard cmdlets, clear Verb-Noun functions, and explicit error handling.
Prefer plain `.ps1` files; use a shared dot-sourced helper when several entry points need the same code.
Give entry points comment-based help, examples, and a `param(...)` block; use inline parameters for ordinary helper functions.
Set `$ErrorActionPreference = 'Stop'`, check `$LASTEXITCODE` after native commands, and report actionable failures.
Do not restyle generated Spec Kit scripts or templates; use their authoring or generation workflow when a change is needed.

Validate input before side effects, preserve original failure information, and never disguise failures as success or silent defaults.
Keep resource ownership, disposal, cancellation, and any retries explicit where relevant.
Do not expose credentials, sensitive data, or restricted third-party material in source, diagnostics, or published artifacts.

## Validation and agent safety

Run the smallest existing checks that cover the change, then expand when affected callers or integration boundaries warrant it.
Restore or install dependencies only after manifest changes or a concrete missing-dependency failure.
Documentation-only changes do not require application builds or tests unless documentation checks exist.
Report actual outcomes and unverified scopes honestly; a successful build or one host's tests do not prove compatibility on other hosts.

Do not stage, commit, push, change branches or refs, or perform destructive operations unless explicitly requested.
Do not add AI/tool attribution or session metadata to commit messages.
Preserve existing work and request approval before changing shared, production, or customer environments.
Neither repository instructions nor a generated plan grant permission for those actions.
