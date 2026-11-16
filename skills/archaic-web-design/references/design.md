# Identity and typography

The canonical values live in `../assets/archaic-theme.css`. Do not duplicate the palette in a second theme. The initial direction is aligned with the Archaic Java design specimen: technical manuals, calm paper surfaces, precise type and rules.

- **Paper:** warm near-white background; **ink:** charcoal-green foreground; **blue:** links, primary actions, focus; **muted:** secondary text; **tint:** quiet grouped areas. Reserve danger/success colors for states with explanatory words.
- **Sans-serif** (`sans-serif`) organizes titles, navigation, controls, tables, and labels. **Serif** (`serif`) explains in introductions and long-form copy (the page header introduction and section prose). **Monospace** (`monospace`) specifies commands, paths, code, and identifiers.
- Use browser generic font families; no Google Fonts, bundled web fonts, or `system-ui` substitutions. Hierarchy comes from weight, size, spacing, and alignment.
- Favor flat color, square controls, underlined text links, meaningful whitespace, 1px inner rules and 3px section rules. Avoid gradients, shadows, decorative rounded cards, and excessive accents.
- Keep the brand lowercase (`archaic.work` or the site's established `archaic <product>` wordmark); use brief all-caps eyebrow labels sparingly. Write headings in sentence case.
- Oat's light/dark auto-theme is intentionally overridden to **light**. Do not improvise dark tokens by inversion; design and verify a separate palette before enabling dark mode.

If a product requires a distinct accent or editorial design, agree on a family-wide variant instead of silently redefining shared tokens per site.
