# Hospital Readmission Penalties & Equity

**[View the interactive dashboard on Tableau Public](https://public.tableau.com/app/profile/camille.stennis/viz/HospitalReadmissionPenaltiesEquity/HospitalReadmissionPenaltiesEquity)**

## Overview

This project analyzes Medicare's Hospital Readmissions Reduction Program (HRRP) to understand which hospitals face the highest financial penalty exposure for excess 30-day readmissions, and whether that exposure is better explained by the community a hospital serves or by the hospital's own ownership structure.

**Core question:** Which hospitals are facing the highest Medicare readmission penalty exposure, and how does that correlate with community demographics versus hospital ownership type?

## Data Sources

- **CMS Hospital Readmissions Reduction Program (FY2026)** — per-hospital, per-condition excess readmission ratios and discharge volumes across six tracked conditions (AMI, heart failure, pneumonia, COPD, hip/knee replacement, CABG)
- **CMS Hospital General Information** — hospital type, ownership, and location data
- **U.S. Census Bureau** — county-level median household income and race/ethnicity population data

## Methodology

**Composite score.** Each hospital's overall readmission penalty exposure is calculated as a discharge-weighted average of its Excess Readmission Ratio across all conditions with valid data (sum of discharges × ratio, divided by total discharges), mirroring the logic CMS itself uses for its composite measure. A score of 1.0 represents exactly expected performance; above 1.0 indicates more readmissions than expected, below 1.0 indicates fewer.

**Handling missing data.** CMS suppresses a hospital/condition pair when a hospital has fewer than 25 eligible discharges for that condition. Rather than excluding hospitals with fewer valid conditions (which would disproportionately remove small and rural hospitals from the analysis), every hospital with at least one valid measure is included in the primary analysis. A `reliability_tier` field (high: 4–6 conditions, medium: 2–3, low: 1) is retained as a visible attribute so results can be filtered or interpreted with appropriate caution, rather than filtered out by default.

**Geography.** Hospitals were joined to county-level Census data via FIPS code. This required resolving several real-world data issues: compound county names split inconsistently across sources (e.g. "DE KALB" vs. "DeKalb"), a handful of data-entry typos in the source file, and Connecticut's 2022 replacement of its traditional counties with nine new planning regions, which required remapping 25 hospitals by town rather than by legacy county name.

**Scope.** This analysis covers 2,469 hospitals with at least one valid HRRP measure. Critical Access Hospitals are structurally absent, as they are exempt from HRRP under a different Medicare payment model. 578 hospitals with insufficient case volume across all six conditions are excluded entirely.

## Key Findings

**Community demographics show only a weak relationship with penalty exposure.** County median household income and racial/ethnic composition are each statistically significantly correlated with a hospital's composite score, but the effect sizes are small (Pearson r ≈ -0.07 to 0.12), suggesting these factors alone explain little of the variation.

**Hospital ownership structure shows a much stronger relationship.** For-profit hospitals have a significantly higher average composite score (1.022) than nonprofit hospitals (1.001), a highly significant difference (t = 6.75, p < 0.000001) that persists even when restricted to only the most data-complete hospitals. This is consistent with prior health services research linking for-profit ownership to shorter lengths of stay and leaner discharge-planning resources. The analysis identifies this association; it does not establish the underlying mechanism.

## Repository Structure

```
hospital-readmission-penalties-equity/
├── raw-data/           # Original, unmodified source files
├── processed-data/      # Cleaned, joined, analysis-ready dataset
└── README.md
```

## Tools

Python (pandas) for data cleaning and statistical analysis, Tableau Public for visualization.
