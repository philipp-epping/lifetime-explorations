# KPI Tracker — Data Model Analysis

## Current State: Everything Lives on One Object

The current prototype puts **all concerns onto a single `KpiNode` object** via the `N()` constructor. Tree structure, KPI definition, values, data source config, view placement, and UI state are all mixed together. This is the root cause of the bloat.

### What KpiNode currently holds (line 947)

```
{
  // --- Tree structure ---
  id, name, children[], exp (expanded)

  // --- KPI definition ---
  fmt (number|currency|percentage), agg (total|average|max|manual),
  period (resets|carries_over), ownerId → User

  // --- Values (hardcoded to one month) ---
  target, actual, status (green|yellow|red),
  weekly: { weekNumber: value },
  pv (paced value, computed), pr (pace ratio, computed)

  // --- Data source config ---
  sourceMode (manual|auto|review), db, source,
  filters: [{ field, operator, value }], dateScope (week|month)

  // --- View placement ---
  visibleIn: [viewId, ...], companyDashboard (redundant),
  focusKpi (redundant), viewOrder: [kpiId, ...],
  refId → KpiNode (for Focus shortcut nodes)

  // --- UI state ---
  starred, exp (expanded/collapsed)
}
```

### Implicit Node Types (determined by heuristics, not explicit)

| Behavior pattern | Examples |
|---|---|
| Root node, has children, no target | MARKETING, SETTING, CLOSING |
| Special root: id='sc' | COMPANY OVERVIEW |
| Special root: id='fk', children have refId | FOCUS KPIs |
| Has children, target===null | Overview, Paid Ads, Funnel (view groups) |
| Has children AND target | Qualified Bookings, Adspend (KPI groups) |
| No children, has target | Leads, CPL, CAC (leaf KPIs) |
| Has refId | Focus KPI references (shortcut nodes) |

---

## Proposed Clean Model

```mermaid
erDiagram
    Department {
        string id PK
        string name
        string color_token
        int sort_order
    }

    User {
        string id PK
        string name
        string role
        string department_id FK
        int sort_order
    }

    KpiNode {
        string id PK
        string name
        string parent_id FK "nullable for roots"
        string node_type "department | view_group | kpi | reference"
        int sort_order
        boolean starred
    }

    KpiConfig {
        string kpi_node_id PK_FK
        string format "number | currency | percentage"
        string aggregation "total | average | max | manual"
        string period_type "resets | carries_over"
        string owner_id FK
    }

    DataSource {
        string id PK
        string name "Plecto CRM Pipeline etc"
        string type "database | integration"
    }

    KpiSourceBinding {
        string id PK
        string kpi_node_id FK
        string data_source_id FK
        string mode "manual | auto | review"
        string date_scope "week | month"
    }

    FieldDefinition {
        string id PK
        string data_source_id FK
        string name "UTM Source Status etc"
        string field_type "select | text | number"
    }

    FieldOption {
        string id PK
        string field_id FK
        string value
        int sort_order
    }

    KpiFilter {
        string id PK
        string source_binding_id FK
        string field_id FK
        string operator "is | is_not | contains | gt | lt | is_empty | is_not_empty"
        string value "nullable for unary ops"
        int sort_order
    }

    Period {
        string id PK "e_g_ 2026-03"
        int year
        int month
    }

    Week {
        string id PK "e_g_ 2026-W10"
        string period_id FK
        int cw_number
        string label "6 to 12 Mar"
        date start_date
        date end_date
    }

    KpiTarget {
        string id PK
        string kpi_node_id FK
        string period_id FK
        float value
    }

    KpiActual {
        string id PK
        string kpi_node_id FK
        string week_id FK
        float value
    }

    KpiStatus {
        string id PK
        string kpi_node_id FK
        string period_id FK
        string status "green | yellow | red"
    }

    KpiReference {
        string id PK
        string from_node_id FK "the shortcut node"
        string to_node_id FK "the real KPI"
    }

    View {
        string id PK
        string name "Company Overview etc"
        string type "company | focus | department | channel"
        string source_node_id FK "nullable"
    }

    ViewPlacement {
        string id PK
        string kpi_node_id FK
        string view_id FK
        int sort_order
    }

    SourceRecord {
        string id PK
        string data_source_id FK
        string week_id FK
        json fields "dynamic schema per source"
    }

    Department ||--o{ User : employs
    Department ||--o| KpiNode : "top-level dept node"
    KpiNode ||--o{ KpiNode : "parent children"
    KpiNode ||--o| KpiConfig : "defined by"
    KpiConfig }o--|| User : "owned by"
    KpiNode ||--o| KpiSourceBinding : "data from"
    KpiSourceBinding }o--|| DataSource : uses
    KpiSourceBinding ||--o{ KpiFilter : "filtered by"
    KpiFilter }o--|| FieldDefinition : "on field"
    DataSource ||--o{ FieldDefinition : "has fields"
    FieldDefinition ||--o{ FieldOption : "has options"
    DataSource ||--o{ SourceRecord : contains
    SourceRecord }o--|| Week : "in week"
    KpiNode ||--o{ KpiTarget : "targets"
    KpiNode ||--o{ KpiActual : "actuals"
    KpiNode ||--o{ KpiStatus : "status"
    KpiTarget }o--|| Period : "for period"
    KpiActual }o--|| Week : "for week"
    KpiStatus }o--|| Period : "for period"
    Week }o--|| Period : "in period"
    KpiNode ||--o{ KpiReference : "referenced by"
    KpiNode ||--o{ ViewPlacement : "placed in"
    ViewPlacement }o--|| View : "on view"
    View }o--o| KpiNode : "derived from node"
```

---

## Key Design Decisions in this Model

### 1. Separate tree structure from values
The `KpiNode` only holds **identity and hierarchy**. All values (`target`, `actual`, `weekly`) move into dedicated time-indexed tables (`KpiTarget`, `KpiActual`, `KpiStatus`). This unlocks month-over-month history — the current prototype can only show one month.

### 2. Explicit node types
Instead of guessing "is this a department? a view group? a KPI?" from heuristics like `hasChildren && target === null`, node_type says it directly.

### 3. KpiConfig splits from KpiNode
Format, aggregation method, period behavior, and owner are **definition metadata** — they rarely change. Separating them keeps the tree structure clean and makes bulk operations (like "change all currency KPIs to 2 decimals") trivial.

### 4. KpiSourceBinding replaces scattered source fields
The current model has `sourceMode`, `db`, `source`, `filters[]`, `dateScope` all floating on KpiNode. This collapses into a single binding entity connecting a KPI to a DataSource, with its own filters.

### 5. Views become first-class entities
Currently, views are derived at runtime from tree structure + `visibleIn` arrays + redundant booleans (`companyDashboard`, `focusKpi`). The clean model makes views explicit with `ViewPlacement` controlling which KPIs appear where and in what order.

### 6. KpiReference replaces refId
The Focus KPIs pattern (nodes with `refId` pointing to another KPI) becomes an explicit join table, making the relationship bidirectional and queryable.

### 7. Time becomes a proper dimension
`Period` (month) and `Week` are first-class entities. Targets are per-period. Actuals are per-week. This means the tracker can naturally show historical data without redesigning the model.

---

## What the Current IC (Individual Contributor) Model Is

The IC view uses a **completely separate** data structure (line 1138-1144) that is disconnected from the main tree:

```
icD = [{
  id, name, target, actual,
  as (auto status), so (status override),
  fmt, agg, cm (Cumulative|Incremental),
  weekly: {weekNumber: value},
  context (text note)
}]
```

**This should not exist as a separate model.** IC KPIs are just regular KPIs viewed through a user-scoped filter. In the clean model, an IC sees all `KpiNode` entries where `KpiConfig.owner_id` matches their user ID. The `context` field (a free-text note) could be a `KpiNote` entity tied to a specific KPI + period.

---

## Entity Count Comparison

| Concern | Current model | Clean model |
|---|---|---|
| Tree structure | KpiNode (mixed) | KpiNode |
| KPI definition | KpiNode (mixed) | KpiConfig |
| Values | KpiNode.target/actual/weekly | KpiTarget + KpiActual + KpiStatus |
| Data sources | KpiNode.db/source/sourceMode | KpiSourceBinding + DataSource |
| Filters | KpiNode.filters[] | KpiFilter → FieldDefinition |
| View placement | KpiNode.visibleIn/companyDashboard | ViewPlacement → View |
| References | KpiNode.refId | KpiReference |
| Time | Hardcoded constants | Period + Week |
| IC KPIs | Separate icD array | Same KpiNode, filtered by owner |
| Users | Flat array | User → Department |
