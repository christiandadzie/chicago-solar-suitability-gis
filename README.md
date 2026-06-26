# Targeting Solar Where It Matters: A GIS Suitability Analysis for Chicago

Chicago is one of the most residentially segregated cities in the United States. That segregation shows up in energy too. This project asks a direct question: where should the city prioritize solar installation to deliver the most benefit to communities that need it most?

*Christian Dadzie | Yale School of the Environment, Modeling Geographic Objects | Fall 2024*

---

## The Question

The analysis was structured around three sequential questions:

1. Where is energy demand concentrated across Chicago's 77 community areas?
2. Do those high-demand areas overlap with socioeconomically underserved communities?
3. Given energy need, demographic vulnerability, and available land, which community area is best suited for solar investment?

---

## What the Data Showed

Mapping electricity and gas consumption across all 77 community areas revealed something worth paying attention to: the neighborhoods with the highest energy demand tend to be wealthier and commercially dense, clustered in the east and north of the city. A model that simply chased demand would direct solar investment toward areas that already have the most resources.

The more useful question is where energy burden and demographic vulnerability overlap. Cross-referencing consumption data with CMAP hardship and demographic indicators narrowed the field to 14 community areas where both conditions are present. From those, six rose to the top: Belmont Cragin, Humboldt Park, Brighton Park, Gage Park, Chicago Lawn, and Auburn Gresham.

Both the manual analysis and the automated model pointed to the same conclusion. Belmont Cragin is a large, predominantly Latino community on Chicago's northwest side with an above-average energy burden and available land for planned development. Both analyses pointed there independently.

---

## The Methodology

**Data Interpretation**

To rank the six priority communities, I built a composite score weighting three factors:

- Shape area (50%) as a proxy for installation capacity
- Population-weighted Hardship Index (30%) to capture concentrated need at scale
- Composite Energy Demand Index (20%) to reflect where consumption is highest

This produced a final ranked list:

| Rank | Community Area |
|------|----------------|
| 1    | Belmont Cragin |
| 2    | Auburn Gresham |
| 3    | Humboldt Park  |
| 4    | Chicago Lawn   |
| 5    | Brighton Park  |
| 6    | Gage Park      |

---

**Automation Model**

For the second phase, I rebuilt the entire analysis as a parameterized pipeline in ArcGIS Pro ModelBuilder so the same framework can be rerun with updated data or applied to a different city.

The pipeline is a supermodel composed of three submodels:

**Submodel 1: Energy Demand** ingests raw electricity and gas consumption layers, normalizes them, and applies a 60/40 weighting to produce the Composite Demand Index.

**Submodel 2: Socioeconomic Overlay** calculates a Population-Weighted Hardship Index and flags community areas where non-white population and hardship both exceed city thresholds.

**Submodel 3: Land Use and Efficiency** steps through land use categories to assess development viability, producing scored layers that feed into the final supermodel output.

Both Belmont Cragin and Gage Park were recommended for Solar PV installation.

---

## Conclusion

Two independent analyses, one manual and one automated, pointed to the same place.

The broader takeaway is methodological. Demand alone is not a good proxy for need. A siting framework that chases high consumption will consistently favor energy-intensive, wealthier areas. Embedding equity metrics into the scoring shifts the outcome toward communities that stand to benefit most but are least likely to attract private solar investment on their own.

---

## Tools and Data

**Software:** ArcGIS Pro, ArcGIS ModelBuilder, Python (field calculations within ModelBuilder)

**Data sources:**
- Chicago Data Portal: Community area energy benchmarking, community area boundaries
- CMAP Data Portal: Hardship Index, demographic data by community area

**Course:** Modeling Geographic Objects, Yale School of the Environment, Fall 2024
