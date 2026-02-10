# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file Hebrew (RTL) fullscreen slideshow presentation — "מהפכת הוייב-קודינג" (The Vibe Coding Revolution). The core lives in `slideshow.html` (HTML + CSS + JS, no build tools). Assets are organized in `assets/`.

## How to Run

Open `slideshow.html` directly in a browser. No server or build step needed.

## Architecture

### Single File Structure (`slideshow.html`)
1. **CSS** (~1400 lines) — all styles including layout-specific rules
2. **HTML** — slide markup (29 slides), navigation UI, progress bar
3. **JS** — navigation logic, keyboard/touch/click handlers, sound effects

### RTL Navigation (Critical)
Navigation is **swapped for RTL** — this is intentional, do not "fix" it:
- `nextBtn` (right arrow, appears on LEFT) calls `prev()` — goes backward
- `prevBtn` (left arrow, appears on RIGHT) calls `next()` — goes forward
- `ArrowLeft` key → `next()`, `ArrowRight` key → `prev()`
- Back button disabled at slide 0: `nextBtn.disabled = current === 0`
- Forward button disabled at last slide: `prevBtn.disabled = current === slides.length - 1`

### Slide Layout System
Each slide uses `data-layout` attribute for styling. Available layouts:
- `title` — centered text, green radial gradient (used for section openers: Part 1–5)
- `content` — left-aligned padded content
- `split` — two-column layout
- `guru` — full-bleed centered image
- `video` — video player slide
- `about` — profile/bio split layout
- `quote` — centered quote with `<mark>` highlights in green
- `timeline` — horizontal scrollable timeline
- `cards` — card grid layout
- `prd` — split row: `.prd-content` (text) + `.prd-visual` (image in `.prd-frame`). Also used as general two-column content layout
- `agents` — hierarchical org chart (orchestrator → sub-agents)
- `mvp` — split row: `.mvp-content` + `.mvp-visual` with `.browser-frame` (Mac-style browser mockup)
- `tools` — 2×2 card grid for tool comparison

### Design Tokens
- Background: `#1b1b1b`
- Text: `#c5c1b9` (body), `#dcdad5` (headings), `#a09d96` (secondary)
- Accent: `#22C55E` (neon green) — used everywhere for glows, borders, highlights
- Neon glow pattern: `text-shadow: 0 0 8px rgba(34, 197, 94, 0.6)` / `box-shadow: 0 0 12px rgba(34, 197, 94, 0.4)`
- Font: Heebo (Google Fonts), weights 300–800
- Language: Hebrew (`lang="he" dir="rtl"`)

### Reusable CSS Components
- **Browser frame** (`.browser-frame`): Mac-style window with `.browser-bar` (3 colored dots + URL) — used in slides 14, 18, 19, 20
- **PRD frame** (`.prd-frame`): Neon green border + glow around images — used in slides 12, 21, 23
- **PRD frame large** (`.prd-frame-large`): Enlarged variant (1050px max-width)
- **Terminal frame** (`.terminal-frame`): Dark terminal with green text — used in slide 25
- **Contact cards** (`.contact-cards`): Flex-wrap grid of cards — used in slide 28

### Sound Effects
Configured in JS via `slideSounds` object. Key = slide index, value = Audio object.
Currently: `{ 2: new Audio('assets/media/sound3.mp3') }`

### Adding a New Slide
1. Add HTML inside `.slideshow` div, before `<!-- Navigation Buttons -->`
2. Set `data-layout` attribute to an existing layout, or create new CSS rules
3. Dot navigation and progress bar auto-generate from slide count — no manual update needed

## Key Conventions
- All arrows/flow in content should use `←` (not `→`) for RTL reading direction
- Use `.slide-number` span for top label/badge on slides
- When overriding `prd` layout direction (e.g., centering), use inline `style="flex-direction:column"` on the slide div
- Images go in `assets/images/`, media (video/audio) in `assets/media/`
- Reference assets with relative paths: `assets/images/filename.png`, `assets/media/filename.mp4`
