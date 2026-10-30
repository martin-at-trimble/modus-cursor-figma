---
name: modus-figma-context
description: >-
  Trimble Modus 2.0 Figma library context — atomic design, default library URL,
  destination vs library files, and naming hints when searching the design system.
---

# Modus 2.0 Figma context

## Canonical library

- **File:** Modus 2.0 — Atomic Design System
- **URL:** https://www.figma.com/design/y9H5ucQKBjzI8JLuVrGcb3/Modus-2.0---Atomic-Design-System?node-id=11218-2226
- **Defaults:** `config/figma.defaults.json`

## File roles

| Role | Purpose |
|------|---------|
| **Library file** | Source of truth for components, variables, text styles |
| **Destination file** | Where product pages and prototypes are created (`PROJECT.md`) |

Do not add product screens to the library file unless the team explicitly works there.

## Atomic design (Figma)

When interpreting references or briefs, prefer existing **atoms → molecules → organisms** from the library before composing raw frames.

Search terms that often match Modus naming: `Button`, `Text input`, `Select`, `Checkbox`, `Switch`, `Badge`, `Chip`, `Avatar`, `Card`, `Navbar`, `Side navigation`, `Table`, `Modal`, `Alert`, `Tabs`, `Breadcrumb`, `Progress`.

## Theme

Modus Modern uses semantic variables (surface, content, primary, danger, etc.). Bind fills and text to **variables**, not one-off colors, so light/dark collections stay valid.
