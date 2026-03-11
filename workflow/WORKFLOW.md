# Prototyping Workflow

Lightweight HTML prototyping. Every output is a self-contained HTML file — no build step, no server, no framework. Get the flow right, get the feel right, then translate to the production project.

```
  PROTOTYPE (this project)                    PRODUCTION (separate project)
  ──────────────────────────                  ──────────────────────────────

             CURSOR
       (prototyping space)
       ─────────────────────

  Voice ──►  ┌──────────────────┐
  Figma      │ 01 VOICE BRIEF   │
             │                  │
             │ Out: Phase 1+2   │
             │ Loop until right │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ 02 SPEC WRITER   │
             │ Out: SPEC.md     │
             │ (optional)       │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ 03 BUILD         │
             │ Out: HTML proto  │
             │ Loop until right │
             └────────┬─────────┘
                      │
                      │  When prototype is ready
                      │
                      ▼
             ┌──────────────────┐
             │ 04 TRANSLATE     │          ┌──────────────────────┐
             │ In: HTML proto   │────────► │                      │
             │   + Figma links  │          │  BUILD IN CURSOR     │
             │   + cheat sheet  │          │  (production project)│
             │ Out: FLOW.md,    │          │                      │
             │   SPEC.md,       │          └──────────────────────┘
             │   IMPL_NOTES.md  │
             └──────────────────┘
```

---

## Steps

### 01 — Voice Brief (Cursor)

Record a voice note describing the feature. Optionally attach screenshots or Figma links. Reference `workflow/01_VOICE_BRIEF.md` — it switches to UX thinking partner mode.

**Input:** Raw voice transcription + optional visual references
**Output:** Phase 1 (what was said/meant) + Phase 2 (UX-Challenger analysis)
**Loop:** Iterate until the direction feels right. This is a conversation, not a one-shot.

### 02 — Spec Writer (Cursor, optional)

Once the direction from the voice brief is clear, write a spec using `workflow/02_SPEC_WRITER.md`. It synthesizes Phase 1+2 into a clean decision document.

**Input:** Phase 1 + Phase 2 from voice brief
**Output:** `prototypes/<feature-name>/SPEC.md`
**Skip when:** The feature is simple enough that the voice brief output already captures everything.

### 03 — Build (Cursor or Claude web)

Build the prototype. Use the boilerplate and token reference from `workflow/03_SKETCH_IN_CLAUDE.md` as the starting template. Build directly in Cursor, or paste the spec into Claude web for a fresh take.

**Input:** The spec or voice brief output
**Output:** `prototypes/<feature-name>/index.html` — a self-contained, interactive HTML file
**Loop:** Iterate until it feels right. This is where you explore freely.

### 04 — Translate for Production (when ready)

When the prototype is nailed down, use `workflow/04_TRANSLATE_FOR_CURSOR.md` to generate structured docs for the production codebase. This step bridges from prototype to real implementation.

**Input:** HTML prototype + Figma links + component cheat sheet
**Output:** FLOW.md, SPEC.md, IMPLEMENTATION_NOTES.md — build brief for the production project

### 05 — Refinement Brief (production project)

For refining features that already exist in the production codebase. Uses `workflow/05_REFINEMENT_BRIEF.md`. Typically happens in the production project, not here.

---

## File organization

```
prototypes/
└── <feature-name>/
    ├── index.html              The prototype (self-contained HTML)
    ├── SPEC.md                 Optional: spec from step 02
    ├── v2.html, v3.html        Optional: iterations
    └── FLOW.md, IMPL_NOTES.md  Optional: translation output from step 04

workflow/
├── WORKFLOW.md                 This file
├── 01_VOICE_BRIEF.md           UX thinking partner (new features)
├── 02_SPEC_WRITER.md           Synthesize into spec
├── 03_SKETCH_IN_CLAUDE.md      HTML prototype boilerplate + tokens
├── 04_TRANSLATE_FOR_CURSOR.md  Extract docs for production build
└── 05_REFINEMENT_BRIEF.md      UX thinking partner (refinements)
```

---

## Why this project exists

This is the exploration space. No framework constraints, no component system, no build pipeline. Just HTML files you can open in a browser. The goal is to get a feature to the point where it *feels right* — the flow, the hierarchy, the interactions — and then translate it into the production project with full confidence in what to build.
