# Page composition

Use `../assets/page-shell.html` as the reference structure and `../assets/archaic-theme.css` for actual dimensions. Apply the shell consistently to both marketing/documentation sites and working apps.

- **Masthead:** lowercase wordmark at the left, short primary navigation at the right; bottom 3px ink rule. Mark the current link with `aria-current="page"`.
- **Title block:** small context label (optional), sans `h1`, optional serif introduction, bottom 1px rule. Give each page exactly one `h1`.
- **Content:** centered maximum width, consistent side gutters. Long prose stays at a comfortable measure. Separate major sections with a 3px ink rule; separate data within sections with thin rules.
- **Data:** prefer lists and semantic tables before generic cards. Use `<div class="table">` around wide tables per Oat guidance. Keep actions near the data they change.
- **Columns:** use `.archaic-columns` only for meaningful main + supporting information. Stack at narrow widths. Do not introduce a dashboard of ornamental cards on a content page.
- **Footer:** simple horizontal rule and a brief product/family label; no oversized marketing block by default.

Allow density to vary with purpose: documentation is spacious; operational screens may use compact tables/forms. They must still share the same header, type roles, palette, controls, and rule language. Avoid copying demo content or inventing navigation destinations.
