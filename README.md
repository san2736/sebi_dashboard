# SEBI Mutual Fund Complaint Analytics Dashboard

An end-to-end data analytics pipeline and BI dashboard analyzing mutual fund complaint data disclosed by AMCs under SEBI/AMFI regulatory reporting.

**Live Dashboard:** https://san2736.github.io/sebi_dashboard/

---

## Overview

This project tracks and analyzes mutual fund investor complaints across all 55 AMCs registered with AMFI, covering SEBI's complaint disclosure taxonomy (categories I-A through VII, Part A/B structure) from January 2022 onward. It was built as a portfolio project targeting analytics roles in the AMC, fintech, and BFSI space.

The pipeline spans scraping, cleaning, relational storage, and two parallel BI layers — a Streamlit app and a Power BI dashboard — so it demonstrates both the data engineering and the visualization/analysis sides of the workflow.

## Pipeline

**1. Data Collection**
A Selenium-based scraper pulls monthly complaint disclosures from the AMFI portal across all 55 AMCs and all report parts (A–D), covering January 2022–May 2026. Handles ASP.NET `__VIEWSTATE` blocking, Cloudflare WAF protections, and uses direct `By.ID` lookups with retry logic and per-month CSV writes for crash resilience.

**2. Data Cleaning & Storage**
Raw per-month CSVs are merged and loaded into a PostgreSQL database (hosted on Supabase) via an idempotent upsert loader, structured as a star schema:
- `dim_amc` — AMC dimension
- `dim_complaint_type` — SEBI/AMFI complaint category dimension
- `fact_monthly` — monthly complaint facts
- `fact_yearly` — yearly complaint facts

**3. Analysis Layer — Streamlit**
An interactive Streamlit dashboard with 8 sections: KPI cards, trend lines, AMC scorecard, pending-complaint aging heatmap, historical yearly view, and complaint-type deep dive — all filterable via sidebar controls.

**4. Analysis Layer — Power BI**
A four-page Power BI dashboard connected directly to the Supabase PostgreSQL database:
- **Industry Overview** — headline KPIs and trends across the industry
- **AMC Drill-down** — complaint patterns by individual AMC
- **Category Analysis** — breakdown by SEBI/AMFI complaint category
- **AMC Leaderboard** — confidence-aware AMC rankings with RAG (red/amber/green) color coding

DAX measures include SLA-based resolution metrics and anomaly detection, with Part A page-level filters applied throughout to prevent double-counting across complaint report parts.

## Tech Stack

| Layer | Tools |
|---|---|
| Scraping | Python, Selenium, BeautifulSoup |
| Storage | PostgreSQL (Supabase) |
| Analysis/App | Streamlit, Pandas |
| BI/Visualization | Power BI (DAX, Power Query) |
| Deployment | GitHub Pages |

## Deployment

The Power BI report is published via Power BI's "Publish to Web" feature and embedded in a static `index.html`, deployed on GitHub Pages.

## Data Source

All complaint data originates from public SEBI/AMFI regulatory disclosures published on the AMFI portal.

## Contributors

Made by **Alisha Rao** and **Sanaz Katpitia**
