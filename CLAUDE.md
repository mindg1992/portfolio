# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file Korean-language personal investment portfolio tracker (`index.html`, ~143KB). There is no build system, no package manager, and no external dependencies beyond CDN-loaded libraries. All application logic, styles, and markup live in one file.

## No Build/Test Commands

There is no build, lint, or test process. To run the app, open `index.html` in a browser. Changes take effect immediately on reload.

## Architecture

### Single-File Structure

The file is organized in order:
1. `<style>` block — all CSS
2. `<script>` tag with `window.PRELOADED` — optional embedded JSON data for offline distribution
3. `<body>` — HTML markup (password overlay + main app with 7 tabs)
4. Inline `<script>` at end of body — all application JavaScript (~3000+ lines)

### External Libraries (CDN only)
- **Chart.js 4.4.1** + **chartjs-plugin-datalabels 2.2.0** — all charting
- No framework; pure vanilla JS

### Data Model

```javascript
assets[]           // Array of investment holdings (id, name, type, er, color)
months[]           // Ordered array of month strings ("2026년 1월")
D{}                // D[assetId][month] = number (portfolio values)
monthSavings{}     // monthSavings[month] = number
yearAssets{}       // Year-level snapshots
goalData{}         // Financial goals (targetYear, targetAmount, etc.)
simTargetYear      // Simulation end year (default 2050)
```

### Persistence

- **Primary:** `localStorage` key `'pf-v18'`
- **Portable:** "Save as HTML" feature embeds full JSON into `window.PRELOADED` in a new HTML file for offline distribution
- **Import/Export:** JSON backup/restore and CSV export

### Password Protection

4-digit numeric PIN hashed with SHA-256 (implemented inline, no library). Stored in localStorage. The overlay covers the app until correct PIN is entered or skipped.

### Asset Types

`개별주식`, `ETF`, `채권`, `적금`, `현금`, `현물`, `부동산`, `대출` — each has a default expected return (ER) rate used in growth simulations.

### Key UI Patterns

- All tab switching is done by toggling CSS classes (no routing)
- Modals are shown/hidden via `display` style
- Calculator overlay intercepts cell input for expression evaluation
- Drag handles on asset rows use HTML5 drag-and-drop API for reordering
- AI integration (Claude/ChatGPT/Gemini/Grok) works by copying formatted text to clipboard, then opening the AI service URL

### Editing Guidelines

- Searching for a function: use `Grep` on `index.html` since all JS is inline
- The file is large — use `Read` with `offset`/`limit` to navigate specific sections
- When modifying chart rendering, Chart.js instances are recreated (not updated) on data change
