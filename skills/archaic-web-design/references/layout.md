# Page composition

Use `../assets/page-shell.html` as the reference structure and `../assets/archaic-theme.css` for actual dimensions. Apply its classless semantic shell consistently to marketing/documentation sites and working apps. The visual rules live in CSS, not in page-specific IDs or `archaic-*` classes.

- **Masthead:** lowercase wordmark at the left, short primary navigation at the right; bottom 3px ink rule. Mark the current link with `aria-current="page"`.
- **Title block:** small context label (optional), sans `h1`, optional left-aligned sans-serif introduction. The first section owns the separator below it; do not also underline the title block. Give each page exactly one `h1`.
- **Separators:** exactly **one visible rule at a boundary**. The masthead owns its bottom 3px rule; each major section owns its top 3px rule; the footer owns its top 1px rule. Do not add adjacent `<hr>`, borders, pseudo-element lines, or underlines to reinforce an existing rule. Use `<hr>` only for a thematic break not already marked by a section boundary. Put adequate whitespace around rules.
- **Spacing:** reuse the canonical tokens `--archaic-space-sm` (0.75rem), `--archaic-space-md` (1.5rem), `--archaic-space-lg` (2.5rem), and `--archaic-space-xl` (4rem). Major sections need `xl` separation plus `lg` inset from their top rule; related content gets `md`; individual control groups can use `sm`. Keep ample gutters, avoid compressing page sections to fit the viewport, and do not create arbitrary spacing values.
- **Content:** centered maximum-width page container with left-aligned text and consistent side gutters. Long prose remains about 68ch or narrower. Centering a page container never implies centering paragraph text.
- **Data:** prefer lists and semantic tables before generic cards. The shell uses a direct `<table>`; for tables too wide for the viewport, add the Oat `.table` overflow wrapper only where needed. Keep actions near the data they change.
- **Columns:** a section with a direct `<aside>` receives the canonical main + supporting-information grid through `section:has(> aside)`; it stacks at narrow widths. Use a specific class or wrapper only when the semantic structure cannot express the layout reliably. Do not use ornamental card grids.
- **Footer:** simple horizontal rule and a brief product/family label; no oversized marketing block by default.

Allow density to vary with purpose: documentation is spacious; operational screens may use compact tables/forms. They must still share the same header, type roles, palette, controls, and rule language. Avoid copying demo content or inventing navigation destinations.
