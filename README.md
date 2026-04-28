# Heat Exposure Among Migrant Workers: A Thematic Network Analysis

---
## The Question Behind the Code

Every year, hundreds of thousands of migrant workers labor under extreme heat conditions in agriculture, construction, and outdoor industries across the globe. The science on this is fragmented: dozens of studies, different countries, different populations, different vocabularies. No one had drawn the map connecting them all.

This project draws that map.

Using keyword co-occurrence network analysis, we traced the conceptual skeleton of 20 studies on heat exposure among migrant workers, including a 2025 systematic review covering 2,293 workers across six countries: the USA, Bahrain, Nepal, Costa Rica, Cyprus, and the Dominican Republic. The result is a living diagram of how researchers think about this crisis, who they study, what harms they measure, and what they overlook.

---

## What We Built

Two Jupyter notebooks and a presentation that together tell one story:

**`Thematic_Network_Analysis.ipynb`** constructs the full network from scratch: raw keyword ingestion, regularization of synonyms into unified concepts, co-occurrence edge creation, community detection via the Louvain method, and a suite of network visualizations showing centrality, betweenness, and thematic clusters.

**`Thematic_Network_-_Older_vs_Newer_Studies_Comparison_.ipynb`** splits the corpus at the median publication year (2020) and builds two parallel networks, one for older studies and one for newer ones, to trace how the field's thinking has shifted over time.

The analysis is written in R, using `igraph`, `ggraph`, `tidygraph`, and the `tidyverse`.

---

## Tech Stack

The analysis runs entirely in R, chosen for its mature ecosystem of graph and tidy data tools.

| Tool | Role |
|---|---|
| `igraph` | Graph construction, density calculation, and centrality metrics |
| `tidygraph` | Tidy interface over igraph objects, enabling dplyr-style graph mutations |
| `ggraph` | Network visualization built on the grammar of graphics |
| `ggplot2` | Underlying plotting engine for all charts |
| `tidyverse` | Data wrangling: `dplyr`, `tidyr`, `stringr`, `purrr` for keyword parsing and regularization |
| `viridis` | Colorblind-friendly color palettes for community and centrality mapping |

The notebooks are written as Jupyter notebooks with an R kernel (IRkernel), making them reproducible in any environment that supports Jupyter and R.

---

## The Data

- **20 articles** total: 19 cited studies plus the systematic review itself (assigned `doc_id = 1`)
- **156 raw keywords** extracted from keyword lists and MeSH (Medical Subject Headings) terms
- After regularization: synonyms merged, MeSH noise removed, leaving **24 standardized nodes** and **185 co-occurrence edges**
- Publication years range from **2014 to 2025**

The primary data file is `Keywords_List_Per_Article.xlsx`, which contains two sheets:

**Sheet 1: Raw keyword inventory** with one row per article, recording the document ID, PMID (where available), year of study, number of keywords, and the full raw keyword string as published or indexed.

**Sheet 2: Mapping current** with one row per keyword, showing the original term alongside its standardized equivalent after regularization. This sheet is the audit trail for every synonym decision made in the analysis.

The regularization step was the most consequential preprocessing decision. Terms like *Latina*, *Latino*, *Hispanic or Latino*, and *Mexico / ethnology* were unified under a single node called `hispanic`. Terms like *heat strain*, *heat exhaustion*, and *heat-related illness* became `heat stress & illness`. This compression made the underlying structure visible.

---

## What the Network Reveals

### The Anchors of the Field

Two keywords tied for the highest centrality degree of 22: `migrants` and `geographic context`. Close behind, with a degree of 21, came `age group`, `gender`, `population`, and `occupational health & exposure`. These are the load-bearing concepts of the literature: everything connects through them.

`covid-19` sat at the periphery with a degree of just 5, confirming that the pandemic entered this research space only recently and has not yet been woven into the core conceptual fabric.

Network density came out at **0.670**, indicating moderately high interconnectivity. This is a field that talks to itself, but not so tightly that every concept is redundant.

### Three Communities, Three Questions

Using the Louvain community detection method, the 24 nodes organized themselves into three distinct clusters:

**Community 1: Social and Demographic Factors**
Anchored by `migrants`, `age group`, `gender`, `population`, and `mental health`. This cluster asks: *who is vulnerable?* It situates heat exposure within social structures, life stages, and demographic filters, treating heat as one stressor among many facing migrant populations.

**Community 2: Disease and Health Outcomes**
Centered on `hispanic`, `agricultural workers`, `heat stress & illness`, `chronic kidney conditions`, `dehydration & hydration`, and `preventive practices`. This cluster answers: *how does heat harm the body?* It is the most clinically concrete of the three, focused on a specific demographic and the biological cascade from exposure to organ damage.

**Community 3: Occupational and Environmental Vulnerabilities**
Built around `geographic context`, `occupational health & exposure`, `climate`, `agriculture`, and `other workers`. This cluster asks: *where and under what conditions does exposure happen?* The presence of `covid-19` here suggests the pandemic was understood as a workplace hazard, not just a health event.

### Bridges Between Worlds

Betweenness centrality reveals which keywords carry traffic between communities. `Social health` emerged as the primary bridge, linking demographic identity to clinical outcomes. `Research` connected broad conceptual frameworks to applied agricultural contexts. `Geographic context` carried information between territorial settings and population-level vulnerabilities. `Gender` and `population` stitched together the demographic and clinical communities.

Nodes like `checklist standards` and `covid-19`, while present, connected almost exclusively through central hubs rather than spanning communities independently.

### The Shift Over Time

The older studies (pre-2020) produced a dense, cluttered network. Many themes are interconnected, suggesting an era of exploratory research where the field was still mapping its own territory. Concepts sprawl across the diagram without a clear organizing spine.

The newer studies (2021 onward) show a leaner, more intentional network. The field has reorganized around specific biological and environmental mechanisms: how heat stress causes kidney disease, how geographic context shapes mental health among agricultural workers, how dehydration is measured and prevented. The research is narrowing toward causation and intervention.

`covid-19` appears only in the newer network, where it is coded as an emerging occupational hazard rather than a public health emergency.

---

## Research Gaps Surfaced by the Analysis

The network does not just show what is studied. Its absences are equally informative.

- The link between migrant workers' mental health and preventive practices is understudied. Community 1 (mental health, social vulnerability) and Community 2 (preventive practices, physical outcomes) are connected but thin on direct edges.
- The evidence base on climate change's direct contribution to heat stress and illness remains sparse, despite `climate` and `heat stress & illness` appearing in different communities.
- Non-Hispanic migrant worker populations are underrepresented. The dominance of `hispanic` and `agricultural workers` in the network reflects a geographic and demographic skew in the underlying literature, not in the reality of global heat exposure.
- Truly interdisciplinary studies, ones that simultaneously address health, labor policy, and social determinants, remain rare despite the network's structure hinting at how they should be designed.

---

## How to Run the Code

**Files**

| File | Description |
|---|---|
| `Keywords_List_Per_Article.xlsx` | Raw keyword data and regularization mapping, two sheets |
| `Thematic_Network_Analysis.ipynb` | Full network construction and visualization |
| `Thematic_Network_-_Older_vs_Newer_Studies_Comparison_.ipynb` | Temporal comparison notebook |

**Requirements**

```r
install.packages(c("igraph", "tidyverse", "ggraph", "tidygraph", "ggplot2", "viridis"))
```

**Order of execution**

1. Run `Thematic_Network_Analysis.ipynb` first to generate the full network, community assignments, centrality metrics, and all visualizations.
2. Run `Thematic_Network_-_Older_vs_Newer_Studies_Comparison_.ipynb` to reproduce the temporal comparison using 2020 as the partition year.

Both notebooks are self-contained. The keyword data is embedded directly in the code; no external data files are required.



---

## Reference

The systematic review at the center of this analysis: *International migrant workers, heat exposure and climate change: a systematic review of health risks and protective interventions* (2025, `doc_id = 1`).