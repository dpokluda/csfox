# CsFox

## Overview

CsFox is an ongoing experiment to build a simple C# interpreter for DOS FoxPro programs with text-based screens.
The project is in its initial setup stage; no interpreter is implemented yet.

## Goal

Run a useful subset of FoxPro source programs on Windows, macOS, and Linux without requiring an original FoxPro installation.
Start with small programs that combine database access, program logic, and interactive text screens, then expand compatibility through working examples.
Full FoxPro compatibility is not an initial goal.

## Approach

Build incrementally in C# and .NET, implementing only the language, database, and screen behavior needed by concrete examples.
Use independently written PRG samples, synthetic DBF data, indexes, and screen examples to demonstrate behavior and guide compatibility tests.
Favor simple, readable code, long-term maintainability, test-driven development, and classical, state-based unit tests over mock frameworks.

Use [GitHub Spec Kit](https://github.com/github/spec-kit) to define small features, plan their implementation, and break the work into tasks.
The repository includes Copilot skills under `.github/skills/` and shared templates and PowerShell scripts under `.specify/`.
Repository-wide engineering instructions are in [AGENTS.md](AGENTS.md); project principles are in the [constitution](.specify/memory/constitution.md).

## Development tooling

Spec Kit supports the development workflow; it is not an application runtime dependency.
Follow the [official installation guide](https://github.github.io/spec-kit/installation.html) for Python and uv prerequisites, then install the latest CLI:

```text
uv tool install specify-cli
```

Show the installed version and check for newer releases without changing the installation:

```text
specify version
specify self check
```

To upgrade an installation made with the uv command above:

```text
uv tool upgrade specify-cli
```

The repository is already initialized, so do not rerun `specify init` after cloning.
Its checked-in scripts use PowerShell (`pwsh`) on all platforms.
CLI upgrades do not refresh the checked-in Spec Kit skills and templates; update those separately and review the resulting changes.
