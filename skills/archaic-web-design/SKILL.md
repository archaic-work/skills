---
name: archaic-web-design
description: Build, edit, and review archaic.work websites and web apps with one consistent visual identity using Oat UI. Use for component choices, HTML, typography, colors, responsive layouts, navigation, forms, interactions, and accessibility across the archaic.work family.
---

# Archaic web design

Build related sites that look unmistakably part of one family: Heritage paper and ink by day, charcoal-green and soft ivory by night, restrained blue, native typography, square controls, aligned information, and visible rules. Prefer semantic HTML and Oat over bespoke widgets. Keep implementation plain and small; prefer classless semantic structure over presentation wrappers.

## Workflow

1. Read the project's `AGENTS.md`, current CSS, and pinned Oat version. Preserve explicit product constraints; flag conflicting designs instead of silently mixing styles.
2. Use [assets/archaic-theme.css](assets/archaic-theme.css) **after Oat CSS**. Treat its paired light/dark tokens as authoritative: default to OS preference, allow explicit light/dark choice, and never hard-code page colors. Reuse the shared revision instead of hand-copying colors or inventing typefaces; never add web fonts. Read [theme behavior](references/design.md#theme-behavior) when changing colors or theme controls.
3. Start from [assets/page-shell.html](assets/page-shell.html) for a new page. Keep the same masthead, wordmark, title block, widths, and footer; vary content, not identity. Give each boundary one separator and leave generous space between regions. See [layout](references/layout.md).
4. Select native/Oat controls with [component guide](references/oat.md). Verify unclear APIs against the **installed Oat version** and [official docs](https://oat.ink/components/). Never recreate an available interaction or add a UI framework just for appearance.
5. Use **Lucide only** for interface icons. Prefer a small set of locally stored SVGs over an icon runtime, icon font, or mixed libraries; keep icons functional rather than decorative. Read [icon guidance](references/icons.md) when adding or reviewing icons. Respect an existing project's icon implementation while aligning new icons with Lucide.
6. Follow [design rules](references/design.md): sans structures, serif explains, mono specifies; headings use moderate weight, prose stays left-aligned, and sections must breathe. Style semantic elements in the shared stylesheet. Do not introduce presentational `div` wrappers or classes by default; allow a purposeful wrapper or class only where semantics or a specific Oat component require it. For a new UI pattern or markup review, consult the concise [prefer/avoid examples](references/examples.md); skip them for routine content edits.
7. Run the [verification checklist](references/checks.md) for each UI change; report checks not performed.

## Boundaries

This skill governs presentation, not application language, runtime, or deployment. Project-specific `AGENTS.md` and user requirements take precedence; new shared visual decisions belong in this skill, not ad hoc site overrides. No decorative gradients, shadows, card grids, stock imagery, or animation without a task-specific reason.
