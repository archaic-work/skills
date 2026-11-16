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