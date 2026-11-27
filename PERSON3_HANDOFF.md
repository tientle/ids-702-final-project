# Person 3 assessment handoff

Yiran Chen | IDS 702 Team 5 | Updated November 27, 2026

## Agreed scope

Use ordinary logistic regression, an expanded adjustment set, and a log-duration alternative. Retain familiar diagnostics: correlation/VIF, partial residuals, convergence/separation screening, residuals versus fitted values, Cook's distance, fit comparisons and brief classification assessment. Spline models and their related tests have been removed from the analysis, figures, tables and report.

Exercise QMDs and earlier assignment prompts were reviewed privately to keep the methods within the requested scope. The report does not identify individual homework assignments or slide decks.

## Existing team work

- Thomas's data preparation and EDA are merged in PR #1. The contributor account is ThomasXu2026; published report authorship uses Haoye Xu.
- Tien's original main-effects and interaction models, regression tables, visitor-specific estimates and predicted probabilities are on `statistical-modeling` in PR #2.
- The Person 3 contribution starts from modeling commit `1b2b90b713104bd7bbd8f4726d60027c310fb409`, and preserves A/B in the detailed workbook.

## Deliverables

| File | Purpose |
|---|---|
| `final-report.qmd` | Full report, combining all three members' work; all submission computations are in this one R/Quarto file |
| `final-report.pdf` | Rendered stakeholder memo, technical appendix and supplementary coefficient tables |
| `Data Preparation & Exploratory Data Analysis.qmd` | Original A/B plus the detailed Person 3 Section C |
| `outputs/diagnostics/` | 16 diagnostic CSVs and 7 PNGs for the current four-model analysis |

The full report is broader than Person 3's individual contribution. It includes Thomas's data/EDA findings and Tien's original modeling, then incorporates Person 3's checks and revised comparisons.

## Models and findings

- M1: original main-effects model.
- M2: original duration-by-visitor interaction, controlling for product page count and weekend.
- M3: expanded raw-duration interaction, additionally controlling for administrative and informational page counts, exit rate, month and traffic category.
- M4: same expanded controls with log(1 + minutes) and its visitor interaction.

Original raw-duration interaction LR p = 0.0000054; expanded raw-duration interaction p = 0.0013; expanded log-duration interaction p = 0.120. M4 AIC = 9117.20 versus 9198.12 for M3 and 10092.25 for M2. Raw and log specifications are nonnested and compared by AIC, not an LR test between them.

For M4, the 10-to-20-minute odds ratios are approximately 1.36 (95% CI 1.24-1.48) for new visitors and 1.26 (1.19-1.33) for returning visitors. This is evidence of positive associations within each group, not clear evidence that the associations differ. The comparison remains similar after removal of the largest one/five Cook's-distance cases or exact duplicate rows.

Remaining curvature is a limitation. A log transformation and a nonsignificant Hosmer-Lemeshow result do not establish that the model is exactly correct. All numerical separation checks are screening diagnostics, not formal proofs. No flagged observations are automatically deleted.

## Real-world interpretation

The report considers the possibility that extra browsing reflects learning among new visitors, whereas returning visitors may already know the site. This is a proposed explanation rather than a measured mechanism. Returning visitors actually browsed longer on average (21.49 versus 10.61 minutes). The report therefore distinguishes how time may relate to purchase odds within each group from average time differences between groups.

The original 60-minute prediction at 18 pages lacks support among new visitors. Updated conditional curves use 5-25 minutes, with the other characteristics stated. The practical recommendation is to investigate navigation needs and test specific design changes, not to attempt to cause purchases simply by increasing time on site.

## Review before submission

- Review the expanded covariate set and remaining functional-form concerns with Tien.
- Reconcile the initial modeling narrative with the specification-sensitive final conclusions.
- Have all three members review/contribute to each portion, as required by the project description.
- Confirm the designated submitter and incorporate any optional-draft feedback.

No Canvas submission has been made. The GitHub pull request remains a draft.

## Reproduce

```r
install.packages(c("tidyverse", "knitr", "corrplot", "broom", "scales", "car", "pROC", "ResourceSelection"))
```

```sh
quarto render final-report.qmd --to pdf
```

The main report targets a two-page memo and four-page technical appendix, with large coefficient tables on supplementary pages. Current PDF page counts and layout are verified after rendering. Diagnostic outputs are regenerated from the QMD; the cached model RDS and session-info file are not tracked.
