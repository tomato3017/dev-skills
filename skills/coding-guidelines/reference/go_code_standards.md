# Go Code Standards

Use these as general Go defaults for implementation, refactoring, debugging, and review. Repository-local instructions, supported Go versions, existing code patterns, and configured tooling take precedence when they intentionally differ.

For optional package and architecture preferences for new projects, see [go_preferred_stack.md](go_preferred_stack.md). Do not introduce those dependencies into an existing repository solely to conform to this document.

## Before editing

1. Read repository-local instructions such as `AGENTS.md` and contribution documentation.
2. Inspect `go.mod`, `go.work`, `Makefile`, CI configuration, and formatter, linter, generator, and test configuration.
3. Identify the supported Go version and existing package conventions before using language features or adding dependencies.
4. Inspect nearby packages and tests. Preserve established public APIs, error semantics, serialization formats, and dependency patterns unless the task explicitly changes them.

## Project and package organization

- Follow the repository's existing layout before adding directories or packages.
- Organize packages around cohesive domain responsibilities. Avoid circular imports and deep, incidental package hierarchies.
- Use `cmd/<application>/` for executable entry points when the repository has multiple binaries or follows that convention.
- Use `internal/` for implementation packages that must not be imported outside the module tree.
- Use `pkg/` only for packages deliberately intended for external consumption; do not create it by default.
- Keep package names short, lowercase, and descriptive. Avoid stuttering, such as `http.Server.HTTPServer`.
- Use filenames that describe their contents. Lowercase names with underscores are normal when they improve readability, such as `http_server.go`; preserve special suffixes such as `_test.go` and `_linux.go`.

## Formatting, imports, and naming

- Run `gofmt` on changed Go source files. Use `goimports` when it is configured by the repository.
- Keep imports explicit. Avoid dot imports and blank imports unless their side effect is required and documented.
- Let the repository's formatter/import tool organize import ordering and groups. Use aliases only to resolve a real collision or clarify an otherwise ambiguous package name.
- Use `MixedCaps` and `mixedCaps`, not snake_case or SCREAMING_SNAKE_CASE. This includes constants: `DefaultTimeout`, `APIHost`, and `maxRetries`.
- Use conventional initialisms consistently: `API`, `ASCII`, `CPU`, `DB`, `DNS`, `EOF`, `GUID`, `HTML`, `HTTP`, `HTTPS`, `ID`, `IP`, `JSON`, `QPS`, `RAM`, `RPC`, `SQL`, `SSH`, `TCP`, `TLS`, `TTL`, `UDP`, `UI`, `UID`, `UUID`, `URI`, `URL`, `UTF8`, `VM`, `XML`, and `XSRF`.
- Prefer descriptive domain names. Do not use non-descriptive one-letter variable names except for conventional, narrow-scope iterators such as `i` or `j` in loops. Use `err` for error values.
- Choose pointer receivers when methods mutate state, copying is unsafe or expensive, or method-set consistency requires them. Value receivers are appropriate for small, immutable, value-like types. When in doubt, use a pointer receiver.

## APIs, types, and interfaces

- Prefer clear, small APIs and concrete types unless an abstraction has a demonstrated consumer-side need.
- Define small interfaces near the code that consumes them. One-method interfaces are often appropriate.
- Accept interfaces where callers need substitution; return concrete types by default.
- Do not create interfaces solely for mocking. Use an interface when it represents a stable capability required by the consuming code.
- Use compile-time interface assertions when they document an intentional contract or catch a non-obvious implementation relationship.
- Do not impose an arbitrary return-value limit. Use a result struct when several returned values form a cohesive result or when the signature is difficult to read.
- Use functional options or an options struct only when optional configuration and defaults justify the additional indirection. Keep required dependencies explicit and validate invalid options.
- Avoid package-level mutable state and `init()`-based wiring. Prefer explicit construction at a composition root.

## Errors and resources

- Name error values `err`. Return errors to the layer that can make the recovery, retry, user-facing, or termination decision. Avoid logging an error and returning it unchanged at multiple layers.
- Add useful operation context at meaningful boundaries.
- Use `%w` when callers should be able to inspect the underlying cause with `errors.Is` or `errors.As`; do not expose an error chain unintentionally.
- Use `errors.Is` and `errors.As` instead of comparing error strings.
- Define sentinel errors sparingly for stable, expected conditions on which callers need to branch. Use typed errors when callers need structured details.
- Handle errors from `Close`, `Flush`, `Commit`, and similar finalization operations when they can affect correctness. Do not globally discard or print close errors from a generic helper.
- Use `defer` for cleanup once a resource has been successfully acquired. Keep deferred cleanup close to acquisition.
- Restrict `log.Fatal`, `os.Exit`, and process termination to executable entry points after cleanup and error-reporting decisions have been made.

## Context and concurrency

- Accept `context.Context` as the first parameter for operations that can block, perform I/O, or should be cancelable. Do not pass a nil context; use `context.Background()` or `context.TODO()` where appropriate.
- Propagate cancellation and deadlines to downstream work. Do not store request-scoped contexts in long-lived structs.
- Every goroutine must have a defined owner, completion condition, cancellation mechanism, and error-reporting path.
- Document whether lifecycle methods such as `Start`, `Stop`, and `Close` are idempotent, restartable, or safe for concurrent calls. Synchronize lifecycle state accordingly.
- Prefer the simplest synchronization primitive that preserves correctness. Start with `sync.Mutex`; choose `sync.RWMutex` only when its tradeoffs are justified.
- Use channels for communication and ownership transfer, not simply as a replacement for mutexes.
- Stop tickers and timers when they are no longer needed, and include `ctx.Done()` in cancellable blocking loops.

## Logging, configuration, and persistence

- Use the repository's configured logger. Favor structured fields for queryable operational context when the logger supports them.
- Pass logger dependencies explicitly where logging is part of a component's responsibility; do not add global logging state to a codebase that does not use it.
- Validate configuration and untrusted input at system boundaries. Make defaults explicit, and do not include secrets in logs or error messages.
- Use the repository's configured configuration, database, and migration tooling rather than introducing replacements.
- Use a transaction when several persistent changes must succeed or fail together. Ensure all operations that belong to a transaction use its transaction handle.
- Treat schema migrations as production changes: make them ordered, tested, observable, and compatible with the deployment and rollback strategy.

## Testing and tooling

- Use the standard `testing` package by default. Follow repository conventions for assertion, mocking, suite, fuzzing, and benchmark libraries when they already exist.
- Add or update focused, deterministic tests for behavior changes and regressions.
- Test observable behavior where practical. Keep tests isolated, avoid reliance on time or external services, and clean up resources with `t.Cleanup`.
- Use table-driven tests when they improve coverage and readability. Mark test helpers with `t.Helper` so failures identify the caller.
- Run focused package tests first, then broader repository tests and configured checks. Use `go test`, `go vet`, linters, and the race detector where the repository supports them.
- Run `go test -race` for concurrency-sensitive changes and in supported CI workflows. Do not assume every target or environment supports it.
- Modify source inputs rather than generated output, then run `make generate` or the repository's documented generation command. Do not add or run generators without checking repository conventions.
- Run `go mod tidy` when dependency changes or repository workflow requires it; it can intentionally modify `go.mod` and `go.sum`.

## Code generation

- Prefer established code-generation tools over manually maintaining repetitive or mechanically derived code when generation improves consistency, correctness, or maintainability. Examples include `gqlgen` for GraphQL code and `mockery` for mocks of intentional consumer-side interfaces.
- Do not introduce a generator solely to avoid writing a small amount of straightforward code. Check existing repository conventions, configuration, supported versions, and CI integration before adding one.
- Treat schemas, interfaces, templates, and generator configuration as the source of truth. Do not edit generated files directly; modify their inputs and regenerate them.
- Pin generator versions when practical so local development and CI produce reproducible output. Follow the repository's existing dependency and tool-version conventions.
- When a repository uses a `Makefile`, expose generation through a deterministic `make generate` target, or extend the existing generation target. The target should run all required generators and be suitable for local development and CI.
- Decide explicitly, according to repository conventions, whether generated files are committed. If they are committed, CI should be able to detect stale generated output.

## Documentation and review

- Document exported APIs when their behavior, errors, side effects, concurrency guarantees, or ownership rules are not obvious. Follow the repository's documentation conventions.
- Begin exported declaration comments with the identifier name when following standard Go documentation conventions.
- Keep comments focused on intent and constraints rather than restating code.
- In review, prioritize correctness, error and resource handling, context cancellation, data races, public API compatibility, input validation, dependency impact, and regression coverage over cosmetic style.

## Further reading

- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)
- [Package names](https://go.dev/blog/package-names)
- [`context` package](https://pkg.go.dev/context)
- [`errors` package](https://pkg.go.dev/errors)
- [`testing` package](https://pkg.go.dev/testing)
