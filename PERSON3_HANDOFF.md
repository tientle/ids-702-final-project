# Person 3 assessment handoff

Yiran Chen | IDS 702 Team 5 | November 26, 2026

## What was already complete

- Person 1's data preparation and EDA were merged in PR #1. The contributor account is ThomasXu2026; the commit/report use the name Haoye Xu. Existing report authorship is preserved.
- Person 2's statistical modeling is on `statistical-modeling` in PR #2: main-effects and interaction logistic models, LR comparison, exponential-scale summaries, visitor-specific effects, and conditional probability curves.
- This contribution starts from commit `1b2b90b713104bd7bbd8f4726d60027c310fb409` on that modeling branch. It does not merge or modify the status of PR #2.

## Requirements and feedback used

Read `ProjectDescription (1).pdf` before implementing the assessment. The main report must have 5-7 pages, including a 2-3-page memo and a 3-5-page technical appendix, with at least one interaction and suitable confounding adjustment. All analysis and visuals must use R; all code for the submission must be in one QMD. Relevant large coefficient tables may be placed in an additional appendix outside the page limit.

Prioritized IDS 702 course materials: Introduction to GLMs (assumptions and logit link), Logistic Regression 2 (MLE, uncertainty and interpretation), Odds and Odds Ratios, and HW6 (partial residual plots and Hosmer-Lemeshow).

The professor's November 10 feedback supplied during this task asks for more confounder adjustment, an inferential focus with limited predictive assessment, and justification for a duration log transform. The bicycle-data comments are not applicable to this dataset.

## Completed deliverables

| Requirement | Implementation / output |
|---|---|
| Binary outcome and independence | Outcome validation; UCI different-user sampling description; clustering and identifier limitations |
| Functional form | Interaction-aware partial working residuals; nested nonlinear checks; raw, log and flexible duration alternatives |
| Correlation and VIF | Predictor correlation matrix; main-effects/interaction VIF; spline-model dimension-adjusted GVIF |
| Separation and convergence | Convergence, rank, boundary status, extreme probabilities, SEs, tighter-tolerance refit, categorical outcome support; explicitly a numerical separation screen |
| Residuals and influence | Deviance/Pearson residuals, Cook's distance and leverage; actual one/five-case deletion refits |
| Model adjustments | Expanded adjustment, frequency-only traffic pooling, flexible duration interaction; alternative exit-rate and administrative-count specifications |
| Robustness | Deduplication and positive-duration-only sensitivity fits; full-covariance finite contrasts |
| Model fit | Residual deviance, AIC, McFadden pseudo-R2, grouped calibration and Hosmer-Lemeshow |
| Brief classification | Apparent ROC/AUC, justified illustrative 0.50 threshold, prevalence-threshold sensitivity, confusion matrix, sensitivity and specificity |
| Written assessment | Workbook Section C plus integrated executive summary, assumptions, diagnostics, limitations, recommendations and future research |
| Report integration | Independently reproducible `final-report.qmd`; a two-page memo, five-page technical appendix and separate full coefficient tables |

## Files and reproduction

- `Data Preparation & Exploratory Data Analysis.qmd`: existing A/B work, followed by the complete Section C assessment. This is a detailed workbook, not the page-limited submission.
- `final-report.qmd`: the single independently executable submission draft. Includes all data preparation, models, diagnostics, figures and report code; does not source another R/QMD file.
- `final-report.pdf`: rendered report draft, pending model and team review.
- `outputs/diagnostics/`: named CSV tables and PNG figures. The QMD also regenerates a model-object RDS and a session-info file, which are not tracked because they are reproducible and environment-specific.

Install R packages once:

```r
install.packages(c("tidyverse", "knitr", "corrplot", "broom", "scales", "car", "pROC", "ResourceSelection"))
```

Render from the repository root:

```sh
quarto render final-report.qmd --to pdf
```

The workbook uses an explicit population-moment skewness function rather than requiring the otherwise unavailable `moments` package; it gives the same definition for complete data. Its original A/B analyses are preserved.

## Main findings requiring review with Person 2

1. The original M2 has severe functional-form/calibration concerns. It has AIC 10092.25 and an interaction LR p-value around 0.0000054. Its constant per-minute interpretation should remain preliminary.
2. M3 expands adjustment for month, traffic category, administrative/informational page counts and exit rate. These are plausible adjustment variables, not known causal confounders. Traffic codes with fewer than 100 sessions are pooled without looking at outcomes.
3. A simpler expanded log-duration model (M4) improves AIC to 9117.20, with an interaction LR p-value about 0.120. A log transform alone does not adequately capture the duration relationship.
4. The proposed nonlinear duration model (M5) has AIC 9094.70 and joint interaction LR p-value about 0.0000145. The different interaction tests address different hypotheses. Do not summarize this as either “the interaction disappeared” or “a constant extra per-minute effect is established.”
5. In M5, the adjusted 10-to-20-minute odds ratios are approximately 1.81 (95% CI 1.54-2.12) for new visitors and 1.29 (1.19-1.41) for returning visitors. These finite contrasts are stable to the largest-case deletion and deduplication checks. They are not causal effects or universal per-ten-minute effects.
6. Among sessions with 15-21 product pages, no new visitor reaches 60 minutes; its maximum is about 43.1 minutes and its 95th percentile is 26.2 minutes. No local session has zero recorded duration. The revised conditional curve displays 5-25 minutes, with an explicit joint-support limitation.
7. M5's apparent AUC is about 0.763. At 0.50, sensitivity is only 3.65% and specificity is 99.25%. This illustrates the class imbalance and why accuracy/prediction tuning is not the project's main conclusion.

## Decisions remaining before submission

- Person 2 and the team should confirm the substantive adjustment set and whether to adopt the proposed spline specification. The report labels it as proposed throughout.
- Review residual control-variable curvature and the alternate adjustment results; model-selection uncertainty is not included in nominal intervals.
- Reconcile the original modeling narrative with the revised results before presenting one final conclusion.
- Obtain all members' review/contributions to each portion, as required by the project description.
- Incorporate optional-draft feedback after December 3, perform the final team review, and designate the Canvas submitter. No Canvas submission has been made.

## Sources

- Sakar & Kastro (2018), UCI dataset and sampling/variable documentation: https://doi.org/10.24432/C5F88Q
- Sakar et al. (2019), original study: https://doi.org/10.1007/s00521-018-3523-0
- R Core Team, regression deletion diagnostics: https://stat.ethz.ch/R-manual/R-patched/library/stats/html/influence.measures.html
- ResourceSelection documentation: https://cran.r-project.org/web/packages/ResourceSelection/refman/ResourceSelection.html
