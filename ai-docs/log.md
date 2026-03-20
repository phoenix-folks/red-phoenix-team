# Development Log

Append-only log of work sessions. Add new entries at the top, below the separator.

Each entry should follow this format:
## YYYY-MM-DD HH:MM — [brief title]
[log text]

---

## 2026-03-20 — Landing page visual redesign
- Executed 10-task plan `landing-redesign.md` (all tasks completed, moved to history)
- **Color system**: Replaced hex CSS variables with full oklch color system (light + dark themes)
- **Typography**: Added DM Sans for headings, kept IBM Plex Sans body, JetBrains Mono for mono
- **Background**: Replaced grid lines with newsmonitor-style dot grid (7x7 pulsing lines, cursor-following glow, noise texture, vignette)
- **Cards**: Converted to glass morphism (88% opacity backdrop-blur, elevation shadows)
- **Nav**: Glass background with backdrop-blur, theme-aware link colors
- **Buttons**: oklch gradient with inset highlight, glow on hover, shine sweep
- **Inputs**: Glass background, primary ring on focus
- **Theme toggle**: Sun/moon toggle in nav, OS preference default, localStorage persistence, FOUC prevention
- **SVG sketches**: Theme-adaptive stroke colors via CSS variables
- **Animations**: Updated phoenix float (-3px), glow (oklch), fire particles (2.1s/22px)
- **Accessibility**: prefers-reduced-motion support, mobile backdrop-blur disable
- **Cleanup**: All dead CSS/JS removed, copyright updated to 2026

## 2026-03-20 — Ember bootstrap
- Generated initial ai-docs from repository scan
- Project analyzed: Static single-page landing site for Red Phoenix Team (software dev agency), deployed via GitHub Pages to red-phoenix.team. Single index.html with embedded CSS/JS, Tailwind CDN, vanilla JavaScript, getform.io contact form integration.
