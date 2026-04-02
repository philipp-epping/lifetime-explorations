# Dashboard Side Drawer

Two drawer types for investigating KPIs on the Performance dashboard: the **breakdown drawer** (cost/revenue decomposition) and the **trend drawer** (ratio metrics over time).

---

## Context

The Performance dashboard shows financial KPIs in a P&L funnel. Each KPI card opens a side drawer for investigation. The drawer answers the founder's "why" — turning a number into a story without leaving the dashboard context.

The category tree defines the cost hierarchy (OPEX → Marketing/Sales/G&A, COGS → Fulfillment/Contractors/etc). Revenue splits by product/service offering, then by client invoices.

---

## Breakdown Drawer

Handles KPIs that decompose into sub-categories: Profit after Delivery, Profit after Growth, Business Profit, Net Result, Net Revenue, After Business Profit.

### Core behavior

**Opening:** Clicking a KPI card opens a 480px drawer from the right. Subtle backdrop overlay. Escape or backdrop click closes.

**KPI view (first level):**
- KPI name, value, margin %, delta vs P3, margin change
- KPI definition in a subtle box (replaces the info tooltip on the dashboard card)
- "How it's calculated" equation: revenue lines minus cost lines = result, each with delta
- Breakdown section: sub-categories sorted by absolute delta (biggest change first)
- Section headers state comparison baseline: "vs avg. P3"

**Each KPI drawer is self-contained.** The equation shows the full formula, not just the delta layer. Business Profit shows Revenue, COGS, Marketing, Sales, G&A — all five categories — so the founder sees the complete picture without opening multiple drawers.

**Category drill-in:** Click a category to see its children. Same layout — name, total, delta, sorted list. Continues until reaching leaf categories.

**Transaction view:** Leaf categories show individual transactions — vendor, date, type, amount. Flag button appears on hover to add to cost-cutting list. Click a row to open the detail view.

**Transaction detail:** Vendor header (initial, amount, name, date). Details table with editable category (breadcrumb path, dropdown to re-categorize), type, date, internal note. Full-width "Flag for cost-cutting review" button. Notes badge for annotations (e.g., "New hire").

### Navigation

- Breadcrumb trail at each level, every segment clickable
- Back button goes up one level; on first level it closes
- Close button always closes entirely
- Breadcrumb hidden on first level

### Cost-cutting list

- Flag action available on hover in transaction list + prominent button in detail view
- Flagged items persist across navigation within the drawer
- Sticky footer shows count + total flagged amount
- Footer text is link-styled (destination TBD)
- Toast notification on flag action

### Variations by KPI

| KPI | Equation shows | First-level breakdown |
|-----|---------------|----------------------|
| Profit after Delivery | Revenue, COGS | COGS subcategories |
| Profit after Growth | Revenue, COGS, Marketing, Sales | 4 cost groups |
| Business Profit | Revenue, COGS, Marketing, Sales, G&A (essential) | 5 cost groups |
| Net Result | Full P&L | All cost groups |
| Net Revenue | (no equation) | Products/services → client invoices |
| After Business Profit | (no equation) | Optional Costs, Founder Benefits, Owner Distribution |

---

## Trend Drawer

Handles ratio/benchmark KPIs where the question is "is this trending the right way?": Business Profit / Employee.

### Core behavior

**Opening:** Same mechanics as breakdown drawer — click card, drawer slides in.

**Trend view:**
- KPI name, value, employee count, delta vs P3
- KPI definition
- Line chart showing 12 months of data with area fill
- Headcount change events annotated on the chart (dashed vertical markers)
- Current month highlighted with larger dot + value label
- "What drives this" section showing the two components:
  - Business Profit: value + delta vs P3
  - Headcount: count + last change note

No drill-in. No category tree. The trend drawer is a single-level view — the founder sees the shape, understands the drivers, and closes.

---

## Key decisions

| Decision | Choice |
|----------|--------|
| Drawer width | Fixed 480px, overlays dashboard |
| Comparison baseline | Always vs average of past 3 months (P3), stated in section headers |
| Sort order | Categories sorted by absolute delta (biggest change first) |
| Cost delta colors | Increase = red (bad), decrease = green (good) |
| KPI definition placement | In the drawer, not as tooltip on dashboard card |
| Each KPI drawer self-contained | Shows full equation, not just the delta-layer costs |
| Re-categorization | Secondary action in transaction detail via dropdown — not the default workflow |
| Graphs in breakdown drawer | None — numbers only, consistent with dashboard philosophy |
| Graphs in trend drawer | Line chart with area fill — answers "is this trending?" which numbers alone can't |
| Revenue first level | By product/service offering, then drill to client invoices |
| Two drawer templates | Breakdown (equation + sorted list + drill-in) and Trend (chart + driving components) |
