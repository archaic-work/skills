# Oat component decisions

Use the project's pinned [Oat UI](https://oat.ink/) version; its [components](https://oat.ink/components/), [recipes](https://oat.ink/recipes/), and [customizing guide](https://oat.ink/customizing/) are the API authority. Keep Oat's semantic markup; style with the shared theme loaded after Oat.

| Need | Prefer | Avoid |
| --- | --- | --- |
| Disclosure / FAQ | `<details><summary>` | Custom accordion JS |
| Confirmation / form modal | Native `<dialog>` | Homegrown overlay/focus trap |
| Navigation actions | `<a>` / `<button>` | Clickable `div` |
| Menu or split action | `<ot-dropdown>` | Custom menu event plumbing |
| Tabbed local views | `<ot-tabs>` with tablist/tab/panel roles | DIY ARIA tabs |
| Notifications | `ot.toast(message, title?, options?)` | New toast dependency |
| Settings controls | Native `<form>`, `<label>`, `<input>`, `<select>` | Unlabeled fields |
| Boolean settings | `<input type="checkbox" role="switch">` | Custom toggle |
| File upload | `<ot-upload>` wrapping file input | Custom dropzone first |
| Busy, progress, range | `aria-busy`, `<progress>`, `<meter>` | Fake status bars |
| Records | Semantic `<table>` in `.table` wrapper | Grids of faux rows |
| State / notices | Oat `role="alert"`, `.badge`, appropriate text | Color-only meaning |
| Responsive sidebar | Oat `data-sidebar-layout` pattern | Custom sliding nav first |

Use Oat `.card` only when a bounded unit genuinely helps comprehension; never blanket-wrap sections in cards. Use `data-variant="secondary|danger"` on buttons; `.outline` or `.ghost` for lesser actions. For routine nonurgent feedback use accessible status text (`role="status"`), reserving `role="alert"` for urgent updates.

Oat does not choose your application's framework, routing, or state management. If an API is unfamiliar, inspect the installed version rather than guessing from memory. Use progressive enhancement where browser support requires it.
