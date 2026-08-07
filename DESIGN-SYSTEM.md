# Portfolio Design System

The goal: **change things in one place, never rebuild across pages.**

## Architecture

```
assets/design-system.css   ← foundation: tokens, reset, base, focus, radius (ALL pages)
assets/ds-case.css         ← shared case-study components (case pages only)
index.html                 ← links design-system.css, then its own <style>
case-*.html                ← links design-system.css + ds-case.css, then page-specific <style>
```

Load order per page: `design-system.css` → `ds-case.css` (case pages) → inline `<style>`.
So tokens/foundation and shared components come first, and a page can still override.

Every page loads `assets/design-system.css` **before** its inline `<style>`, so:
- Tokens defined in the DS cascade everywhere.
- A page can still override a component locally when it genuinely needs to.

## Tokens (edit these, not the pages)

Defined in `:root` inside `design-system.css`:

| Token | Value | Use |
|---|---|---|
| `--ink` / `--ink-muted` / `--ink-faint` | dark → light text | body, secondary, labels |
| `--surface` / `--surface-alt` / `--card` | backgrounds | page, hover, cards |
| `--accent` / `--accent-soft` | `#0f7078` petrol | **CTAs, links, focus only** — keep it scarce |
| `--line` | `#e4e6ee` | hairlines, borders |
| `--radius` | `10px` | **every** bordered container (matches the hero) |
| `--serif` / `--sans` | Fraunces / General Sans | display / body |

**Example:** to re-theme the whole site, change `--accent` once here. Done.

## Rules baked into the DS

- **Accent discipline** — teal is CTA / link / focus only. Body metrics stay monochrome.
- **Type floor** — no text below **14px** anywhere (WCAG standard we hold to).
- **One radius** — all bordered tables/grids/cards/frames use `--radius`.
- **Focus** — visible `:focus-visible` outline in the accent (WCAG 2.4.7).

## Components

**Shared case components** (`assets/ds-case.css`) — nav, `case-hero`, `case-eyebrow`,
`case-title`, `case-subtitle`, `meta-label`/`meta-value`, `tag`, `case-section`,
`section-label`/`heading`/`body`, `callout`, `next-case`, `fade-up`. Defined **once** here;
edit and all six case pages update. (Extracted from the inline styles with verified zero
visual change.)

**Still inline (per page):** genuinely page-specific components (e.g. `timeline`,
`features-grid`, `breakdown`, `deepdive`, `data-table`), the responsive `@media` blocks, and
small page overrides. These vary by page, so they stay local.

**index.html** keeps its own components (`hero`, `work-card`, `about`, `contact`) inline —
they're homepage-only. Extract later if a second page ever needs them.

## Adding a new case study

1. Copy an existing `case-*.html` as the template.
2. Keep the `<link rel="stylesheet" href="assets/design-system.css">` in `<head>`.
3. Reuse the documented classes; only add page-specific CSS for genuinely new components.
4. Add the card to `index.html` (`.work-card`) with a matching title + illustration.
