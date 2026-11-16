# Archaic skills

Portable [Agent Skills](https://agentskills.io/specification) for the `archaic.work` project family. Each directory under `skills/` is independently installable; no agent-specific runtime or frontend framework is required.

## Available

- **[archaic-web-design](skills/archaic-web-design/SKILL.md)** — shared visual identity, Oat UI component choices, page structure, and verification.

## Install

From a website repository, choose the desired coding agents:

```sh
npx skills add archaic-work/skills --skill archaic-web-design
```

This repository is private: the installer needs GitHub access. If your CLI cannot access it, clone with your existing Git credentials and use `npx skills add /path/to/skills --skill archaic-web-design` instead. Install per project and review changes when updating rather than relying on global mutable conventions.

In each website's `AGENTS.md`, add: `For UI work on this archaic.work project, use the archaic-web-design skill.`

## Applying the theme

The skill contains **[the canonical stylesheet](skills/archaic-web-design/assets/archaic-theme.css)** and a [page shell](skills/archaic-web-design/assets/page-shell.html). Install/pin Oat in the website, then include `archaic-theme.css` **after** Oat CSS. The skill's installation does not automatically inject CSS into a website. For reproducible websites, copy the stylesheet at a known skill revision or publish it as a separately versioned CSS asset; do not independently edit copies.

The initial palette and type roles follow the existing Archaic Java visual specimen. The theme deliberately supports **light mode only** until a separately reviewed dark palette exists. Changes to the shared identity should be reviewed as changes to the entire project family.

## Development

Validate a skill with `skills-ref validate skills/archaic-web-design` when the reference validator is installed. Check the page shell in a browser using the website's pinned Oat assets. No application code or font files are bundled here.
