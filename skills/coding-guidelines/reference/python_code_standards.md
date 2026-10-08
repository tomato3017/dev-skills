# Python Coding Guidelines

Use these guidelines for Python implementation, refactoring, debugging, and code review. They are based on the Python Enhancement Proposals (PEPs), especially:

- [PEP 8 — Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [PEP 257 — Docstring Conventions](https://peps.python.org/pep-0257/)
- [PEP 484 — Type Hints](https://peps.python.org/pep-0484/)
- [PEP 526 — Syntax for Variable Annotations](https://peps.python.org/pep-0526/)
- [PEP 585 — Type Hinting Generics in Standard Collections](https://peps.python.org/pep-0585/)
- [PEP 604 — Allow `X | Y` in Type Annotations](https://peps.python.org/pep-0604/)
- [PEP 621 — Storing Project Metadata in `pyproject.toml`](https://peps.python.org/pep-0621/)

PEPs and this file provide defaults. Project-local instructions, supported Python versions, and existing tool configuration take precedence when they intentionally differ.

## Before editing

1. Identify the supported Python versions and packaging/build system.
2. Read the repository's `AGENTS.md`, `.hermes.md`, `HERMES.md`, or equivalent instructions.
3. Inspect `pyproject.toml`, `setup.cfg`, `tox.ini`, `Makefile`, CI configuration, and formatter/linter/type-checker configuration.
4. Use the repository's existing virtual environment and commands. Do not add tools or dependencies merely to impose a personal preference.

## Formatting and layout

- Use four spaces per indentation level. Do not use tabs for Python indentation.
- Prefer the PEP 8 default of 79 characters for code and 72 for comments/docstrings. Follow an intentional project limit when one is configured; PEP 8 allows a project-wide increase up to 99 characters.
- Prefer implicit continuation inside parentheses, brackets, or braces over backslashes.
- Put two blank lines around top-level functions and classes, and one blank line between methods.
- Keep imports at the top of the file, after a module docstring and module-level comments. Group standard-library, third-party, and local imports with blank lines between groups. Prefer explicit imports and avoid wildcard imports.
- Use UTF-8 and avoid unnecessary non-ASCII identifiers.
- Run the project's formatter rather than hand-formatting around it. Typical tools include `ruff format` or `black`, but do not assume either is installed or authoritative.

## Names and interfaces

- Use `snake_case` for functions, methods, variables, and modules; `CapWords` for classes and exceptions; `UPPER_SNAKE_CASE` for constants.
- Choose descriptive names. Avoid cryptic abbreviations and single-letter names except for established local conventions such as loop indices or mathematical variables.
- Keep public interfaces small and stable. Preserve backwards compatibility unless the task explicitly changes it.
- Prefer clear Python idioms: `isinstance()` for type checks, truth-value testing for collections, and direct iteration over indexed access when the index is not needed.
- Use `with` statements or `try/finally` for resources that require deterministic cleanup.
- Catch the narrowest useful exception. Never use a bare `except` to hide failures; preserve exception context when translating or re-raising errors.

## Documentation and typing

- Follow PEP 257 for docstrings. Add useful triple-double-quoted docstrings to public modules, classes, functions, and methods, especially when behavior, side effects, exceptions, or constraints are not obvious.
- Follow PEP 484 and the repository's checker conventions for annotations. Prefer annotations on public APIs and changed code when they improve clarity; do not add noisy or misleading types just to increase annotation coverage.
- Use modern built-in collection types and `X | Y` unions only when the project's minimum Python version supports PEP 585 and PEP 604. Otherwise use the compatible syntax required by the project.
- Keep annotations statically understandable. Do not imply runtime validation merely because a function is annotated.

## Correctness, safety, and tests

- Prefer simple, explicit control flow and readable code over cleverness.
- Validate external input at boundaries and keep side effects visible.
- Do not use mutable default arguments. Be deliberate about mutation, aliasing, and shared state.
- Avoid catching, suppressing, or logging an exception without deciding whether callers can recover from it.
- Add or update focused tests for behavior changes and regressions. Keep tests deterministic, isolated, and descriptive.
- Run the repository's focused tests first, then its broader test suite and configured checks. Common checks are the project's formatter, linter, `pytest`, `unittest`, `mypy`, or `pyright`; use only the commands the repository supports.

## Dependency and compatibility discipline

- Do not raise the minimum Python version or introduce a dependency without an explicit requirement or strong repository precedent.
- Prefer the standard library when it is sufficient and consistent with the project.
- Preserve public behavior and serialization/API compatibility unless the task requires a breaking change.
- When version compatibility constrains an otherwise preferred PEP feature, follow the supported version rather than forcing the newest syntax.

## Review checklist

For review, prioritize behavior and risk over cosmetic style:

- Correctness, edge cases, and backwards compatibility.
- Exception and resource handling.
- Input validation, injection risks, secret handling, and unsafe deserialization.
- Async/threading behavior and lifecycle cleanup.
- Type-checker and runtime compatibility.
- Test coverage for changed behavior.
- Unnecessary dependencies, import cycles, and performance regressions.
