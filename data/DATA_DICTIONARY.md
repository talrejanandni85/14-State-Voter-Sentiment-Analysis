# Data Dictionary — Baseline Survey Analysis

> All files have been anonymized. Respondent IDs are synthetic UUIDs. Ages, response values, and weights have been lightly perturbed (±5%). Policy question wording has been generalized.

---

## Raw Survey Files (`survey_raw_policy_a.xlsx`, `survey_raw_policy_b.xlsx`)

One row per respondent. Each file corresponds to one policy question.

| Column | Type | Description |
|---|---|---|
| `Client Name` | string | Anonymized client name |
| `Survey Name` | string | Survey series name |
| `User ID` | string | Synthetic UUID — unique respondent identifier |
| `Age` | integer | Respondent age (lightly perturbed) |
| `Gender` | string | Male / Female |
| `State` | string | 2-letter state code (one of 14 battleground states) |
| `Race` | string | White, African American, Latino / Hispanic, Asian, Native American, Other |
| `Party` | string | Democratic Party, Republican Party, Independent, Other |
| `Start Date` | string | Date of survey response (MM/DD/YYYY) |
| `Question ID` | string | Anonymized question identifier |
| `Question Name` | string | Anonymized question label |
| `Response Value` | float | Respondent's score on 0–100 scale (lightly perturbed) |
| `Respondent Weight` | float | Survey weight to correct for sample imbalances |

---

## Demographic Supplement (`survey_demographics.xlsx`)

One row per respondent per demographic question (long format — 3 rows per respondent).

| Column | Type | Description |
|---|---|---|
| `Client Name` | string | Anonymized client name |
| `Survey Name` | string | Survey series name |
| `User ID` | string | Synthetic UUID — links to raw survey files |
| `Age` | integer | Respondent age |
| `Gender` | string | Male / Female |
| `State` | string | State abbreviation |
| `Race` | string | Race/ethnicity |
| `Party` | string | Party affiliation |
| `Start Date` | string | Date of survey response |
| `Question ID` | string | Demographic question ID |
| `Question Name` | string | One of: Income, Education, Area |
| `Question Option ID` | string | Answer option ID |
| `Option Text` | string | Answer text (e.g., "$25,000–$50,000", "College graduate", "Urban") |
| `Response Value` | float | Numeric response where applicable |
| `Respondent Weight` | float | Survey weight |

---

## Analysis Output Files (`analysis_policy_a.xlsx`, etc.)

Multi-sheet Excel files produced by the Rmd pipeline.

### Sheet: `Overall Demos`
Weighted average response value broken out by demographic group — one row = overall panel.

| Column | Description |
|---|---|
| `Overall` | Weighted mean — all respondents |
| `Men` / `Women` | Weighted mean by gender |
| `Democrats` / `Independents` / `Republicans` | Weighted mean by party |
| `White` / `Black/African American` / `Native American` / `Latinx/Hispanic` / `Asian/AAPI` | Weighted mean by race |
| `18-44` / `45-64` / `65+` | Weighted mean by age group |

### Sheet: `Demos by State`
Same demographic columns as above, with one row per state plus an overall row.

| Column | Description |
|---|---|
| `State` | 2-letter state abbreviation |
| `BIPOC` | Weighted mean for all non-White respondents combined |
| *(all other columns same as Overall Demos)* | |

### Sheet: `Dist of Opinion Overall`
Weighted percentage of respondents in each 20-point response band — overall panel.

| Column | Description |
|---|---|
| `0-19` | % of weighted responses scoring 0–19 (strong opposition) |
| `20-39` | % scoring 20–39 |
| `40-59` | % scoring 40–59 (middle / undecided) |
| `60-79` | % scoring 60–79 |
| `80-100` | % scoring 80–100 (strong support) |

### Sheet: `DistofOpinion by State`
Same 5 band columns, one row per state.

### Sheet: `Trendlines`
Date-level average response value over time (from 2019 to pull date), for tracking opinion shifts longitudinally.

---

## Derived Variables (created in analysis)

| Variable | Logic | Description |
|---|---|---|
| `IncomeGroup` | Under $25k / $25k–$50k / $50k–$75k → "Under $75,000"; else "Over $75,000" | Binary income split used for subgroup analysis |
| `EducationGroup` | College graduate / Post-grad / Some college → "College"; else "Non-college" | Binary education split |
| `AgeGroup` | 18–44 / 45–64 / 65+ | Three-bucket age grouping |
| `BIPOC` | Race ∈ {African American, Native American, Latino/Hispanic, Asian, Other} | Non-White aggregate group |
