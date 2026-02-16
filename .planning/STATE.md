# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-02-16)

**Core value:** Users who prefer light mode get a first-class experience — every panel, the map, and all chrome look intentionally designed for light backgrounds, not like an afterthought inversion.
**Current focus:** Phase 1 - CSS Foundation & Color Centralization

## Current Position

Phase: 1 of 4 (CSS Foundation & Color Centralization)
Plan: 5 of 5
Status: Executing
Last activity: 2026-02-16 — Completed 01-04-PLAN.md (dynamic inline style color conversion)

Progress: [████████░░] 80%

## Performance Metrics

**Velocity:**
- Total plans completed: 4
- Average duration: 8 min
- Total execution time: 0.53 hours

**By Phase:**

| Phase | Plans | Total | Avg/Plan |
|-------|-------|-------|----------|
| 01-css-foundation | 4/5 | 32min | 8min |

**Recent Trend:**
- Last 5 plans: 01-01 (5min), 01-02 (5min), 01-03 (5min), 01-04 (17min)
- Trend: consistent (01-04 larger scope)

*Updated after each plan completion*

## Accumulated Context

### Decisions

Decisions are logged in PROJECT.md Key Decisions table.
Recent decisions affecting current work:

- Settings-only toggle to avoid cluttering dense dashboard UI
- Keep accent colors unchanged (reds, greens, yellows work on both backgrounds)
- CSS custom properties approach enables instant theme switching without reload
- (01-01) Split :root into two blocks: theme colors vs semantic colors, preventing accidental light-mode override of semantic values
- (01-01) getCSSColor uses Map cache with auto-invalidation on data-theme attribute change
- (01-02) Semantic-colored rgba tints kept hardcoded: CSS cannot parametrize rgba() individual channels with var()
- (01-02) Overlay vars for backgrounds/borders only; shadow var for box-shadow contexts only; text hierarchy vars for text color
- (01-02) High-opacity dark rgba (>0.6) maps to var(--bg), low-opacity (<0.35) maps to var(--overlay-heavy)
- (01-03) color-mix(in srgb, var(--x) N%, transparent) pattern for alpha-transparent tints from CSS variables
- (01-03) Settings window --settings-* variables alias global theme variables for cascade isolation
- (01-04) THREAT_COLORS kept as deprecated constant; new getThreatColor() function is recommended path
- (01-04) PIPELINE_COLORS and MONITOR_COLORS left as fixed hex — category identifier colors not theme-dependent
- (01-04) Map d3 SVG fills/strokes converted to --map-* CSS variables for theme reactivity

### Pending Todos

None yet.

### Blockers/Concerns

**From Research:**
- ~~124+ hardcoded color instances found via grep - must be systematically converted in Phase 1~~ (resolved: 889 colors converted in 01-02)
- Map basemap URL is hardcoded in DeckGLMap.ts - needs parameterization in Phase 3
- D3 charts have hardcoded color scales - require theme subscriptions in Phase 3
- Unknown if Carto light basemap ocean colors will require Deck.GL overlay adjustments

**Mitigation strategies identified in research for all blockers**

## Session Continuity

Last session: 2026-02-16 (plan execution)
Stopped at: Completed 01-04-PLAN.md
Resume file: None
