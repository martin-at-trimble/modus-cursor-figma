---
name: figma-preflight
description: >-
  Triggers on "let's start", "preflight", "start the session", or
  when a Figma file URL is first shared. Verifies Figma MCP, reads PROJECT.md,
  audits libraries, and loads a Token Map — required before any Figma design work.
---

# Figma Preflight (Cursor)

Run at the start of every design session. Do **not** start design work until all steps pass.

**Prerequisites**

- Figma MCP connected in Cursor (`/add-plugin figma` or `.cursor/mcp.json` → Connect).
- Load **figma-use** (Figma plugin skill) before any `use_figma` call.

**Tool names:** Use the Figma MCP tools exposed in Cursor (e.g. `whoami`, `get_metadata`, `get_libraries`, `search_design_system`, `use_figma`). Do not assume Claude Code `mcp__figma__*` prefixes.

---

## Step A — Connection + config (parallel)

1. **MCP connection:** Call Figma MCP `whoami`. Must return user and plan. Fail → stop; user reconnects in **Cursor Settings → Tools & MCP**.
2. **PROJECT.md:** Read `PROJECT.md` at the workspace root. Extract:
   - Design system library URL (required)
   - Destination file URL (required before **writing** pages — warn if empty)
   - Session goal
   - Font note (auto-fill after Step C from text styles / STRING variables if blank)

---

## Step B — File + libraries (parallel)

Parse `fileKey` from the **library** URL (and destination URL if set). Run in parallel:

1. **Library access:** `get_metadata` with library `fileKey` and optional `node-id`.
2. **Libraries:** `get_libraries` on the **destination** file if set; otherwise on the library file. Store **Library Registry** for `search_design_system`.

---

## Step C — Styles + variables + components (single `use_figma`)

Run one script on the **library file** (read-only inventory):

```javascript
const textStyles = await figma.getLocalTextStylesAsync();
const paintStyles = await figma.getLocalPaintStylesAsync();
const collections = await figma.variables.getLocalVariableCollectionsAsync();
const variables = await figma.variables.getLocalVariablesAsync();

const grouped = {};
for (const v of variables) {
  const key = v.resolvedType;
  if (!grouped[key]) grouped[key] = [];
  grouped[key].push({ name: v.name, scopes: v.scopes });
}

const components = {};
for (const page of figma.root.children) {
  await figma.setCurrentPageAsync(page);
  const sets = page.findAll((n) => n.type === "COMPONENT_SET");
  const solos = page.findAll(
    (n) => n.type === "COMPONENT" && n.parent.type !== "COMPONENT_SET",
  );
  if (sets.length > 0 || solos.length > 0) {
    components[page.name] = {
      sets: sets.map((c) => c.name),
      solos: solos.map((c) => c.name).slice(0, 15),
    };
  }
}

return {
  textStyles: textStyles.map((s) => s.name),
  paintStyles: paintStyles.map((s) => s.name),
  collections: collections.map((c) => c.name),
  variableCount: variables.length,
  byType: Object.fromEntries(
    Object.entries(grouped).map(([type, vars]) => [
      type,
      vars.map((v) => v.name),
    ]),
  ),
  components,
};
```

Store **names only** in context. Resolve IDs on demand during design.

---

## Token Map

Derive a semantic index from variable `scopes` (same roles as claude2figma: background, text, border, gap, padding, radius).

---

## Status report

Print a checklist: MCP, PROJECT.md, library file, destination file (or ⚠️ missing), libraries, styles, variables, component pages, Token Map summary, then **Ready to design** or stop on ❌.

If destination file URL is empty, allow **read-only** sessions (brief, audit) but block **use_figma** writes until the user adds a destination URL to `PROJECT.md`.
