<!--
Sync Impact Report (remove before committing)
Version change: 1.0.0 -> 1.1.0
Rationale: add cross-platform execution; retain existing principles while
moving detailed engineering and agent instructions to repository-root AGENTS.md.
Modified principles (titles unchanged):
- I. Simple, Maintainable Design: retain the principle; move operational detail.
- II. Consistent .NET Engineering: reference the canonical engineering guide.
- III. Test-Driven, Behavior-Focused Development: retain test-first and
  behavior-focused principles; move detailed testing practices.
- V. Reproducible, Shareable Examples: move local tooling instructions.
Added principle: VI. Cross-Platform Execution.
Added sections: none beyond the new principle.
Removed sections: none.
Deferred placeholders: none.
Related files updated separately: AGENTS.md, .github/copilot-instructions.md,
and README.md. No Spec Kit templates or skills changed.
-->

# CsFox Constitution

Read and follow the repository-root [AGENTS.md](../../AGENTS.md) alongside this constitution.
It defines the detailed engineering and agent instructions; this document defines project principles, scope, and governance.

## Core Principles

### I. Simple, Maintainable Design

Build the smallest correct solution for the current requirement.
Optimize for readability and long-term maintainability, not cleverness or minimum line count.
Every abstraction, dependency, configuration option, and project boundary must serve a concrete need.
Do not introduce speculative infrastructure; small duplication is preferable to unnecessary indirection.

### II. Consistent .NET Engineering

Use standard .NET guidance with the explicit CsFox conventions documented in `AGENTS.md`.
Production code must have understandable contracts, readable structure, and clear ownership of state and resources.
Keep detailed style, documentation practices, and mechanical rules in the engineering guide and applicable tooling configuration.

### III. Test-Driven, Behavior-Focused Development

Develop new behavior and bug fixes test-first: demonstrate the failure, implement the smallest correct change, then refactor with the tests green.
If test-first development is impractical, explain why and provide a reproducible alternative check.
Protect observable public contracts with meaningful, deterministic, classical tests rather than tests coupled to implementation details.
Follow the detailed testing practices in `AGENTS.md` and never weaken assertions merely to obtain a passing result.

### IV. Evidence-Based Compatibility

Add language, database, index, and text-screen behavior through bounded examples with explicit expected outcomes.
Distinguish verified compatibility from intended behavior, assumptions, and unsupported features.
Do not claim FoxPro compatibility merely because a program parses or an implementation compiles.
Record the evidence or rationale for compatibility decisions and preserve those decisions in meaningful regression cases.
Report unsupported behavior, malformed input, and runtime failures explicitly rather than silently succeeding or substituting defaults.

### V. Reproducible, Shareable Examples

Public samples and fixtures must be independently authored or have verified redistribution rights and must use synthetic data.
Keep credentials, personal data, and restricted third-party material out of source, diagnostics, and published artifacts.
Describe sample inputs, expected data changes, and observable screen behavior so that results can be reproduced.
Tests and sample runs that modify data must use disposable working copies rather than modify the committed fixtures.

### VI. Cross-Platform Execution

The supported execution subset must run on Windows, macOS, and Linux without requiring an original FoxPro installation.
Required runtime dependencies must support all three platforms; a Windows-only runtime, UI framework, or native library cannot be a prerequisite.
Keep necessary host-specific behavior isolated without making the supported workflows unavailable on another target platform.
Plans affecting files, indexes, text, screens, or keyboard input must identify platform-sensitive assumptions and how they will be validated.
Support claims require relevant execution evidence on each claimed platform; compilation or tests on one host do not establish support on the others.

## Project Scope

CsFox is an experimental interpreter in C# and .NET for DOS FoxPro source programs with text-based screens.
The initial goal is a useful subset, not a complete FoxPro replacement or binary-compatible runtime.
Grow that subset through small, working examples without requiring an original FoxPro installation.
Choose database implementation, interpreter components, and project boundaries during feature planning based on current needs.
A future compiler, virtual machine, or implementation in another language is not a reason to add infrastructure now.

## Development Workflow

Use GitHub Spec Kit to define a bounded feature, resolve material ambiguities, plan its implementation, and produce actionable tasks before implementation.
Specifications must state observable acceptance criteria and scope exclusions.
Plans must identify relevant constitution constraints, including cross-platform execution, and explain meaningful tradeoffs.
Review implementation against the acceptance criteria and record what remains unverified.
Keep specifications and directly affected documentation consistent with deliberate behavior changes.
Use `AGENTS.md` for day-to-day engineering, validation, and agent safety rules.

## Governance

This constitution defines the project's durable engineering principles.
`AGENTS.md` defines the repository-wide engineering instructions independently of Spec Kit.
Maintain both documents together without duplicating detailed rules or depending on personal instruction files.
Tooling configuration, specifications, and plans must remain consistent with these principles and instructions.
Review changes for compliance, including unnecessary complexity, meaningful tests, and unsupported compatibility claims.
When a requirement conflicts with a principle, explain the conflict and obtain maintainer approval for an amendment before proceeding.

Amendments must describe the reason, affected guidance, and any required follow-up changes.
Use a major version for incompatible principle changes or removals, a minor version for new or materially expanded guidance, and a patch for clarifications.
Update the amendment date whenever the constitution changes and preserve the original ratification date.

**Version**: 1.1.0 | **Ratified**: 2026-11-13 | **Last Amended**: 2026-11-13
