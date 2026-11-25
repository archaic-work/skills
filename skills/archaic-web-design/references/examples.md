# Prefer / avoid: concrete UI decisions

These are **contrasting fragments**, not complete pages. `Avoid` snippets are intentionally wrong for archaic.work; never copy them. Use the installed Oat version and the canonical assets for exact APIs and styling. Read this file when implementing new UI patterns or reviewing markup, not for trivial content edits.

## 1. Page structure: semantic regions instead of presentation wrappers

**Avoid** — redundant layout containers and invented classes:

```html
<div class="app-shell">
  <div class="topbar"><span class="logo">archaic.work</span></div>
  <div class="page"><div class="title">Repositories</div></div>
</div>
```

**Prefer** — structure already styled by `archaic-theme.css`:

```html
<header>
  <a href="/">archaic.work</a>
  <nav aria-label="Primary"><a href="/repos" aria-current="page">Repositories</a></nav>
</header>
<main id="main">
  <header><h1>Repositories</h1></header>
  <section aria-labelledby="recent-title">
    <h2 id="recent-title">Recent</h2>
    <!-- Actual content -->
  </section>
</main>
<footer>archaic.work</footer>
```

**Why:** Landmarks, headings, and navigation carry meaning; the shared stylesheet owns the visual shell. Add a class or neutral wrapper only for a demonstrated structural need.

## 2. Tabs: use Oat's component, including its required role wrappers

**Avoid** — private tab behavior and presentation-only markup:

```html
<div class="tabs">
  <div onclick="showTab('account')">Account</div>
  <div onclick="showTab('security')">Security</div>
</div>
```

**Prefer** — Oat's documented tab structure:

```html
<ot-tabs>
  <div role="tablist">
    <button role="tab">Account</button>
    <button role="tab">Security</button>
  </div>
  <div role="tabpanel"><h3>Account</h3><p>Account settings.</p></div>
  <div role="tabpanel"><h3>Security</h3><p>Security settings.</p></div>
</ot-tabs>
```

**Why:** Here the `div` elements implement Oat's documented component contract, not an invented visual wrapper. Do not replace them just to achieve zero `div` elements. Check the pinned Oat version.

## 3. Forms: real controls and explicit feedback

**Avoid** — placeholder as the only label and a clickable container:

```html
<input placeholder="Project name">
<div class="button" onclick="save()">Save</div>
```

**Prefer** — native form semantics styled by Oat:

```html
<form method="post" action="/settings">
  <label data-field>Project name
    <input name="project" required>
  </label>
  <button type="submit">Save</button>
  <p role="status" aria-live="polite"></p>
</form>
```

**Why:** Keyboard interaction, form submission, validation, and status feedback have explicit meanings. Only announce success after the actual save completes.

## 4. Records: a table instead of decorative cards

**Avoid** — repeated card-like containers for comparable data:

```html
<div class="cards">
  <div class="card">Catalog — Available</div>
  <div class="card">Runtime — Updating</div>
</div>
```

**Prefer** — structured data remains scannable:

```html
<table>
  <caption>Services</caption>
  <thead><tr><th scope="col">Service</th><th scope="col">State</th></tr></thead>
  <tbody>
    <tr><th scope="row">Catalog</th><td>Available</td></tr>
    <tr><th scope="row">Runtime</th><td>Updating</td></tr>
  </tbody>
</table>
```

**Why:** Row and column relationships are explicit. For genuinely wide tables, Oat's `.table` overflow wrapper is an acceptable, deliberate exception.

## 5. Visual identity: include the shared theme, never restyle each site

**Avoid** — one-off identity changes:

```css
body { font-family: "Inter", sans-serif; background: white; }
button { border-radius: 12px; box-shadow: 0 2px 8px #0002; }
:root { --primary: tomato; }
```

**Prefer** — load the pinned Oat stylesheet, then the shared Archaic theme:

```html
<link rel="stylesheet" href="/assets/oat.min.css">
<link rel="stylesheet" href="/assets/archaic-theme.css">
```

**Why:** The shared CSS owns fonts, palette, spacing, and controls. Make family-wide design changes there; site-local CSS should express actual site-specific structure, not a competing design system.

## 6. Theme: paired tokens rather than ad hoc page colors

**Avoid** -- fixed colors that fail in dark mode:

```css
body { background: #f7f5ee; color: #242922; }
button { background: #285b78; color: white; }
```

**Prefer** -- load shared CSS after Oat, and keep system mode as the default:

```html
<link rel="stylesheet" href="/assets/oat.min.css">
<link rel="stylesheet" href="/assets/archaic-theme.css">
<!-- No data-theme: follow the OS. Add data-theme="light" or "dark"
     to html for a deliberate user override. -->
```

**Why:** The shared CSS owns both Heritage palettes and Oat's semantic tokens.
Avoid maintaining a competing palette or implementing a theme switch without a
meaningful System option. See [theme behavior](design.md#theme-behavior).

## 7. Rules: one separator owns each boundary

**Avoid** -- stacking an `hr` and a border beneath the title, followed by the section border:

```html
<main>
  <header><h1>Repositories</h1><hr></header>
  <section style="border-top: 3px solid"><h2>Recent</h2></section>
</main>
```

**Prefer** -- let the canonical stylesheet place the single rule at the section boundary:

```html
<main>
  <header><h1>Repositories</h1></header>
  <section aria-labelledby="recent"><h2 id="recent">Recent</h2></section>
</main>
```

**Why:** A boundary needs one mark, not several adjacent marks. The common CSS owns masthead, section, and footer rules.

## 8. Headings: restrained weight, not forceful bold

**Avoid** -- overriding heading weight to 700 or 900:

```css
h1, h2 { font-weight: 800; }
```

**Prefer** -- use the shared font rules, which set a moderate 500 weight:

```html
<h1>A small, explicit system</h1>
<h2>Repositories</h2>
```

**Why:** Size, placement, and whitespace carry hierarchy. Do not make headings bold as a substitute for composition.

## 9. Fonts: roles stay consistent, even when nested

**Avoid** -- overriding explanatory prose to serif or using proportional text for paths:

```html
<article style="font-family: serif">
  <p>Why the build works.</p>
  <span>/etc/archaic/config</span>
</article>
```

**Prefer** -- use native semantics and the stylesheet's role rules:

```html
<article>
  <h2>The build</h2>
  <p>Why the build works.</p>
  <p>Configuration: <code>/etc/archaic/config</code></p>
</article>
```

**Why:** Use sans for all human-readable text, including nested prose, headings, tables, and controls. Use monospace for technical identifiers.

## 10. Space: give sections room to breathe

**Avoid** -- arbitrary tight spacing to fit extra content above the fold:

```css
main > section { margin-block: .5rem; padding-block-start: .25rem; }
```

**Prefer** -- keep the shared section rhythm and apply only its spacing tokens to local groups:

```css
section > form { margin-block-start: var(--archaic-space-md); }
```

**Why:** `lg` separates content from its rule; `xl` separates major regions; `md` separates related groups; `sm` is for small control clusters. Avoid decorative empty space, but do not compress content.

## 11. Paragraph alignment: centered containers, left-aligned prose

**Avoid** -- centering a hero paragraph itself:

```html
<header><h1>Documentation</h1><p style="text-align: center">How the system works.</p></header>
```

**Prefer** -- semantic markup and the default `text-align: start`:

```html
<header><h1>Documentation</h1><p>How the system works.</p></header>
```

**Why:** A constrained, centered page is not a reason to center its text. This applies to introductions, normal prose, and helper paragraphs.
