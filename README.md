# Vagu Labs — Employee Attendance Analysis (Bangalore, Q1 2026)

An exploratory data analysis (EDA) of daily employee attendance, punctuality, and productivity
for Vagu Labs' Bangalore workforce across Q1 2026 (Jan 2 – Mar 31, 2026).

The analysis follows a standard **23-step EDA checklist** (Classroom Computer Institute — Data
Analytics), moving through four phases: Inspect → Clean & Prepare → Analyze → Report.

## Project Structure

```
Vagu_labs_employee_attendence/
├── Raw_dataset/
│   └── employee_attendance_bangalore_q1_2026.csv       # Original HR export (55,374 rows × 22 cols)
├── clean_dataset/
│   └── employee_attendance_bangalore_q1_2026_cleaned.csv  # Cleaned/typed output (55,374 rows × 26 cols)
├── analysis/
│   └── vagu-labs-data-analysis-1.ipynb                  # Full 23-step EDA notebook
└── README.md
```

## Dataset

**Source:** HR attendance export, one row per employee per day.

| Property | Value |
|---|---|
| Rows | 55,374 (employee-day records) |
| Unique employees | 980 |
| Departments | 10 (Engineering, Sales, Operations, Customer Success, Finance, HR, Marketing, Design, Data Science, Product Management) |
| Office locations | 5 Bangalore campuses |
| Date range | 2026-01-02 → 2026-03-31 |
| Raw columns | 22 |
| Cleaned columns | 26 (after dropping 2 PII/ID columns and adding 6 derived columns) |

**Key raw columns:** `employee_id`, `gender`, `department`, `designation`, `employment_type`,
`office_location`, `date_of_joining`, `attendance_date`, `shift_type`, `attendance_status`,
`work_mode`, `login_timestamp`, `logout_timestamp`, `total_hours_worked`, `break_duration_mins`,
`net_productive_hours`, `late_arrival_mins`, `early_exit_mins`, `overtime_hours`, `leave_type`.

**Derived columns added during cleaning:** `attendance_month`, `attendance_weekday`,
`tenure_days`, `is_late`, `is_early_exit`, `productivity_ratio`.

## Problem Statement

What are the attendance, punctuality, and productivity patterns of Vagu Labs' Bangalore workforce
in Q1 2026, and how do they vary by department, work mode, shift type, and employment type?
Specifically, what is associated with late arrivals, early exits, and leave-taking — and is remote
work adoption uneven across departments?

## Methodology (23-Step Checklist)

**Phase 1 — Inspect (Steps 1–8):** row/column counts, per-column understanding, whitespace
checks, duplicate detection, memory usage, dtype checks, and null-value audit.

**Phase 2 — Clean & Prepare (Steps 9–17):** split columns into numerical/categorical/datetime/ID
groups, `describe()` numerical columns, write a dataset summary and problem statement, select
required columns, drop unneeded ones (`employee_name`, `attendance_id`), add derived columns,
clean each column type (text standardization, outlier/skew checks, datetime parsing,
ID-format validation, cross-column consistency checks), and export the final cleaned CSV.

**Phase 3 — Analyze (Steps 18–21):** univariate analysis (histograms, box plots, value counts),
bivariate analysis (scatter plots, correlation, grouped box/bar plots, cross-tabs), multivariate
analysis (pair plots, correlation heatmap, faceted grids), and hypothesis testing (Pearson
correlation, independent t-test, one-way ANOVA, chi-square test of independence).

**Phase 4 — Report (Steps 22–23):** definition of dashboard-ready KPIs and recommended chart
types (KPI cards, trend line, bar chart, pie/donut, heatmap, funnel, scatter).

## Data Quality Findings

The raw dataset was found to be largely clean already:

- **0** fully duplicated rows, **0** duplicated `attendance_id` values
- **0** whitespace issues in headers or string columns
- **0** inconsistent category labels
- Employee-level attributes (name, gender, department, designation, etc.) are fully consistent
  across every row for the same `employee_id`
- The only missing values were `login_timestamp` / `logout_timestamp` (2,635 rows each, 4.76%),
  and these are **structurally correct** — they occur exactly on "On Leave" days, not a data-entry
  gap
- Main cleaning work required was **type correction**: dates and timestamps were stored as text
  and needed conversion to proper datetime types

## Statistical Test Results

| Test | Variables | Result |
|---|---|---|
| Pearson correlation | `total_hours_worked` vs `net_productive_hours` | r = 0.973, p < 0.001 — strong positive correlation |
| Independent t-test | `late_arrival_mins` by `work_mode` (WFH vs WFO) | t = -28.25, p < 0.001 — WFH averages 6.07 min late vs. WFO's 11.50 min |
| One-way ANOVA | `net_productive_hours` by `department` | F = 41.46, p < 0.001 — productive hours differ significantly by department |
| Chi-square test | `department` vs `work_mode` | χ² = 4423.64, p < 0.001 — remote-work adoption is uneven across departments |

## Key KPIs

| KPI | Value |
|---|---|
| Attendance Rate | 95.24% |
| Leave Rate | 4.76% |
| Half-Day Rate | 2.14% |
| On-Time Arrival Rate | 50.37% |
| Average Late Arrival | 9.86 min |
| Average Early Exit | 20.10 min |
| Average Net Productive Hours | 8.12 hrs |
| WFH Adoption | 19.70% |
| Total Overtime Hours (Q1) | 8,308.79 hrs |

## Key Findings

- Total hours worked and net productive hours are strongly positively correlated, as expected.
- Employees working from home arrive later, on average, than those working from the office
  (statistically significant).
- Net productive hours differ meaningfully across departments — Engineering and Sales lead,
  Operations and Data Science trail.
- Work-mode adoption (office vs. home vs. client site) is significantly associated with
  department — remote-work usage is uneven across teams (e.g., only Customer Success and Sales
  have any "Client Site" records).

## Tech Stack

- Python 3, pandas, NumPy
- matplotlib, seaborn (visualization)
- SciPy (`scipy.stats`) for hypothesis testing
- Jupyter Notebook

## Reproducing the Analysis

1. Open [`analysis/vagu-labs-data-analysis-1.ipynb`](analysis/vagu-labs-data-analysis-1.ipynb) in
   Jupyter.
2. Update the `RAW_PATH` variable in the first code cell to point to
   [`Raw_dataset/employee_attendance_bangalore_q1_2026.csv`](Raw_dataset/employee_attendance_bangalore_q1_2026.csv).
3. Run all cells in order — Steps 1–23 execute sequentially and export the cleaned dataset to
   `employee_attendance_bangalore_q1_2026_cleaned.csv`.
