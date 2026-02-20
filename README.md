# Steam_Market_Analysis
Understanding player engagement and commercial success through data-driven insights

Got it — here’s the README updated to reflect that **SQL split tables are not shared/uploaded** (so no link), and including your two links (Python CSV folder + Looker Studio report).

````markdown
# Steam Market Analysis — Data Pipeline, SQL Insights & Executive Dashboard

This repository contains an end-to-end analytics project to study **commercial success** and **engagement drivers** in the Steam ecosystem. It covers the full workflow from **data extraction and enrichment** to **SQL-based analysis**, exported **result CSVs**, and a final **executive dashboard**.

## Overview

**Pipeline (high-level):**
1. **Data extraction** from the base source dataset  
2. **Data narrowing** (filtering/standardizing the working universe)  
3. **Two-stage enrichment** via external API calls (e.g., RAWG)  
4. **Normalization & standardization** in Python (clean, consistent, analytics-ready)  
5. **SQL modeling & analysis** (schema + derived tables + analytical queries)  
6. **Outputs** as CSVs for results + dashboard consumption  
7. **Final deliverable:** executive dashboard (Looker Studio)

---

## External Links

### Python Output CSVs (enriched + standardized base)
Raw outputs from the Python pipeline (narrowed + enriched + normalized datasets):
- https://drive.google.com/drive/folders/1AQsFa6xYqa_hziho9OHlEzAZSD-a_dqa?usp=sharing

### Dashboard (Looker Studio)
Executive dashboard built on the final query outputs:
- https://lookerstudio.google.com/reporting/717b5616-1c57-44ae-96a2-eecf9f6df685

> Note: The **SQL split tables** created during modeling are **not uploaded/shared externally**, as they were not required for the final deliverables.

---

## Repository Structure

> Adjust folder names to match your repo.

```text
.
├── data/
│   ├── raw/                      # Raw CSVs from initial extractions (not stored in repo)
│   ├── intermediate/             # Narrowed dataset + intermediate enrichment steps
│   └── enriched/                 # Final enriched + normalized dataset (analytics-ready)
│
├── src/
│   ├── extraction/               # Scripts to load and narrow the base dataset
│   ├── enrichment/
│   │   ├── api_pass_1/           # First enrichment call(s) to API
│   │   └── api_pass_2/           # Second enrichment call(s) to API
│   ├── transform/                # Normalization, deduping, standardization, feature engineering
│   └── utils/                    # Shared utils (logging, IO, retry, rate limits)
│
├── sql/
│   ├── schema/                   # JSON schema / table definitions and modeling rules
│   ├── ddl/                      # CREATE TABLE statements (if applicable)
│   ├── queries/                  # Analytical queries producing result CSVs
│   └── outputs/                  # Result CSVs generated from SQL queries (dashboard-ready)
│
├── dashboard/                    # Dashboard metadata / notes / export instructions (optional)
├── docs/                         # Glossary, assumptions, notes (optional)
└── README.md
````

---

## Data Flow & Processing Steps

### 1) Base Extraction → Data Narrowing

We start from the base dataset and create a consistent working universe (“data narrowing”):

* keep relevant game universe (optional: remove non-game categories)
* standardize IDs and naming
* remove obvious duplicates / invalid entries
* apply baseline filters required for downstream analysis

---

### 2) Enrichment — Two API Passes

The narrowed dataset is enriched with external market/visibility signals via API calls in **two passes** (coverage + rate-limit control).

**Pass 1:** core external metadata (ratings, tags, platforms, etc.)
**Pass 2:** additional signals / backfill (e.g., social presence proxies such as Twitch/YouTube/Reddit)

---

### 3) Python Normalization & Standardization

After enrichment, the dataset is standardized for analysis:

* field normalization (types, missing values, ranges)
* categorical standardization (genres/tags/platform flags)
* derived features (price bands, success tiers, engagement proxies)
* quality checks and basic validation

> The Python pipeline outputs can be found here:
> [https://drive.google.com/drive/folders/1AQsFa6xYqa_hziho9OHlEzAZSD-a_dqa?usp=sharing](https://drive.google.com/drive/folders/1AQsFa6xYqa_hziho9OHlEzAZSD-a_dqa?usp=sharing)

---

### 4) SQL Modeling & Analysis (JSON Schema)

SQL is used for modeling and analysis:

* a JSON schema defines entities and relationships
* the base is structured into analysis-friendly tables
* analytical queries generate the final result CSVs used in reporting

> The intermediate SQL split tables are not shared externally because they were not required for final consumption.

---

### 5) Analytical SQL Queries → Result CSVs

A curated set of SQL queries generates the final result datasets used in:

* insight reporting
* the executive dashboard

Examples of result topics:

* Price vs success tiers
* Genre supply vs demand
* DLC / achievements vs engagement proxies
* Multiplayer advantage by genre
* Social visibility impact (Twitch/YouTube/Reddit presence)

---

## How to Run (Local)

### Requirements

* Python 3.10+
* SQL engine (your choice: DuckDB / Postgres / BigQuery / Snowflake)
* API key(s) for enrichment (e.g., RAWG)

### Environment Variables

Create a `.env` file (do **not** commit it):

```bash
RAWG_API_KEY="your_key"
API_RATE_LIMIT_PER_SEC="X"
API_RETRY_MAX="Y"
```

### Recommended Execution Order

1. Download raw CSVs into `data/raw/`
2. Run narrowing → enrichment pass 1 → enrichment pass 2 → normalization
3. Load final base into your SQL engine and run `sql/queries/`
4. Export results into `sql/outputs/` and refresh the dashboard

---

## Notes & Assumptions (Important)

* Some “date” fields may be stored as **parsed numeric values** (not real timestamps).
  The analysis is therefore built around **segmentations, distributions, cohort-like buckets, and relationships**, not dynamic time-series.
* External visibility signals (Twitch/YouTube/Reddit) are treated as **proxies of presence/visibility**, not causal attribution.

---

## Dashboard

Looker Studio report:

* [https://lookerstudio.google.com/reporting/717b5616-1c57-44ae-96a2-eecf9f6df685](https://lookerstudio.google.com/reporting/717b5616-1c57-44ae-96a2-eecf9f6df685)



