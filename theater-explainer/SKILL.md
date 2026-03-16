---
name: theater-explainer
description: Transform technical articles, documents, codebases, or concepts into interactive "theater" web pages where each component/role/concept becomes a personified character that explains the content through chat-style dialogue. Use this skill whenever the user wants to make technical content more accessible, asks to "explain this article/codebase/concept in a fun way", wants a visual/interactive explainer, mentions "theater", "dialogue", "personified", "让角色来讲", "对话式", "剧场", or wants to turn dry technical material into something engaging and educational. Also trigger when the user pastes a long article or uploads a technical document and asks for a creative or interactive explanation, or says things like "help me understand this", "make this easier to read", "turn this into something visual". This skill produces a single self-contained HTML file with animated chat bubbles, character avatars, and structured acts/phases.
---

# Theater Explainer

Turn any technical content into an interactive "theater" web page where concepts become characters and explanations become conversations.

## What This Skill Does

The user provides technical content (an article, a document, a codebase walkthrough, a concept description, etc.) and this skill transforms it into a single self-contained HTML file styled as a chat-based "theater" — each key entity (component, role, service, concept) becomes a personified character with its own avatar, color, and voice, explaining the content through dialogue.

The result is a polished, dark-themed, mobile-friendly web page that makes complex technical content genuinely enjoyable to read.

## Step 1: Analyze the Source Material

Before writing any code, deeply understand the content:

1. **Identify the cast** — What are the key entities? These could be software components, team roles, abstract concepts, system layers, API endpoints, or anything that "does something" in the content.
2. **Map the relationships** — Who talks to whom? Who depends on whom? What's the flow?
3. **Find the narrative arc** — Is there a linear flow (request→processing→response)? A timeline (week 1→week 2→...)? A hierarchy (boss→team)? A cycle?
4. **Extract the "aha moments"** — What insights, design decisions, or surprising details does the content contain? These become the most memorable lines of dialogue.
5. **Note practical/actionable content** — Code snippets, file structures, configuration examples, cost breakdowns. These get embedded as special visual elements (code blocks, file trees, tables, tip cards).

## Step 2: Choose a Theater Mode

Based on the narrative arc identified above, select the mode that best fits. Do not ask the user — just pick the best one and execute.

### Mode A: Relay Dialogue (接力对话)
Best for: **linear flows, request-response cycles, pipeline architectures**
Structure: Characters speak in sequence as data/control flows through them. Each character receives something, does its job, passes it on. Organized into "Acts" (幕).
Example use: "How does a web request travel through a microservice architecture?"

### Mode B: Onboarding Theater (入职培训)  
Best for: **tutorials, how-to guides, setup walkthroughs, multi-role team introductions**
Structure: Progressive week-by-week or step-by-step chapters. New characters are "hired"/introduced as complexity grows. Heavy on actionable content: code blocks, file trees, config examples, tip cards, cost tables.
Example use: "How to set up an AI agent team that runs 24/7"

### Mode C: Standup Meeting (站会/晨会)
Best for: **system overviews, comparing multiple parallel components, capability showcases**
Structure: Characters gather and each takes turns presenting what they do, their strengths, how they relate to others. Less sequential, more "show and tell".
Example use: "Explain the components of Kubernetes"

### Mode D: Investigation / Debug Theater (探案剧场)
Best for: **debugging stories, incident postmortems, "why does X happen" explanations**
Structure: A "detective" character investigates a problem, interrogating each component/suspect. Creates tension and reveals the answer step by step.
Example use: "Why does my API timeout after 30 seconds?"

### Mode E: Timeline Documentary (时间线纪录片)
Best for: **historical overviews, evolution of a technology, project retrospectives**
Structure: Chronological sections with timestamps. Characters from different eras narrate what changed and why.
Example use: "The evolution of JavaScript frameworks from jQuery to React"

If the content doesn't clearly fit one mode, default to **Mode A (Relay Dialogue)** — it's the most versatile.

## Step 3: Design the Characters

For each character, define:

- **Emoji avatar** — One emoji that captures the character's essence. Be creative but recognizable.
- **Display name** — A friendly name in the output language. Can be a personified name (e.g., "网关大叔") or a descriptive name (e.g., "Gateway Server").
- **Technical tag** — The actual technical term in English, shown as a small badge. This anchors the fun name to the real concept.
- **Color** — A unique gradient for the avatar and name. Use vibrant, distinct colors. No two characters should look alike.
- **Voice/personality** — Each character should have a distinct way of speaking. The gateway is authoritative, the cache is impatient and fast, the database is calm and methodical, etc. This personality should come through naturally in the dialogue, not be forced.

Aim for 4-12 characters. Fewer than 4 feels empty; more than 12 becomes hard to track.

## Step 4: Write the Script

The script is a JSON-like array of items. Each item is one of:

- **Character message** (`who`, `text`) — A chat bubble from a character. The `text` supports HTML: `<b>` for emphasis, `<code>` for inline code, `<br>` for line breaks, and embedded `<div class="codeblock">` for code blocks.
- **Phase divider** (`type: 'phase'`, `label`) — A horizontal line with a centered label, used to mark sub-sections within an act.
- **Narration** (`type: 'narration'`, `text`) — Centered italic text for stage directions and transitions.
- **Week/chapter banner** (`type: 'week'`) — A larger section header with title and description. Used in Mode B.
- **Tip card** (`type: 'tip'`, `text`) — A highlighted box for practical advice, gotchas, or "pro tips".
- **File tree** (`type: 'filetree'`, `content`) — A monospace block showing directory structures.
- **Cost/comparison table** (`type: 'cost'`, `items`) — A simple two-column table.

### Script Writing Principles

1. **Every character speaks from their own perspective.** Not "The gateway routes messages" but "I'm the Gateway. When a message comes in, I check which session it belongs to and route it there."

2. **First-person, conversational, slightly theatrical.** Characters are proud of their jobs. They explain what they do with enthusiasm, sometimes with humor. But never at the expense of accuracy.

3. **Preserve ALL technical depth from the source.** The theater format should make content more accessible, not dumber. Every important concept, design decision, code example, and insight from the original must appear in the script. If the source mentions a specific config file format, show it. If it explains why a design choice was made, have the relevant character explain the reasoning.

4. **Use analogies and metaphors liberally.** "I'm like the airport immigration hall — no matter what passport you carry, I convert it to a universal format." These stick in memory.

5. **Interleave explanation with action.** Don't have five characters monologue in a row. Show the flow: User sends message → Adapter processes it → narration ("message normalized, delivering to gateway") → Gateway receives it. The reader should feel the content moving.

6. **Code blocks, file trees, and tables are dialogue props.** Characters "show" these to the reader as part of their speech, not as separate disconnected elements.

7. **End with a summary section** where key characters distill the main takeaways. This reinforces learning.

## Step 5: Build the HTML

Produce a single self-contained HTML file. Read `references/html-template.md` for the exact CSS architecture, component patterns, and JavaScript rendering logic to follow.

Key requirements:
- Self-contained: no external dependencies except Google Fonts
- Dark theme with vibrant character colors
- Mobile-friendly (max-width container, responsive text)
- Staggered fade-up animations on each element
- Progress bar at the top showing scroll position
- A "replay" button at the bottom
- All text in the user's specified language (or default to the language the user is speaking)

## Step 6: Output

Save the HTML file to `/mnt/user-data/outputs/` and present it using `present_files`. Give a brief summary of:
- Which theater mode was chosen and why
- How many characters / acts
- A one-line teaser of the content

Do NOT write a long explanation of what's in the file — the user can open it and see for themselves.

## Language Handling

- If the user specifies a language, use it for all character names, dialogue, phase labels, narration, and UI text (header, button labels, etc.)
- If the user doesn't specify, match the language of the conversation
- Technical tags on character badges are always in English (they serve as anchors to the real technical terms)
- Code blocks stay in their original language (code is code)
