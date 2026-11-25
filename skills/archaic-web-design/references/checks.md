# UI verification

For each changed site, check:

1. **Integration:** Oat CSS loads first, the shared Archaic CSS second, and Oat JS loads where interactive components need it. No competing font, palette, radius, or shadow overrides.
2. **Visual identity:** page shell, wordmark, title block, rules, type roles, action colors, and square controls match `assets/page-shell.html` in both Heritage light and dark modes; compare another archaic.work site if available.
3. **Themes:** test OS light, OS dark, forced light under OS dark, and forced dark under OS light. Verify the dark page canvas is exactly `#000000` and tinted surfaces remain distinct. Verify Oat controls, links, primary/danger/success states, readable contrast, focus, and native control appearance; no hard-coded light browser theme color.
4. **Responsive:** review narrow phone width (~320px), desktop, and 200% text zoom. No horizontal document overflow; wide tables scroll in their own container.
5. **Interaction:** use keyboard only through navigation, forms, menus, tabs, and dialogs. Check text inputs, selects, and textareas in both themes: Oat's focused border/shadow must not gain a second outline. Check that buttons, links, checkboxes, and radios still have a visible keyboard focus indicator. Verify Escape/close behavior, labels, error messages, and spoken status updates.
6. **Accessibility:** headings in order, one `h1`, meaningful link/button labels, sufficient text/control contrast, reduced-motion behavior, and no color-only states.
7. **Five regression checks:** (a) exactly one line per section boundary, no redundant `hr`/border stacking; (b) headings stay moderate-weight (500), never default 700+; (c) sans for structure, operations, and prose even when nested, mono for code/identifiers; (d) major sections and content groups use shared spacing tokens and do not feel cramped; (e) paragraphs are start-aligned, including hero text. Review at phone and desktop sizes.
8. **Consistency:** confirm new styles are either reusable family rules or genuinely local layout; do not fork design tokens. Test any changed JavaScript using the project test commands.

For skill-level regression testing, ask two or three different coding agents to implement the same small page and inspect these five criteria side by side. Do not claim agent reliability improved without comparing results.

Report what was actually exercised and any unsupported browsers or intentional deviations. Do not claim a visual/browser test without running one.
