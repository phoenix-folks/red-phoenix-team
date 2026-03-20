# Stack

## Project

**Name:** Red Phoenix Team Landing Page
**Purpose:** Single-page marketing/landing site for Red Phoenix Team, a software development agency. Deployed at `red-phoenix.team` via GitHub Pages.

## Language & Runtime

- **HTML5** — single `index.html` file, no templating
- **CSS** — embedded in `<style>` block, uses CSS custom properties for theming
- **JavaScript** — vanilla JS, embedded in `<script>` block at bottom of `index.html`

## Dependencies (CDN)

- **Tailwind CSS** — loaded via `cdn.tailwindcss.com` (utility classes)
- **Google Fonts** — JetBrains Mono (headings), IBM Plex Sans (body)

## External Services

- **getform.io** — contact form backend (`https://getform.io/f/ayvqkypb`)
- **GitHub Pages** — hosting, auto-deploys from `main` branch
- **Custom domain** — `red-phoenix.team` (configured via `CNAME` file)

## Architecture

**Single-file application.** No build process, no bundler, no framework. The entire site is `index.html` with embedded CSS and JS.

### Section Structure

1. Hero — phoenix icon with fire particle animations
2. Services — 4 cards in 2x2 grid (UI/UX, Web Dev, Custom Software, Mobile)
3. Team — 6 member cards in 3-column grid
4. Contact — form with client-side validation, loading states, toast notifications
5. Footer — email, copyright, LinkedIn

### Visual Systems

- **Animated grid background** — JS-generated grid lines with CSS pulse animations
- **SVG sketch decorations** — parallax-scrolling wireframe illustrations positioned relative to sections
- **Dark theme** — CSS custom properties (`--bg-primary`, `--accent-orange`, etc.)

## File Structure

```
index.html          — entire site
CNAME               — GitHub Pages custom domain
images/             — logo variants (SVG), team photos (JPG/PNG), favicon
  horizontal_*.svg  — horizontal logos at various heights
  vertical_*.svg    — vertical logos
  logo_*.svg        — square logos at various sizes
  people/           — team member photos
  favicon.png       — browser favicon
```
