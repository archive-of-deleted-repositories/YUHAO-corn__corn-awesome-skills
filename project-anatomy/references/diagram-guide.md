# Diagram Guide

How to produce the final architecture diagram for a project anatomy analysis.

## Layout Structure

The diagram follows a specific pattern (modeled after the YT Digest / Deskhand architecture diagram style):

### 1. Title Area (top)
- Project name in large text
- One-line description as subtitle (e.g., "Electron App — AI Desktop Agent")

### 2. Process Zones (middle, side by side)
One large colored rectangle per process, arranged horizontally. Each zone contains:

- **Zone label** — process name + runtime (e.g., "RENDERER PROCESS — Chromium", "SERVICE WORKER — background.js")
- **Entry file** — highlighted box at the top of the zone
- **Contents** — the objective units inside this process:
  - For UI processes: components as individual boxes, each with a brief plain-language description underneath
  - For logic processes: functions as individual boxes, grouped visually by what they interact with (AI API, OS, storage), each with a description
- Components and functions that are closely related can be visually grouped, but each box should still be a single objective unit

### 3. Communication Arrows (between zones)
- Labeled with the mechanism: IPC, chrome.messages, HTTP, WebSocket, etc.
- Show direction of data flow

### 4. External APIs (below process zones)
- One box per external service (Anthropic API, Supabase, etc.)
- Brief description of what it's used for
- Arrow connecting it to the process that calls it

### 5. Supporting Files (optional row, below APIs)
- Config files, manifest, prompt templates — only if they're important to understanding the product
- These are not objective units but provide useful context

### 6. Data Flow (bottom)
A horizontal chain of boxes showing one representative user action end-to-end. Each box states: **"[Subject] does [action]"**

Good examples:
- "User types in InputToolbar"
- "Main receives via IPC"  
- "Agent.chat() calls Anthropic API"
- "Events stream back via IPC"
- "useAgentEvents() updates ChatArea"

Bad examples (too vague):
- "User input"
- "IPC → main"
- "API call"
- "UI updates"

## Color Conventions

Assign consistent colors so the viewer can immediately tell what type of thing they're looking at:

| Element Type | Background Color | Border Color | Example |
|---|---|---|---|
| Process zone (UI) | Light blue `#dbe4ff` | Blue `#4a9eed` | Renderer process |
| Process zone (Logic) | Light purple `#e5dbff` | Purple `#8b5cf6` | Main process |
| Process zone (OS/System) | Light green `#d3f9d8` | Green `#22c55e` | Filesystem |
| Components (UI elements) | Light red `#ffc9c9` | Red `#ef4444` | ChatArea, Sidebar |
| Functions (logic actions) | Light pink `#eebefa` | Purple `#8b5cf6` | chat(), stop() |
| Storage/OS functions | Light teal `#c3fae8` | Green `#22c55e` | sessions r/w |
| Event listeners/hooks | Light yellow `#fff3bf` | Amber `#f59e0b` | useAgentEvents() |
| External APIs | Light orange `#ffd8a8` | Orange `#f59e0b` | Anthropic API |
| Communication (IPC etc.) | — | Red `#ef4444` | IPC arrows |
| Data flow boxes | Match the process color they belong to | — | — |

## Sizing and Readability

- Each box should be large enough to read comfortably (minimum 150×40)
- Description text goes BELOW each box, not inside it
- Leave generous spacing between boxes
- Entry files should be visually distinct (placed at top of zone, slightly larger)
- Use Excalidraw's camera updates to create a good viewport — the full diagram should be visible at the default zoom level

## Example

For a project like Deskhand (Electron AI desktop agent), the layout would be:

```
[Title: DESKHAND — Architecture]
[Subtitle: Electron App — AI Desktop Agent]

[RENDERER PROCESS zone]        [MAIN PROCESS zone]           [OS/FILESYSTEM zone]
  main.tsx → App.tsx              index.ts                      Clipboard
  ┌─────────┐ ┌──────────┐       ┌──────────────┐              File Dialogs
  │ChatArea  │ │InputBar  │  IPC  │DeskhandAgent │              ~/.deskhand/
  │          │ │          │ ←───→ │ chat()       │              skills/
  │Sidebar   │ │Artifact  │       │ stop()       │
  │          │ │Panel     │       │tool-matching │
  │AuthForm  │ │PermReq   │       │Insight       │
  └─────────┘ └──────────┘       │ pipeline     │
  useAgentEvents()                │sessions/conf │
                                  └──────────────┘
                                         │
                                  [Anthropic API]

[DATA FLOW: User types in InputToolbar → Main receives via IPC → Agent.chat() calls API → Events stream back → useAgentEvents() updates ChatArea]
```

This is a rough text sketch. The actual diagram should be produced using Excalidraw with proper boxes, colors, and arrows.
