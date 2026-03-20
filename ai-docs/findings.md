# Findings

Cumulative discoveries from development work. Add new findings at the top, below the separator.

Each finding should follow this format:
## YYYY-MM-DD HH:MM — [brief title]
[description of the discovery and its implications]

When a finding is no longer accurate (the underlying issue was fixed, the dependency
was upgraded, the pattern changed), REMOVE it. Stale findings are worse than no
findings — they mislead agents into solving problems that no longer exist.

---

## 2026-03-20 — oklch relative color syntax used throughout
The site now uses `oklch(from var(--foo) l c h / alpha%)` relative color syntax extensively for glass panels, buttons, inputs, and animations. This requires Chrome 111+, Safari 16.4+, Firefox 128+. No fallbacks are provided — this was a deliberate decision.

## 2026-03-20 — SVG stop-color uses hardcoded hex
SVG `<stop>` elements inside `<linearGradient>` definitions (phoenix logo, service card icons) use hardcoded hex values (#ff4d00, #ff1744, #FF8C34, etc.). These cannot easily use CSS variables inside inline SVG `style` attributes. This is acceptable — the brand gradient colors are stable.

## 2026-03-20 — Theme toggle FOUC prevention script in head
An inline `<script>` in `<head>` sets the `.dark` class before render based on localStorage/OS preference. This must remain above the `<body>` to prevent flash of wrong theme. Do not move it to the bottom script block.
