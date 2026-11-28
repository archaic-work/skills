---
name: archaic-go-visualtest
description: Implement, review, and maintain deterministic browser UI scenarios and screenshot-based pull-request comparisons for Go web applications. Use when creating stable scenario IDs, test fixtures, headless Chromium capture, Rod-based browser tests, capture manifests, image diffs, and PR visual review workflows. This is a specialized complement to archaic-go, not a requirement for Go CLI or API-only projects.
---

# Archaic Go visualtest

Validate visible browser behavior through reproducible scenarios and real Chromium captures. Screenshots are evidence of defined, asserted behavior; they do not replace behavioral tests.

## Workflow

1. Inspect the application's existing browser tests, scenario definitions, fixtures, CI configuration and project maintenance skill. Reuse a working Playwright or other browser suite rather than replacing it solely for consistency.
2. Define stable scenario IDs, checkpoints, fixtures and variants using [scenario-driven browser testing](references/ui-scenarios.md). Keep assertions and browser interactions in application test code, not in scenario JSON.
3. Run the actual checked-out application assets in an isolated Chromium context. Pin browser, OS, fonts, viewport, theme, locale, timezone and data; verify interactive behavior before taking screenshots.
4. Use the optional external Go module's `visualtest` and `visualtest/rod` packages when they fit the project. Read [visualtest API and CLI usage](references/visualtest-api.md) for imports, capture manifests, fixture contracts and compare commands.
5. For PR comparisons, resolve the project's visual baseline policy and record the exact baseline and candidate commit SHAs. Render both with identical toolchains. Compare only matching scenario contracts; classify added, removed and contract-changed views separately from actual pixel differences.
6. Report changed scenarios, failures, missing captures and checks not run. Keep CI artifacts short-lived and synthetic; never publish real sessions, user data or credentials.

## Boundaries

- This skill provides testing instructions, not a Go runtime dependency. Installing it does not install Chromium, Rod, a binary or the Go module.
- The Go library is maintained separately at `github.com/archaic-work/visual-test`; use this module path for new imports and command examples. Do not import the historical `github.com/mitschwimmer/go-skill` module.
- Leave app-specific fixtures, assertions, UI assets and capture code in the application repository. Keep generated captures in approved CI/review storage.
- Discover CI execution, artifact delivery, and retention from the project and its infrastructure documentation. Do not assume a CI provider, GitOps controller, runtime, hosted status check, or artifact-upload mechanism.
- Browser tests are not needed for pure CLI or API projects. Use `archaic-go` for general Go engineering and `archaic-web-design` for shared site presentation when relevant.
- Do not run untrusted PR code with privileged CI credentials or fabricate screenshots from static mocks.
