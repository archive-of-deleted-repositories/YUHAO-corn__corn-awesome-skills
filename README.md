<p align="center">
  <img src="https://img.shields.io/badge/⚡-Corn's_Awesome_Skills-blueviolet?style=for-the-badge&labelColor=black" alt="Corn's Awesome Skills" height="60">
</p>

<p align="center">
  <strong>A collection of Claude Code skills for analysis, visualization, and content creation.</strong>
</p>

<p align="center">
  <a href="https://github.com/YUHAO-corn/corn-awesome-skills">
    <img src="https://img.shields.io/github/stars/YUHAO-corn/corn-awesome-skills?style=flat-square" alt="Stars">
  </a>
  <a href="https://twitter.com/godcorn">
    <img src="https://img.shields.io/twitter/follow/godcorn?style=flat-square" alt="Twitter">
  </a>
</p>

<hr/>

# Corn's Awesome Skills

A practical collection of Claude Code skills designed to solve real problems. Each skill is ready to use out of the box.

## Why This Repo?

- 🛠️ **Plug & Play** — Install and trigger naturally in conversation, no complex setup
- 🎯 **Real Problem Solvers** — From code analysis to diagram generation, technical writing to frontend design
- 🎨 **Quality First** — Focus on visual output quality, not just "it works"

## What's Inside

| # | Skill | Description |
|---|-------|-------------|
| 01 | [project-anatomy](./project-anatomy) | Analyze software architecture using objective units — processes, entry files, components, functions |
| 02 | [theater-explainer](./theater-explainer) | Transform technical articles into interactive "theater" web pages with character dialogues |
| 03 | [excalidraw](./excalidraw) | Generate professional Excalidraw diagrams with bound text and arrows |
| 04 | [concept-diagram-prompt](./concept-diagram-prompt) | Generate prompts for AI image generation of concept diagrams |

## How to Use

### Install a Single Skill

```bash
npx skills add https://github.com/YUHAO-corn/corn-awesome-skills/tree/main/project-anatomy --agent claude-code
```

### Install All Skills

```bash
npx skills add https://github.com/YUHAO-corn/corn-awesome-skills --agent claude-code
```

### Trigger a Skill

After installation, use natural language in your Claude Code conversation:

- "Help me analyze this codebase" → triggers project-anatomy
- "Make this into a theater format" → triggers theater-explainer
- "Draw an architecture diagram" → triggers excalidraw

## Who Is This For

- Developers who want to deeply understand codebases
- Engineers needing to generate technical docs/diagrams
- Content creators wanting creative ways to explain technical concepts

## Contributing

Have a useful skill to share? PRs welcome!
