# Quantifying Data Quality: A Statistical Framework for Scoring and Monitoring Scientific Datasets

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org)
[![Kaggle](https://img.shields.io/badge/Kaggle-Notebook-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A reproducible statistical framework for quantifying, scoring, and monitoring data quality across four theoretically-grounded dimensions — applied to the UCI Air Quality dataset (9,357 hourly sensor observations).

**Module:** Quantitative Data Analytics | FOM Hochschule Essen | Summer Semester 2026  
**Team:** John Gomez · Atharva Satam · Prathamesh Patil · Nikhilvarma Kandula

---

## The Problem

Scientific datasets drive critical decisions in healthcare, climate science, and environmental monitoring. Yet no standardised framework exists for quantifying data quality across multiple dimensions simultaneously. Existing approaches are either purely theoretical or too domain-specific to generalise.

**Research question:** How can data quality be quantified, scored, and monitored using a reproducible statistical framework?

---

## Framework at a Glance

| Dimension | Formula | Source | Score |
|-----------|---------|--------|-------|
| Completeness | `1 − (missing / total)` | Pipino et al. (2002) | **0.930** ✅ |
| Consistency | `1 − (violations / checks)` | Batini & Scannapieco (2016) | **0.978** ✅ |
| Accuracy | `1 − (MAE / range)`, normalised | Heinrich et al. (2018) | **0.824** ✅ |
| Timeliness | `e^(−0.01 × age_days)` | Batini & Scannapieco (2016) | **0.626** ❌ |
| **Composite DQI** | Equal-weighted average | — | **0.840** |

> **Key finding:** The dataset is structurally sound — but its freshness is not. Three of four dimensions exceed the 0.7 acceptability threshold; Timeliness scores lowest due to dataset age (2004–2005) and temporal recording gaps.

---

## Dataset

**UCI Air Quality** — De Vito (2016)  
9,358 hourly observations from an Italian city (March 2004 – February 2005)  
5 electrochemical gas sensors + certified reference analyzers + temperature/humidity

**Cleaning pipeline:**

```
Raw data         Sentinel fix         Column drop         Final sample
9,358 × 15   →  -200 → NaN (200)  →  NMHC(GT) dropped  →  9,357 × 12
               per UCI docs          (90% missing)         ready for analysis
```

---

## Hypotheses & Results

| # | Hypothesis | Method | Result | Decision |
|---|-----------|--------|--------|----------|
| H₁ | Completeness ↔ Usability | Pearson correlation | r = 0.998, p < 0.001 | ✅ Supported |
| H₂ | Consistency → fewer errors | Welch t-test | p = 0.517 | ❌ Not supported |
| H₃ | DQI internal reliability | Cronbach's α | Low α (multidim. framework — expected) | ℹ️ Expected |
| H₄ | Temporal drift present | KS two-sample test | 7/8 columns significant | ✅ Supported |

**Key insight from H₂:** More complete columns contained fewer extreme outliers, but no significant difference in error *rates* was found between high- and low-consistency segments — suggesting DQ scores measure data structure, not sensor calibration quality.

**Key insight from regression:** DQ scores predict sensor error with R²=0.013. That's a feature, not a bug — DQ scores measure whether data is clean and complete; sensor calibration is a different question entirely.

---

## Figures

| Figure | Description |
|--------|-------------|
| `fig_01_missing_data.png` | Missing values by column + temporal heatmap |
| `fig_02_dq_scores.png` | Four dimension scores vs 0.7 threshold |
| `fig_03_distributions.png` | Completeness distribution + normalised boxplots |
| `fig_04_correlation.png` | Pearson correlation matrix (lower triangle) |
| `fig_05_h1.png` | H₁: Completeness vs usability scatter |
| `fig_06_h2.png` | H₂: Error rate distributions (high/low consistency groups) |
| `fig_07_h4_drift.png` | H₄: KS drift — CDF + density overlay (NOx) |
| `fig_08_dqi.png` | Composite DQI distribution + 5-fold CV bars |
| `fig_09_blue.png` | BLUE assumption plots (residuals, normality, homoscedasticity) |
| `fig_10_regression.png` | Actual vs predicted + 5-fold CV results |
| `fig_11_validity.png` | Discriminant validity heatmap |

---

## Project Structure

```
qda-project/
├── Data_Quality_Analysis.ipynb   ← full analysis notebook (54 cells)
├── AirQualityUCI.csv             ← UCI Air Quality dataset
├── AirQualityUCI.xlsx            ← Excel version
├── requirements.txt
├── fig_01_missing_data.png
├── fig_02_dq_scores.png
├── fig_03_distributions.png
├── fig_04_correlation.png
├── fig_05_h1.png
├── fig_06_h2.png
├── fig_07_h4_drift.png
├── fig_08_dqi.png
├── fig_09_blue.png
├── fig_10_regression.png
└── fig_11_validity.png
```

---

## Reproduce the Analysis

```bash
# Clone
git clone https://github.com/nikhilvarmakandula/quantifying-data-quality.git
cd quantifying-data-quality/qda-project

# Install dependencies
pip install pandas numpy matplotlib seaborn scipy pingouin statsmodels scikit-learn

# Run
jupyter notebook Data_Quality_Analysis.ipynb
```

---

## Key Correlations (from fig_04)

- CO ↔ Benzene: r = 0.93
- CO ↔ PT08.S1: r = 0.88
- Temperature ↔ Humidity: r = −0.58
- PT08.S3(NOx) negatively correlated with all pollutants (proxy sensor inversion)

---

## Future Work

- Cross-domain validation (healthcare, finance, climate datasets)
- Real-time streaming pipeline with automated quality alerts
- ML-based weight optimisation (PCA or domain expert input) vs equal weighting
- Random Forest / XGBoost for error prediction

---

## References

Batini, C., & Scannapieco, M. (2016). *Data and information quality: Dimensions, principles and techniques*. Springer.

Cronbach, L. J. (1951). Coefficient alpha and the internal structure of tests. *Psychometrika*, *16*(3), 297–334.

De Vito, S. (2016). Air quality dataset. UCI Machine Learning Repository. https://doi.org/10.24432/C59K5F

Field, A. (2018). *Discovering statistics using IBM SPSS statistics* (5th ed.). Sage Publications.

Heinrich, B., et al. (2018). Requirements for data quality metrics. *Journal of Data and Information Quality*, *9*(2), 1–32.

Massey, F. J. (1951). The Kolmogorov-Smirnov test for goodness of fit. *JASA*, *46*(253), 68–78.

Pipino, L. L., Lee, Y. W., & Wang, R. Y. (2002). Data quality assessment. *Communications of the ACM*, *45*(4), 211–218.

Wang, R. Y., & Strong, D. M. (1996). Beyond accuracy: What data quality means to data consumers. *JMIS*, *12*(4), 5–33.

---

## Related Projects

| Project | Tools | Scale |
|---------|-------|-------|
| [Skill Demand in German Tech Market (NLP)](https://github.com/nikhilvarmakandula/skill-demand-german-tech-market) | Python · spaCy · K-Means | 3,200 job postings |
| [Cyclistic Bike-Share Analysis](https://github.com/nikhilvarmakandula/cyclistic-bikeshare-case-study) | BigQuery SQL · Tableau | 5.5M rides |

---

## Contact

**Nikhilvarma Kandula** — M.Sc. Big Data & Business Analytics, FOM Essen  
[nikhilvarma@kandula.studio](mailto:nikhilvarma@kandula.studio) · [kandula.studio](https://kandula.studio) · [LinkedIn](https://linkedin.com/in/kandulanikhilvarma) · [GitHub](https://github.com/nikhilvarmakandula)
