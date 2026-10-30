---
name: component-rules
description: >-
  Triggers when building UI in Figma — cards, nav, sections, forms. Library-first
  lookup, Auto Layout, semantic naming. Visual binding defers to figma-style-binding.
---

# Component Rules (Cursor + Modus)

Governs Figma construction. Load **figma-generate-design** when assembling full pages. Defer colors, type, spacing to **figma-style-binding**.

Load **modus-figma-context** when mapping component names to Modus atomic patterns.

---

## Rule 1 — Library-first hierarchy

```
1. search_design_system → importComponentByKeyAsync → createInstance()
2. Local file scan → createInstance()
3. Build from scratch — ONLY if nothing matches
```

Never rebuild Modus primitives if the library provides them: Button, Text field, Checkbox, Switch, Badge, Chip, Avatar, Icon, Tabs, Breadcrumb, Alert, Modal shell, Card, Navbar patterns, Table row patterns, etc.

```javascript
const comp = await figma.importComponentByKeyAsync("key_from_search");
const instance = comp.createInstance();
parent.appendChild(instance);
```

---

## Rule 2 — Auto Layout

Every container uses Auto Layout. Set `layoutMode` first; sizing after `appendChild`; `layoutMode` before `setBoundVariable`.

---

## Rule 3 — Node naming

Semantic slash names: `Page / Hero`, `Card / Title`, `Form / Field group`, `Actions / Primary`.

---

## Rule 4 — Incremental building

One section per `use_figma` call. Screenshot after each step. Return created node IDs for QA in **figma-style-binding**.

**Write target:** Use the **destination file** from `PROJECT.md`, with **Modus 2.0** library enabled — not ad-hoc frames inside the library file unless the user explicitly requests it.
