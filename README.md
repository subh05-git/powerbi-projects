# HR Workforce & Employee Attrition Analytics (Power BI)

An end-to-end **Power BI People Analytics** portfolio project analyzing workforce headcount, voluntary vs. involuntary turnover, engagement drivers, and estimated replacement cost across departments and job roles.

## Business Problem
HR Business Partners and executive leaders need visibility into where attrition is concentrated, how engagement and work mode (Remote, Hybrid, Onsite) impact retention, and the financial cost of employee turnover.

## Repository Structure
```text
hr-attrition-dashboard/
├── data/
│   ├── dim_department.csv           # Department, business unit, and executive owner
│   ├── dim_job_role.csv             # Job roles, levels, families, and replacement cost
│   └── fact_employee_attrition.csv  # 1,200 employee records with hire/exit & engagement data
├── powerbi/
│   ├── dax_measures.dax             # Headcount, Attrition %, Cost, and USERELATIONSHIP DAX
│   └── power_query_m_scripts.pq     # Power Query (M) script for Dim_Date
├── docs/
│   └── powerbi_build_guide.md       # Step-by-step instructions to build the .pbix report
├── .gitignore
└── README.md
```

## Data Model (Star Schema)
- **`fact_employee_attrition`**: One row per employee (`EmployeeID`, `HireDate`, `TerminationDate`, `AttritionType`, `AttritionReason`, `EngagementScore`, `TenureYears`, `BaseSalaryUSD`).
- **`dim_department`**: Maps `DepartmentID` to Department, Business Unit, and Cost Center.
- **`dim_job_role`**: Maps `JobRoleID` to Job Role, Job Level, Job Family, and `AvgReplacementCostUSD`.
- **`dim_date`**: Calendar dimension generated via Power Query (`power_query_m_scripts.pq`) with an active relationship on `HireDate` and an inactive relationship on `TerminationDate` (activated via `USERELATIONSHIP`).

## Key KPIs & DAX Logic
- **Workforce & Turnover**: `Active Headcount`, `Attrition Count`, `Attrition Rate %`, `Retention Rate %`, and `Voluntary vs. Involuntary` split.
- **Cost of Turnover**: `Estimated Attrition Cost $` using `SUMX` + `RELATED(dim_job_role[AvgReplacementCostUSD])`.
- **Time-Series Exits**: `Terminations by Period`, `YTD Terminations`, and `PY Terminations` using `USERELATIONSHIP`.

## Getting Started
1. Clone or download this repository (or place it inside your `powerbi-portfolio` repo).
2. Follow [`docs/powerbi_build_guide.md`](docs/powerbi_build_guide.md) to load the CSVs into Power BI Desktop, configure relationships, and add the DAX measures.
3. Save your finished `.pbix` file in `powerbi/` and add dashboard screenshots to this `README.md`.
