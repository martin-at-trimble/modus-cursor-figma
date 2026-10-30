---
name: reference-interpreter
description: >-
  Triggers on screenshot, image, URL, or design description — "analyze this",
  "design brief", "interpret this reference". Outputs a Design Brief mapped to
  Modus tokens; waits for confirmed before any Figma writes.
---

# Reference Interpreter (Modus)

Prerequisite: **figma-preflight** (Token Map available).

## Phase 1 — Analyze

Layout, typography, color, spacing, visual anchor, visible components.

## Phase 2 — Map to Modus 2.0

Map each observation to a name from the session Token Map / Modus text styles and variables. Flag gaps with ⚠️ and nearest-match options.

Use **modus-figma-context** to suggest atomic components (e.g. primary button, compact card) when naming "Components needed".

## Phase 3 — Design Brief

Output sections: Reference, Section, Aesthetic, Layout, Typography, Colors, Spacing, Components needed, Gaps.

## Phase 4 — Wait

```
Brief complete. Type `confirmed` to begin building, or tell me what to adjust.
```

Do **not** call `use_figma` until the user types `confirmed`.
