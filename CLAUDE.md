# Lightweight HTML Prototyping

This project is for fast, visual prototyping. Every output is a self-contained HTML file that opens in any browser — no build step, no server, no framework.

## Purpose

Explore features visually before translating them into a production codebase. Speed and iteration over perfection. Get the flow right, get the feel right, then move it to the real project.

## How prototypes are built

Each prototype is a single HTML file using:
- **Tailwind CSS v4** via CDN (`@tailwindcss/browser@4`)
- **Alpine.js** for interactivity
- **Design tokens** from the Lifetime design system (embedded in each file)
- **Inter** font from Google Fonts

Use the boilerplate from `workflow/03_SKETCH_IN_CLAUDE.md` as the starting point for every prototype.

## File structure

```
prototypes/
└── <feature-name>/
    ├── index.html          # The prototype
    ├── SPEC.md             # Optional: spec from voice brief
    └── v2.html, v3.html    # Optional: iterations
```

## Workflow

See `workflow/WORKFLOW.md` for the full process. The short version:

1. **Voice Brief** (`workflow/01_VOICE_BRIEF.md`) — UX thinking partner conversation
2. **Spec Writer** (`workflow/02_SPEC_WRITER.md`) — synthesize into a clean spec (optional)
3. **Build** — create the HTML prototype using the sketch boilerplate
4. **Translate** (`workflow/04_TRANSLATE_FOR_CURSOR.md`) — when ready, generate docs for the production project

## Rules

- Only token classes for color, type, radius, shadows — no arbitrary values (`#hex`, `text-[13px]`, `rounded-[7px]`)
- Standard Tailwind utilities for layout and spacing
- Alpine.js for all interactivity
- One HTML file per prototype, fully self-contained
- Real-feeling data (names, amounts, dates) — never "Lorem ipsum" or "Item 1"
- Build complete flows, not just single screens
- Include empty states
