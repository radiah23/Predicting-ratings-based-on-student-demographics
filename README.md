# Do Texas High Schools with More Economically Disadvantaged Students Get Lower Accountability Ratings?

**Multiple linear regression on 1,356 Texas high schools, using the Texas Education Agency's 2022–23 accountability data.**

![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![Quarto](https://img.shields.io/badge/Quarto-39729E?logo=quarto&logoColor=white)
![Regression](https://img.shields.io/badge/method-multiple%20regression-blue)

Final project for **SDS 291: Multiple Regression** at Smith College, by **Radiah Khan, Mercer Mercer, Nafisa Mohamed and Catherine Weeks**. Full report: [`final_project.pdf`](final_project.pdf).

---

## Why it matters

The Texas Education Agency (TEA) uses its A–F accountability ratings to decide when the state takes over a district, as it did with Houston ISD and Fort Worth ISD, both of which serve large shares of economically disadvantaged students. With the rating criteria recently changed, we asked whether a school's overall score is tied to the share of its students who are economically disadvantaged, even after accounting for how much its students grow and progress.

## Data

TEA 2022–23 statewide accountability ratings: one row per campus.

```
10,253 campuses  →  1,394 high schools (grades 9–12)  →  1,356 after removing missing values
```

**Processing:** standardized column names; reshaped seven "distinction" columns (ELA, Math, Science, Social Studies, Progress, Closing the Gaps, Postsecondary Readiness) from wide to long, then collapsed them into a 0–7 **distinction score**; converted character fields to numeric.

## Model

Response: **overall score** (0–100). A reduced model (academic growth, distinction score, school progress) was compared against a full model that adds **% economically disadvantaged** and **number of students**, using a nested F-test and forward selection by AIC.

### Full model coefficients

| Predictor | Estimate | p-value |
|---|---|---|
| School progress score | +1.020 | < 0.001 |
| Distinction score (0–7) | +0.382 | < 0.001 |
| Academic growth score | −0.054 | < 0.001 |
| **% economically disadvantaged** | **−0.089** | **< 0.001** |
| Number of students | −0.0002 | 0.035 |

## Findings

- **Yes: schools with more economically disadvantaged students score lower**, even when their students' progress and growth are held constant. Each additional 10 percentage points of economically disadvantaged students is associated with an overall score about **0.9 points lower**.
- Adding % economically disadvantaged and enrollment **lowered AIC** at each step of forward selection, so both belong in the model.
- **No multicollinearity concerns**: all VIFs < 5.
- **Five potentially influential schools** (high leverage and large standardized residuals) were identified; refitting without them did not meaningfully change the results, so they were kept.

## Limitations

- Residual and Q-Q plots show violations of linearity, equal variance and normality, so p-values should be read with caution.
- The overall score is partly built from some of the predictors, which raises independence concerns.
- One year of data from one state's rating system; the results don't generalize beyond Texas public and charter high schools.

## Files

| File | Purpose |
|---|---|
| `final_project.qmd` / `.pdf` / `.html` | Report source and rendered versions |
| `data_processing.qmd` | Cleaning and reshaping pipeline |
| `School_Year_2022-2023_Statewide_Accountability_Ratings_20251119.csv` | TEA campus-level data |
| `references.bib` | Bibliography |

## Reproduce

```r
install.packages(c("tidyverse", "broom", "kableExtra", "car"))
quarto::quarto_render("final_project.qmd")
```
