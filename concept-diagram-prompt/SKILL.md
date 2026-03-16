---
name: concept-diagram-prompt
description: Generate image-generation prompts for professional concept diagrams, architecture diagrams, infographics, flowcharts, funnels, pyramids, and similar visual explanations of technical or business concepts. Use this skill whenever the user wants to create a concept diagram, system architecture diagram, comparison chart, process flow, funnel, pyramid, or any visual that explains logical relationships between concepts. Also trigger when the user says things like "help me draw", "make a diagram of", "visualize this concept", "create an infographic", or asks for a visual explanation of any topic. This skill does NOT generate the image itself — it generates an optimized prompt that the user takes to an image generation model (like Google Gemini/Imagen).
---

# Concept Diagram Prompt Generator

This skill generates high-quality prompts for AI image generation models to create professional concept diagrams. The output is a text prompt — not an image, not code, not an SVG. The user will take this prompt to an image generation model (defaulting to Google Gemini) to produce the final visual.

## Why this skill exists

Concept diagrams are hard for AI to generate well. The key insight we've discovered through experimentation:

- **Generating code to draw diagrams** (SVG, React, Mermaid) fails at complex layouts — text overlaps, arrows misalign, shapes are limited to rectangles.
- **Generating images directly from AI** works much better, but only if the prompt is written correctly. Most people write prompts that over-specify visual layout ("put X on the left, Y on the right") which constrains the model and produces worse results.

The correct approach: **describe the logical relationships and let the image model make its own design decisions.** This skill encodes that methodology.

## Core methodology: Three-layer prompt structure

Every prompt this skill generates follows three layers:

### Layer 1: Anchor the direction (WHAT type of visual, WHAT style)

Tell the model what kind of output you need and give it a stylistic reference point. This is necessary because the space of possible visuals is too large — without anchoring, the model may produce something completely different from what the user expects.

What to include:
- The type of visual (infographic, architecture diagram, flowchart, comparison chart, funnel, pyramid, etc.)
- A style reference (default: "ByteByteGo system design diagram style" — clean, modern, pastel colors, rounded nodes, dashed connection lines, small icons)
- The overall subject in one sentence

What NOT to include:
- Specific colors, fonts, or spacing
- Layout directions like "horizontal" or "left-to-right" (unless the concept inherently requires a specific orientation)
- Exact dimensions or proportions

Example:
> Create a professional tech infographic comparing three AI architecture patterns: MCP, RAG, and AI Agents. Style similar to ByteByteGo system design diagrams — clean, modern, with pastel colors, rounded nodes, dashed connection lines, and small icons.

### Layer 2: Describe the logic, not the layout (the SUBSTANCE)

This is the core of the prompt and where most people go wrong. For each concept or section in the diagram, describe:

1. **What problem it solves** — this gives the model semantic context to differentiate sections visually
2. **The core mechanism** — how it works, described as a logical flow
3. **How information moves** — which components connect to which, and why

Write this as if you're explaining the concept to a smart colleague, not as if you're giving drawing instructions to a designer.

What to include:
- Logical relationships: "A communicates with B through C"
- Causal flows: "First X happens, which triggers Y, resulting in Z"
- Hierarchies: "There are three types of X, each serving a different purpose"
- Feedback loops: "The output is fed back as input for the next cycle"

What NOT to include:
- "On the left side, draw..."
- "Use an arrow pointing from..."
- "Make this box bigger than..."
- "Place X above Y..."

The reasoning: image models have seen millions of well-designed diagrams. When you describe logic clearly, they naturally choose appropriate visual representations. When you dictate layout, you create conflicts between the model's design instincts and your spatial instructions — resulting in worse output.

Example:
> MCP solves the problem of how LLMs connect to external tools. It works through a standardized client-server architecture. Multiple LLM applications (like Claude Desktop, IDEs, AI Tools) each contain an MCP Client. These clients communicate through a unified MCP Protocol with multiple MCP Servers. Each server is a gateway to a different type of external resource — one server connects to Web APIs like GitHub and Slack, another executes database queries, another reads and writes to the filesystem. The key insight is that the protocol is standardized, so any client can talk to any server.

### Layer 3: Reinforce the big-picture constraints (the GUARDRAILS)

End with any high-level structural requirements. These are things the model might otherwise get wrong because they're about the overall composition, not any single concept.

What to include:
- How multiple sections relate to each other ("show all three as distinct sections in one unified infographic")
- Overall composition requirements ("each section should feel visually distinct")
- Readability requirements ("text should be crisp and readable")

What NOT to include:
- Micro-level design details
- Repeated instructions from Layer 1

## Default style

When the user does not specify a visual style, default to the ByteByteGo system design diagram style. This is characterized by: clean and modern aesthetic, pastel color backgrounds for different sections, rounded rectangle nodes, dashed connection lines, small meaningful icons, clear typography, white or very light overall background. This is a well-known reference point that image models understand well and consistently produce good results for.

If the user specifies a different style (hand-drawn, minimalist, corporate, etc.), replace the style reference in Layer 1 accordingly, but keep the three-layer structure unchanged.

## Process

When a user asks for a concept diagram:

1. **Understand the concept.** Ask clarifying questions if the user's description is ambiguous or incomplete. You need to understand the logical relationships well enough to describe them clearly.

2. **Identify the diagram type.** Is this a comparison, a flow, a hierarchy, a cycle, a funnel, a pyramid? This determines the Layer 1 anchor.

3. **Write the prompt** following the three-layer structure. Read the reference example in `references/example-prompt.md` before writing — it demonstrates the methodology applied to a real case.

4. **Present the prompt** to the user as a copyable text block. Remind them to paste it into their image generation model (Google Gemini by default).

5. **After the user generates the image**, help them review the logical accuracy. Visual design quality is the model's job; logical correctness is what needs human verification.

## Common pitfalls to avoid

- **Over-specifying layout**: The #1 failure mode. If you catch yourself writing positional words (left, right, above, below, next to), stop and rewrite as logical relationships.
- **Mixing logic and visuals**: Don't say "draw a circle for X and connect it with a line to Y." Say "X communicates with Y."
- **Being too abstract**: "The system has multiple components" is useless. Name the components and explain how they relate.
- **Forgetting the problem statement**: Each concept should start with what problem it solves. This gives the model the semantic hook to differentiate concepts visually.
- **Cramming too much**: If the concept has more than 5-6 major components, consider splitting into multiple diagrams rather than one overloaded image.

## Reference

Read `references/example-prompt.md` for a complete example of a prompt generated with this methodology, along with commentary on why each part works.
