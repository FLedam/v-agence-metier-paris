# AGENTS.md — V Agence Métier Paris

## Project architecture

This is a **single-file project**. Everything lives in `index.html`:

- Embedded `<style>` tag with all CSS (CSS custom properties, responsive layout, animations)
- Embedded `<script>` tag with all JavaScript (data, state machine, rendering)
- No build system, no bundler, no npm packages needed at runtime

## Key directories / files

```
index.html      # The entire application
README.md       # User-facing documentation
AGENTS.md       # This file
```

## Application structure (inside index.html)

### Screens (4 total)
Each screen is a `<div id="screen-*" class="screen">`. Only one is `.active` at a time. `showScreen(id)` handles transitions.

| Screen | ID | Purpose |
|---|---|---|
| Home | `screen-home` | Landing page with hero + about section |
| Quiz | `screen-quiz` | 25-question questionnaire |
| Loading | `screen-loading` | Animated processing interstitial |
| Result | `screen-result` | Coach profile + optional admin panel |

### Data layer (JavaScript)
- `CLANS` — object mapping clan code to full name (e.g. `VE → Ventrue`)
- `questions` — array of 25 question objects, each with `text` and `options[]`. Each option has `text` and `scores: {CLAN_CODE: points}`.
- `coaches` — object keyed by clan code. Each entry has: `name`, `specialty`, `phone`, `email`, `quote`, `bio`, `color` (for avatar background), `initials`.

### State
- `currentQuestion` — current question index (0-based)
- `answers` — array of selected option indices (null = unanswered)
- `scores` — running totals per clan code
- `answerLog` — array of `{qNum, qText, chosen, scores}` objects for admin mode

### Scoring
Each answer option carries a `scores` object. Typical weighting: 3 pts to primary clan, 1-2 pts to secondary. `scores` accumulate as the user navigates forward. Back-navigation does **not** subtract scores (scores are recalculated from `answers` array on submission — important: current impl adds on `nextQuestion()`, so back/forward navigation may cause double-counting if a user goes back and re-answers).

> **Known limitation**: If a user goes back to a previous question and changes their answer, the original scores are already added and the new scores will also be added. For a 25-question quiz this doesn't significantly affect the result but could be improved by recalculating from scratch at submission time.

### Admin mode
Triggered by `?admin=true` in the URL query string. Checked in `showResult()`. Renders a dark panel below the coach card with clan name, score bars, and per-answer mapping.

## Coding conventions

- Vanilla JS — no frameworks, no TypeScript
- CSS custom properties defined on `:root` for the color palette
- Google Fonts loaded via `@import` inside `<style>` (Cormorant Garamond + Montserrat)
- SVG avatars generated programmatically in `generateAvatar(initials, color)`
- All coaching profiles are fictional; all contact details are fake

## Non-obvious decisions

- **No external dependencies**: The project must open directly in a browser with no internet for the RPG table use case.
- **Google Fonts import**: This is the one soft external dependency (fonts load from Google CDN). If offline use is required, the import should be removed and system fonts will gracefully substitute.
- **Admin panel placement**: The admin panel appears *below* the coach card rather than replacing it, so a MJ can show the coach result to players while reading clan data themselves (different scroll position).
- **Match percentage formula**: `matchPct = min(97, 72 + (topScore / maxPossible) * 25)` — artificially floors at 72% and caps at 97% to look like a plausible professional compatibility score regardless of actual quiz performance.
- **The Masquerade**: Questions are deliberately worded in HR/coaching language. Clan names never appear in any user-visible output. The admin panel is the only place clan names appear.
