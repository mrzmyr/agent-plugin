# create-mermaid

Same prompt, two agents: [without skill](7395faf1-66fc-454f-99cb-baf55d538052) vs [with skill](29cb29a1-dfe8-4c70-9683-d76c3239dd51).

**Rows returned** (info `#81A1C1`) · **Usable records** (success `#3FA266`) · empty finding (warning `#F1B467`)

**Usable** = well-formed id or non-empty roster. Counts are **raw rows**, not unique people.

## 1. Rows vs usable

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
xychart-beta
    title "Rows returned vs usable records"
    x-axis [listOrgs, listProjects, listMembers, getProject, getProjectMembers]
    y-axis "Count" 0 --> 3
    bar [0, 3, 0, 1, 0]
    bar [0, 1, 0, 1, 0]
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

Source: Catalog API · snapshot date · raw row counts · Usable = well-formed id or non-empty roster

</td>
</tr>
</table>

## 2. Discovery chain

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
flowchart TD
    listOrgs --> orgFilter[optional org filter]
    orgFilter --> listProjects
    listProjects --> getProject["getProject(id)"]
    listProjects --> getProjectMembers["getProjectMembers(id)"]
    listMembers -.-> listProjects
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
flowchart LR
  listOrgs:::warning
  listMembers:::warning
  listProjects:::primary
  subgraph getPair[getProject]
    getProject:::success
    getProjectMembers:::warning
  end
  listOrgs -.->|org filter| listProjects
  listOrgs -.-> listMembers
  listProjects -->|id| getProject
  listProjects -->|id| getProjectMembers
  classDef info fill:#81A1C133,stroke:#81A1C1,color:#E4E4E4EB
  classDef success fill:#3FA26633,stroke:#3FA266,color:#E4E4E4EB
  classDef warning fill:#F1B46733,stroke:#F1B467,color:#E4E4E4EB
  classDef neutral fill:#E4E4E411,stroke:#E4E4E414,color:#E4E4E4EB
  classDef primary fill:#599CE7,stroke:#599CE7,color:#191c22
```

Source: Catalog API · snapshot date · dashed = optional filter · solid = required id

</td>
</tr>
</table>

## 3. Agent API calls

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
    participant API
    Agent->>API: listOrgs
    API-->>Agent: 0 rows
    opt optional
        Agent->>API: listMembers
        API-->>Agent: 0 rows
    end
    Agent->>API: listProjects
    API-->>Agent: 3 rows, 1 usable
    Agent->>API: getProject(id)
    API-->>Agent: 1 usable record
    Agent->>API: getProjectMembers(id)
    API-->>Agent: 0 member rows
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411', 'actorBkg': '#E4E4E411', 'actorBorder': '#E4E4E414', 'actorTextColor': '#E4E4E4', 'signalColor': '#E4E4E48D', 'signalTextColor': '#E4E4E4'}}}%%
sequenceDiagram
  actor Agent
  participant Catalog
  Agent->>Catalog: listOrgs
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: listProjects
  Catalog-->>Agent: 3 rows
  Agent->>Catalog: getProject(id)
  Catalog-->>Agent: 1 usable
  Agent->>Catalog: getProjectMembers(id)
  Catalog-->>Agent: 0 rows
  Agent->>Catalog: listMembers
  Catalog-->>Agent: 0 rows
```

Source: Catalog API · snapshot date · raw row counts · Usable = well-formed id or non-empty roster

</td>
</tr>
</table>

## 4. Row quality states

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
stateDiagram-v2
    [*] --> Fetched
    Fetched --> Usable: well-formed id or non-empty roster
    Fetched --> Polluted: malformed id
    Usable --> [*]
    Polluted --> [*]
```

</td>
<td valign="top">

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#181818', 'primaryTextColor': '#E4E4E4', 'primaryBorderColor': '#E4E4E433', 'lineColor': '#E4E4E48D', 'secondaryColor': '#141414', 'tertiaryColor': '#E4E4E411'}}}%%
stateDiagram-v2
  [*] --> returned
  returned --> usable: well-formed id
  returned --> polluted: malformed id
  classDef info fill:#81A1C133,stroke:#81A1C1,color:#E4E4E4EB
  classDef success fill:#3FA26633,stroke:#3FA266,color:#E4E4E4EB
  class returned info
  class polluted info
  class usable success
```

Source: Catalog API · snapshot date · one listProjects row · Usable = well-formed id

</td>
</tr>
</table>

## 5. listProjects quality mix

<table>
<tr>
<th width="50%">Without skill</th>
<th width="50%">With skill</th>
</tr>
<tr>
<td valign="top">

```mermaid
pie title listProjects row quality
    "Usable" : 1
    "Polluted" : 2
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

Source: Catalog API · snapshot date · **Usable records** (success) · **Polluted rows** (warning) · n=3

</td>
</tr>
</table>
