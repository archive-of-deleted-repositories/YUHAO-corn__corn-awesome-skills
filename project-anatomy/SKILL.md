---
name: project-anatomy
description: Analyze and visualize the real architecture of a software project using objective units — processes, entry files, components, and functions — instead of abstract layers or folder structures. Use this skill whenever the user wants to understand a codebase's structure, asks "how is this project organized", wants an architecture diagram, asks to "break down" or "dissect" a project, or wants to understand how a product works from an engineering perspective. Also trigger when the user uploads or links to a GitHub repo and asks what's inside, or when they say things like "explain this project to me", "what are the main parts", "draw me an architecture diagram", or "help me understand the codebase". This skill is specifically about finding the real, physical, meaningful units in a project — not about generating generic diagrams with abstract boxes like "Business Logic Layer" or "Data Access Layer".
---

# Project Anatomy

Analyze software projects by identifying their **objective units** — building blocks that are physically real in the code AND have concrete business meaning.

## Core Principle

An objective unit has three properties:

1. **Physically real** — it exists as a tangible thing (a running process, an entry file, a callable function, a visible UI component)
2. **Stable across developers** — two different developers building the same product would still have these units, regardless of how they organize folders
3. **Business-meaningful** — maps to something a human can point to: "that's the chat window", "that's the function that talks to the AI"

**Never** use folders, abstract layers ("Business Logic Layer"), or technology labels ("React frontend") as primary organizational units. These are filing or labeling conventions, not objective structure.

## Objective Unit Hierarchy

From largest to smallest:

- **Processes** — independent runtime environments, visible in task manager
- **Entry files** — the file where each process begins execution (e.g. `index.ts`, `background.js`)
- **Components** — (UI side) each maps to a visible screen area
- **Functions** — (logic side) each maps to a concrete action ("sends message to AI", "opens folder picker")

## Analysis Workflow

Follow these steps in order. Each builds on the previous.

1. **Identify processes** — how many independent runtimes exist? (Electron: main + renderer. Browser extension: content script + service worker + sidepanel. Web app: browser + server.)
2. **Find entry files** — for each process, which exact file does execution start from? Check `package.json`, `manifest.json`, framework configs.
3. **Trace downward from each entry file** — what does it import and call?
   - UI processes → list major **components** (each = a visible screen area)
   - Logic processes → list key **functions** (each = a concrete action)
   - Code in separate packages that gets imported into a process is PART of that process, not a separate entity
4. **Map external dependencies** — APIs, databases, OS capabilities
5. **Trace one user action end-to-end** — pick the most representative action and walk through every file/function/component it touches, naming each one specifically
6. **Produce architecture diagram** — see `references/diagram-guide.md` for the exact layout pattern and conventions

## Communication Style

Always use concrete language:
- Say "this function sends your message to the AI" not "this module handles message dispatch"
- Say "this component draws the chat bubble" not "this is the message rendering layer"

When the user asks "what's inside X", answer with the next level of objective units, not file/folder names.

## References

- `references/diagram-guide.md` — Layout pattern, color conventions, and example for producing the architecture diagram. Read this before Step 6.
- `references/process-patterns.md` — How to identify processes and entry files for common project types (Electron, browser extensions, web apps, mobile). Read this if you're unsure about Step 1-2.
