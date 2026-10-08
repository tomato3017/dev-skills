# Preferred Go Stack

These are preferred defaults for **new Go projects or new subsystems** when the repository has no intentional alternative. They are not universal Go requirements.

## Precedence

Apply choices in this order:

1. User requirements and repository-local instructions.
2. Existing repository dependencies, architecture, supported Go version, and tooling.
3. Project-specific conventions.
4. This preferred stack.
5. The general defaults in [go_code_standards.md](go_code_standards.md).

Do not add, replace, or migrate a dependency merely to align an existing repository with this stack. Check compatibility, licensing, security policy, maintenance status, and the need introduced by the task before adding a dependency.

## Preferred packages

| Concern | Preferred package | Use when | Notes |
| --- | --- | --- | --- |
| Structured application logging | [`github.com/rs/zerolog`](https://github.com/rs/zerolog) | A new service or application needs structured logs | Pass loggers explicitly; use child loggers for component context; avoid global mutable logger state. |
| Multi-command CLIs | [`github.com/spf13/cobra`](https://github.com/spf13/cobra) | A CLI has meaningful subcommands, flags, completion, or command-specific help | Do not add for a small, single-purpose executable. |
| YAML configuration | [`gopkg.in/yaml.v3`](https://pkg.go.dev/gopkg.in/yaml.v3) | Configuration is intentionally file-based and YAML is an appropriate user-facing format | Reject unknown fields when compatibility requirements allow it. |
| Environment overrides | [`github.com/kelseyhightower/envconfig`](https://github.com/kelseyhightower/envconfig) | A configuration struct needs environment-based overrides | Keep configuration parsing and validation near application startup. |
| Struct validation | [`github.com/go-playground/validator/v10`](https://github.com/go-playground/validator) | Structured external input or configuration needs declarative validation | Keep validation at trust boundaries; produce actionable errors. |
| SQL persistence | [`github.com/uptrace/bun`](https://bun.uptrace.dev/) | A SQL-backed application benefits from Bun's query builder and ORM features | Keep transaction boundaries explicit; use an interface such as `bun.IDB` only when a consumer needs to accept both a database handle and transaction. |
| Top-level process orchestration | [`github.com/oklog/run`](https://github.com/oklog/run) | A long-running application coordinates multiple actors with shared shutdown | Do not use it for ordinary request-scoped goroutines. |
| Assertions and suites | [`github.com/stretchr/testify`](https://github.com/stretchr/testify) | A project benefits from its assertions, mocks, or suite lifecycle support | Use `require` when a failed precondition prevents a valid continuation; use `assert` when collecting independent failures improves diagnosis. Standard `testing` remains valid. |
| Interface mock generation | [`github.com/vektra/mockery`](https://github.com/vektra/mockery) | The project has stable, consumer-side interfaces with enough repeated test doubles to justify generation | Do not create broad interfaces only to generate mocks. |
| Outbound HTTP mocking | [`github.com/h2non/gock`](https://github.com/h2non/gock) | Isolated tests need request matching beyond what `httptest` conveniently supplies | Prefer `httptest` when a local HTTP server provides a clearer test. |
| Linting | [`golangci-lint`](https://golangci-lint.run/) | A repository wants one configured lint command | Commit and follow its configuration; do not assume its default rule set. |
| String representations for typed constants | [`stringer`](https://pkg.go.dev/golang.org/x/tools/cmd/stringer) | Generated `String` methods add clear diagnostic or presentation value | Do not require it for every enum-like type; define zero-value and unknown-value behavior deliberately. |

## Preferred application architecture

For a new long-running service, bot, or daemon, prefer:

- explicit dependency injection from `main` or another composition root;
- configuration loading, validation, and dependency construction before serving work;
- lifecycle-managed components with clearly defined initialization, start, and shutdown behavior;
- `context` cancellation and completion coordination for background work;
- structured logs enriched with component and operation context;
- transaction-aware persistence operations; and
- focused unit tests, plus integration tests for persistence and external-service boundaries.

For a CLI with substantial subcommands, prefer a command layer that separates argument parsing, authorization/validation middleware where needed, and domain operations. Do not retrofit this architecture into an existing project without an explicit requirement.

## Preferred conventions when this stack is used

- Keep required dependencies as explicit constructor parameters. Use functional options only for optional configuration.
- Avoid package-level globals and implicit `init()` wiring.
- Make each background component's shutdown behavior explicit and testable.
- Use database migrations with a repository-defined ordering, test strategy, deployment process, and rollback policy. Do not assume every migration has a safe automatic down migration.
- Keep generator configuration and generated files under version control when the project workflow requires it. Regenerate output through documented commands rather than editing it by hand.
