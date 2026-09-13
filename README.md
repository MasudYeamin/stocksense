# StockSense — Retail Demand-Risk Analysis on Microsoft Fabric

A portfolio project built on Microsoft Fabric (Lakehouse, Notebook, Data
Pipeline, Power BI) that mirrors an entry-level **Data Engineer** role:
ingest and transform raw transaction data, orchestrate the pipeline
end-to-end, and surface business risk in a report.

**Stack:** Microsoft Fabric (Lakehouse, Notebook, Data Pipeline, Delta
Tables) · Python (Pandas) · Power BI

## Why this project

Built to close a specific gap against Data & AI Engineer job descriptions
that name a cloud data platform (Microsoft Fabric, Databricks, Snowflake,
etc.) as a plus — every step below demonstrates the core Fabric workflow:
Lakehouse ingestion, notebook transformation, Delta table storage, pipeline
orchestration, and BI reporting.

## What it does

1. **Ingests** the UCI "Online Retail II" dataset (1,067,371 raw
   transaction rows, 8 columns) into a Fabric Lakehouse.
2. **Transforms** it in a Fabric Notebook (Python/Pandas): cleans
   nulls/duplicates/returns, aggregates daily demand per product across
   4,985 unique SKUs.
3. **Computes** a reorder point per product using a standard safety-stock
   formula (avg daily demand × lead time + Z-score-based safety stock).
4. **Flags risk**: since the source data has no live inventory feed,
   current stock is simulated as 30 days of average demand per SKU — a
   modeling choice made explicit here and in code comments, not hidden.
   92 of 4,985 products flag at risk under this model.
5. **Loads** results into a Lakehouse Delta table.
6. **Orchestrates** the full ingest → transform → load flow via a
   scheduled Fabric Data Pipeline (Notebook activity), instead of manual
   cell-by-cell execution.
7. **Reports** the result in Power BI: at-risk SKU count and a
   demand-by-SKU bar chart, both filtered live off the Delta table.

## How to run

```
1. Upload online_retail_II.csv to a Fabric Lakehouse's Files area
2. Import notebook/StockSenseNotebook.ipynb into a Fabric Notebook
   attached to that Lakehouse
3. Run all cells, then use "Load to Tables" on the output CSV (or adapt
   the final cell to write directly to a Delta table)
4. Build a Fabric Data Pipeline with a Notebook activity pointing at
   this notebook; schedule it
5. Build a Power BI report against the resulting Delta table
```

## Key result

- Processed 1,067,371 raw transactions across 4,985 unique products
- Flagged 92 products (1.8%) at simulated stockout risk using a 7-day
  lead time, ~95% service-level safety-stock model

## Methodology & assumptions

- Reorder point = (avg daily demand × 7-day lead time) + safety stock,
  where safety stock = 1.65 × demand std dev × √(lead time) — a standard
  inventory formula.
- The source dataset only contains sales history, not live inventory
  levels. Current stock is therefore simulated as 30 days of average
  demand per product, to demonstrate the reorder-point/risk-flagging
  logic. In a production setting this would be replaced with a live feed
  from an ERP/warehouse system.

## Repo layout

```
stocksense/
├── notebook/
│   └── StockSenseNotebook.ipynb   full transformation pipeline (Python/Pandas)
└── README.md
```

## Resume bullet (example)

> Built a Microsoft Fabric data pipeline flagging retail stockout risk
> from transaction data — cleaned and transformed 1,067,371 raw
> transaction records via a Fabric Notebook (Python/Pandas) orchestrated
> through a Fabric Data Pipeline, computing demand statistics and reorder
> points for 4,985 unique products; flagged 92 products (1.8%) at
> stockout risk and surfaced results in a Power BI report.
