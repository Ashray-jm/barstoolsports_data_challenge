# BarstoolSports — Data Challenge

Note for reviewers

This is an intentionally semi‑star schema built for rapid, ad‑hoc insight in Databricks. It keeps the raw flexibility of arrays and embedded GEO and LOG fields while still giving analysis a single wide fact table (consumption_events_logs) they can query right away.
---

I have added Notes and comments in the main `BarstoolSports_Data_Challenge_Notebook.ipynb` file 

## 1 . Project Contents

| Path / file | Purpose |
|-------------|---------|
| **`data_flow_diagram.png`** | High‑level ETL and table lineage |
| **`BarstoolSports_Data_Challenge_Notebook.ipynb`** | Full extraction → transformation workflow; final Δ‑table **`consumption_events_logs`** |
| **`Analysis.html`** | Interactive results (tables + dashboards).<br>**Download and open in a browser**, then click the “Visualization” button in each result cell to view dashboards. |

---

## 2 . Files & Artifacts

| File | Description |
|------|-------------|
| **`data_flow_diagram.png`** | Data‑flow diagram |
| **`Barstoolsports_Data_Challenge_Notebook.ipynb`** | Main ETL notebook — extracts, transforms, and writes the final Delta table **`consumption_events_logs`** |
| **`Analysis.html`** | All analyses (tables + dashboards). **Download, open in a browser, and click the “Visualization” button in each result** to view dashboards |

---
## 3 . Data Issues & Assumptions

| Issue | Treatment |
|-------|-----------|
| 4 fully‑null rows | dropped |
| `franchise` has 459 nulls | replaced with `'UNKNOWN'` |
| `franchise` 83, 91 missing in content catalog | left as‑is |
| Some content lacks talent **and** franchise info | tolerated — queries must LEFT JOIN |
| Mismatched `content_id` between logs & catalog (e.g. live events) | excluded from dim joins |
| `31235576` appears under `TALENT` but is a franchise_id | redirected to franchise column |

---

## 4 .  Better approach is to have complete star schema model

1. **`geo_dim`** (country, region, lat/long) + `geo_sk` FK to shrink wide fact.  
2. **Bridge tables** (`talent_content_bridge`, `tag_content_bridge`) for many‑to‑many resolution.  
3. **Surrogate integer keys** for `franchise_dim`, `talent_dim` to speed joins.  
4. **Storage tuning**:  
   * Partition fact by **`event_date`** only.  
   * Z‑ORDER on high‑cardinality columns (e.g. `content_id`) for faster selective reads.

---
