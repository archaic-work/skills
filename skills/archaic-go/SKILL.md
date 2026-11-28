---
name: archaic-go
description: Develop, refactor, review, test, and maintain Go projects using Archaic conventions. Use for Go architecture, APIs, HTTP services, structured logging and flight-recorder compliance reviews, database access, concurrency, project scaffolding, NixOS development shells, Helix integration, SOPS/age setup, and project-specific maintenance guidance. Prefer idiomatic Go and existing project patterns; use the separate archaic-go-visualtest skill for browser screenshot scenarios and PR visual comparisons.
---

# Archaic Go

Develop minimal, explicit, idiomatic Go projects. Keep Go responsible for modules, compilation, tests, and developer feedback; use Nix for toolchain provisioning and reproducible packaging.

## Working approach

1. Inspect the repository, project-specific maintenance skill (if present), `AGENTS.md`, and current code patterns before modifying files. Preserve existing public APIs and compatible conventions unless the task requires changes.
2. Favor the simplest design that satisfies actual requirements. Start with the Go standard library; justify third-party dependencies, additional packages, interfaces, concurrency, and code generation against demonstrated need.
3. Keep clear ownership of state and resources. Prefer explicit dependencies, contexts, cancellation, and operationally meaningful errors over hidden globals and speculative abstractions.
4. Choose the smallest relevant workflow below. Read its linked reference before applying specialized conventions; do not load unrelated references for ordinary changes.
5. Format and validate the affected Go code with `gofmt`, relevant `go test` commands and `go vet ./...` where applicable. Run `go test -race` when concurrency changes warrant it. Report checks not performed.

## Workflow references

- **Go design and code review:** Read [Go design conventions](references/go-design.md) for package boundaries, error handling, database access, testing, HTTP defaults and protocol compatibility.
- **Creating a Go repository or development shell:** Read [new project setup](references/new-project.md) before choosing a flake, editor tooling, secrets handling or build packaging.
- **HTTP request diagnostics:** Read [HTTP flight recorder](references/http-observability.md) when building or reviewing HTTP services, adding or changing request paths, diagnosing failures, or auditing logging compliance. Treat diagnostic coverage and meaningful failure reports as acceptance criteria, not just recorder presence. Do not impose request buffering on CLI or background-only tools.
- **Project documentation and recurring maintenance:** Read [project maintenance skills](references/project-maintenance.md) when creating or updating a project's own maintenance skill.
- **Browser screenshot scenarios or visual PR comparisons:** Use the separate `archaic-go-visualtest` skill; this general-purpose skill does not bundle a browser testing library.

## Boundaries

- Do not replace standard Go modules with Nix-managed imports or make `nix build` the interactive edit-test loop.
- Use direct Go commands inside `nix develop`. Add flake apps for meaningful, repeatable entry points rather than treating `nix run` as a universal task runner.
- Keep workstation-global shell, Helix, and Git settings distinct from project toolchains and editor dependencies.
- On NixOS, prefer native packages or source builds when precompiled Linux executables depend on incompatible FHS interpreter paths.
- Treat conventions as defaults, not permission to break upstream code, project-specific requirements, or backwards compatibility. Explain departures and tradeoffs.
