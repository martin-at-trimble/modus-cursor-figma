---
name: figma-style-binding
description: >-
  Triggers on color, typography, spacing, padding, gap, or radius in Figma. Enforces
  Style/Variable binding only; post-write QA on returned node IDs.
---

# Style Binding + QA (Cursor)

Prerequisite: **figma-preflight** ran this session.

Binding order: library (`search_design_system`) → local style → local variable → report gap and wait.

Same forbidden/required table as claude2figma: no raw `fontSize`, raw RGB fills, raw padding numbers; use `textStyleId`, variable-bound paints, `setBoundVariable` for spacing and radius.

After every mutating `use_figma`, audit returned node IDs (TEXT → `textStyleId`; fills → style or bound variables; auto-layout padding/gap/radius → `boundVariables`). Fix failures before the next section.

Report `✅ All N nodes passed` or `❌ FAIL` with node name and unbound props.
