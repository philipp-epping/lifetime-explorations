# KPI Structure Settings

Configure the company's KPI hierarchy — departments, groups, and which KPIs appear in each view — from a single settings experience.

## Domain model

**Three layers:**

- **KPI Catalog** — the master list of all KPI definitions. Each KPI has a name, optional child KPIs (forming a sub-tree), and a data source. Managed in the KPI tree editor. The catalog is the single source of truth for KPI structure.
- **Views** — named compositions of KPIs. Each view selects which KPIs from the catalog to display. Views are organized hierarchically: Company level shows a few headline KPIs; department level shows a curated overview; group level shows the operational detail.
- **Hierarchy** — Company > Department > Group. A department (e.g., Marketing) contains groups (e.g., Paid Ads, Funnel, Outbound Mail, Organic). Each group is a view. Each department also has an Overview view.

**Two placement patterns:**

- **Reference** — a KPI from one department appears in another (e.g., "Units Closed" from Sales shown in Marketing for ROAS). Same data, same source.
- **Filtered instance** — the same KPI structure (e.g., FE-ROI with its 6 children) appears in multiple groups, but each group applies its own filter rule (e.g., UTM=paid for Paid Ads, UTM=outbound for Outbound Mail), producing different values.

## Core behavior

### Hierarchy management (settings page)

The settings page has a "KPI Structure" section showing the full hierarchy as a nested list:

1. **Departments** are top-level items. Each shows its name and the number of groups it contains. Departments can be added, reordered, and renamed.
2. **Groups** are nested under departments. Each shows its name, KPI count, and an optional filter rule badge. Groups can be added, reordered, renamed, and removed.
3. Clicking a group opens the **view composer** — a panel or modal where the CEO picks which KPIs appear in that group's view.

### View composer

When composing a view (a group's KPI list):

1. The left side shows the current KPIs in this view, ordered top to bottom as they'll appear in the tracker. Items can be reordered.
2. The right side (or a dropdown/search) shows the KPI catalog — all available KPIs. Selecting one adds it to the view.
3. KPIs that have children in the catalog bring their children along automatically. The child tree is not editable per-view — it's inherited from the catalog definition.
4. Removing a KPI from a view doesn't delete it from the catalog. It only removes it from this group's display.
5. A KPI already in this view shows as disabled/checked in the catalog picker.

### Filter rules

Each group can have a filter rule displayed as an editable badge (e.g., "utm_source = paid"). The filter scopes all KPI data within that group. Filter rules are a property of the group, not individual KPIs.

### Per-KPI "appears in" summary

When a KPI is selected in the view composer, a secondary line shows where else this KPI appears: "Also in: Company Overview, Marketing Overview, Funnel". Read-only — placement is always managed from the view side, not the KPI side.

## States

- **Empty department** — no groups yet. Shows a prompt: "Add your first group" with an add button.
- **Empty group** — no KPIs selected. Shows: "No KPIs in this view yet" with a button to open the catalog picker.
- **Populated group** — list of KPIs with reorder handles, each showing name and child count.
- **Filter rule empty** — group has no filter rule. Shows an "Add filter" link.

## Key decisions

| Decision | Choice |
|----------|--------|
| Primary configuration direction | View-first: compose views by adding KPIs, not KPI-first with visibility flags |
| Where structure config lives | Settings page — new "KPI Structure" section alongside General, Members, Categories |
| Where view composition lives | Inline panel or modal opened from a group in settings |
| Child KPI handling | Children inherited from catalog definition, not configurable per-view |
| Filter rules scoping | Per-group, not per-KPI — a group's filter applies to all its KPIs |
| Cross-view visibility info | Read-only "appears in" line shown in view composer, not in the daily tracker |
| Access control | CEO-only for now |
| Overview group | Manually composed like any other group — same catalog picker pattern |

## Sample data (for prototype)

Use the Marketing department with these groups: Overview, Paid Ads, Funnel, Outbound Mail, Organic. Populate with realistic KPIs from the voice brief (Qualified Bookings, Ad Spend, FE-ROI, CAC, Leads, CPL, etc.). Include a Company level with departments Marketing and Sales visible in the hierarchy.
