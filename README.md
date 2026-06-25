# Targeting Solar Where It Matters: A GIS Suitability Analysis for Chicago

Chicago is one of the most residentially segregated cities in the United States. That segregation shows up in a lot of ways, including energy. This project asks a direct question: where should the city prioritize solar installation to deliver the most benefit to the communities that need it most?

*Christian Dadzie | Yale School of the Environment, ENV 756: Data Automation | Fall 2024*

---

## The Question

The analysis was structured around three sequential questions:

1. Where is energy demand concentrated across Chicago's 77 community areas?
2. Do those high-demand areas overlap with socioeconomically underserved communities?
3. Given energy need, demographic vulnerability, and available land, which community area is best suited for solar investment?

---

## What the Data Showed

**Energy demand does not map onto disadvantage.** Using utility data from the Chicago Data Portal, I built a Composite Energy Demand Index that weighted electricity consumption at 60% and gas consumption at 40%, then normalized both to a 0 to 100 scale. The resulting index placed eastern and northern neighborhoods at the top, areas that tend to be wealthier and predominantly white. The highest-demand community had over 1.1 million kWh per square foot. The index mean was 3.06, but the median was only 0.67, which tells you how skewed the distribution is.

This finding shapes everything that follows. High energy consumption is partly a sign of economic activity and dense commercial infrastructure. The communities with the greatest equity need often have *lower* measured demand, not higher. A purely demand-driven targeting model would systematically miss underserved populations.

**The socioeconomic overlay changed the picture.** Cross-referencing the energy data with Chicago Metropolitan Agency for Planning (CMAP) demographic data, I identified community areas where over 79% of residents identify as non-white and where the hardship index sits above the city's moderate threshold. That filter produced 14 community areas with meaningful overlap between energy demand and concentrated disadvantage. Six rose to the top: Belmont Cragin, Humboldt Park, Brighton Park, Gage Park, Chicago Lawn, and Auburn Gresham.

---

## The Methodology

**Data Interpretation Analysis**

To rank the six priority communities, I built a composite scoring formula that weights three factors:

- Shape area (50%) as a proxy for installation capacity
- Population-weighted Hardship Index (30%) to capture concentrated need at scale
- Composite Energy Demand Index (20%) to reflect where consumption is highest

This produced a final ranked list:

| Rank | Community Area |
|------|---------------|
| 1 | Belmont Cragin |
| 2 | Auburn Gresham |
| 3 | Humboldt Park |
| 4 | Chicago Lawn |
| 5 | Brighton Park |
| 6 | Gage Park |

Belmont Cragin led on all three dimensions: it is large, it has a high population-hardship product, and it registers above-average energy demand for a predominantly residential area. Final score: 56,103,518.

---

## The Automation Model

For the second phase of the project, I built a full automation pipeline in ArcGIS Pro ModelBuilder to replicate the analysis with a repeatable, adjustable workflow.

The pipeline is structured as a supermodel composed of three distinct submodels:

**Submodel 1: Energy Demand**
Ingests raw electricity and gas consumption layers, normalizes them, and applies the 60/40 weighting to produce the Composite Demand Index. An escalation factor of 0.17 adjusts for baseline load differences across community types.

**Submodel 2: Socioeconomic Overlay**
Calculates a Population-Weighted Hardship Index by multiplying the Hardship Index by total population in each community. An escalation factor of 1.35 amplifies the signal in the most vulnerable areas. The submodel flags community areas where non-white population and hardship both exceed thresholds (mean values: 73% non-white, hardship score 1,563,272).

**Submodel 3: Land Use and Efficiency**
Uses an iterator to step through land use categories and assess development viability for each. Outputs four scored layers corresponding to land use types, which are then joined to the energy and socioeconomic outputs in the supermodel.

**Final scoring formula:**

```
TotalScore = (Shape_Area × 0.50) + (Pop_HardIx × 0.30) + (Comp_Index × 0.20)
```

The model ran end-to-end in 38 seconds, processing all 14 steps without interruption.

**Supermodel results:**

- Belmont Cragin (Planned Development land use): score 1,553,811
- Gage Park (Open/Residential land use): score 970,585

Both sites were recommended for Solar PV installation.

---

## Conclusion

Two independent analyses, one interpretive and one automated, pointed to the same place. Belmont Cragin is a large, predominantly Latino community on Chicago's northwest side with above-average energy burden, a high population-hardship product, and available land zoned for planned development. It meets every threshold the analysis was designed to surface.

The broader lesson from the project is methodological: demand alone is not a good proxy for need. Any solar siting framework that chases high consumption will consistently favor wealthier, energy-intensive areas. Embedding equity metrics directly into the scoring formula shifts the outcome toward communities that stand to benefit the most but are least likely to attract private capital on their own.

---

## Tools and Data

**Software:** ArcGIS Pro, ArcGIS ModelBuilder, Python (field calculations within ModelBuilder)

**Data sources:**
- Chicago Data Portal: Community area energy benchmarking, community area boundaries
- CMAP Data Portal: Hardship Index, demographic data by community area

**Course:** ENV 756: Data Automation, Yale School of the Environment, Fall 2024
