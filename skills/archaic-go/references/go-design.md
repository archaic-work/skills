# Go design conventions

Apply these conventions proportionally to the task and project. Do not rewrite working code just to enforce a preferred layout.

## Software Design

**Design principle:** Prefer the simplest design that satisfies current requirements while preserving clear boundaries for foreseeable change. Add complexity only to address demonstrated needs. When departing from these conventions, explain the tradeoff. Simplicity is not the same as minimizing lines, packages, or dependencies.

### Dependencies
- Prefer the Go standard library. Justify third-party dependencies against maintenance, security, and conceptual cost.
- Reuse suitable existing dependencies before adding alternatives.
- Avoid libraries that only wrap straightforward standard-library functionality.

### Package organization and abstractions
- Organize packages around cohesive capabilities, not prescribed architectural layers.
- Start with few packages; extract boundaries when responsibilities, ownership, or dependencies justify them.
- Keep public APIs small and implementation details unexported. Use `internal` when it enforces useful boundaries.
- Avoid speculative layers, generalized frameworks, and premature code generation.
- Define small interfaces near consumers when substitution is useful; return concrete types by default. Do not create interfaces solely for mocking.
- Use generics for genuine type-safe reuse, not for building an abstraction framework.

### Control flow
- Avoid nested loops and nested `if` clauses. Prefer guard clauses, early returns, and `continue` to keep the main path flat.
- Extract cohesive operations into small, clearly named functions when iteration or branching would otherwise nest. Keep ordinary error checks inside loops where needed; do not obscure the algorithm merely to eliminate indentation.

### Errors and state
- Return errors for expected failures; reserve panics for unrecoverable programming defects.
- Add operational context with `fmt.Errorf` and `%w`; use `errors.Is` and `errors.As` for meaningful classification.
- Expose sentinel or typed errors only as needed by callers. Emit failure logs at handling boundaries rather than repeating the same error at every layer; allow diagnostic events throughout a request to feed its flight recorder.
- Make ownership and lifecycle of mutable state explicit; avoid mutable package-level globals.
- Construct and validate configuration at startup, then pass the required values explicitly. Keep SOPS out of application logic.

### Concurrency and resources
- Start synchronous. Introduce concurrency only to satisfy a real requirement.
- Give every goroutine a clear owner, termination condition, and error policy; propagate cancellation using `context.Context`.
- Bound concurrent work. Use channels for communication and coordination, and mutexes where shared state needs protection.
- Close files, rows, responses, and other resources promptly; define graceful shutdown for long-lived services.

### Database access
- Prefer `database/sql` with explicit, handwritten SQL.
- Keep queries readable, reviewable, and close to their consumers; parameterize values.
- Define transaction boundaries deliberately; pass contexts and close rows and other resources.
- Add ORMs, query builders, or SQL generators only for demonstrated benefits.

### HTTP applications and observability
- Prefer `net/http` and its standard `http.ServeMux`, with standard handlers and middleware.
- Separate transport details from application behavior when it makes boundaries clearer.
- Configure appropriate timeouts and graceful shutdown; justify framework or router dependencies.
- Use `log/slog` for structured logging. Propagate request context and avoid leaking secrets.
- For HTTP servers, follow the [flight recorder requirements](http-observability.md): instrument complete request lifecycles for failure investigation, retain bounded diagnostics, and verify causal detail in tests. Apply them when reviewing HTTP code as well as when implementing it.

### Testing
- Use the standard `testing` package and test observable behavior rather than implementation details.
- Prefer real components and lightweight fixtures to mocks; use test doubles only at meaningful boundaries.
- Keep tests deterministic with explicit setup and cleanup. Use table-driven tests when they improve readability.
- Run relevant unit and integration tests; use race detection when concurrency is involved.
- For browser-facing Go applications needing screenshot scenarios or PR visual review, use `archaic-go-visualtest` when available. Preserve existing proven browser suites.

### API protocols and compatibility
- Choose API protocols per project based on interoperability, performance, evolution, tooling, and operations.
- Follow established protocol conventions instead of inventing custom ones; impose no universal REST, gRPC, or GraphQL choice.
- Once APIs have consumers, define compatibility expectations explicitly and avoid accidental breaking changes.

## Compatibility and deployment

- Do not require a specific REST, gRPC, or GraphQL protocol across all projects; choose based on interoperability, evolution, performance, tooling, and operations.
- Treat external protocol and API compatibility as a deliberate contract once consumers exist.
- On NixOS, avoid assuming third-party prebuilt binaries have usable Linux interpreter paths. Prefer native nixpkgs packages or building from source.
