# Iowa County-Level Nitrogen Load Dashboard

An interactive data exploration tool for visualizing nitrogen loads and streamflow patterns across Iowa counties — built to support nutrient management research and water quality decision-making.



---

## Problem

Iowa faces a persistent nitrogen contamination challenge. Agricultural runoff drives nitrate into surface water bodies, threatening drinking water supplies for hundreds of thousands of residents. Understanding *where* and *when* nitrogen loads are highest across Iowa's 99 counties is critical for targeting nutrient reduction strategies effectively.

## What This Tool Does

This dashboard provides interactive visualizations of:

- **County-level annual nitrogen loads** estimated from USGS streamflow data and flow-concentration relationships
- **Streamflow patterns** across Iowa counties over time
- **Spatial and temporal trends** to identify high-priority counties for nutrient management intervention

## Data Sources

| Dataset | Source | Coverage |
|---------|--------|----------|
| `counties_with_flow_yearly.csv` | Derived from USGS streamflow records | Iowa counties, annual |
| `counties_with_load_yearly.csv` | Nitrogen load estimates using Beale ratio estimator | Iowa counties, annual |

## Methods

Nitrogen loads are estimated using a **Beale ratio estimator hierarchy** applied to USGS monitoring data and Iowa DNR grab samples. The approach:

1. Pairs streamflow records with available water quality grab samples
2. Applies flow-weighted mean concentration (FWMC) estimation
3. Scales to county-level annual loads using drainage area ratios

This methodology is detailed in:
> Soetan, S. M. (2024). *Analyzing Nitrate Transport Across Iowa Counties Using GIS and Regression Models.* M.S. Thesis, Iowa State University. [ProQuest Publication No. 31328714](https://www.proquest.com/docview/31328714)

## Quick Start

```bash
git clone https://github.com/researchsam/Nitrogen_load_dashboard.git
cd Nitrogen_load_dashboard
pip install -r requirements.txt
jupyter notebook git_note.ipynb
```

## Tech Stack

- **Python** — pandas, numpy, matplotlib, plotly
- **Jupyter Notebook** — interactive exploration
- **GIS Integration** — county-level spatial analysis

## Related Work

- [Streamflow Map](https://github.com/researchsam/streamflow-map) — Interactive web map of Iowa streamflow monitoring stations
- *County-Level Nitrogen Load Estimation in Iowa* — Under review at Journal of Environmental Quality (submitted Jan 2026)
- *Climate-Gated Nitrogen Loads* — Under review at ACS ES&T Water (received Mar 2026)

## Author

**Samuel Mayokun Soetan**
Ph.D. Student, Agricultural & Biosystems Engineering, Iowa State University
[GitHub](https://github.com/researchsam) · [LinkedIn](https://www.linkedin.com/in/samuel-mayokun-soetan?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3Bt2wnnqHyT%2BaHh38HLHVmFQ%3D%3D)

---

*This research is part of ongoing work on watershed-scale nutrient modeling and water quality decision support for Iowa's drinking water infrastructure.*
