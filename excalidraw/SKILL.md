---
name: excalidraw
description: >
  Generate high-quality Excalidraw diagram files (.excalidraw). Use this skill whenever the user asks
  to create architecture diagrams, flowcharts, sequence diagrams, data flow diagrams, mind maps,
  system overviews, or any visual diagram in Excalidraw format. Also trigger when the user mentions
  "excalidraw", "draw a diagram", "visualize this architecture", "make a flowchart", or wants to
  explain a concept visually. This skill ensures Claude uses labeled shapes (text embedded inside
  shapes), arrow bindings (arrows that snap to shape edges), and proper color palettes — producing
  professional diagrams instead of broken layouts with misaligned text.
---

# Excalidraw Diagram Generator

Generate `.excalidraw` files that open directly in excalidraw.com or the Excalidraw VS Code extension.

## Output Format

Write a valid JSON file with `.excalidraw` extension. The file structure is:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "claude",
  "elements": [ ... ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": null
  },
  "files": {}
}
```

The `elements` array contains all shapes, text, and arrows. This is the only part you need to build.

---

## Critical Rules (Read First!)

### Rule 1: ALWAYS use labeled shapes — NEVER float text over shapes

**WRONG** — separate text element positioned on top of a rectangle:
```json
{"type": "rectangle", "id": "r1", "x": 100, "y": 100, "width": 200, "height": 80},
{"type": "text", "id": "t1", "x": 150, "y": 125, "text": "My Label", "fontSize": 20}
```
This WILL misalign. The text is not bound to the rectangle.

**RIGHT** — use `boundElements` on the shape + `containerId` on the text:
```json
{
  "type": "rectangle", "id": "r1", "x": 100, "y": 100, "width": 200, "height": 80,
  "backgroundColor": "#a5d8ff", "fillStyle": "solid", "roundness": {"type": 3},
  "boundElements": [{"id": "t1", "type": "text"}]
},
{
  "type": "text", "id": "t1", "x": 105, "y": 112, "width": 190, "height": 56,
  "text": "My Label", "fontSize": 20,
  "textAlign": "center", "verticalAlign": "middle",
  "containerId": "r1"
}
```
When `containerId` is set, Excalidraw auto-centers the text inside the shape. The x/y/width/height of the text are approximate — Excalidraw recalculates on load.

### Rule 2: ALWAYS use arrow bindings — NEVER guess arrow endpoints

**WRONG** — arrow with hardcoded points that don't track shapes:
```json
{"type": "arrow", "id": "a1", "x": 300, "y": 140, "width": 150, "height": 0, "points": [[0,0],[150,0]]}
```

**RIGHT** — arrow bound to source and target shapes:
```json
{
  "type": "arrow", "id": "a1", "x": 300, "y": 140, "width": 150, "height": 0,
  "points": [[0,0],[150,0]],
  "endArrowhead": "arrow",
  "startBinding": {"elementId": "r1", "focus": 0, "gap": 4, "fixedPoint": [1, 0.5]},
  "endBinding": {"elementId": "r2", "focus": 0, "gap": 4, "fixedPoint": [0, 0.5]}
}
```
Also add the arrow to each shape's `boundElements`:
```json
{"type": "rectangle", "id": "r1", ..., "boundElements": [{"id": "t1", "type": "text"}, {"id": "a1", "type": "arrow"}]}
```

**fixedPoint reference:**
- Right edge center: `[1, 0.5]`
- Left edge center: `[0, 0.5]`
- Top edge center: `[0.5, 0]`
- Bottom edge center: `[0.5, 1]`

### Rule 3: Arrow labels use the same binding pattern

To label an arrow, create a text element with `containerId` pointing to the arrow:
```json
{
  "type": "arrow", "id": "a1", ...,
  "boundElements": [{"id": "al1", "type": "text"}]
},
{
  "type": "text", "id": "al1", "x": 350, "y": 125, "width": 60, "height": 20,
  "text": "JSON", "fontSize": 14,
  "textAlign": "center", "verticalAlign": "middle",
  "containerId": "a1"
}
```

---

## Element Defaults

Every element should include these base properties (adjust as needed):

```json
{
  "strokeColor": "#1e1e1e",
  "backgroundColor": "transparent",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "roughness": 1,
  "opacity": 100,
  "angle": 0,
  "seed": 1,
  "groupIds": [],
  "frameId": null,
  "boundElements": [],
  "updated": 1,
  "locked": false,
  "link": null
}
```

For text elements, also include:
```json
{
  "fontFamily": 5,
  "textAlign": "center",
  "verticalAlign": "middle",
  "lineHeight": 1.2,
  "baseline": 18
}
```

Use `fontFamily: 5` (Excalifont/hand-drawn) for the default Excalidraw look. Use `fontFamily: 3` for monospace if needed.

---

## Color Palette

Use these colors consistently for a professional look.

### Shape Fills (pastel backgrounds)
| Purpose | Hex | Use For |
|---------|-----|---------|
| Light Blue | `#a5d8ff` | Input, sources, primary nodes |
| Light Green | `#b2f2bb` | Success, output, completed |
| Light Orange | `#ffd8a8` | Warning, pending, external |
| Light Purple | `#d0bfff` | Processing, middleware, special |
| Light Red | `#ffc9c9` | Error, critical, alerts |
| Light Yellow | `#fff3bf` | Notes, decisions, planning |
| Light Teal | `#c3fae8` | Storage, data, memory |

### Stroke / Arrow Colors (bold)
| Name | Hex | Use |
|------|-----|-----|
| Blue | `#4a9eed` | Primary actions, links |
| Amber | `#f59e0b` | Warnings, highlights |
| Green | `#22c55e` | Success, positive |
| Red | `#ef4444` | Errors, negative |
| Purple | `#8b5cf6` | Accents, special |
| Cyan | `#06b6d4` | Info, secondary |

### Background Zones (use opacity: 30)
| Color | Hex | Use |
|-------|-----|-----|
| Blue zone | `#dbe4ff` | UI / frontend layer |
| Purple zone | `#e5dbff` | Logic / agent layer |
| Green zone | `#d3f9d8` | Data / tool layer |

---

## Sizing Guidelines

- Minimum shape size: **140×70** for labeled rectangles
- Minimum gap between elements: **30px**
- Font sizes: **20+** for titles, **16–18** for labels, **14** for annotations only
- Prefer fewer, larger elements over many tiny ones
- Use `roundness: {"type": 3}` on rectangles for rounded corners

---

## Complete Example: Two Connected Labeled Boxes

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "claude",
  "elements": [
    {
      "type": "rectangle", "id": "box1", "x": 100, "y": 100, "width": 200, "height": 80,
      "strokeColor": "#4a9eed", "backgroundColor": "#a5d8ff", "fillStyle": "solid",
      "strokeWidth": 2, "roughness": 1, "opacity": 100, "angle": 0,
      "roundness": {"type": 3},
      "seed": 1, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "boundElements": [
        {"id": "box1_label", "type": "text"},
        {"id": "arrow1", "type": "arrow"}
      ]
    },
    {
      "type": "text", "id": "box1_label", "x": 105, "y": 118, "width": 190, "height": 44,
      "text": "Start Here", "fontSize": 20,
      "fontFamily": 5, "textAlign": "center", "verticalAlign": "middle",
      "lineHeight": 1.2, "baseline": 18,
      "strokeColor": "#1e1e1e", "opacity": 100, "angle": 0,
      "seed": 2, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "containerId": "box1"
    },
    {
      "type": "rectangle", "id": "box2", "x": 450, "y": 100, "width": 200, "height": 80,
      "strokeColor": "#22c55e", "backgroundColor": "#b2f2bb", "fillStyle": "solid",
      "strokeWidth": 2, "roughness": 1, "opacity": 100, "angle": 0,
      "roundness": {"type": 3},
      "seed": 3, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "boundElements": [
        {"id": "box2_label", "type": "text"},
        {"id": "arrow1", "type": "arrow"}
      ]
    },
    {
      "type": "text", "id": "box2_label", "x": 455, "y": 118, "width": 190, "height": 44,
      "text": "End Here", "fontSize": 20,
      "fontFamily": 5, "textAlign": "center", "verticalAlign": "middle",
      "lineHeight": 1.2, "baseline": 18,
      "strokeColor": "#1e1e1e", "opacity": 100, "angle": 0,
      "seed": 4, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "containerId": "box2"
    },
    {
      "type": "arrow", "id": "arrow1", "x": 300, "y": 140, "width": 150, "height": 0,
      "points": [[0, 0], [150, 0]],
      "strokeColor": "#1e1e1e", "strokeWidth": 2, "roughness": 1, "opacity": 100, "angle": 0,
      "endArrowhead": "arrow", "startArrowhead": null,
      "seed": 5, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "startBinding": {"elementId": "box1", "focus": 0, "gap": 4, "fixedPoint": [1, 0.5]},
      "endBinding": {"elementId": "box2", "focus": 0, "gap": 4, "fixedPoint": [0, 0.5]},
      "boundElements": [{"id": "arrow1_label", "type": "text"}]
    },
    {
      "type": "text", "id": "arrow1_label", "x": 345, "y": 120, "width": 60, "height": 20,
      "text": "next", "fontSize": 16,
      "fontFamily": 5, "textAlign": "center", "verticalAlign": "middle",
      "lineHeight": 1.2, "baseline": 14,
      "strokeColor": "#1e1e1e", "opacity": 100, "angle": 0,
      "seed": 6, "groupIds": [], "frameId": null, "locked": false, "link": null,
      "updated": 1,
      "containerId": "arrow1"
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": null
  },
  "files": {}
}
```

---

## Drawing Order

Build the diagram progressively: **background zones → shape + its label → arrows from that shape → next shape + label → ...**

Do NOT create all rectangles first, then all text, then all arrows. Keep related elements together so the file is readable and maintainable.

---

## Checklist Before Finishing

1. Every shape with text uses `boundElements` + a text element with `containerId` (no floating text!)
2. Every arrow uses `startBinding` / `endBinding` with `fixedPoint` (no disconnected arrows!)
3. Arrow references appear in the `boundElements` of both source and target shapes
4. Arrow labels use `containerId` pointing to the arrow, and the arrow has the label in `boundElements`
5. Colors are from the palette above — consistent and readable
6. Font sizes are 16+ for labels, 20+ for titles
7. All `id` values are unique strings
8. File extension is `.excalidraw`
