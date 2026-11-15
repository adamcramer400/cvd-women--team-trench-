# Beyond Age and Blood Pressure

### In more than 44,000 women, cholesterol carried information about cardiovascular disease that age and blood pressure missed. Glucose did not.

*By Adam, Ava, Gabriella and Maneli · Harvard Undergraduate OpenBio Datathon 2026, Track 2*

## Introduction

Cardiovascular disease (CVD) is the leading cause of death worldwide, killing 19.2 million people in 2023; that year, ischemic heart disease (narrowed or blocked heart arteries) killed 3.94 million women and stroke killed 3.29 million ([GBD 2023 CVD Collaborators, 2025](https://doi.org/10.1016/j.jacc.2025.08.015)). Even so, CVD remains under-recognized and under-researched in women ([Fan et al., 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12825910/)), and researchers have called for better recognition, especially in younger women, who have long been overlooked in prevention ([Li et al., 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12237364/)).

Age and blood pressure are the first risk factors anyone checks. Cholesterol and blood glucose are routine blood tests too, and diabetes raises the risk of coronary heart disease more in women than in men ([Peters et al., 2014](https://dro.deakin.edu.au/articles/journal_contribution/Diabetes_as_risk_factor_for_incident_coronary_heart_disease_in_women_compared_with_men_A_systematic_review_and_meta-analysis_of_64_cohorts_including_858_507_individuals_and_28_203_coronary_events/20737474)). So we asked:

**Among women aged about 30 to 65, do cholesterol and glucose levels tell us anything about cardiovascular disease beyond age and blood pressure?**

The short answer: cholesterol does, a little. Glucose, surprisingly, does not.

## The Data and Our Methods

We used the [Cardiovascular Disease dataset](https://www.kaggle.com/datasets/sulianova/cardiovascular-disease-dataset) from the [OpenBio Datathon repository](https://github.com/Harvard-Undergraduate-OpenBio/openbio-datathon-2026), the only datathon dataset with real, individual records (the other two are synthetic or describe whole countries). It holds 70,000 medical exam records with age, height, weight, blood pressure (systolic, the top number, and diastolic, the bottom one), cholesterol and glucose, plus whether the patient has CVD. Cholesterol and glucose are recorded only as normal, above normal or well above normal, and the dataset doesn't say where or when it was collected. Of its 45,530 women, we removed 21 duplicate rows and 870 rows with impossible values (such as a systolic reading lower than the diastolic one), leaving 44,639 ([cleaning rules](#appendix)). We used logistic regression, which estimates how strongly each factor is linked to a yes-or-no outcome while holding the others fixed, and we scored every model on 20% of women it had never seen. Because the data are a single snapshot, our results show association, not cause.

## What We Found

### Before adjusting, CVD rises with both

The women averaged 53 years old, and 49% had CVD. The share with CVD rose from 43% at normal cholesterol to 58% and 77% at the higher levels, and from 47% at normal glucose to 58% and 63% (Figure 1). But women with high cholesterol or glucose also tend to be older and to have higher blood pressure, so we needed to account for both.

![Bar charts of the share of women with CVD at each cholesterol and glucose level](../figures/fig1_raw_rates.png)

*Figure 1. Share of women with CVD at each cholesterol and glucose level, before adjusting for anything (44,639 women).*

### At the same age and blood pressure, cholesterol still matters

An odds ratio (OR) compares the odds of CVD between two groups: 1 means no difference, and the 95% confidence interval (CI) is the range of values our data support. Compared with women of the same age, blood pressure and glucose level whose cholesterol was normal, women with well above normal cholesterol had about three times the odds of CVD (OR 3.09, 95% CI 2.84 to 3.37), and women with above normal cholesterol had 1.45 times the odds (Figure 2).

**This held up in every check we ran.** The odds ratio stayed between 2.96 and 3.08 when we added body mass index, smoking, alcohol and physical activity, added diastolic blood pressure, treated systolic blood pressure as bands (such as 120 to 129) instead of a straight line, or cleaned the data with stricter blood pressure ranges (see the Appendix).

![Forest plot of adjusted odds ratios for cholesterol and glucose levels](../figures/fig2_adjusted_odds.png)

*Figure 2. Odds of CVD at each cholesterol and glucose level compared with a normal level, from one logistic regression model that also includes age and systolic blood pressure. Lines show 95% confidence intervals; the dashed line marks no difference.*

### Glucose flips once cholesterol is included

In a model with only age and systolic blood pressure, very high glucose went with higher odds of CVD (OR 1.41). Once cholesterol joined the model, that reversed to 0.74 (95% CI 0.68 to 0.82), and above normal glucose showed no clear link, since its confidence interval includes 1 (Figure 3).

That does not mean high glucose protects the heart. The two measures travel together: 67% of women with very high glucose also had very high cholesterol. Among women with very high cholesterol, 69% of those with very high glucose had CVD, compared with 83% of those with normal glucose. One possible reason, which we could not test, is that women diagnosed with diabetes may already be on treatment.

![Glucose odds ratios from models without and with cholesterol](../figures/fig3_glucose_flip.png)

*Figure 3. Glucose odds ratios from a model without cholesterol (grey) and with it (blue). Very high glucose goes from higher odds to lower odds once cholesterol is included.*

### Cholesterol adds only a little to prediction

A strong link is not the same as better prediction. We measured how well each model separated women with and without CVD using AUC (area under the curve), where 0.5 is a coin flip and 1 is perfect. Age and systolic blood pressure alone scored 0.787. Adding cholesterol raised that to 0.798, a gain of 0.011 (95% CI 0.008 to 0.014), while glucose added just 0.001 (Figure 4). The model's predicted chances were also close to reality: within about 4 percentage points of actual rates, on average (see the Appendix).

![Bar chart of held-out AUC for the four models](../figures/fig4_auc.png)

*Figure 4. How well each model separates women with and without CVD (AUC), measured on 20% of women held out from fitting. The scale starts at 0.5, a coin flip.*

### What this looks like for one woman

Picture 100 women aged 54 with a systolic blood pressure of 120 and normal glucose. If their cholesterol is normal, our model predicts about 38 of them have CVD. If it is well above normal, about 66 (Figure 5). **That is 3.09 times the odds, but about 1.7 times the chance**, a gap worth remembering whenever you see an odds ratio. Real women who match this profile show the same pattern: 32% versus 75%.

![Dot charts showing 38, 47 and 66 out of 100 women with CVD by cholesterol level](../figures/headline.png)

*Figure 5. Our headline visual: the predicted chance of CVD out of 100 women aged 54 with systolic blood pressure of 120 mmHg and normal glucose, by cholesterol level, with 95% confidence intervals.*

## Unanswered Questions

- **Why does very high glucose go with less CVD once cholesterol is included?** Without medication or diagnosis data, we can't test the treatment idea.
- **What does the 49% rate mean?** We don't know how patients were chosen, so the dataset may over-sample people who are already ill.
- **Does high cholesterol come before CVD?** A single snapshot can't tell us.

## Conclusions, Limitations and Next Steps

In 44,639 women, well above normal cholesterol was linked to about three times the odds of CVD at the same age and blood pressure, and that held in every check. Yet it improved prediction only slightly, and glucose added nothing reliable. So, to our question: cholesterol tells us something beyond age and blood pressure; glucose does not. For women's heart health, the takeaway is that in this data a routine cholesterol test carried information that age and blood pressure missed, while glucose, which we expected to matter, did not.

These results have clear limits. They show association, not cause: we can't say that lowering cholesterol would prevent CVD, which would take studies that follow people over time. We don't know where, when or how the patients were chosen, and nearly half have CVD, so they may not represent women in general. The measurements are coarse: cholesterol and glucose come in three broad levels rather than lab values, there are no diabetes diagnoses or medication data, and CVD is a single yes-or-no label. Finally, sex is recorded only as a binary code, so our results can't speak beyond it; the code for women was inferred from average height (161 vs 170 cm), as the datathon's [data dictionary](https://github.com/Harvard-Undergraduate-OpenBio/openbio-datathon-2026/blob/main/track-02/data/data_dictionary.md) notes.

Next, we would test the cholesterol finding with real lab values, such as LDL (often called "bad") cholesterol and HbA1c (a measure of average blood sugar over the past few months), along with medication records and data that follow women over time.

*This article is a student data analysis. It is not medical advice or clinical guidance.*

## Data and Code

- Team repository: [github.com/adamcramer400/cvd-women--team-trench-](https://github.com/adamcramer400/cvd-women--team-trench-)
- Notebooks: `notebooks/01_cleaning.ipynb` (writes the cleaning log, `docs/cleaning_log.md`) and `notebooks/02_analysis.ipynb`, which reproduces every number and figure in this article (figures are saved in `figures/`)
- Data: [Cardiovascular Disease dataset](https://www.kaggle.com/datasets/sulianova/cardiovascular-disease-dataset), Svetlana Ulianova, Kaggle, 2019 (license listed as "Unknown"), distributed through the [OpenBio Datathon repository](https://github.com/Harvard-Undergraduate-OpenBio/openbio-datathon-2026). The data contain no names or direct identifiers.

## Acknowledgements

We thank the Harvard Undergraduate OpenBio Datathon organizers and mentors, and the [Harvard Undergraduate Data Project](https://www.hudp.org/), whose article guide we followed.

## Appendix

### Cleaning rules (applied in order)

Age was converted from days to years (days ÷ 365.25) before any rule was applied.

| Rule | Removed | Remaining |
|---|---:|---:|
| Duplicate patient IDs | 0 | 70,000 |
| Rows identical in every column except ID | 24 | 69,976 |
| Keep women (sex code 1) | 24,467 | 45,509 |
| Systolic blood pressure 70–250 mmHg | 151 | 45,358 |
| Diastolic blood pressure 40–150 mmHg | 604 | 44,754 |
| Height 130–200 cm | 55 | 44,699 |
| Weight 35–200 kg | 12 | 44,687 |
| Systolic higher than diastolic | 48 | 44,639 |

### Models compared

AUC is measured on the same held-out 20% of women for every model. Gains are against the first model; their 95% confidence intervals come from bootstrapping, which re-scores the models on 500 random resamples of the held-out women to see how much the gain could vary.

| Model | AUC | Gain (95% CI) | Cholesterol OR, above / well above | Glucose OR, above / well above |
|---|---:|---:|---:|---:|
| Age + systolic blood pressure | 0.787 | — | — | — |
| + cholesterol | 0.798 | +0.011 (0.008 to 0.014) | 1.46 / 2.70 | — |
| + glucose | 0.788 | +0.001 (0.000 to 0.002) | — | 1.22 / 1.41 |
| + cholesterol + glucose (main model) | 0.798 | +0.011 (0.008 to 0.014) | 1.45 / 3.09 | 1.04 / 0.74 |

A likelihood-ratio test checks whether adding a variable makes the model fit the data better than chance alone would. Each addition passed it (p < 0.001, where the p-value is the chance of seeing an improvement at least this large if the variable truly added nothing), but only cholesterol noticeably improved prediction. Cholesterol's "well above normal" odds ratio also rises from 2.70 to 3.09 once glucose is added, another sign that the two are tangled together. Confidence intervals for the main model's odds ratios are in Figure 2, and for both glucose models in Figure 3.

### Robustness checks

Odds ratios for well above normal vs normal levels when one choice is changed.

| Check | Women | Cholesterol OR (95% CI) | Glucose OR (95% CI) |
|---|---:|---:|---:|
| Main model | 44,639 | 3.09 (2.84 to 3.37) | 0.74 (0.68 to 0.82) |
| + body mass index, smoking, alcohol, activity | 44,639 | 3.00 (2.76 to 3.27) | 0.74 (0.67 to 0.81) |
| + diastolic blood pressure | 44,639 | 3.07 (2.82 to 3.34) | 0.74 (0.68 to 0.82) |
| Systolic blood pressure in bands | 44,639 | 2.96 (2.71 to 3.23) | 0.75 (0.68 to 0.82) |
| Stricter cleaning (systolic 90–200, diastolic 60–120 mmHg) | 44,465 | 3.08 (2.83 to 3.35) | 0.74 (0.68 to 0.82) |

With the stricter cleaning, adding cholesterol and glucose still raised AUC by 0.011 (from 0.781 to 0.792).

### Calibration and a raw-data check

The average gap between the model's predicted and actual CVD rates was 3.6 percentage points (largest 6.4; Figure 6). As a check that doesn't rely on the model, among real women aged 50 to 58 with systolic blood pressure of 120 and normal glucose, CVD rates were 32% with normal cholesterol (6,112 women), 41% with above normal (541) and 75% with well above normal (228).

![Calibration plot of predicted versus actual CVD rates](../figures/calibration.png)

*Figure 6. Predicted vs actual CVD rates in held-out women, split into 10 equal groups by predicted chance. Points near the dashed line mean the model's predicted chances match reality.*
