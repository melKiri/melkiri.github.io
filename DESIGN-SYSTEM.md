# Portfolio Design System

The goal: **change things in one place, never rebuild across pages.**

## Architecture

```
assets/design-system.css   ← single source of truth (tokens + shared rules)
index.html                 ← links the DS, then its own <style> for page-specific bits
case-*.html                ← same pattern
```

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

## Components (current homes)

Shared primitives (nav, buttons, `case-hero`, `section`, `meta`, `tag`→bullets, `lightbox`,
expandable `breakdown`) still live in each page's inline `<style>` today.

**Roadmap — Phase 2:** extract these into `design-system.css` (or a `components.css`) so
they're defined once too. Until then, when changing a shared component, apply it across all
pages (or ask Claude to).

## Adding a new case study

1. Copy an existing `case-*.html` as the template.
2. Keep the `<link rel="stylesheet" href="assets/design-system.css">` in `<head>`.
3. Reuse the documented classes; only add page-specific CSS for genuinely new components.
4. Add the card to `index.html` (`.work-card`) with a matching title + illustration.
