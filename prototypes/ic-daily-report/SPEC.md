# IC Daily Report — v2

A personal KPI view with two zones: an EOD prompt card that opens a guided data-entry modal, and a full editable KPI overview table below.

---

## Context

Every IC logs in and sees their owned KPIs in a familiar table (same as CEO/Lead views). At the top, a prompt card nudges them to complete their end-of-day report. Clicking the card opens a centered modal — a clean, single-form data entry flow that shows only what needs their attention. The table below remains fully interactive for exploring performance trends.

---

## Zone 1: EOD Prompt Card

A single card at the top of the page, styled after the Lifetime Message Stack pattern (white card, 1px border, 10px radius, icon + text + action button).

**Default state:**
- Icon (clock/checklist) with a colored background
- Title: "End-of-Day Report"
- Sub-label: "[N] KPIs need your input · [Day], [Date]"
- Primary button: "Complete Report →"

**Submitted state:**
- Green check icon
- Title: "EOD Report Submitted"
- Sub-label: "Submitted at [HH:MM] · [Day], [Date]"
- Ghost button: "Edit Report" (reopens the modal)

**Day off state:**
- Muted icon
- Title: "Marked as Day Off"
- Sub-label: "[Day], [Date]"
- Ghost button: "Undo"

---

## Zone 2: EOD Modal

A centered overlay that opens when "Complete Report" is clicked. This is the guided data entry experience.

**Layout:**
- Header: "End-of-Day Report · [Day], [Date]"
- Progress indicator: "2 of 5 updated"
- Body: grouped list of KPIs

**KPI rows — two types:**

| Type | Appearance |
|------|-----------|
| Manual input | KPI name on left, clean number input on right (editable) |
| Auto-pulled | KPI name on left, pre-filled value with source badge and checkmark on right (read-only confirmation) |

KPIs are grouped by department/sub-group with lightweight section labels between groups.

**Footer:**
- "Submit Report" primary button
- "Not working today" ghost/text link — marks the day as skipped, closes modal, card updates to day-off state

**After submit:** Modal closes. Card transforms to submitted state. Values are reflected in the table below.

---

## Zone 3: KPI Overview Table

The same editable dark spreadsheet table used by CEO/Lead views, minus Owner and Source columns.

**Columns:** KPI name, Target, Actual, Pace, Status, weekly columns (CW9–CW13)

**Behavior:**
- Editable — current week column accepts input, past weeks are read-only
- Grouped by department/sub-group with section header rows
- Status dropdown (On-Track / Behind / Off Track)
- Pace tooltip on hover
- Connected source indicator on auto-pulled cells (clickable for source popover)
- No "add context" field

---

## States

**No KPIs owned** — Empty state: "No KPIs assigned to you."

**All submitted** — Card shows submitted state. Table reflects the values.

**Day off** — Card shows day-off state. No data expected.

---

## Key decisions

| Decision | Choice |
|----------|--------|
| Card role | Notification/trigger only — no inputs on the card |
| Data entry | Centered modal, single form, all KPIs at once |
| Auto-pulled in modal | Shown as read-only confirmation rows with source badge |
| "Not working today" | Inside the modal as ghost/text link |
| Table behavior | Full editable, same patterns as CEO/Lead |
| Context notes | Removed |
| Cadence | Weekly only |
| User switching | Dropdown simulator in header |
