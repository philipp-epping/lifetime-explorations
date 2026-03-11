# Translate for Cursor

> **Used in: Claude web** — reads the HTML prototype + Figma key screens + component cheat sheet, and generates structured docs that a developer can use with Cursor to rebuild the feature with real components.

You help me take a finished HTML prototype and generate spec files that serve as a build brief. A developer will use these docs — together with the codebase's own instructions and Figma MCP — to rebuild the feature in Cursor.

---

## Inputs you'll receive

1. **HTML prototype** — a self-contained HTML file with Alpine.js interactivity (source of truth for behavior and logic)
2. **Figma key screens** — links labeled by screen/state (source of truth for visual design). Format: `Screen name: https://figma.com/design/<fileKey>/...?node-id=<nodeId>`
3. **Component cheat sheet** — a reference of available DS components with key props and usage hints

---

## Before generating: check for missing Figma screens

After reading the prototype, identify every distinct screen or state the user sees. Compare against the Figma links provided. If any screen in the prototype has no matching Figma link, **ask me before generating** — don't assume it's intentional. List which screens appear to be missing and ask whether I want to provide links or whether those screens should be documented without visual reference.

Only proceed with generation after this check is resolved.

---

## Output

**All output files go into the prototype folder for this feature** (e.g. `prototypes/<feature-name>/`).

**File 1: FLOW.md**

Structure it exactly like this:

## Flow: [Name]

### Screens
Number each distinct screen or state the user sees.

### Transitions
What takes the user from one screen/state to another (click, hover, escape, etc.)

### Screen: [Name] (repeat for each)

Start each screen with its Figma reference:

**Figma:** [full Figma URL as provided]

If no Figma link was provided for this screen (and confirmed as intentional), write:

**Figma:** none provided

Then describe:
- **Layout:** Overall structure (sidebar + content? Full screen? Modal?)
- **Content:** What elements, in what order, what data they show
- **Active states:** What changes when the user interacts (selections, hover, expanded)
- **Empty state:** What it looks like with no data (if applicable)

Don't describe the HTML or CSS. Describe what the user sees and does. Use plain language like "a table with columns: Date, Name, Amount" not "`<table><tr>...`"

Focus on behavior — the Figma link handles the visual design. The AI rebuilding this has Figma MCP and will fetch visual details from the linked frame directly.

**File 2: SPEC.md** (only if there's complex interaction logic)

Cover:
- Behavior rules (what happens on each action)
- Edge cases and empty states
- Key decisions table (decision | choice)

Skip this file if the flow is straightforward enough that the screen descriptions already cover it.

**File 3: IMPLEMENTATION_NOTES.md** (always generate this)

Extract technical details for rebuilding in a precompiled Tailwind CSS environment with the project's component system.

### Component mapping

Using the cheat sheet provided, map each interactive element in the prototype to a DS component:

| Element | Component | Notes |
|---------|-----------|-------|
| Category dropdown | `<c-ds.dropdown>` | With `<c-ds.dropdown.search>`, `keyboard_nav`. See `dropdown/stories/default.html` |
| Override confirmation | `<c-ds.dialog>` | `primary_label`, `secondary_label`. See `dialog/stories/default.html` |
| Filter checkboxes | `<c-ds.checkbox>` | `data-state` for checked/unchecked/indeterminate |
| Category color dot | No DS component | Inline: `rounded-full size-2` + dynamic bg class |

For each element:
- **DS component fits** → name it, list key props, reference its story path (`<component>/stories/default.html`)
- **No DS component fits** → write "No DS component" and briefly note how to build with tokens
- **Partial fit** → name the component and note what's custom on top

### Dynamic classes (safelist)

List **every** Tailwind/CSS class that is applied dynamically via JavaScript (`:class`, `x-bind:class`, ternary expressions, variable references like `item.color`). Group them:

- **Colors applied via data:** e.g. `bg-a8`, `bg-y6` — list every value that appears in the JS data structures (arrays, objects, trees)
- **Conditional classes:** e.g. `hover:bg-g2`, `bg-g2` — list every class toggled by interaction state (hover, focus, active, selected)
- **Transition/animation classes:** e.g. `rotate-90`, `opacity-0` — list classes used in transitions or animations

Format them as a ready-to-use safelist:
```html
<!-- Safelist: paste into template so the CSS compiler includes these classes -->
<span class="hidden
  bg-a8 bg-y6 bg-s7
  hover:bg-g2 bg-g2
  rotate-90
"></span>
```

### Icons used

Map each icon in the prototype to its DS equivalent.

| Prototype icon | DS icon name | Context |
|----------------|-------------|---------|
| Magnifying glass | `search` | Search input prefix |
| Down arrow | `chevron-down-small` | Trigger button |
| Right arrow | `chevron-right` | Drill-down indicator |
| Sparkle/stars | `thunder` | AI badge |
| X mark | `x-clear` | Clear filter |

If a prototype icon has no DS equivalent, describe it and write "No DS icon — needs custom SVG or alternative."

### Data shape

If the data structure is small (under ~50 lines), include it inline. If large (like a full category tree), put it in a **separate file** (e.g. `CATEGORY_TREE.js`) and reference it from here.

Show the structure with one full example entry, so the rebuilding AI knows exact field names, nesting, and types:
```js
{
  id: 'ad-meta',
  name: 'Meta',
  color: 'bg-a5',       // dynamic class — must be safelisted
  children: [...]        // nested categories (omitted on leaves)
}
```

Include Alpine.js state shapes for each context (what reactive data the component manages).

---

**Rules:**
- Write for an AI that has access to the full codebase, Figma MCP, DS component stories, and project-level instructions (CLAUDE.md)
- The AI will use Figma MCP to fetch visual details from the linked frames — don't duplicate visual specs in the text, focus on behavior and interaction
- Be precise about what's interactive and what's static
- Include the data shown (column names, field labels, placeholder values)
- Note where the prototype uses hardcoded/fake data
- If something feels unfinished or placeholder-y in the prototype, flag it as "needs design"
- Extract ALL dynamic class values by reading the JS code, not just the ones visible in the HTML. If a color like `bg-a8` only appears inside a JavaScript array or object, it still needs to be safelisted.
- When mapping to components, be specific about props and reference story paths — don't just name the component
