---
name: coding-guidelines
description: Apply repository-aware coding guidelines when implementing, refactoring, debugging, or reviewing code, with general Go and Python standards plus an opt-in preferred Go stack for new work.
---

# Coding Guidelines

Use this skill for implementation, refactoring, debugging, and code review. Apply the target repository's instructions and established conventions first; use these guidelines as the language-specific default where the repository does not intentionally differ.

## Workflow

1. Identify the language, supported versions, build system, formatter, linter, type checker, test framework, and repository-local instructions before editing.
2. Inspect nearby code and configuration to preserve existing patterns and compatibility.
3. Read the standard for every language whose code is being changed or reviewed:
   - Go: [reference/go_code_standards.md](reference/go_code_standards.md)
   - Python: [reference/python_code_standards.md](reference/python_code_standards.md)
4. Make minimal, behaviorally correct changes. Prefer the project's existing dependencies, abstractions, and tooling over introducing new ones.
5. Add or update focused, deterministic tests when behavior changes or a regression is fixed.
6. Run the most focused relevant checks first, followed by broader project checks when practical. Report commands run and any failures accurately.

## General Engineering Expectations

- Prioritize correctness, clarity, security, resource cleanup, and backward compatibility over cosmetic refactors.
- Validate untrusted input at system boundaries. Do not expose secrets, weaken authorization, or suppress meaningful errors.
- Preserve public APIs, wire formats, persistence formats, and supported-version compatibility unless the task explicitly requires a breaking change.
- Keep side effects explicit and ensure asynchronous, concurrent, or background work has a defined lifecycle and cleanup path.
- Use descriptive names, small cohesive units, and idiomatic control flow. Avoid speculative abstractions, unnecessary dependencies, and broad unrelated cleanup.
- During review, report concrete findings by severity with file and line references when available; distinguish confirmed defects from optional style suggestions.

## Language-specific Instructions

### Go

For any Go code, read and follow [reference/go_code_standards.md](reference/go_code_standards.md). It defines portable defaults for package organization, formatting, naming, APIs, errors, resources, concurrency, testing, tooling, and review.

For a new Go project or subsystem with no intentional repository alternative, also read [reference/go_preferred_stack.md](reference/go_preferred_stack.md). It contains preferred packages and architecture patterns; it never overrides existing repository dependencies, tools, or user requirements.

Respect the repository's configured Go version and tools. Run the relevant `go` formatting, test, vet, lint, generation, and build commands supported by the repository rather than assuming a toolchain or framework.

### Python

For any Python code, read and follow [reference/python_code_standards.md](reference/python_code_standards.md). In particular, determine the project's supported Python versions before selecting annotation syntax or standard-library features; follow its conventions for formatting, imports, naming, docstrings, typing, exception handling, resources, tests, dependencies, and compatibility.

Use only the formatter, linter, type checker, test runner, virtual environment, and packaging workflow configured by the repository. Do not introduce new Python dependencies, tools, or minimum-version requirements without an explicit need.

## Mixed-language Changes

When a task changes both Go and Python, read both reference files and validate each language with its own configured tooling. Preserve contract compatibility across language boundaries, including API schemas, serialization formats, error semantics, and configuration behavior.
