# Iowa County-Level Nitrogen Load Dashboard

An interactive map of estimated annual nitrogen export for all 99 Iowa counties, 2001–2019, built to
support county-scale nutrient-management planning and water-quality decision-making.

**▶ Live map:** https://researchsam.github.io/Nitrogen_load_dashboard/
*(enable GitHub Pages → Settings → Pages → deploy from `main`, root)*

---

## What it shows

- **County annual nitrogen load** (kg N ha⁻¹ yr⁻¹), by year and as a 2001–2019 mean
- **Flow-weighted mean nitrate concentration** (mg L⁻¹) per county-year
- **Monitoring coverage** — the fraction of each county's area supported by same-watershed
  observations, so the reader can see where estimates are well- or thinly-constrained

Hover a county for its values; drag the year slider or press **Play** to animate; toggle the
2001–2019 mean.

## Data sources

| Dataset | Source | Coverage |
|---|---|---|
| Daily streamflow | USGS NWIS (parameter 00060) | Iowa stream gauges, 2000–2020 |
| Stream nitrate | Iowa DNR ambient monitoring **+ USGS discrete nitrate** (WQP, parameters 00631/00618, as N) | 2000–2020 |
| County & watershed boundaries | Iowa Geospatial Data Clearinghouse; USGS Watershed Boundary Dataset (HUC-8) | statewide |

## Methods

County loads are estimated directly from measured discharge and nitrate concentration — **not** from a
process model. For each HUC-8 watershed and month, monthly runoff depth (mm) and mean nitrate
concentration (mg L⁻¹) are combined, and the resulting load intensity is allocated to counties by the
area of each county–HUC-8 intersection:

```
L(county, year) = Σ_pieces  area_piece × Σ_months ( flow_mm(HUC8) × conc_mgL(HUC8) × 0.01 )   ÷  county area
```

Flow and concentration are always drawn from the **same HUC-8 in the same month** (no cross-watershed
pairing). Nitrate from Iowa DNR and USGS is pooled to raise sampling density. HUC-8 is used because it
is the finest watershed scale at which co-located flow and nitrate observations cover the state:
measured coverage falls from ~96% of Iowa area at HUC-8 to ~52% at HUC-10 and ~11% at HUC-12.

Because loads are built from discrete samples aggregated to the year, they are **estimated loads, not
gauged flux**; annual aggregation is used because sampling error is far smaller at annual than at
monthly resolution. Statewide mean county load (≈18 kg N ha⁻¹ yr⁻¹) is consistent with published Iowa
estimates.

Full pipeline (retrieval → geometry → load → panel) is in the companion analysis repository / archive
cited below.

## Files

| File | Purpose |
|---|---|
| `index.html` | the interactive map (GitHub Pages entry point) |
| `index_data/county_loads.geojson` | county polygons + per-year load / FWMC / coverage |
| `build_dashboard_data.py` | rebuilds the GeoJSON from the corrected load panel |
| `requirements.txt` | Python dependencies for the build script |

## Rebuild the data

```bash
pip install -r requirements.txt
python build_dashboard_data.py       # reads out/county_year_load.csv + county shapefile
                                      # writes index_data/county_loads.geojson
```

## Related work

- *County-Level Nitrogen Load Estimation in Iowa* — in preparation for the Journal of Soil and Water
  Conservation.
- *Climate-Gated Nitrogen Export in Iowa* — in preparation.
- [streamflow-map](https://github.com/researchsam/streamflow-map) — companion map of Iowa gauges.

## Citation

If you use this dashboard or its data, please cite the archived release (Zenodo DOI: _pending_) and:

> Soetan, S. M., & Kaleita, A. (in prep.). County-Level Nitrogen Load Estimation in Iowa.

## Author

**Samuel Mayokun Soetan** — Ph.D. student, Agricultural & Biosystems Engineering, Iowa State University
· [GitHub](https://github.com/researchsam)

---

*Loads shown are observational estimates intended for screening and prioritization, not regulatory
determination.*
