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

实用优先的 Claude Code Skills 集合。每个 Skill 都是为了解决具体问题而设计的开箱即用工具。

## 🤔 Why This Repo?

- 🛠️ **开箱即用** — 安装后立即在对话中触发，无需复杂配置
- 🎯 **解决实际问题** — 从代码分析到图表生成，从技术写作到前端设计
- 🎨 **高设计质量** — 注重输出物的视觉呈现，不只是"能用"

## 🗂️ Skills 目录

| # | Skill | 用途 |
|---|-------|------|
| 01 | [project-anatomy](./project-anatomy) | 分析软件项目架构，用客观单位（进程、入口文件、组件、函数）理解代码库 |
| 02 | [theater-explainer](./theater-explainer) | 将技术文章转化为交互式"剧场"网页，让概念变成角色对话 |
| 03 | [excalidraw](./excalidraw) | 生成专业的 Excalidraw 图表，支持绑定文本和箭头 |
| 04 | [concept-diagram-prompt](./concept-diagram-prompt) | 生成用于 AI 图像生成的概念图提示词 |

## 🚀 如何使用

### 安装单个 Skill

```bash
npx skills add https://github.com/YUHAO-corn/corn-awesome-skills/tree/main/project-anatomy --agent claude-code
```

### 安装全部 Skills

```bash
npx skills add https://github.com/YUHAO-corn/corn-awesome-skills --agent claude-code
```

### 触发 Skill

安装后，在 Claude Code 对话中直接使用自然语言触发：

- "帮我分析一下这个项目" → 触发 project-anatomy
- "把这个做成剧场形式" → 触发 theater-explainer
- "画个架构图" → 触发 excalidraw

## 👤 谁适合用

- 想深入理解代码库的开发者
- 需要生成技术文档/图表的工程师
- 想要创意化表达技术内容的内容创作者

## ⭐ 欢迎贡献

如果你有实用的 Skill想要分享，欢迎提交 PR！
