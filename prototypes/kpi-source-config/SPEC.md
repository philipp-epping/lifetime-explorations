# KPI Source Configuration

A modal that lets managers define where each KPI's data comes from — and verify it's pulling the right records.

---

## Domain model

Every KPI has a **source mode** that determines how its value is populated:

- **Manual** — a person enters the number. No database connection. The simplest case.
- **Connected** — the KPI queries a database (initially Airtable) using filter criteria. Connected has two sub-modes:
  - **Auto-fill** — the system writes the value directly. No human review. Used for high-trust, tool-generated metrics (ad spend from Ads Manager, dials from Aircall).
  - **Review** — the system prefills the value, but a human validates it each period. Used for derived metrics where judgment matters (bookings by UTM source, qualified leads).

Filter criteria follow a fixed shape: records from a **table** where one or more **field = value** conditions are true, scoped to a **date range** (the current week/month).

---

## Core behavior

### Opening the modal

Clicking the Source cell in the KPI tracker (e.g., "Marketing Tracker") opens a centered modal. The modal opens in **view mode** by default.

### View mode (default)

Shows a read-only summary of the source configuration:

- **Mode badge** at the top — "Manual", "Connected — Auto-fill", or "Connected — Review"
- **Filter description** as a plain-text sentence — e.g., "Every booking from Marketing Tracking where UTM Source = Paid Ads AND Status = Show Up"
- **Record preview** — a small table showing the records that match the current filters (read-only). Shows key columns from the source data so the user can visually verify correctness
- **Edit button** — secondary/ghost style, bottom or top-right corner. Intentionally low-prominence to prevent accidental edits

For Manual KPIs, view mode shows the mode badge and a note like "Value entered manually by [owner name]." No preview.

### Edit mode

Entered by clicking "Edit" from view mode. Shows the full configuration:

1. **Mode toggle** — switch between Manual and Connected
2. **Filter configuration** (Connected only):
   - Source database / table selector (dropdown)
   - Filter conditions: field, operator, value — presented as a readable template with editable parts, not a raw query builder
   - Date scoping: which date field to use, and the range (current week / current month)
3. **Confidence toggle** (Connected only) — "Auto-fill (no review needed)" vs. "Prefill for review each period"
4. **Live preview** — updates as filters change, showing matching records
5. **Save / Cancel** actions

### Closing

Modal closes on Save, Cancel, Escape, or clicking the backdrop. Unsaved changes are discarded on Cancel/Escape/backdrop.

---

## States

| State | What the user sees |
|-------|-------------------|
| Manual — view | Mode badge + "entered manually by [owner]" |
| Connected — view | Mode badge + filter sentence + record preview table |
| Connected — edit | Mode toggle + filter inputs + confidence toggle + live preview |
| No source configured | Prompt to set up a source — mode selection as first step |
| Preview loading | Skeleton rows in the preview table |
| Preview empty | "No matching records" message with hint to adjust filters |

---

## Key decisions

| Decision | Choice |
|----------|--------|
| Modal vs. popover | Centered modal — more space for preview table and filter config |
| Default state | View mode (read-only). Edit requires explicit click to prevent accidental changes |
| Three modes vs. two | Two modes (Manual / Connected) with Connected having a sub-toggle (auto-fill / review). Simpler decision tree |
| Filter UI | Template-style ("fill in the blanks") rather than freeform query builder. Keeps complexity bounded |
| Filter display | Plain-text sentence in view mode. Structured inputs in edit mode |
| Preview | Always visible for Connected KPIs. Shows matching records so user can validate the query |
| Source cell display | After configuration, shows source label + subtle mode indicator (distinguishes manual / auto / review at a glance) |
| Weekly editability | Weekly values remain editable in the tracker regardless of source mode |
| Who can edit | CEO/founder configures initially. KPI owner can adjust |

---

## Out of scope

- KPI value drill-down (clicking the actual number to inspect/check/uncheck individual records) — separate prototype
- Airtable API integration — prototype uses static mock data
- Multiple database connections per KPI
