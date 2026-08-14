# create-mermaid

Writes Mermaid in a canvas-derived design language: one metric, named axes with units, two semantic tones, zeros kept, no rainbow / emoji / shadow / gradient.

**Rows returned** (info `#81A1C1`) · **Usable records** (success `#3FA266`) · empty finding (warning `#F1B467`)

**Usable** = well-formed id or non-empty roster. Counts are **raw rows**, not unique people.

Source: Catalog API · snapshot date

## 1. Grouped comparison

X-axis: Catalog tool · Y-axis: Count (rows)

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411', 'xyChart': {'plotColorPalette': '#81A1C1, #3FA266'}}}}%%
xychart-beta
  title "Rows returned (bars) vs usable records (line) by list tool"
  x-axis [listOrgs, listProjects, listMembers, getProject, getProjectMembers]
  y-axis "Count (rows)" 0 --> 4
  bar [0, 3, 0, 1, 0]
  line [0, 1, 0, 1, 0]
```

Source: Catalog API · snapshot date · raw row counts · mermaid xychart has no series legend, so names live in the title

## 2. Discovery chain

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
flowchart TB
  listOrgs["listOrgs"]
  optOrg{{"optional org"}}
  listProjects["listProjects"]
  listMembers["listMembers"]
  emptyOrgs["0 orgs"]
  emptyMembers["0 members"]

  subgraph api["Project API surface"]
    getProject["getProject"]
    getProjectMembers["getProjectMembers"]
  end

  listOrgs --> optOrg
  optOrg -.-> listProjects
  listProjects --> getProject
  listProjects --> getProjectMembers
  listProjects -.-> listMembers
  listOrgs --> emptyOrgs
  listMembers --> emptyMembers

  class listProjects primary
  class getProject success
  class optOrg neutral
  class listOrgs,listMembers,emptyOrgs,emptyMembers,getProjectMembers warning
  classDef primary fill:#599CE7,stroke:#599CE7,color:#191c22
  classDef success fill:#1e3328,stroke:#3FA266,color:#E4E4E4
  classDef warning fill:#3a3020,stroke:#F1B467,color:#E4E4E4
  classDef neutral fill:#2a2a2a,stroke:#3d3d3d,color:#E4E4E4
```

Source: Catalog API · snapshot date · solid = required id · dashed = optional filter · **listProjects** is the focal mark

## 3. Call protocol

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411', 'actorBkg': '#2a2a2a', 'actorBorder': '#3d3d3d', 'actorTextColor': '#E4E4E4', 'signalColor': '#8d8d8d', 'noteBkgColor': '#3a3020', 'noteBorderColor': '#F1B467', 'noteTextColor': '#E4E4E4'}}}%%
sequenceDiagram
  participant Agent
  participant Catalog as Catalog API
  Agent->>Catalog: listOrgs
  Catalog-->>Agent: 0 rows
  Note over Catalog: empty finding
  Agent->>Catalog: listProjects
  Catalog-->>Agent: 3 rows, 1 usable
  Agent->>Catalog: listMembers
  Catalog-->>Agent: 0 rows
  Note over Catalog: empty finding
  Agent->>Catalog: getProject(id)
  Catalog-->>Agent: 1 usable record
  Agent->>Catalog: getProjectMembers(id)
  Catalog-->>Agent: 0 member rows
  Note over Catalog: empty finding
```

Source: Catalog API · snapshot date · raw row counts · optional org filter never applied (0 orgs)

## 4. Quality state of one listProjects row

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
stateDiagram-v2
  [*] --> Returned
  Returned --> Usable : well-formed id
  Returned --> Polluted : malformed id
  Returned --> Empty : 0 rows
  Usable --> Detail : getProject
  Detail --> RosterEmpty : getProjectMembers 0
  Polluted --> [*]
  Empty --> [*]
  RosterEmpty --> [*]
  class Returned info
  class Usable,Detail success
  class Polluted,Empty,RosterEmpty warning
  classDef info fill:#1a2a33,stroke:#81A1C1,color:#E4E4E4
  classDef success fill:#1e3328,stroke:#3FA266,color:#E4E4E4
  classDef warning fill:#3a3020,stroke:#F1B467,color:#E4E4E4
```

Source: Catalog API · snapshot date · 3 returned rows: 1 **Usable**, 2 **Polluted**; **Empty** kept for listOrgs / listMembers

## 5. Catalog quality mix

Parts of a whole (not a grouped comparison).

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'pie1': '#3FA266', 'pie2': '#F1B467', 'pieTitleTextColor': '#E4E4E4', 'pieSectionTextColor': '#191c22', 'pieLegendTextColor': '#E4E4E4'}}}%%
pie showData
  title "listProjects row quality (n=3)"
  "Usable records" : 1
  "Polluted rows" : 2
```

Source: Catalog API · snapshot date · **Usable records** (success) · **Polluted rows** (warning) · listOrgs 0 and listMembers 0 stay off-slice as empty findings
