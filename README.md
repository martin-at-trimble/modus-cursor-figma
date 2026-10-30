# modus-cursor-figma

Cursor workspace to adapt [claude2figma](https://github.com/senlindesign/claude2figma) for **Trimble Modus 2.0** in Figma (MCP + skills).

Open this folder in Cursor when you run design sessions against the Modus atomic library.

## Quick start

1. **Open this folder in Cursor** (`File → Open Folder` → `modus-cursor-figma`).

2. **Figma MCP**
   - Recommended: in chat, run `/add-plugin figma`, then **Cursor Settings → Tools & MCP → Connect** (Figma OAuth).
   - Or copy `.cursor/mcp.json.example` to `.cursor/mcp.json` and Connect.

3. **Configure**
   - Edit [PROJECT.md](./PROJECT.md):
     - Design system library URL is pre-filled (Modus 2.0 Atomic).
     - Add a **destination file** URL (team playground or product file with the Modus library **enabled**).

4. **Start a session**
   - Say: `let's start` → agent runs **figma-preflight**.
   - For screenshot → Figma page: attach image, ask for a Design Brief; type `confirmed` to build.

## Skills included

| Skill | Role |
|-------|------|
| `figma-preflight` | MCP check, PROJECT.md, Token Map |
| `reference-interpreter` | Screenshot / description → Design Brief |
| `component-rules` | Library-first instances, Auto Layout |
| `figma-style-binding` | Variables/styles + QA |
| `modus-figma-context` | Modus 2.0 library + file roles |

Also use Figma plugin skills in Cursor when installed (**figma-use**, **figma-generate-design**, etc.).

## Example prompt

```text
Let's start. I'm attaching a dashboard screenshot.
Propose a Design Brief using Modus 2.0 components. Do not build until I type confirmed.
```

## Config

- [config/figma.defaults.json](./config/figma.defaults.json) — library URL and destination placeholder
- [ATTRIBUTION.md](./ATTRIBUTION.md) — claude2figma MIT adaptation

## License

Adaptation MIT; see ATTRIBUTION.md for upstream claude2figma credit.
