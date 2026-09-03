# Emergency Treatment Wait Times Across NSW

How does the timeliness of starting emergency treatment in NSW differ across
triage categories and Local Health Districts, and has the gap widened over time?

- **Author:** Tuan Phuc Tran (Lucas Tran)
- **Course:** Informatics: Data and Computation, University of Sydney
- **Tools:** Python

---

## Data

[Bureau of Health Information](https://www.bhi.nsw.gov.au/) quarterly emergency
department results, January 2010 to March 2026 — 65 quarters, 108,664 rows.

Two sheets are used: **Results by triage** for treatment timeliness, and
**Overall results** for attendance counts.

The data is in long format: one row per measure, per entity, per quarter. Four
reporting levels sit stacked in the same table — NSW as a whole, seven peer
groups, seventeen Local Health Districts, and individual hospitals. Levels are
never mixed in the analysis, since the higher ones aggregate the same patients
as the lower ones.

## Approach

**Quality checks before cleaning.** 1,324 blank values (1.22%). BHI explains 724
as suppressed for privacy or small numbers; 600 have no reason given. No
duplicates. `measure_value` mixes percentages, minutes and counts in one column.
`reporting_period` is text, so it sorts alphabetically — April before January.

**Cleaning decisions.**

| Decision | Reason |
|---|---|
| Analyse "% starting treatment on time" | The only quality measure reported across all 65 quarters |
| Exclude T1 Resuscitation | BHI reports patient counts for T1 but no timeliness result |
| Remove suppressed values rather than impute | Imputing would invent data BHI deliberately withheld |
| Convert quarters to dates | So periods sort and plot chronologically |
| Add a metropolitan / regional split | NSW Health's own classification, used to test a city–country divide |
| Keep triage and hospital tables separate | Different units of observation; merging would repeat attendance counts |

Outliers were kept. Hospital results span 10.6% to 100.0% (median 77.4%) — the
extremes are real performance, not data errors.

## Key findings

**Urgent cases have slipped the furthest.** Between 2010 and 2026, T2 on-time
rates fell 12.3 points to 57.2%, while T5 held steady near 90%. The system is
increasingly missing its targets for the patients who can least afford to wait.

**Regional districts outperform metropolitan ones.** For T2 in Q1 2026, regional
districts averaged 65.2% against 51.3% for metropolitan. Murrumbidgee reached
83.4%; South Eastern Sydney managed 35.3%.

**Volume, not location, is what drives it.** Across 73 hospitals, attendances
correlate negatively with timeliness (r = −0.59). Large urban referral hospitals
handle a median 20,098 attendances a quarter at 60.9% on time; smaller regional
hospitals handle 2,808 at 82.3%.

**But scale is not destiny.** The correlation leaves roughly two thirds of the
variance unexplained. Westmead and Liverpool see comparable caseloads and differ
by 37 percentage points — the clearest practical opening for improvement is the
gap between similar hospitals, not the gap between big and small ones.

**An accidental natural experiment.** T5 performance spiked to 98.4% in early
2020, almost certainly because lockdowns suppressed attendances. That reinforces
the volume–timeliness relationship found in the scatter plot.

## What this means

Clinical urgency no longer reliably determines how fast a patient is treated. A
T2 emergency patient at a large metropolitan hospital has a lower chance of
timely care than a T5 patient almost anywhere else.

A single state-level average would place NSW mid-table and conceal a 48-point
spread between its own districts.

## Limitations

- The analysis covers T2–T5 only. T1 patients, the most critically ill, cannot
  be assessed with this measure.
- 600 blank values have no stated reason. They were removed, which may bias
  results if suppression is not random.
- Correlation is not causation. Attendance volume explains about a third of the
  variance in timeliness; the rest is unaccounted for.

## Files

```
├── code_notebook.ipynb    # Analysis: loading, quality checks, cleaning, charts
├── ed_triage_clean.csv    # Cleaned timeliness data by triage category
├── ed_hospitals_clean.csv # Cleaned hospital-level workload and performance
└── README.md
```

## Reproducing

```bash
pip install pandas matplotlib numpy openpyxl
```

Place `BHI_DATA_HQ_ED_Jan2010Mar2026.xlsx` beside the notebook, then run all
cells. The cleaned CSVs and three charts are written to the same folder.

## Data source

Bureau of Health Information, NSW. *Healthcare Quarterly — Emergency Department
results*. https://www.bhi.nsw.gov.au/
