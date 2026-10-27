# InsightFlow AI

Financial performance analysis across 2024–2025 using BigQuery SQL and Python, with Power BI and DAX as the dashboard layer of the wider project.

## Business objective

Evaluate revenue growth, cost pressure, and gross profitability across Core, Plus, and Premium products in Central, North, and South regions. Translate the analysis into questions that support pricing, cost, and performance reviews.

## Analysis workflow

BigQuery financial table → Python data-quality checks and reconciliation → annual, product, regional, and monthly analysis → financial insights.

The notebook loads `insightflowraw.insightflow_raw.financial_detail`: 216 observations and 19 source columns. The source is accessed through an authenticated BigQuery connection.

## Results from saved notebook outputs

| KPI | Value |
| --- | ---: |
| Total revenue | 23,744,507.80 |
| Total COGS | 11,279,072.96 |
| Gross profit | 12,465,434.84 |
| Gross profit margin | 52.50% |
| Revenue growth, 2025 vs 2024 | 13.23% |
| COGS growth, 2025 vs 2024 | 14.62% |

Amounts use the source dataset's units. Confirm currency consistency before interpreting aggregate amounts as a single reporting currency.

- COGS grew faster than revenue; annual gross margin declined from 52.81% to 52.23%, approximately 0.58 percentage points.
- Plus generated the most revenue: 8.76 million.
- Premium achieved the highest aggregate product margin, 53.95%, and led margins in all three regions. Core's aggregate margin was 49.87%.
- Central generated 4.96 million in gross profit, approximately 39.8% of the total. Regional margins were similar, so profit leadership alone does not establish superior efficiency.
- Revenue peaked in March 2025 at approximately 1.15 million.
- No monthly revenue-growth observation exceeded the selected absolute Z-score threshold of 2.

## Methods and validation

Revenue is calculated as gross sales minus discounts. Gross profit is revenue minus COGS. Aggregate gross margin is total gross profit divided by total revenue; it is not an average of row-level margins.

Saved outputs report zero missing values, zero identical duplicate rows, zero revenue mismatches, and zero gross-profit mismatches using a 0.01 absolute tolerance. Charts cover product revenue, regional gross profit, product-by-region margins, and monthly financial performance.

The notebook contains an additional assertion cell for row count and financial reconciliation. That cell has no saved execution count or output and still needs to be run. The notebook was inspected locally; the BigQuery analysis has not been rerun during this review.

## Run the notebook

1. Open `notebooks/InsightFlow_AI_Financial_EDA.ipynb` in Google Colab.
2. Use a Google account with access to the referenced BigQuery project and table, and permission to run query jobs.
3. Run all cells in order and complete Google authentication when prompted.
4. Confirm that the assertion cell prints `PASS: row count and core financial reconciliations`, then save the executed notebook.

The current acquisition cell uses `google.colab.auth` and requires adaptation for local VS Code execution. Python dependencies include pandas, NumPy, matplotlib, google-cloud-bigquery, db-dtypes, and a compatible BigQuery DataFrame conversion environment.

The raw dataset is not included. A local CSV alternative requires replacing the acquisition cell, loading the same source columns and compatible numeric types, and importing pandas, NumPy, and matplotlib. No sample CSV is currently supplied.

## Business recommendations

- Investigate monthly COGS and discount changes to identify possible drivers of margin compression.
- Review Core's pricing, discounts, and unit costs before proposing changes.
- Evaluate product mix and regional sales volume alongside margins when allocating resources.
- Extend the time series before forecasting or drawing conclusions about seasonality.

These are investigation priorities; the descriptive analysis does not establish causal drivers.

## Repository contents and status

| Path | Status |
| --- | --- |
| `notebooks/InsightFlow_AI_Financial_EDA.ipynb` | Portfolio notebook with saved analysis outputs |
| `sql/` | Reserved for reviewed SQL scripts; none included yet |
| `powerbi/dashboard_screenshots/` | Reserved for dashboard screenshots; none included yet |
| `docs/` | Reserved for supporting analysis documentation |
| `assets/` | Reserved for project assets |

Power BI files and DAX measures have not been included or inspected in this repository. dbt and an AI Financial Analyst are planned extensions.

## Limitations

The analysis covers only 24 months and aggregated financial observations. Z-score screening uses 23 month-over-month changes, does not model seasonality, and can miss unusual patterns. Zero flagged observations does not prove the absence of anomalies. Independent reproduction requires access to the private source or an equivalent input dataset.
