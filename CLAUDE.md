# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page landing site for Red Phoenix Team, a software development agency. The site is deployed via GitHub Pages to the custom domain `red-phoenix.team`.

## Architecture

**Single-File Application**: The entire site is contained in `index.html` - a self-contained HTML file with embedded CSS and JavaScript. There is no build process.

**Key Design Patterns**:
- **Dark Tech Forge Theme**: Uses CSS variables for consistent theming throughout
- **Animated Background Layers**: Two parallax decoration systems work together:
  - Grid background with pulsing vertical/horizontal lines
  - SVG sketch decorations positioned relative to sections with parallax scrolling
- **Dynamic Sketch Positioning**: JavaScript calculates SVG sketch positions based on section offsets on page load and window resize

## Color System

The site uses CSS custom properties defined in `:root`:
```css
--bg-primary: #0a0a0f;
--bg-secondary: #12121a;
--accent-orange: #ff4d00;
--accent-red: #ff1744;
--text-primary: #ffffff;
--text-secondary: #e5e5e5;
--text-muted: #9ca3af;
```

When modifying colors, always update these variables rather than hardcoding values.

## Typography

Two Google Fonts are used:
- **JetBrains Mono**: For headings and display text (applied via `.font-display` class)
- **IBM Plex Sans**: For body text (default)

## Assets

The `images/` directory contains the complete Red Phoenix logo suite:
- **Horizontal logos**: `horizontal_h16.svg` through `horizontal_h120.svg` - Used in navigation
- **Vertical logos**: `vertical_h42.svg` through `vertical_h312.svg`
- **Square logos**: `logo_24x24px.svg` through `logo_2000x2000px.svg`
- **Social media**: `logo_linkedin_300x300_[dark|light].svg`, `personal_cover_1584x396_[dark|light].svg`
- **Stacked**: `stacked_logo_h240.svg`

Current header uses: `images/horizontal_h32.svg`

## Form Integration

Contact form submits to getform.io endpoint: `https://getform.io/f/ayvqkypb`

The form includes:
- Client-side email validation with regex
- Loading states with spinner animation
- Toast notification system for success/error feedback
- Auto-dismiss toasts after 5 seconds

## JavaScript Architecture

Three main initialization systems run on `DOMContentLoaded`:

1. **Grid Background** (`initGrid()`): Dynamically creates animated grid lines
2. **Sketch Positioning** (`positionSketches()`): Calculates SVG decoration positions based on section offsets
3. **Parallax Updates** (`updateParallax()`): Applies parallax transform to sketch items based on scroll position

Throttled scroll listeners optimize performance for parallax effects.

## Section Structure

The page follows this section order:
1. **Hero** - Phoenix icon with fire particles, main headline
2. **Services** - 4 service cards in 2x2 grid
3. **Team** - 6 team member cards in 3-column grid
4. **Contact** - Contact form
5. **Footer** - Email, copyright, LinkedIn link

Each section has corresponding SVG sketch decorations positioned at calculated offsets.

## Customization Points

**Team Members**: 6 cards starting at line ~749. Each includes:
- Avatar (UI Avatars API placeholder)
- Name, role, LinkedIn profile

**Contact Email**: Footer email is `hello@red-phoenix.team`

**Social Links**: LinkedIn company link in footer

**Service Cards**: 4 cards with custom SVG icons and gradient fills

## Development Workflow

This is a static site with no build process. To work on it:

1. Edit `index.html` directly
2. Open in browser to preview (can use `python -m http.server` or similar)
3. Commit and push to `main` branch
4. GitHub Pages automatically deploys to `red-phoenix.team`

## Domain Configuration

The `CNAME` file contains `red-phoenix.team` for GitHub Pages custom domain setup.