# barstoolsports_data_challenge

Note for reviewers

This is an intentionally semi‑star schema built for rapid, ad‑hoc insight in Databricks. It keeps the raw flexibility of arrays and embedded GEO and LOG fields while still giving analysis a single wide fact table (consumption_events) they can query right away.

Please see `data_flow_diagram.png` for data flow diagram.

we needed a more formal, performance‑tuned warehouse, we could evolve this model by:

1. breaking out a dedicated geo_dim (country, region, city, latitude/longitude, etc.) and replacing the long GEO column set with a single geo_sk foreign key;
2. adding a talent_content_bridge (and similar tag_content_bridge) to resolve the many‑to‑many between content and talent/tags;
3. introducing surrogate integer keys for franchise_dim and talents_dim;
4. partitioning facts on event_date only and Z‑ORDERing on high‑cardinality columns for faster reads.
