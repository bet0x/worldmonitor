---
phase: 01-css-foundation-color-centralization
plan: 04
subsystem: ui
tags: [getCSSColor, runtime-colors, inline-styles, theme-colors, typescript]

# Dependency graph
requires:
  - "01-01: CSS custom properties and getCSSColor() utility"
  - "01-03: Settings window and embedded style block color conversion"
provides:
  - "All 13 component files with inline style= colors converted to getCSSColor()"
  - "6 service files with color-returning functions converted to getCSSColor()"
  - "getThreatColor() function for runtime threat-level CSS variable reads"
  - "getPipelineStatusColor() function for runtime pipeline status CSS variable reads"
affects: [01-05]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "getCSSColor('--semantic-*') pattern for inline style color values in template literals"
    - "Threat-level CSS variable mapping via getThreatColor() function"
    - "Color helper functions (getScoreColor, getLevelColor, etc.) return getCSSColor() calls"

key-files:
  created: []
  modified:
    - src/components/StrategicRiskPanel.ts
    - src/components/CountryBriefPage.ts
    - src/components/CountryIntelModal.ts
    - src/components/RegulationPanel.ts
    - src/components/NewsPanel.ts
    - src/components/CIIPanel.ts
    - src/components/SignalModal.ts
    - src/components/MapPopup.ts
    - src/components/MonitorPanel.ts
    - src/components/GeoHubsPanel.ts
    - src/components/TechHubsPanel.ts
    - src/components/CascadePanel.ts
    - src/components/Map.ts
    - src/services/threat-classifier.ts
    - src/services/oil-analytics.ts
    - src/services/weather.ts
    - src/services/climate.ts
    - src/services/data-freshness.ts
    - src/services/unhcr.ts
    - src/config/pipelines.ts
    - src/config/panels.ts

key-decisions:
  - "THREAT_COLORS kept as deprecated constant for backward compat; new getThreatColor() function is recommended path"
  - "PIPELINE_COLORS and MONITOR_COLORS left as fixed hex — category identifier colors not theme-dependent"
  - "Map d3 SVG fills/strokes converted to --map-* CSS variables for theme reactivity"
  - "Tech legend colors (purple, cyan, amber) left as fixed hex — no matching semantic CSS variables"

patterns-established:
  - "Color helper functions return getCSSColor('--semantic-*') instead of hardcoded hex"
  - "Inline style= template interpolation uses getCSSColor() for runtime CSS variable reads"
  - "Service color functions accept domain values and map to CSS variable names internally"

# Metrics
duration: 17min
completed: 2026-02-16
---

# Phase 1 Plan 04: Dynamic Inline Style Color Conversion Summary

**All 13 component helper functions and 6 service color functions converted from hardcoded hex to getCSSColor() runtime CSS variable reads, with getThreatColor() function for threat-level colors**

## Performance

- **Duration:** 17 min
- **Started:** 2026-02-16T09:01:22Z
- **Completed:** 2026-02-16T09:18:22Z
- **Tasks:** 2
- **Files modified:** 21

## Accomplishments
- Converted 13 component files: all getScoreColor, getLevelColor, getTrendColor, getPriorityColor, getImpactColor and similar helper functions now use getCSSColor() with semantic CSS variable names
- Converted 6 service/config files: getTrendColor (oil), getSeverityColor (weather, climate), getStatusColor (data-freshness), getDisplacementBadge (unhcr) all use getCSSColor()
- Created getThreatColor() function in threat-classifier.ts for runtime threat-level CSS variable reads
- Created getPipelineStatusColor() function in pipelines.ts for runtime status color reads
- Converted Map.ts d3 SVG background/grid/country/stroke fills to --map-* CSS variables
- Eliminated all `style="color: #..."` patterns from component template literals
- Zero TypeScript compilation errors after all changes

## Task Commits

Each task was committed atomically:

1. **Task 1: Convert component inline style colors and helper functions** - `125a678` (feat)
2. **Task 2: Convert service and config color functions** - `ff8fb73` (feat)

## Files Created/Modified
- `src/components/StrategicRiskPanel.ts` - getScoreColor, getTrendColor, getPriorityColor converted to getCSSColor
- `src/components/CountryBriefPage.ts` - levelColor, componentBars, updateNews threat colors converted
- `src/components/CountryIntelModal.ts` - levelBadge, scoreBar colors converted
- `src/components/RegulationPanel.ts` - impactColors, typeColors, stanceColors objects converted
- `src/components/NewsPanel.ts` - Replaced THREAT_COLORS import with inline getCSSColor threat var map
- `src/components/CIIPanel.ts` - getLevelColor converted to semantic CSS vars
- `src/components/SignalModal.ts` - priorityColors in showAlert converted
- `src/components/MapPopup.ts` - escalationColors, trendColors objects converted
- `src/components/MonitorPanel.ts` - Monitor color fallback converted
- `src/components/GeoHubsPanel.ts` - Tooltip inline style colors converted
- `src/components/TechHubsPanel.ts` - Tooltip inline style colors converted
- `src/components/CascadePanel.ts` - getImpactColor converted to semantic CSS vars
- `src/components/Map.ts` - Map background, grid, country, stroke, fire, AIS density colors converted
- `src/services/threat-classifier.ts` - Added getThreatColor() function, deprecated THREAT_COLORS constant
- `src/services/oil-analytics.ts` - getTrendColor uses --semantic-* vars
- `src/services/weather.ts` - getSeverityColor uses --semantic-* vars
- `src/services/climate.ts` - getSeverityColor uses --semantic-* vars
- `src/services/data-freshness.ts` - getStatusColor uses --semantic-* and --text-* vars
- `src/services/unhcr.ts` - getDisplacementBadge uses --semantic-* vars
- `src/config/pipelines.ts` - Added getPipelineStatusColor(), documented fixed category colors
- `src/config/panels.ts` - Documented MONITOR_COLORS as fixed category colors

## Decisions Made
- Kept THREAT_COLORS as deprecated export: DeckGLMap.ts and story-renderer.ts still reference it (story-renderer has local copy, DeckGLMap uses for RGB conversion in Phase 3). New getThreatColor() is the recommended path.
- Left PIPELINE_COLORS and MONITOR_COLORS as fixed hex constants: these are category identifier colors (oil=orange, gas=blue) or user palette colors persisted to localStorage. They are not theme-dependent severity colors.
- Tech variant legend colors (#8b5cf6 purple, #06b6d4 cyan, etc.) left as fixed hex: no matching semantic CSS variables exist, and these are fixed category identifiers.
- Map d3 SVG attributes converted to --map-* CSS variables: enables map to respond to theme changes.
- EconomicPanel.ts not modified: it delegates all color logic to service functions (getTrendColor, getChangeClass) which were converted.

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness
- All inline style= hex colors in components and service helper functions now read CSS variables at runtime
- Theme switching will immediately affect all converted colors via getCSSColor() cache invalidation
- Plan 01-05 (remaining cleanup) can proceed to address any remaining Phase 1 items
- Phase 3 items (DeckGL RGB arrays, canvas drawing, story-renderer) remain as noted exclusions

## Self-Check: PASSED

All 21 modified files verified present. All commit hashes (125a678, ff8fb73) verified in git log.

---
*Phase: 01-css-foundation-color-centralization*
*Completed: 2026-02-16*
