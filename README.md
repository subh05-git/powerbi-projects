# Enterprise General Ledger & Budget Variance Analytics (Power BI)

An end-to-end **Power BI Financial Analytics** portfolio project modeling multi-year General Ledger (GL) journal entries across **Actual, Budget, and Forecast** scenarios. Designed to showcase star-schema data modeling, DAX time-intelligence, and executive P&L variance reporting.

## Business Problem
Finance and accounting teams need a unified view of monthly P&L performance, cost-center spending, and budget variance across ERP subledgers (Billing, Payroll, AP, PSA, Expense) and manual journal entries—without relying on manual spreadsheet reconciliations.

## Repository Structure
```text
powerbi-gl-variance-analytics/
├── data/
│   ├── dim_account.csv              # GL Chart of Accounts & P&L rollup hierarchy
│   ├── dim_cost_center.csv          # Cost centers, departments, and regions
│   └── fact_gl_transactions.csv     # Monthly Actual, Budget, and Forecast journal lines
├── powerbi/
│   ├── dax_measures.dax             # Reusable DAX measures (P&L, Variance, YTD, YoY)
│   └── power_query_m_scripts.pq     # Power Query (M) script for Dim_Date
├── docs/
│   └── powerbi_build_guide.md       # Step-by-step instructions to assemble the .pbix report
├── .gitignore
└── README.md
```

## Data Model (Star Schema)
- **`fact_gl_transactions`**: Grain is one row per Journal Entry line (`PostingDate`, `AccountID`, `CostCenterID`, `Scenario`, `AmountUSD`, `SourceSystem`).
- **`dim_account`**: Maps `AccountID` to `AccountType` (`Revenue`, `COGS`, `OpEx`), category, and P&L rollup group.
- **`dim_cost_center`**: Maps `CostCenterID` to Department, Region, and Owner Role.
- **`dim_date`**: Calendar dimension generated via Power Query (`power_query_m_scripts.pq`) and marked as a Date Table.

## Key KPIs & DAX Logic
- **P&L Rollups**: Total Revenue, COGS, Gross Profit, Gross Margin %, OpEx, and Operating Income.
- **Sign-Aware Favorable Variance**: Automatically treats `Actual > Budget` as favorable for Revenue and `Budget > Actual` as favorable for COGS/OpEx.
- **Time Intelligence**: `YTD Actual`, `YTD Budget`, `PY Actual`, and `YoY Growth %`.

## Getting Started
1. Clone or download this repository.
2. Follow [`docs/powerbi_build_guide.md`](docs/powerbi_build_guide.md) to load the CSVs into Power BI Desktop, create the star-schema relationships, and add the DAX measures.
3. Save your finished `.pbix` file in the `powerbi/` folder and add screenshots of your dashboard to this `README.md`.
