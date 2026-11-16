---
name: archaic-web-design
description: Build, edit, and review archaic.work websites and web apps with one consistent visual identity using Oat UI. Use for component choices, HTML, typography, colors, responsive layouts, navigation, forms, interactions, and accessibility across the archaic.work family.
---

# Archaic web design

Build related sites that look unmistakably part of one family: warm paper, dark ink, restrained blue, native typography, square controls, aligned information, and visible rules. Prefer semantic HTML and Oat over bespoke widgets. Keep implementation plain and small.

## Workflow

1. Read the project's `AGENTS.md`, current CSS, and pinned Oat version. Preserve explicit product constraints; flag conflicting designs instead of silently mixing styles.
2. Use [assets/archaic-theme.css](assets/archaic-theme.css) **after Oat CSS**. Treat its tokens as authoritative. Reuse the shared revision instead of hand-copying colors or inventing typefaces; never add web fonts.
3. Start from [assets/page-shell.html](assets/page-shell.html) for a new page. Keep the same masthead, wordmark, title block, separators, widths, and footer; vary content, not identity. See [layout](references/layout.md).
4. Select native/Oat controls with [component guide](references/oat.md). Verify unclear APIs against the **installed Oat version** and [official docs](https://oat.ink/components/). Never recreate an available interaction or add a UI framework just for appearance.
5. Follow [design rules](references/design.md) for type hierarchy, density, states, and allowable deviations. Use CSS classes only for project-specific structure, not to rebuild Oat.
6. Run the [verification checklist](references/checks.md) for each UI change; report checks not performed.

## Boundaries

This skill governs presentation, not application language, runtime, or deployment. Project-specific `AGENTS.md` and user requirements take precedence; new shared visual decisions belong in this skill, not ad hoc site overrides. No decorative gradients, shadows, card grids, stock imagery, or animation without a task-specific reason.
