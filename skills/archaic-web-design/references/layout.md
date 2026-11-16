# Page composition

Use `../assets/page-shell.html` as the reference structure and `../assets/archaic-theme.css` for actual dimensions. Apply its classless semantic shell consistently to marketing/documentation sites and working apps. The visual rules live in CSS, not in page-specific IDs or `archaic-*` classes.

- **Masthead:** lowercase wordmark at the left, short primary navigation at the right; bottom 3px ink rule. Mark the current link with `aria-current="page"`.
- **Title block:** small context label (optional), sans `h1`, optional serif introduction, bottom 1px rule. Give each page exactly one `h1`.
- **Content:** centered maximum width, consistent side gutters. Long prose stays at a comfortable measure. Separate major sections with a 3px ink rule; separate data within sections with thin rules.
- **Data:** prefer lists and semantic tables before generic cards. The shell uses a direct `<table>`; for tables too wide for the viewport, add the Oat `.table` overflow wrapper only where needed. Keep actions near the data they change.
- **Columns:** a section with a direct `<aside>` receives the canonical main + supporting-information grid through `section:has(> aside)`; it stacks at narrow widths. Use a specific class or wrapper only when the semantic structure cannot express the layout reliably. Do not use ornamental card grids.
- **Footer:** simple horizontal rule and a brief product/family label; no oversized marketing block by default.

Allow density to vary with purpose: documentation is spacious; operational screens may use compact tables/forms. They must still share the same header, type roles, palette, controls, and rule language. Avoid copying demo content or inventing navigation destinations.
