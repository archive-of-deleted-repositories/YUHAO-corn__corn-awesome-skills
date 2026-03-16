# HTML Template Reference

This document defines the CSS architecture, component library, and JavaScript rendering patterns for theater-explainer output files. Follow these patterns exactly to ensure consistent, polished output.

## Page Structure

```html
<!DOCTYPE html>
<html lang="[language-code]">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[emoji] [title]</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700;900&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
  <!-- For English/Latin content, replace Noto Sans SC with a distinctive sans-serif like:
       DM Sans, Outfit, Plus Jakarta Sans, or Sora -->
  <style>/* all styles inline */</style>
</head>
<body>
  <div class="progress-wrap"><div class="progress-bar" id="progress"></div></div>
  <div class="header">
    <h1>[emoji + title]</h1>
    <p class="sub">[2-line description of what this theater covers]</p>
  </div>
  <div class="scene" id="scene"></div>
  <script>/* all JS inline */</script>
</body>
</html>
```

## CSS Variables & Theme

Always use a dark theme as the base. Define these CSS variables:

```css
:root {
  --bg: #0a0a0f;        /* page background - near black */
  --surface: #12121a;   /* card/bubble background */
  --surface2: #1a1a26;  /* secondary surface (code bg, tags) */
  --border: #2a2a3a;    /* borders and dividers */
  --text: #e0e0e8;      /* primary text */
  --text-dim: #8888a0;  /* secondary/muted text */
  --accent: #ff6b4a;    /* accent color for highlights */
}
```

## Character Color System

Each character needs three CSS rules. Use vibrant gradients for avatars. Ensure high contrast between characters — no two should feel similar.

```css
.c-[id] .avatar { background: linear-gradient(135deg, [color-dark], [color-light]); }
.c-[id] .name { color: [color-light]; }
.c-[id] .tag { background: rgba([color-rgb], 0.12); color: [color-light]; }
```

Good color palette to pick from (use gradient pairs):
- Blue: #4a6cf7 → #6b8cff
- Green: #22c55e → #4ade80
- Amber: #f59e0b → #fbbf24
- Orange: #f97316 → #fb923c
- Purple: #a855f7 → #c084fc
- Indigo: #6366f1 → #818cf8
- Pink: #ec4899 → #f472b6
- Teal: #14b8a6 → #2dd4bf
- Red: #ef4444 → #f87171
- Cyan: #06b6d4 → #22d3ee
- Lime: #84cc16 → #a3e635
- Sky: #0ea5e9 → #38bdf8
- Fuchsia: #d946ef → #e879f9
- Yellow: #eab308 → #facc15
- Emerald: #22c55e → #86efac

## Component Patterns

### 1. Chat Message (`.msg`)

The core building block. Left-aligned by default, right-aligned for the "user/boss" character.

```css
.msg {
  display: flex;
  gap: 12px;
  margin-bottom: 6px;
  opacity: 0;
  transform: translateY(16px);
  animation: fadeUp 0.45s ease forwards;
}
.msg.right { flex-direction: row-reverse; }
.msg.right .bubble {
  border-radius: 18px 4px 18px 18px;
  margin-left: 36px;
  margin-right: 0;
}
```

Avatar: 42-44px square, border-radius 13-14px, centered emoji, subtle box-shadow.

Bubble: surface background, 1px border, asymmetric border-radius (4px on the speaker's side, 18px on others). Contains `.name` (with `.tag` badge) and `.text`.

### 2. Phase Divider (`.phase`)

A centered label with horizontal lines extending left and right.

```css
.phase {
  text-align: center;
  margin: 36px 0 22px;
  position: relative;
}
.phase::before, .phase::after {
  content: '';
  position: absolute;
  top: 50%;
  width: calc(50% - 80px);
  height: 1px;
  background: var(--border);
}
.phase::before { left: 0; }
.phase::after { right: 0; }
.phase-label {
  display: inline-block;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 5px 16px;
  font-size: 0.72em;
  font-weight: 500;
  color: var(--text-dim);
  letter-spacing: 0.05em;
  text-transform: uppercase;
  font-family: 'JetBrains Mono', monospace;
}
```

### 3. Week/Chapter Banner (`.week-banner`)

Used in Mode B (Onboarding Theater). A larger card with a colored left border.

```css
.week-banner {
  margin: 48px 0 12px;
  padding: 20px 24px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  position: relative;
  overflow: hidden;
}
.week-banner::before {
  content: '';
  position: absolute;
  top: 0; left: 0;
  width: 4px; height: 100%;
  background: [chapter-color];
}
```

Contains: `.wk-num` (small monospace label like "WEEK 1"), `h2` (title), `.wk-desc` (description). Each chapter gets its own color for the left border and label.

### 4. Narration (`.narration`)

Centered italic text for transitions.

```css
.narration {
  text-align: center;
  color: var(--text-dim);
  font-size: 0.8em;
  margin: 18px 0;
  font-style: italic;
}
```

### 5. Tip Card (`.tip-card`)

A highlighted box for practical advice. Uses a warm accent color.

```css
.tip-card {
  margin: 16px 0;
  padding: 14px 18px;
  background: rgba(255, 107, 74, 0.06);
  border: 1px solid rgba(255, 107, 74, 0.18);
  border-radius: 12px;
  font-size: 0.85em;
  line-height: 1.6;
  color: #ffb899;
}
```

### 6. Code Block (`.codeblock`)

Embedded inside bubbles or standalone. Monospace, darker background.

```css
.codeblock {
  background: #0d0d14;
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 10px 14px;
  margin-top: 10px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.76em;
  line-height: 1.6;
  color: #a0a0b8;
  overflow-x: auto;
  white-space: pre;
}
```

### 7. File Tree (`.file-tree`)

Same styling as codeblock but standalone (not inside a bubble). Use `<span class="highlight">` for important paths and `<span class="dim">` for comments.

### 8. Cost/Data Table (`.cost-table`)

Simple two-column table with subtle borders.

```css
.cost-table {
  width: 100%;
  border-collapse: collapse;
  margin: 12px 0;
  font-size: 0.82em;
}
.cost-table th {
  text-align: left;
  padding: 8px 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text-dim);
  font-weight: 500;
}
.cost-table td {
  padding: 8px 12px;
  border-bottom: 1px solid rgba(42, 42, 58, 0.5);
  color: #ccccd8;
}
.cost-table tr:last-child td {
  border-bottom: none;
  font-weight: 700;
  color: var(--text);
}
```

## JavaScript Rendering Pattern

Use a data-driven approach: define CHARACTERS and SCRIPT as constants, then render them with a single `render()` function.

```javascript
// 1. Character definitions
const CHARACTERS = {
  charId: { emoji: '🔧', name: 'Display Name', tag: 'TechnicalTag' },
  // ...
};

// 2. Script array — the entire content as ordered items
const SCRIPT = [
  { type: 'phase', label: 'Act 1 · Title' },
  { who: 'charId', text: 'Dialogue with <b>HTML</b> and <code>code</code>.' },
  { type: 'narration', text: '— transition text —' },
  { type: 'tip', text: '💡 Practical advice here.' },
  { type: 'week', cls: 'w1', num: 'WEEK 1', title: 'Chapter Title', desc: 'Description' },
  { type: 'filetree', content: '<span class="highlight">src/</span>\n├── index.ts' },
  { type: 'cost', items: [['Item', 'Value'], ['Total', '$100']] },
  // ...
];

// 3. Render function
function render() {
  const scene = document.getElementById('scene');
  scene.innerHTML = '';
  let delay = 0;

  SCRIPT.forEach(item => {
    let el;
    // Create the appropriate DOM element based on item type
    // Set el.style.animationDelay = delay + 'ms'
    // Append to scene
    // Increment delay: ~160ms for messages, ~100ms for dividers, ~200ms for banners
  });

  // Add replay button at the end
}

// 4. Progress bar
window.addEventListener('scroll', () => {
  const h = document.documentElement;
  const pct = (h.scrollTop / (h.scrollHeight - h.clientHeight)) * 100;
  document.getElementById('progress').style.width = pct + '%';
});

render();
```

### Animation Timing

Stagger delays to create a natural "messages appearing" feel:
- Week/chapter banners: +200ms
- Chat messages: +160ms
- Phase dividers, narrations, tips: +100ms
- File trees, tables: +150ms

Use `animation: fadeUp 0.45s ease forwards` with `opacity: 0; transform: translateY(16px)` as the initial state.

## Layout

- Max-width: 680-700px, centered
- Padding: 24px horizontal on mobile
- Scene bottom padding: 100-120px (room for the replay button)
- Header: centered, gradient text for h1, muted text for subtitle

## Quality Checklist

Before outputting, verify:
- [ ] Every major concept from the source material appears in the script
- [ ] Each character has a unique color — visually scan the avatar gradients
- [ ] Code blocks use proper whitespace and are readable
- [ ] No character monologues for more than 3 consecutive messages without another character or a divider
- [ ] The script ends with a summary/takeaway section
- [ ] The replay button works
- [ ] All text is in the correct language
- [ ] Technical tags on badges are in English
- [ ] The file is fully self-contained (no external JS/CSS except Google Fonts)
