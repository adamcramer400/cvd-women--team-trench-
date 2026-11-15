# Team Trench

OpenBio 2026 Datathon, Track 2: Cardiovascular Health in Women

> This project is a student data analysis. It is not medical advice, a diagnostic tool or clinical guidance.

## Team Trench

| Name | Role |
|---|---|
|Adam |Repo and cleaning |
|Gabriella |Analysis |

## Research Question

Among women aged 30 to 65, do cholesterol and glucose levels tell us anything about cardiovascular disease beyond age and blood pressure?

## Datasets Used

- [x] Dataset 1: Cardiovascular Disease Dataset
- [ ] Dataset 2: Heart Attack Risk Prediction Dataset (synthetic)
- [ ] Dataset 3: Global CVD in Women, Tables S1 and S2

## How to Run

1. The raw files are already in `track-02/data/` (see the [data guide](../data/README.md)). On Colab the first notebook cell downloads them.
2. Install packages: `pip install -r requirements.txt`
3. Run `notebooks/01_cleaning.ipynb`, then `notebooks/02_analysis.ipynb`, top to bottom.

## Files

| Path | Contents |
|---|---|
| `notebooks/01_cleaning.ipynb` | Loads and cleans the data, writes the cleaning log |
| `notebooks/02_analysis.ipynb` | Summary statistics, models, checks and the headline visual |
| `src/` | Helper functions (optional) |
| `figures/` | Exported graphs, including `headline.png` |
| `docs/cleaning_log.md` | Each cleaning rule and the records affected |
| `docs/article.md` | Science communication article, following the [HUDP article guide](../../docs/article-guide.md) |
| `requirements.txt` | Python packages |

## Key Findings

Among women aged 30 to 65, cholesterol level stay strongly associated with cardiovascular disease after accounting for age and blood pressure. Once cholesterol was accounted for in the model, changes in glucose accounted for negligible changes in overall gain of predictive accuracy. 

| Factor | Odds Ratio | 95% CI |
|---|---|---|
|Cholesterol above normal (vs normal) | 1.45 | 1.36 to 1.55 | 
| Cholesterol well above normal (vs. normal) | 3.07 | 2.82 to 3.34 |
| Glucose above normal (vs normal) | 1.04 | 0.95 to 1.13 |
| Glucose well above normal (vs normal) | 0.75 | 0.68 to 0.82|
| Age, per year | 1.06 | 1.05 to 1.06 |
| Systolic pressure, per mmHg | 1.06 | 1.06 to 1.06 |
| Diastolic pressure, per mmHg | 1.01 | 1.01 to 1.02 |

How well each model predicts (held-out AUC, where 0.5 is a coin flip and 1 is perfect):

| Model | AUC |
|---|---|
|Age and blood pressure only |0.787 |
| Adding glucose | 0.789 |
| Adding cholesterol | 0.798 |
| Adding both | 0.799 |

The gain from adding both is about 0.011 (95% interval 0.009 to 0.014). Small but does exist.



| Glucose well above normal, compared with normal | Odds ratio |
|---|---|
|On its own | 1.93 |
| Accounting for age and blood pressure | 1.41 |
| Also accounting for cholesterol | 0.75



## Limitations

[Synthetic data, ecological fallacy, binary sex variable, anything else that limits your claims.]

## Presentation

Video link: [add your unlisted YouTube link]
