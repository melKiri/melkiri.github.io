# Style Guide

## Colors

| Token | Value | Usage |
|---|---|---|
| `--ink` | `#1a1916` | Primary text, headings |
| `--ink-muted` | `#504f4a` | Body text, secondary info |
| `--ink-faint` | `#5a5955` | Tertiary labels |
| `--surface` | `#f5f3ef` | Main background |
| `--surface-alt` | `#edeae4` | Hover states, alt backgrounds |
| `--line` | `#d8d5cf` | Borders, dividers |

## Typography

- Serif (display/headings): `DM Serif Display` → Georgia → serif
- Sans (body/UI): `Inter` → system-ui → sans-serif
- Weights: 300 (light), 400 (regular), 500 (medium)

**Accessibility note:** `Inter` replaces `DM Sans` for body/UI text. Inter has a larger x-height (~73% vs ~65%), which improves on-screen legibility at the same pixel size and supports WCAG AA readability expectations without requiring any font-size changes. `DM Serif Display` is retained for display headings only, where size is never the constraint.

### Type scale

- Hero name: `clamp(44px, 5vw, 68px)`
- Section headings: `clamp(32–36px, 4–4.5vw, 52–58px)`
- Work card title: 24px
- Body text: 16–18px
- Labels/tags: 10–13px

## Spacing

- Section padding: 100px vertical, 48px horizontal (desktop) → 60px / 24px (mobile)
- Max content width: 1440px, auto-centered on wider viewports via `padding: Xpx max(48px, calc((100% - 1440px) / 2 + 48px))`
- Breakpoint: 768px

## Design Principles

- No border radius — sharp corners throughout
- All borders are `1px solid #d8d5cf`
- Letter-spacing on uppercase labels: `0.06–0.12em`
- Fade-up animations on scroll (0.6s ease, 20px translateY)
- Hover transitions: 0.2s

## Structure

All CSS is embedded per-page in `<style>` tags — no external stylesheet. The six pages (`index.html` + 5 case studies) share identical CSS. No frameworks, no utility classes.

### Font loading

Google Fonts is loaded once per page via:

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
```

CSS tokens:

```css
--serif: 'DM Serif Display', Georgia, serif;
--sans: 'Inter', system-ui, sans-serif;
```

## Deployment & password protection

The site deploys to GitHub Pages via the `.github/workflows/deploy.yml` workflow. The workflow triggers on every push to `main` (or manually via *Actions → Deploy to GitHub Pages → Run workflow*).

Three case studies are password-gated with a client-side overlay: `case-appstudio.html`, `case-giving.html`, `case-staq.html`. The password lives as a GitHub repository secret called `PORTFOLIO_PASSWORD` and is injected into the HTML at build time. The source files contain only the placeholder `__PORTFOLIO_PASSWORD__` — the real password is never committed to the repo.

**To rotate the password:**

1. Go to *Settings → Secrets and variables → Actions*
2. Click `PORTFOLIO_PASSWORD` → *Update secret* → paste new value → *Update*
3. Either push a new commit, or run the workflow manually from the *Actions* tab

**Security posture:** This is a lightweight speed bump, not real security. The injected password is visible in the deployed HTML when someone views source. For stronger protection of NDA content (encryption at rest), switch to `staticrypt` or move content off GitHub Pages. See session notes for options.

**Important GitHub Pages setting:** Pages must be set to build from GitHub Actions, not from a branch. *Settings → Pages → Build and deployment → Source: GitHub Actions.*

