# Identity and typography

The canonical **Heritage light/dark** values live in `../assets/archaic-theme.css`. Do not duplicate the palette in a second theme. The initial direction is aligned with the Archaic Java design specimen: technical manuals, calm paper surfaces, precise type and rules.

- **Paper:** warm near-white background; **ink:** charcoal-green foreground; **blue:** links, primary actions, focus; **muted:** secondary text; **tint:** quiet grouped areas. Reserve danger/success colors for states with explanatory words.
- **Sans-serif** (`sans-serif`) organizes titles, navigation, controls, tables, and labels. **Serif** (`serif`) explains in introductions and long-form copy (the page header introduction and section prose). **Monospace** (`monospace`) specifies commands, paths, code, and identifiers.
- Use browser generic font families; no Google Fonts, bundled web fonts, or `system-ui` substitutions. Hierarchy comes from weight, size, spacing, and alignment.
- Favor flat color, square controls, underlined text links, meaningful whitespace, 1px inner rules and 3px section rules. Avoid gradients, shadows, decorative rounded cards, and excessive accents.
- Keep the brand lowercase (`archaic.work` or the site's established `archaic <product>` wordmark); use brief all-caps eyebrow labels sparingly. Write headings in sentence case.
- Both modes are deliberate, not inversions. Light uses paper `#F7F5EE`, ink `#242922`, blue `#285B78`; dark uses charcoal-green `#171D1A`, soft ivory `#ECEDE4`, blue `#93C5DF`. Keep new colors in the canonical stylesheet, using semantic tokens rather than hard-coded literals in page styles.

## Theme behavior

- Default to **system preference**: leave `data-theme` unset. The shared CSS uses `color-scheme: light dark` and `light-dark()` token pairs (no JavaScript needed).
- For an explicit choice, set `data-theme="light"` or `data-theme="dark"` on `<html>` (also supported on `<body>`). Remove the attribute to return to system mode. Theme changes must apply before first paint when restoring a saved preference.
- If a site offers theme selection, use an accessible **System / Light / Dark** control. Store only the user's explicit choice; do not force a default on visitors. Persistent selection requires project code, not a theme dependency.
- Let `color-scheme` update native controls and scrollbars. Do not leave a hard-coded light `theme-color` meta tag in a dark-capable site; if browser chrome theming is needed, maintain it for both modes.
- Use the shared Oat variables for controls; never overwrite light and dark palettes in individual page styles. Test system dark and both explicit overrides.

If a product requires a distinct accent or editorial design, agree on a family-wide variant instead of silently redefining shared tokens per site.
