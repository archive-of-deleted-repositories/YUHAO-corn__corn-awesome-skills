# Example: MCP vs RAG vs AI Agents comparison diagram

## Context

The user wanted a professional concept diagram comparing three AI architecture patterns. This prompt was used with Google Gemini's image generation and produced a high-quality result on the first attempt.

## The prompt

```
Create a professional tech infographic comparing three AI architecture patterns: MCP, RAG, and AI Agents. Style similar to ByteByteGo system design diagrams — clean, modern, with pastel colors, rounded nodes, dashed connection lines, and small icons.

MCP (Model Context Protocol):
MCP solves the problem of how LLMs connect to external tools. It works through a standardized client-server architecture. Multiple LLM applications (like Claude Desktop, IDEs, AI Tools) each contain an MCP Client. These clients communicate through a unified MCP Protocol with multiple MCP Servers. Each server is a gateway to a different type of external resource — one server connects to Web APIs like GitHub and Slack, another executes database queries, another reads and writes to the filesystem. The key insight is that the protocol is standardized, so any client can talk to any server.

RAG (Retrieval Augmented Generation):
RAG solves the problem of LLMs lacking specific knowledge. It's a 5-step loop: (1) A user sends a query, (2) a Retriever searches a Knowledge Base containing PDFs, vector databases, and code, (3) relevant documents are returned, (4) the original query plus the retrieved documents are sent together to an LLM like GPT, Gemini, or Claude, (5) the LLM generates a response informed by the retrieved knowledge and returns it to the user.

AI Agents:
AI Agents solve the problem of LLMs being passive — they enable LLMs to take autonomous actions. A human sets goals and an autonomy level. The AI Agent receives tasks, accesses persistent memory, and can invoke tools including API calls, internet access, and code interpretation. The core mechanism is an autonomous reasoning loop: the Agent decides on an action, executes it in the environment, observes the results, and feeds those observations back into its reasoning to decide the next action. This loop continues until the task is complete.

Show all three concepts in one unified infographic, each as a distinct visual section.
```

## Why this prompt works — layer by layer

### Layer 1 (first paragraph)
- **"professional tech infographic"** — anchors the output type immediately
- **"comparing three AI architecture patterns"** — tells the model this is a comparison layout, so it knows to create distinct sections
- **"Style similar to ByteByteGo"** — gives a concrete visual reference without dictating specific colors or fonts
- **"clean, modern, with pastel colors, rounded nodes, dashed connection lines, and small icons"** — a few high-level style adjectives to reinforce the direction, but nothing that constrains specific choices

### Layer 2 (the three concept paragraphs)
Each paragraph follows the same internal structure:
1. **"X solves the problem of..."** — gives the model a semantic anchor to understand what makes each concept different
2. **Core mechanism described as logical flow** — "clients communicate through a protocol with servers" rather than "draw lines from left boxes to right boxes"
3. **Named components with relationships** — "Claude Desktop, IDEs, AI Tools" not "three boxes on the left"

Notice what's absent: no positional words (left, right, above), no visual instructions (draw, color, size), no layout directions (horizontal, vertical). The model decides all of this based on its understanding of the logic.

### Layer 3 (last line)
- **"one unified infographic, each as a distinct visual section"** — two big-picture constraints: (1) it's one image not three, (2) the sections should be visually distinguishable

## Result

The generated image had:
- Three clearly separated sections with distinct pastel backgrounds
- Correct logical relationships within each section
- Professional typography and clean layout
- Appropriate icons for each component
- The model chose its own layout and color scheme, which was better than what we would have specified manually

## Minor issues in the result

- RAG section had a duplicated "LLM" node — the model created two visual representations of the same concept
- AI Agent section's reasoning loop was slightly ambiguous in its circular flow

These are the kinds of issues to watch for: **logical accuracy** needs human review, but **visual design** was handled well by the model.
