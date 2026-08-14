# create-mermaid

Same catalog snapshot on both sides. **Without skill** is default Mermaid. **With skill** adds the design language (theme, two tones, units, caption).

**Rows returned** (info `#81A1C1`) · **Usable records** (success `#3FA266`) · empty finding (warning `#F1B467`)

**Usable** = well-formed id or non-empty roster. Counts are **raw rows**, not unique people.

## 1. Rows vs usable

X-axis: Catalog tool · Y-axis: Count (rows) · series: **Rows returned** `[0, 3, 0, 1, 0]` · **Usable records** `[0, 1, 0, 1, 0]`

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
xychart-beta
  title "Rows returned vs usable records by list tool"
  x-axis [listOrgs, listProjects, listMembers, getProject, getProjectMembers]
  y-axis "Count (rows)" 0 --> 4
  bar [0, 3, 0, 1, 0]
  line [0, 1, 0, 1, 0]
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411', 'xyChart': {'plotColorPalette': '#81A1C1, #3FA266'}}}}%%
xychart-beta
  title "Rows returned (bars) vs usable records (line) by list tool"
  x-axis [listOrgs, listProjects, listMembers, getProject, getProjectMembers]
  y-axis "Count (rows)" 0 --> 4
  bar [0, 3, 0, 1, 0]
  line [0, 1, 0, 1, 0]
```

</td>
</tr>
</table>

Source: Catalog API · snapshot date · raw row counts · Usable = well-formed id or non-empty roster

## 2. Discovery chain

Nodes: `listOrgs` → `optional org filter` → `listProjects` → `getProject` / `getProjectMembers`. `listMembers` is optional from `listProjects`.

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
flowchart TB
  listOrgs["listOrgs"]
  optOrg["optional org filter"]
  listProjects["listProjects"]
  listMembers["listMembers"]
  getProject["getProject"]
  getProjectMembers["getProjectMembers"]
  listOrgs --> optOrg
  optOrg -.-> listProjects
  listProjects --> getProject
  listProjects --> getProjectMembers
  listProjects -.-> listMembers
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
flowchart TB
  listOrgs["listOrgs"]
  optOrg["optional org filter"]
  listProjects["listProjects"]
  listMembers["listMembers"]
  subgraph api["Project API surface"]
    getProject["getProject"]
    getProjectMembers["getProjectMembers"]
  end
  listOrgs --> optOrg
  optOrg -.-> listProjects
  listProjects --> getProject
  listProjects --> getProjectMembers
  listProjects -.-> listMembers
  class listProjects primary
  class getProject success
  class optOrg neutral
  class listOrgs,listMembers,getProjectMembers warning
  classDef primary fill:#599CE7,stroke:#599CE7,color:#191c22
  classDef success fill:#1e3328,stroke:#3FA266,color:#E4E4E4
  classDef warning fill:#3a3020,stroke:#F1B467,color:#E4E4E4
  classDef neutral fill:#2a2a2a,stroke:#3d3d3d,color:#E4E4E4
```

</td>
</tr>
</table>

Source: Catalog API · snapshot date · dashed = optional filter · solid = required id

## 3. Agent API calls

Same five calls, same replies, same order.

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
sequenceDiagram
  participant Agent
  participant Catalog
  Agent->>Catalog: listOrgs
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: listProjects
  Catalog-->>Agent: 3 rows, 1 usable
  Agent->>Catalog: listMembers
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: getProject(id)
  Catalog-->>Agent: 1 usable record
  Agent->>Catalog: getProjectMembers(id)
  Catalog-->>Agent: 0 member rows
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411', 'actorBkg': '#2a2a2a', 'actorBorder': '#3d3d3d', 'actorTextColor': '#E4E4E4', 'signalColor': '#8d8d8d', 'signalTextColor': '#E4E4E4'}}}%%
sequenceDiagram
  participant Agent
  participant Catalog
  Agent->>Catalog: listOrgs
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: listProjects
  Catalog-->>Agent: 3 rows, 1 usable
  Agent->>Catalog: listMembers
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: getProject(id)
  Catalog-->>Agent: 1 usable record
  Agent->>Catalog: getProjectMembers(id)
  Catalog-->>Agent: 0 member rows
```

</td>
</tr>
</table>

Source: Catalog API · snapshot date · raw row counts · Usable = well-formed id or non-empty roster

## 4. Row quality states

Same states: Returned → Usable / Polluted / Empty.

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
stateDiagram-v2
  [*] --> Returned
  Returned --> Usable: well-formed id
  Returned --> Polluted: malformed id
  Returned --> Empty: 0 rows
  Usable --> [*]
  Polluted --> [*]
  Empty --> [*]
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
stateDiagram-v2
  [*] --> Returned
  Returned --> Usable: well-formed id
  Returned --> Polluted: malformed id
  Returned --> Empty: 0 rows
  Usable --> [*]
  Polluted --> [*]
  Empty --> [*]
  class Returned info
  class Usable success
  class Polluted,Empty warning
  classDef info fill:#1a2a33,stroke:#81A1C1,color:#E4E4E4
  classDef success fill:#1e3328,stroke:#3FA266,color:#E4E4E4
  classDef warning fill:#3a3020,stroke:#F1B467,color:#E4E4E4
```

</td>
</tr>
</table>

Source: Catalog API · snapshot date · one listProjects row · Usable = well-formed id

## 5. listProjects quality mix

Same slices: **Usable records** 1 · **Polluted rows** 2.

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
pie showData
  title "listProjects row quality"
  "Usable records" : 1
  "Polluted rows" : 2
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'pie1': '#3FA266', 'pie2': '#F1B467', 'pieTitleTextColor': '#E4E4E4', 'pieSectionTextColor': '#191c22', 'pieLegendTextColor': '#E4E4E4'}}}%%
pie showData
  title "listProjects row quality"
  "Usable records" : 1
  "Polluted rows" : 2
```

</td>
</tr>
</table>

Source: Catalog API · snapshot date · **Usable records** (success) · **Polluted rows** (warning) · n=3
