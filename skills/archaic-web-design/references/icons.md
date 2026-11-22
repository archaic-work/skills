# Lucide icons

Use [Lucide](https://lucide.dev/icons/) as the **sole source of new interface icons** across the archaic.work family. Lucide SVGs are available under the [ISC license](https://github.com/lucide-icons/lucide/blob/main/LICENSE); retain applicable license notices when distributing copied assets. Do not mix Lucide with Heroicons, Tabler, Phosphor, emoji, or ad hoc pictograms to fill gaps. The Archaic wordmark and existing product illustrations are not interface icons.

## Selection and assets

- Prefer no icon when a short text label communicates the action. Use icons for recognizable actions, status cues, and compact navigation, not ornament.
- Search the [Lucide catalog](https://lucide.dev/icons/) and reuse the same icon for the same action across sites. Pick a meaningful name, not a merely similar silhouette.
- Keep only actually used SVGs in each site's local assets. Do not vendor the full collection or require a Lucide JavaScript package, icon font, CDN, or a new UI framework just to display icons.
- Preserve Lucide's geometry and its `24 24` viewBox. Default to Lucide's 2-unit stroke; scale rendered size to **16 px** for dense actions, **20 px** for ordinary controls, and **24 px** for larger standalone indicators. Keep stroke weight consistent instead of individually redrawing icons.
- For inline SVG, use `fill="none"`, `stroke="currentColor"`, rounded line caps and joins as provided by Lucide. This inherits the foreground in both Heritage themes; do not hard-code color values. Note that `currentColor` inside an external SVG loaded with `<img>` does **not** inherit the parent text color: choose inline SVG when theme inheritance is needed.
- Leave Oat/native button and control behavior intact. Add an SVG inside a semantic control rather than replacing its interaction or styling with custom icon widgets. Reuse a project's existing SVG delivery mechanism if it meets these constraints.

## Accessibility

- A text-labeled control's icon is decorative: mark its inline SVG `aria-hidden="true"` and `focusable="false"`; do not repeat its accessible name.
- Give icon-only buttons an accessible name via `aria-label` and ensure their purpose is unambiguous; the icon itself remains hidden from assistive technology.
- If a standalone icon conveys information not available in nearby text, provide an equivalent accessible text name or visible label. Do not convey status through shape/color alone.
- Icons must not be the only visible indicator of keyboard focus, errors, or destructive actions. Preserve sufficient contrast by inheriting semantic foreground colors.
- Check icons at their actual rendered size in light, dark, and keyboard-focus states.

## Example

```html
<button type="button" aria-label="Search">
  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20"
       viewBox="0 0 24 24" fill="none" stroke="currentColor"
       stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
       aria-hidden="true" focusable="false">
    <circle cx="11" cy="11" r="8" />
    <path d="m21 21-4.35-4.35" />
  </svg>
</button>
```

The example illustrates semantic use of Lucide's search icon, not a new component API. Prefer the icon file from the site's pinned Lucide source when implementing production markup.
