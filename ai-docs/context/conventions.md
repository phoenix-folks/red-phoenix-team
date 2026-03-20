# Conventions

## File Organization

- Everything lives in a single `index.html` — CSS in `<style>`, JS in `<script>`
- Images in `images/` directory, team photos in `images/people/`

## CSS

- **oklch color system** — all theme colors defined in `:root` (light) and `.dark` (dark) using oklch values
- **CSS custom properties** for all colors, shadows, and effects — never hardcode hex/rgba in CSS rules
- **oklch relative color syntax** used for opacity/alpha variants: `oklch(from var(--primary) l c h / 50%)`
- **Tailwind utilities** for layout and spacing (loaded via CDN)
- **Custom CSS** for animations, component styles, and theme variables
- **Glass morphism** via `.glass-panel` class: 88% opacity, backdrop-blur 8px, saturate 1.3
- **Elevation shadows**: `var(--elevation-1)` (rest), `var(--elevation-2)` (hover), `var(--elevation-3)` (raised)
- Class naming: `.service-card`, `.team-card`, `.btn-primary`, `.glass-panel`, `.nav-link`, `.dot-grid-bg` — descriptive kebab-case

## Typography

- **DM Sans** — headings and display text (`h1-h6, .font-display`)
- **IBM Plex Sans** — body text (default on `*`)
- **JetBrains Mono** — monospace, available via `.font-mono`

## JavaScript

- Vanilla JS, no modules or imports
- Initialization on `DOMContentLoaded`: `initDotGrid()`, `initThemeToggle()`, `positionSketches()`, `updateParallax()`
- `throttle()` helper for scroll performance
- Toast notification via `showToast(message, type)`
- Form submission uses `fetch` with `FormData`
- FOUC prevention: inline `<script>` in `<head>` sets `.dark` class before render

## Theming

- Light theme: `:root` — warm cream/parchment backgrounds (oklch hue 40)
- Dark theme: `.dark` — deep warm neutral backgrounds (oklch hue 25)
- Default: OS preference via `prefers-color-scheme`, overridable via localStorage
- Theme toggle in nav bar persists to `localStorage.theme`

## HTML Structure

- Semantic sections with `id` attributes for anchor navigation
- Team cards: `.team-card.glass-panel` with avatar, name, role, optional LinkedIn
- Service cards: `.service-card.glass-panel` with SVG icon, title, description
- Each section separated by `.section-divider`

## SVG Decorations

- Background sketch items use `data-speed` (slow/medium/fast) and `data-section` attributes
- Theme-adaptive via `var(--sketch-stroke)` and `var(--sketch-opacity)`
- Positioned via JS `positionSketches()` based on section offsets
- Parallax transform applied on scroll

## Accessibility

- `prefers-reduced-motion: reduce` disables all animations
- Mobile (<640px): backdrop-filter disabled, solid backgrounds used
- Theme toggle has `aria-label`
