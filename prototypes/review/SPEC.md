# Review Queue

Fast, focused flow for resolving transactions the AI couldn't fully process — missing categories, low-confidence matches, missing receipts. The user works through items one by one until the queue is empty.

---

## Domain model

- **Transaction** — a bank transaction (amount, date, counterparty, description). Every transaction must have a category. Every transaction must have a receipt resolution (matched, uploaded, private, or "no receipt").
- **Category** — from a large list (dozens). AI may suggest one with a confidence score, or leave it blank.
- **Receipt** — a document (PDF/image) from the document database. AI auto-matches most; some remain unmatched.
- **Review item** — a transaction that needs human attention. Has one or both issues: missing/uncertain category, missing receipt.

---

## Entry & scope

The user enters the queue from a prompt elsewhere in the app ("12 items need your attention" → "Review"). They land directly on the first item — no setup screen, no month picker gate.

A **month filter** is available inside the queue (e.g. "January 2026", "All"). Default is "All". Changing the filter rescopes the queue and resets to the first item in that scope. The filter shows item counts per month so the user can see where the work is.

---

## Core flow

### Layout

Split view. Left panel: transaction details and actions. Right panel: document preview (when a receipt is present or being searched for).

The right panel **adapts**:
- Receipt is matched → shows the receipt (read-only preview)
- Receipt is missing → shows upload zone + search option
- Transaction only needs categorization (receipt is fine) → right panel shows the matched receipt as context, no action needed there

### Per-item flow

The user sees one item at a time. For each item they must:

1. **Resolve the category** (if needed)
   - AI suggestion shown pre-filled when available (with confidence indicator: "AI suggestion")
   - User confirms or changes via a searchable category dropdown
   - Top 5 likely categories shown as quick-pick chips above the dropdown
   - Category is mandatory — cannot proceed without one

2. **Resolve the receipt** (if needed)
   - Four options, in order of prominence:
     - **Upload** — drag-and-drop or file picker in the right panel
     - **Search documents** — text search against the document database (by name, amount, invoice number)
     - **Private expense** — marks transaction as personal, no receipt required
     - **No receipt** — flags the transaction for export ("receipt missing")
   - Receipt resolution is mandatory — the user must pick one path

3. **Submit** — confirms all decisions, item leaves the queue, next item appears

### Navigation

- **Next / Submit** — resolves the current item, advances to next
- **Skip** — moves to next item without resolving; skipped item stays in queue
- **Back** — returns to previous item (in case user wants to change a decision)

Progress indicator shows position and remaining count: "3 of 17".

### Keyboard hints

Buttons show keyboard shortcut hints (e.g. `Enter` for submit, `S` for skip). Not the focus of the prototype, but visually indicated so the intent is clear.

---

## States

### Queue entry
User enters from app prompt. First item loads immediately.

### Active item — categorize only
Left panel: transaction details + category selector. Right panel: matched receipt preview (read-only context). User picks/confirms category, submits.

### Active item — missing receipt only
Left panel: transaction details (category already set). Right panel: upload zone with search and escape-hatch options (private / no receipt).

### Active item — both needed
Left panel: transaction details + category selector. Right panel: upload zone. User resolves both before submitting.

### Item submitted
Brief transition (item slides out, next slides in). Smooth, fast — reinforces momentum.

### Skipped item
Same transition as submitted but item remains in queue. Visual distinction not needed — the progress counter reflects it.

### Queue complete
All items resolved. Celebratory but professional: "You're all caught up." Clear next actions: "View dashboard" or "Export to tax advisor." If filtering by month: "January is complete — 8 items remain in February."

### Queue empty on entry
No items need attention. "Nothing to review — you're all set." Quick link back to wherever they came from.

---

## Key decisions

| Decision | Choice |
|----------|--------|
| Entry | Direct to first item, no setup screen |
| Month filter | Available inside queue, not a gate; default is "All" |
| Item grouping | Categorization items first, then receipt items, then both |
| Category mandatory | Yes — every transaction must have a category |
| Receipt mandatory | Yes — but "private" and "no receipt" are valid resolutions |
| "No receipt" handling | Flagged on export, not an error |
| Layout | Split view, right panel adapts to context |
| AI suggestions | Pre-filled in category dropdown with "AI suggestion" label |
| Category selection | Searchable dropdown with quick-pick chips for top suggestions |
| Skip behavior | Item stays in queue, must be resolved eventually |
| Back navigation | Allowed — user can revisit previous item |
| Transition between items | Slide animation, fast, reinforces momentum |
| Keyboard shortcuts | Visually hinted on buttons, not core to prototype |
| Urgency indicators | None — clean, neutral, trust the user |
| Completion state | "You're all caught up" with next-action links |
