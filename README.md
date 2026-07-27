# County-Scale Nitrogen Export in Iowa

## Watershed Observation and Administrative Delivery

This repository contains the data-processing, spatial-analysis, and figure-generation workflow supporting the manuscript:

> **County-Scale Nitrogen Export in Iowa and the Divergence Between Watershed Observation and Administrative Delivery**
>
> Samuel M. Soetan and Amy Kaleita  
> Department of Agricultural and Biosystems Engineering, Iowa State University

The study estimates annual nitrate-nitrogen export for **98 of Iowa's 99 counties during 2001–2019** and examines the mismatch between:

- the **watershed scale**, at which streamflow and nitrate transport are observed; and
- the **county scale**, through which conservation programs and soil and water conservation districts operate.

Taylor County is not assigned a load estimate because no usable same-HUC streamflow–nitrate pairing supported its watershed intersections. It is retained as missing rather than interpolated or assigned a zero value.

## Main findings

- The final nitrate dataset contains **38,335 observations**:
  - Iowa DNR: 30,676
  - USGS: 7,659
- A flow gauge-month is retained only when it contains at least **25 valid daily discharge observations**.
- A complete watershed-year requires **12 same-HUC, same-year, same-month flow–nitrate pairings**.
- Watersheds with at least one complete paired year cover:
  - **86.9%** of Iowa at HUC-8;
  - **23.9%** at HUC-10; and
  - **4.2%** at HUC-12.
- The corresponding county reach is **98, 85, and 55 counties**, respectively.
- At a 20% statewide-area targeting budget, county- and watershed-based selections have a load-capture overlap of approximately **0.31**.
- Approximately **17.5%** of estimated statewide load lies in weakly linked county–watershed pieces under the study's prespecified 25% area-share definition.
- Iowa's nine Water Quality Initiative priority HUC-8 watersheds span a mean of **9.6 conservation districts**, while approximately **64%** of estimated statewide load lies outside the priority set.

These results support HUC-8 as the finest of the three tested watershed scales that can be supported by statewide annual measurements. HUC-10 and HUC-12 provide finer spatial resolution but substantially lower measurement-supported coverage.

## Repository structure

```text
.
├── README.md
├── CITATION.cff
├── LICENSE
├── requirements.txt
│
├── notebooks/
│   ├── Iowa_N_Load_Pipeline_v4.ipynb
│   ├── HUC_Scale_Justification_v4.ipynb
│   ├── Phase3_County_Watershed_v3.ipynb
│   └── CountyScalePaper_Figures_FINAL.ipynb
│
├── data/
│   ├── raw/                 # inputs that may be redistributed
│   ├── cache/               # archived API responses used for reproducibility
│   ├── spatial/             # Iowa county and watershed geometry
│   └── processed/           # analysis-ready tables used by the manuscript
│
├── outputs/
│   ├── tables/
│   └── figures/
│
└── dashboard/
    └── README.md            # link and notes for the interactive companion
```

The Zenodo release associated with this repository should contain the processed tables, cached API-derived inputs required for reproducibility, final notebooks, and manuscript figures. Upstream datasets that cannot be redistributed should be represented by retrieval code and complete provenance information.

## Analysis workflow

Run the notebooks in the following order.

### 1. County nitrogen-load pipeline

```text
notebooks/Iowa_N_Load_Pipeline_v4.ipynb
```

This notebook:

1. retrieves or loads Iowa streamflow, nitrate, precipitation, and spatial data;
2. applies the final streamflow and nitrate quality-control rules;
3. pairs flow and nitrate only within the same HUC-8, year, and month;
4. intersects HUC-8 watersheds with Iowa counties;
5. allocates watershed loads to county–watershed pieces;
6. aggregates the pieces to county-month and county-year estimates; and
7. exports the processed county-load and coverage tables.

Monthly load intensity is calculated as:

```text
Load (kg N ha⁻¹) = runoff (mm) × nitrate-N concentration (mg L⁻¹) × 0.01
```

The county estimate is the area-weighted aggregation of the HUC-8 load intensities intersecting that county. Flow and concentration are never paired across different watersheds or calendar months.

### 2. Watershed-scale feasibility

```text
notebooks/HUC_Scale_Justification_v4.ipynb
```

This notebook applies identical monitoring and temporal-pairing requirements at HUC-8, HUC-10, and HUC-12. It reports:

- spatial co-location of qualifying flow and nitrate monitoring;
- same-month paired coverage;
- complete 12-month paired watershed-years;
- supported watershed units;
- percentage of Iowa area covered; and
- counties reached at each scale.

### 3. County–watershed divergence

```text
notebooks/Phase3_County_Watershed_v3.ipynb
```

This notebook evaluates:

- the number of HUC-8 watersheds intersecting each county;
- the number of counties intersecting each HUC-8;
- county- versus watershed-based load targeting;
- load-capture overlap across statewide-area budgets;
- weakly linked county–watershed pieces; and
- coordination requirements for Iowa's nine Water Quality Initiative priority watersheds.

### 4. Manuscript figures

```text
notebooks/CountyScalePaper_Figures_FINAL.ipynb
```

This notebook generates the five manuscript figures from the frozen processed outputs. Figure 3 must report the complete-year coverage values of **86.9%, 23.9%, and 4.2%** for HUC-8, HUC-10, and HUC-12.

## Data sources

| Dataset | Provider | Use |
|---|---|---|
| Daily mean discharge, parameter 00060 | [USGS National Water Information System](https://waterdata.usgs.gov/nwis) | Watershed runoff |
| Discrete nitrate, parameters 00631 and 00618 | [Water Quality Portal](https://www.waterqualitydata.us/) / USGS | Stream nitrate-N |
| Ambient stream nitrate | Iowa Department of Natural Resources | Stream nitrate-N |
| Daily precipitation | [Iowa Environmental Mesonet](https://mesonet.agron.iastate.edu/) | County precipitation |
| County boundaries | Iowa Geospatial Data Clearinghouse | Administrative geometry |
| HUC-8, HUC-10, and HUC-12 boundaries | [USGS Watershed Boundary Dataset](https://www.usgs.gov/national-hydrography/watershed-boundary-dataset) | Hydrologic geometry |
| Tile-drained area | [USDA Census of Agriculture](https://www.nass.usda.gov/AgCensus/) | County drainage context |

Data retrieved through external services are restricted to the study's documented spatial, temporal, parameter, and quality-control criteria. The notebooks preserve the retrieval and filtering logic used for the archived analysis.

## Installation

Python 3.11 or a compatible recent Python version is recommended.

```bash
git clone https://github.com/researchsam/Nitrogen_load_dashboard.git
cd Nitrogen_load_dashboard

python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.\.venv\Scripts\Activate.ps1

# macOS or Linux
source .venv/bin/activate
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Start Jupyter:

```bash
jupyter lab
```

## Reproducibility notes

- Use the archived `v1.0.0` files when reproducing the published manuscript.
- Do not substitute later API responses without recording the retrieval date and resulting record counts.
- Preserve county and HUC identifiers as strings so leading zeros are not lost.
- Perform all area calculations in the projected coordinate reference system documented in the notebooks.
- Treat missing county-years as missing; do not replace them with zero.
- The county-scale values are **estimated loads**, not directly gauged county fluxes.
- County allocation expresses a hydrologic estimate in an administrative unit; it does not provide finer source localization than the watershed observations.

## Interactive dashboard

The interactive county-load dashboard is available at:

**https://researchsam.github.io/Nitrogen_load_dashboard/**

Dashboard source:

**https://github.com/researchsam/Nitrogen_load_dashboard**

The dashboard is a visualization companion. This repository and its Zenodo release are the authoritative sources for the paper's reproducibility materials.

## Citation

The permanent Zenodo citation will be added after publication of release `v1.0.0`.

```text
Soetan, S. M., and Kaleita, A. (2026).
County-Scale Nitrogen Export in Iowa and the Divergence Between
Watershed Observation and Administrative Delivery: Data and Analysis Code,
version 1.0.0. Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX
```

Until the DOI is issued, cite the GitHub repository:

```text
Soetan, S. M., and Kaleita, A. (2026).
County-Scale Nitrogen Export in Iowa: Reproducibility Code.
https://github.com/researchsam/County_load
```

## Data availability statement

After Zenodo issues the DOI, the manuscript statement should read:

> The processed data and analysis code supporting this study are archived in Zenodo at https://doi.org/10.5281/zenodo.XXXXXXX. The archived release contains the county-scale nitrogen-load data, county–HUC-8 spatial allocation framework, HUC-scale monitoring-feasibility analysis, county–watershed targeting analysis, and figure-generation code. An interactive visualization is available at https://researchsam.github.io/Nitrogen_load_dashboard/. Source observations obtained from the USGS, Iowa Department of Natural Resources, Iowa Environmental Mesonet, USDA Census of Agriculture, and USGS Watershed Boundary Dataset are documented in the archived release and remain available from their respective providers.

## License

Code is released under the license provided in `LICENSE`. Processed data and third-party inputs remain subject to the terms specified by their original providers. Any separate data license should be stated explicitly in `data/README.md`.

## Contact

**Samuel M. Soetan**  
Department of Agricultural and Biosystems Engineering  
Iowa State University  
GitHub: [researchsam](https://github.com/researchsam)
