---
name: stabilize-ui-layout
description: Diagnose and fix frontend layout jitter, page wobble, scrollbar-induced horizontal shifts, dynamic text width movement, and unstable filter or toolbar controls. Use when a user reports UI shaking, layout jump, horizontal jitter, content shifting between routes or filters, scrollbar changes, or wants a reusable checklist for stabilizing frontend layout.
---

# Stabilize UI Layout

## Workflow

1. Reproduce the movement before editing.
   - Compare the exact states that shift: routes, tabs, filters, search results, pagination, expanded panels, and loading/empty states.
   - Decide whether the movement is global page shift, local component shift, or both.
   - Inspect at least one desktop and one narrow viewport when the UI is responsive.

2. Fix global scrollbar and overflow jitter first.
   - Check global CSS before changing feature components.
   - If vertical scrollbar presence changes between states, add or verify `scrollbar-gutter: stable` on `html`.
   - If horizontal overflow should never be user-scrollable, add or verify `overflow-x: hidden` on `body`.
   - Do not make pages artificially equal height just to hide scrollbar changes; content count differences are valid business state.

3. Stabilize dynamic local content.
   - Look for text whose width changes between states: result counts, selected filters, prices, badges, status labels, sort labels, and button text.
   - Give dynamic text a stable slot with `w-*`, `min-w-*`, grid columns, or a fixed flex basis.
   - Add `shrink-0` when nearby flex items must not pull the text narrower.
   - Use `tabular-nums` for changing numeric text so digit glyph widths do not create subtle movement.

4. Stabilize toolbars and filter rows.
   - Give wrapping toolbar/filter containers a stable `min-h-*` when state changes can alter row height.
   - Prefer predictable flex or grid tracks over content-sized controls when controls must stay aligned.
   - Keep gaps and alignment consistent across active, empty, and filtered states.

5. Preserve behavior.
   - Do not change routing, data, filtering, product counts, sorting, or API behavior unless the user explicitly asks.
   - Prefer layout-only class and CSS changes.
   - Keep changes scoped to the component or global style rule responsible for the movement.

## Verification

- Use `rg` to confirm the stabilizing rules/classes exist after editing.
- Run the repository's normal checks, usually `npm run lint`, and any relevant type or test command already used by the project.
- Manually compare the original shifting states after the fix.
- For visual apps, capture or inspect desktop and mobile states if tooling is available.

## Common Fix Patterns

```css
html {
  scrollbar-gutter: stable;
}

body {
  overflow-x: hidden;
}
```

```tsx
<div className="flex min-h-[68px] flex-wrap items-center gap-4">
  <span className="w-20 shrink-0 text-sm font-medium tabular-nums">
    {resultCount} items
  </span>
</div>
```
