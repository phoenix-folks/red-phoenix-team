# Landing Page Visual Redesign

created: 2026-03-20

## Goal

Redesign the Red Phoenix Team landing page to align with the brand guidelines (`branding.md`) and adopt the visual system from the newsmonitor app. This includes oklch colors, glass morphism, dot grid background with cursor glow, DM Sans typography, light/dark theme toggle, and updated component styling. Content and section structure remain unchanged.

## Current State

The entire site lives in a single `index.html` with embedded CSS and JS. No build process.

Key files:
- `index.html` — all CSS, HTML, and JS for the site
- `images/` — logos (SVG), team photos (JPG/PNG), favicon
- `CNAME` — GitHub Pages custom domain (`red-phoenix.team`)

**Current visual system:**
- Colors use hex CSS custom properties (`--bg-primary: #0a0a0f`, `--accent-orange: #ff4d00`)
- Dark theme only, no toggle
- Typography: JetBrains Mono for all headings, IBM Plex Sans for body
- Background: JS-generated grid lines with CSS pulse animation + parallax SVG sketch decorations
- Cards: flat `bg-secondary` with orange border, translateY hover
- Buttons: gradient with shine sweep (already close to target)
- Inputs: flat dark bg with orange border
- Phoenix: float animation + glow + fire particles

**Target visual system (from branding.md + newsmonitor):**
- Colors in oklch with full light/dark theme support
- DM Sans for headings, IBM Plex Sans for body, JetBrains Mono for monospace
- Dot grid background: 7x7 pulsing lines, cursor-following glow overlay, noise texture, vignette
- Glass morphism cards: 88% opacity, backdrop-blur 8px, saturate 1.3, elevation shadows
- Buttons: gradient with inset highlight, shine sweep, glow on hover
- Inputs: glass background, primary ring focus, ambient glow
- Theme toggle in nav bar, default to OS preference

## Approach

**Chosen: Layered CSS-first rewrite within single file**

Work through `index.html` in logical layers: CSS foundation first (variables, themes, keyframes), then update HTML component classes, then rewrite JS for background + theme toggle. This preserves the single-file architecture while completely updating the visual system.

**Rejected: Multi-file split** — Extracting CSS/JS into separate files would be architecturally cleaner but changes the project convention (single-file). Out of scope for a visual redesign.

**Rejected: Incremental theme addition** — Adding light theme to the existing hex color system would create tech debt. Better to port the full oklch system from newsmonitor at once.

### Constraints

- Single-file architecture must be preserved — all CSS/JS stays embedded in `index.html`
- All content (text, team members, links, form endpoint) stays unchanged
- SVG sketch decorations kept with theme-adaptive colors
- Modern CSS only — oklch + relative color syntax, no browser fallbacks
- `prefers-reduced-motion: reduce` must disable all animations
- Mobile (<640px): disable backdrop-blur for GPU performance
- Domain stays `red-phoenix.team`

## Tasks

- [x] **1. Replace CSS color system with oklch theme tokens**
  - area: CSS custom properties in `<style>` block
  - context: [`index.html` lines 20-29]
  - depends: none
  - Replace all hex `:root` variables with the full oklch color system from newsmonitor. Define `:root` as light theme, `.dark` class for dark theme. Include: background, foreground, card, primary, muted, border, glass, glass-border, surface variants, gradient tokens, background component tokens (radial center/edge, grid line, vignette, grid opacity, cursor opacity), glow color, shadow system (elevation 1-3, glow-primary, ambient-primary). Add `color-scheme: light dark` to `:root`.
  - tests: Open in browser, verify light theme renders with warm cream background. Add `class="dark"` to `<html>`, verify dark theme renders with deep warm background.
  - acceptance: All oklch variables defined for both themes. No hex color values remain in `:root` or `.dark` (accent hex values `#ff4d00`/`#ff1744` only in logo SVG gradients and fire particle gradients). Page renders in both themes.

- [x] **2. Update typography — add DM Sans, reassign font roles**
  - area: Google Fonts link, CSS font assignments
  - context: [`index.html` lines 14-15, 31-37]
  - depends: 1
  - Add DM Sans (weights 400, 500, 600, 700) to Google Fonts import. Change heading font-family from JetBrains Mono to DM Sans. Keep IBM Plex Sans for body. Keep JetBrains Mono loaded but reserve for `.font-mono` class only. Update the `h1-h6, .font-display` rule.
  - tests: Inspect headings in browser — should render in DM Sans. Body text in IBM Plex Sans.
  - acceptance: Three fonts loaded. Headings use DM Sans. Body uses IBM Plex Sans. JetBrains Mono available via `.font-mono`.

- [x] **3. Rewrite background system — dot grid, cursor glow, noise, vignette**
  - area: CSS keyframes, JS `initGrid()` function, background HTML
  - context: [`index.html` lines 46-83 (grid CSS), 358-359 (grid HTML), 876-898 (initGrid JS)]
  - depends: 1
  - Replace the current `grid-background` div and `initGrid()` JS with the newsmonitor dot grid system. Port the implementation from `dot-grid-background.tsx` and `cursor-glow.tsx` to vanilla HTML/CSS/JS. The background should be a fixed, full-viewport div with 5 layers (bottom to top): warm radial gradient, dim base grid (7x7 pulsing lines), bright cursor-following grid overlay (masked to 280px circle at cursor), noise grain texture (SVG data URI at 3% opacity), vignette (radial gradient darkening edges). Use CSS variables `--bg-radial-center`, `--bg-radial-edge`, `--bg-grid-line`, `--bg-grid-opacity`, `--bg-cursor-opacity`, `--bg-vignette` so the background adapts to theme. Port cursor tracking: `mousemove` with RAF throttle, `radial-gradient` mask, hide on touch-only devices via `matchMedia('(pointer: fine)')`, fade out on `mouseleave`. Replace keyframes `pulseVertical`/`pulseHorizontal` with `grid-pulse-v`/`grid-pulse-h` from newsmonitor (12px movement, 0.25-0.5 opacity range).
  - tests: Open in both themes. Grid lines should pulse. Moving cursor should brighten nearby grid lines. Noise grain and vignette visible. On mobile viewport, no cursor glow.
  - acceptance: 5-layer background renders. Grid animates with 4s staggered pulse. Cursor glow tracks mouse and fades on leave. Background adapts to light/dark theme. Old `initGrid()` JS and `.grid-background` CSS removed.

- [x] **4. Update card styles to glass morphism**
  - area: `.service-card`, `.team-card` CSS classes, card HTML elements
  - context: [`index.html` lines 117-132 (card CSS), 614-691 (service card HTML), 709-787 (team card HTML)]
  - depends: 1
  - Replace flat `background-color: var(--bg-secondary)` with glass morphism. Add `.glass-panel` class (from newsmonitor): `background-color: oklch(from var(--card) l c h / 88%); backdrop-filter: blur(8px) saturate(1.3)`. Update borders to use `var(--glass-border)`. Add elevation shadows: `var(--elevation-1)` at rest, `var(--elevation-2)` on hover. Update hover effect to `-translate-y-0.5` (subtle 2px lift instead of current 8px/4px). Remove inline `style="background-color: var(--bg-secondary);"` from all card HTML elements — use class-based styling instead. Add mobile fallback: `@media (max-width: 640px)` disables backdrop-filter and uses solid `var(--card)` background.
  - tests: Cards should show translucent glass effect with background grid visible through them. Hover should lift slightly with shadow upgrade. On mobile viewport width, cards should use solid background.
  - acceptance: All 10 cards (4 service + 6 team) use glass morphism. Glass effect visible (grid bleeds through). Hover transitions smooth. Mobile fallback works.

- [x] **5. Update nav, buttons, inputs, and form styling**
  - area: nav CSS/HTML, `.btn-primary` CSS, input/textarea CSS, footer
  - context: [`index.html` lines 134-175 (button/input CSS), 541-554 (nav HTML), 803-847 (form HTML), 852-868 (footer HTML)]
  - depends: 1, 4
  - **Nav:** Change from `rgba(10, 10, 15, 0.9)` to glass-panel style: `oklch(from var(--background) l c h / 85%)`, backdrop-blur, glass-border bottom. Update nav link colors to use `var(--muted-foreground)` with hover to `var(--foreground)`.
  - **Buttons:** Update `.btn-primary` to use oklch gradient (`from-primary to-primary/85`), add `box-shadow: inset 0 1px 0 0 oklch(1 0 0 / 15%)` for top-edge highlight, hover glow uses `var(--glow-primary)`. Keep existing shine sweep animation.
  - **Inputs:** Update to glass background with blur: `background-color: var(--glass); backdrop-filter: blur(4px)`, border `var(--glass-border)`. Focus: `border-color` to primary/50, `box-shadow` to `0 0 0 3px` primary/20 + ambient glow.
  - **Footer:** Update background to `var(--card)`, update text colors to use theme variables.
  - tests: Nav should be translucent with blur. Buttons should have inset highlight and glow on hover. Inputs should show glass effect and orange ring on focus. Footer adapts to both themes.
  - acceptance: Nav, buttons, inputs, footer all use oklch theme colors. Glass effects where specified. All interactive states work (hover, focus). Renders correctly in both themes.

- [x] **6. Update SVG sketch decorations for theme adaptivity**
  - area: `.sketch-item` CSS, SVG stroke attributes
  - context: [`index.html` lines 85-107 (sketch CSS), 362-536 (sketch SVGs)]
  - depends: 1
  - Add CSS variables for sketch colors: `--sketch-stroke` (dark: `oklch(0.72 0.20 45)`, light: `oklch(0.55 0.18 45)`) and `--sketch-opacity` (dark: `0.08`, light: `0.06`). Update `.sketch-item` to use `stroke: var(--sketch-stroke); opacity: var(--sketch-opacity)`. Remove hardcoded `stroke: var(--accent-orange)` reference. For SVG circles with `fill="var(--accent-orange)"`, update to `fill: var(--sketch-stroke)`.
  - tests: Sketches should be visible but subtle in both themes. Orange-toned in dark, slightly muted in light.
  - acceptance: Sketch decorations adapt to theme via CSS variables. No hardcoded colors in sketch elements.

- [x] **7. Add theme toggle and theme persistence**
  - area: nav HTML, JS theme logic
  - context: [`index.html` lines 541-554 (nav HTML), 874-1090 (JS block)]
  - depends: 1, 5
  - Add a sun/moon toggle button in the nav bar (right side, after nav links). The toggle should: (1) read OS preference via `matchMedia('(prefers-color-scheme: dark)')` on load, (2) check `localStorage` for saved preference (overrides OS), (3) toggle `.dark` class on `<html>`, (4) save choice to `localStorage`, (5) swap icon between sun (light mode active) and moon (dark mode active). Add inline script in `<head>` (before page renders) to set initial theme class — prevents flash of wrong theme. The toggle should be a simple button with SVG sun/moon icons, styled to match nav link aesthetics.
  - tests: Toggle switches between light and dark. Preference persists across page reload. Removing localStorage falls back to OS preference. No flash of wrong theme on load.
  - acceptance: Theme toggle visible in nav. Toggles `.dark` class. Persists to localStorage. Defaults to OS preference. No FOUC. Both theme icons render correctly.

- [x] **8. Update phoenix animations and gradient text to oklch**
  - area: phoenix CSS animations, `.gradient-text` class, `.section-divider`
  - context: [`index.html` lines 109-115 (gradient text), 223-346 (phoenix/animations CSS), 559-580 (phoenix HTML)]
  - depends: 1
  - Update `.gradient-text` to use theme-aware gradient: `var(--text-gradient-primary)` (light: `linear-gradient(135deg, oklch(0.65 0.22 50), oklch(0.50 0.24 25))`, dark: `linear-gradient(135deg, oklch(0.80 0.18 50), oklch(0.65 0.24 25))`). Update phoenix glow keyframe to use `var(--glow-color)` with oklch drop-shadow (from newsmonitor). Update fire particle animation to newsmonitor values (smaller: -22px rise, 2.1s duration). Update `.section-divider` to use `var(--border)` or a primary-tinted gradient with oklch. Update phoenix float to 3s/-3px (from newsmonitor, currently -10px which is too large).
  - tests: Gradient text should be orange-to-red in both themes. Phoenix should float gently and glow. Fire particles should rise from phoenix. Section dividers visible in both themes.
  - acceptance: All animation keyframes use oklch/CSS variables. Gradient text adapts to theme. Phoenix float reduced to -3px. Fire particles use 2.1s/22px. Section dividers theme-aware.

- [x] **9. Add reduced-motion and mobile accessibility**
  - area: CSS media queries
  - context: [`index.html` lines 347-354 (current responsive CSS)]
  - depends: 3, 4, 5
  - Add `@media (prefers-reduced-motion: reduce)` that disables all animations (set duration to 0.01ms, iteration count to 1, transition duration to 0.01ms). Add `@media (max-width: 640px)` to disable all `backdrop-filter` on glass-panel elements (nav, cards, inputs) and use solid backgrounds instead. Ensure theme toggle is accessible (aria-label, keyboard focusable).
  - tests: Enable reduced motion in OS settings — no animations should play. Resize to mobile — no glass blur effects, solid backgrounds instead. Theme toggle reachable via keyboard.
  - acceptance: `prefers-reduced-motion` respected. Mobile performance optimized (no backdrop-blur). Theme toggle has proper aria-label and is keyboard accessible.

- [x] **10. Final cleanup — remove unused CSS/JS, verify both themes**
  - area: entire `index.html`
  - context: [`index.html`]
  - depends: 1, 2, 3, 4, 5, 6, 7, 8, 9
  - Remove all dead CSS (old hex variables, old grid classes `.grid-background`/`.grid-line`/`.grid-line-vertical`/`.grid-line-horizontal`, old `pulseVertical`/`pulseHorizontal` keyframes). Remove dead JS (old `initGrid()` function if fully replaced). Verify no inline `style` attributes use hardcoded hex colors. Update copyright year to 2026. Verify the entire page in both light and dark themes — every section, every card, every interactive element. Check that Tailwind CDN classes still work with the new color system.
  - tests: Full visual review in both themes. Check all sections scroll correctly. Form submission still works. All links functional. No console errors.
  - acceptance: No dead CSS or JS remains. No hardcoded hex colors outside SVG gradients. Both themes render completely. Form works. No console errors. Copyright says 2026.

### Execution Order

- Batch 1: Task 1 (color foundation — everything depends on this)
- Batch 2: Tasks 2, 3, 6, 8 (independent areas: typography, background, sketches, animations)
- Batch 3: Tasks 4, 7 (cards need colors; toggle needs colors)
- Batch 4: Task 5 (nav/buttons/inputs — needs cards done for consistency)
- Batch 5: Task 9 (accessibility — needs all visual components done)
- Batch 6: Task 10 (cleanup — depends on everything)

## Risks and Open Questions

- **oklch `from` syntax browser support**: The relative color syntax `oklch(from var(--card) l c h / 88%)` is modern-only. User confirmed this is acceptable, but worth noting that some older browsers will show fallback colors.
- **Tailwind CDN + oklch**: Tailwind CDN's utility classes (e.g., `text-white`, `bg-gray-400`) use hex/rgb internally. Some Tailwind classes may need to be replaced with custom CSS using oklch values to stay consistent. Watch for conflicts.
- **Glass morphism performance**: Backdrop-blur can be GPU-intensive. The mobile media query disabling blur mitigates this, but test on real devices if possible.
- **Single-file size**: Adding both theme token sets, new keyframes, and new JS will increase the file size. Currently ~1090 lines. Expect ~1400-1500 after redesign. This is acceptable for a single-page site.

## Scope Estimate

**Large** (10 tasks, single file but cross-cutting CSS/HTML/JS changes). All tasks modify `index.html` but touch different sections. Sequential dependency chain prevents splitting into multiple plans — the color foundation must be laid first.
