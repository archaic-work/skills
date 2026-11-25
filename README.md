# Archaic skills

Portable [Agent Skills](https://agentskills.io/specification) for the `archaic.work` project family. Each directory under `skills/` is independently installable; no agent-specific runtime or frontend framework is required.

## Available

- **[archaic-web-design](skills/archaic-web-design/SKILL.md)** — shared visual identity, Oat UI component choices, page structure, and verification.
- **[archaic-go](skills/archaic-go/SKILL.md)** — idiomatic Go development, architecture, NixOS tooling, HTTP diagnostics, and project maintenance.
- **[archaic-go-visualtest](skills/archaic-go-visualtest/SKILL.md)** — deterministic browser scenarios, Chromium captures, and screenshot-based PR review for Go web applications.

## Install

Install only the skills a project needs:

```sh
npx skills add archaic-work/skills --skill archaic-web-design
npx skills add archaic-work/skills --skill archaic-go
npx skills add archaic-work/skills --skill archaic-go-visualtest
```

The repository is public. Alternatively, clone it and install a skill from the local path. Install per project and review changes when updating instead of relying on global mutable conventions.

In a project's `AGENTS.md`, name the applicable skills. For example: `For Go development, use archaic-go. For browser screenshot scenarios and PR visual comparisons, use archaic-go-visualtest.` For a website's visual styling, use `archaic-web-design`.

## Applying the theme

The skill contains **[the canonical stylesheet](skills/archaic-web-design/assets/archaic-theme.css)** and a [classless, semantic page shell](skills/archaic-web-design/assets/page-shell.html). Install/pin Oat in the website, then include `archaic-theme.css` **after** Oat CSS. The skill's installation does not automatically inject CSS into a website. For reproducible websites, copy the stylesheet at a known skill revision or publish it as a separately versioned CSS asset; do not independently edit copies.

The **Heritage** palette follows the existing Archaic Java specimen in light mode and has a deliberately paired OLED-black dark mode with subtle green-tinted functional surfaces. Use generic sans-serif for human-readable text and monospace for technical identifiers in both modes. Leave `data-theme` unset for automatic OS preference, or set `data-theme="light"` / `"dark"` on `<html>` for an explicit override. An optional theme picker belongs in each site, not the skill. Changes to the shared identity should be reviewed as changes to the entire project family.

## Go visualtest module

The `archaic-go-visualtest` skill describes a browser-testing workflow and the optional external Go library, maintained at [archaic-work/visual-test](https://github.com/archaic-work/visual-test). Installing the skill does not install that Go module or a Chromium executable. Use `github.com/archaic-work/visual-test` for imports and pin a reviewed module version or commit.

## Development

Validate each independently installable skill with the reference validator when available:

```sh
skills-ref validate skills/archaic-web-design
skills-ref validate skills/archaic-go
skills-ref validate skills/archaic-go-visualtest
```

Check the web-design page shell in a browser using the website's pinned Oat assets. The Go visualtest library has its own Go tests and CI in its separate source repository. No application runtime code or font files are bundled in this skill collection.
