# Workday Integration & RaaS Pipeline Analytics (Power BI)

An end-to-end **Workday Technical Architecture & Power BI** portfolio project for monitoring **Workday Studio, EIB, Core Connector, Orchestrate, Extend, and RaaS** pipelines across Production, Sandbox, and Preview tenants.

## Business Problem
Enterprise Workday teams manage dozens of critical interfaces—such as `Import_Journal_Entries` bank subledger loads, RaaS feeds to Snowflake, and HCM-to-Okta syncs. Relying on manual tenant checks makes it hard to spot SLA drift, month-end bottlenecks, or recurring XSLT/schema failures.

## Repository Structure
```text
workday-analytics-powerbi/
├── data/
│   ├── dim_integration.csv          # Integration catalog (Studio, EIB, RaaS, Extend, Orchestrate)
│   ├── dim_tenant.csv               # Workday tenants (Production, Sandbox, Preview)
│   └── fact_integration_runs.csv    # 6 months of integration run logs, SLAs, and error metrics
├── powerbi/
│   ├── dax_measures.dax             # Success Rate %, SLA Compliance %, Throughput, Journal Lines
│   └── power_query_m_scripts.pq     # Parameterized Workday RaaS JSON connector + Dim_Date
├── docs/
│   └── powerbi_build_guide.md       # Step-by-step instructions to build the .pbix report
├── .gitignore
└── README.md
```

## Data Model (Star Schema)
- **`fact_integration_runs`**: One row per integration execution (`RunID`, `RunDate`, `IntegrationID`, `TenantID`, `Status`, `DurationSeconds`, `RecordsProcessed`, `ErrorCount`, `SLAMetFlag`, `FailureCategory`).
- **`dim_integration`**: Maps `IntegrationID` to `IntegrationType`, `Domain` (Financials, HCM, Payroll, Procurement), `ConnectedSystem`, and `SLATargetSeconds`.
- **`dim_tenant`**: Maps `TenantID` to `EnvironmentType`, `DataCenter`, and `WorkdayRelease`.
- **`dim_date`**: Calendar dimension generated via Power Query (`power_query_m_scripts.pq`).

## Key Features
- **Workday RaaS M-Script Template**: Uses `Web.Contents` with `RelativePath` and Power BI parameters (`pWorkdayBaseUrl`, `pRaaSReportPath`) for clean Power BI Service refresh without hardcoded credentials.
- **Month-End & Financial Interface Tracking**: Dedicated KPIs for `Import_Journal_Entries` throughput and SLA compliance during close windows.
- **Root-Cause Failure Taxonomy**: Categorizes failures by `XSLT Mapping Error`, `Validation Error (Unbalanced JE)`, `API Timeout (504)`, `Auth / ISU Token Expired`, and `Schema Mismatch`.

## Getting Started
1. Clone or download this repository (or add it to your `powerbi-portfolio` repo).
2. Follow [`docs/powerbi_build_guide.md`](docs/powerbi_build_guide.md) to load the CSVs into Power BI Desktop, configure relationships, and paste the DAX measures.
3. Save your `.pbix` file in `powerbi/` and add dashboard screenshots to this `README.md`.
