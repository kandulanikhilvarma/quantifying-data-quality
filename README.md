<!-- ═══════════════════════════════════════════════════════════════
     HEADER BANNER
═══════════════════════════════════════════════════════════════ -->
<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a237e,100:0d1117&height=220&section=header&text=Quantifying%20Data%20Quality&fontSize=42&fontColor=ffffff&fontAlignY=38&desc=A%20Statistical%20Framework%20for%20Scoring%20%26%20Monitoring%20Scientific%20Datasets&descAlignY=58&descSize=17&descColor=9fa8da&animation=fadeIn" />

<!-- ═══════════════════════════════════════════════════════════════
     BADGE ROW
═══════════════════════════════════════════════════════════════ -->
<div align="center">

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![Kaggle](https://img.shields.io/badge/Kaggle-Open_Notebook-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)
[![FOM](https://img.shields.io/badge/FOM_University-Essen,_Germany-1a237e?style=for-the-badge&logo=academia&logoColor=white)](https://fom.de)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Published-f59e0b?style=for-the-badge)]()

<br/>
**Dataset:** UCI Air Quality · 9,357 hourly observations · March 2004 – February 2005

</div>

---

<!-- ═══════════════════════════════════════════════════════════════
     COMPOSITE SCORE DASHBOARD — THE HEADLINE NUMBER
═══════════════════════════════════════════════════════════════ -->

## 📊 Data Quality Index — At a Glance

<div align="center">

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    COMPOSITE DATA QUALITY INDEX (DQI)                       ║
║                                                                              ║
║                              ★  0.840 / 1.000  ★                            ║
║                         ████████████████████░░░░  84%                       ║
║                                                                              ║
╠══════════════╦═══════════════╦═══════════════╦══════════════════════════════╣
║ Completeness ║ Consistency   ║ Accuracy      ║ Timeliness                   ║
║   0.930 ✅   ║   0.978 ✅   ║   0.824 ✅   ║   0.626 ❌                   ║
║ ██████████░  ║ █████████████ ║ ████████████░ ║ ████████░░░░░░               ║
║ Exceeds 0.7  ║ Exceeds 0.7  ║ Exceeds 0.7  ║ Below threshold               ║
╚══════════════╩═══════════════╩═══════════════╩══════════════════════════════╝

Threshold line: 0.700 — three of four dimensions pass. One fails: time.
The dataset is structurally sound. Its freshness is not.
```

</div>

---

<!-- ═══════════════════════════════════════════════════════════════
     THE PROBLEM
═══════════════════════════════════════════════════════════════ -->

## 🎯 The Problem

Scientific datasets drive critical decisions in healthcare, climate science, and environmental monitoring. Yet **no standardised framework exists** for quantifying data quality across multiple dimensions simultaneously.

Existing approaches are either purely theoretical — no implementation — or too domain-specific to generalise beyond their original context. This leaves researchers and data teams with a difficult choice: adopt an unsuitable framework or build nothing.

**Our research question:**
> *How can data quality be quantified, scored, and monitored using a reproducible, theoretically-grounded statistical framework?*

**Our answer:** A four-dimension scoring system, grounded in peer-reviewed measurement theory, validated with real sensor data, and packaged as a fully reproducible Python/Jupyter pipeline.

---

<!-- ═══════════════════════════════════════════════════════════════
     FRAMEWORK
═══════════════════════════════════════════════════════════════ -->

## 🧱 The Framework — Four Dimensions, One Score

Each dimension is grounded in a published measurement theory and produces a normalised [0–1] score. Equal weighting produces the composite **Data Quality Index (DQI)**.

| Dimension | What It Measures | Formula | Literature Basis | Score |
|-----------|-----------------|---------|-----------------|:-----:|
| **Completeness** | Are all expected values present? | `1 − (missing / total)` | Pipino et al. (2002) | **0.930** ✅ |
| **Consistency** | Do values conform to defined rules? | `1 − (violations / checks)` | Batini & Scannapieco (2016) | **0.978** ✅ |
| **Accuracy** | How close are values to the truth? | `1 − (MAE / range)`, normalised | Heinrich et al. (2018) | **0.824** ✅ |
| **Timeliness** | How fresh is the data? | `e^(−0.01 × age_days)` | Batini & Scannapieco (2016) | **0.626** ❌ |
| **Composite DQI** | Equal-weighted average of all four | — | Wang & Strong (1996) | **0.840** |

**Why equal weighting?**  
Equal weights are the methodologically conservative choice — they avoid introducing domain-specific assumptions about which dimension matters most. The framework explicitly supports custom weighting via PCA or expert elicitation (see Future Work).

---

<!-- ═══════════════════════════════════════════════════════════════
     DATASET & CLEANING PIPELINE
═══════════════════════════════════════════════════════════════ -->

## 🗂️ Dataset & Cleaning Pipeline

**UCI Air Quality** — De Vito (2016) · [UCI ML Repository](https://doi.org/10.24432/C59K5F)

9,358 hourly observations from an Italian city monitoring station (March 2004 – February 2005) — 5 electrochemical gas sensors paired with certified reference analyzers, plus temperature and humidity.

```
Step 1 — Raw ingestion          Step 2 — Sentinel fix           Step 3 — Column drop
────────────────────            ─────────────────────           ────────────────────
9,358 rows × 15 cols    ──►    -200 → NaN for 200 rows  ──►   NMHC(GT) dropped
                               per official UCI docs           (>90% values missing)

Step 4 — Final corpus
──────────────────────
9,357 rows × 12 cols
ready for DQ analysis
```

**Why drop NMHC(GT)?** The 90%+ missingness rate means no imputation strategy is valid — retaining it would contaminate completeness scores across the entire dataset. Documented as a cleaning decision, not silently removed.

**Key variables retained:**

| Sensor | Measures | Type |
|--------|---------|------|
| CO(GT) | Carbon monoxide — certified reference | Ground truth |
| PT08.S1(CO) | CO proxy — tin oxide sensor | Electrochemical |
| NOx(GT), NO2(GT) | Nitrogen oxides — certified reference | Ground truth |
| PT08.S3(NOx) | NOx proxy | Electrochemical |
| C6H6(GT) | Benzene — certified reference | Ground truth |
| PT08.S2(NMHC), PT08.S4(NO2), PT08.S5(O3) | Chemical proxies | Electrochemical |
| T, RH, AH | Temperature, relative & absolute humidity | Environmental |

---

<!-- ═══════════════════════════════════════════════════════════════
     HYPOTHESES & RESULTS
═══════════════════════════════════════════════════════════════ -->

## 🔬 Hypotheses & Results

Four hypotheses were pre-registered and tested with appropriate statistical methods. Three were confirmed.

```
╔═══════╦══════════════════════════════════════════════╦══════════════════╦══════════╗
║  H    ║  Hypothesis                                  ║  Method          ║  Result  ║
╠═══════╬══════════════════════════════════════════════╬══════════════════╬══════════╣
║  H₁   ║  Completeness scores correlate with usability║  Pearson r       ║  ✅ YES  ║
║       ║  — higher completeness = more usable data    ║  r = 0.998       ║          ║
║       ║                                              ║  p < 0.001       ║          ║
╠═══════╬══════════════════════════════════════════════╬══════════════════╬══════════╣
║  H₂   ║  Higher consistency → fewer measured errors  ║  Welch t-test    ║  ❌ NO   ║
║       ║  in sensor readings                          ║  p = 0.517       ║          ║
╠═══════╬══════════════════════════════════════════════╬══════════════════╬══════════╣
║  H₃   ║  DQI dimensions share internal reliability   ║  Cronbach's α    ║  ℹ️ N/A  ║
║       ║  (i.e., measure one latent construct)        ║  Low α — expected║          ║
╠═══════╬══════════════════════════════════════════════╬══════════════════╬══════════╣
║  H₄   ║  Temporal drift is present in sensor data    ║  KS two-sample   ║  ✅ YES  ║
║       ║  across the recording period                 ║  7/8 cols sig.   ║          ║
╚═══════╩══════════════════════════════════════════════╩══════════════════╩══════════╝
```

**Reading the results honestly:**

**H₁ (r = 0.998)** — Near-perfect correlation between completeness score and variable usability. The framework's completeness dimension is a valid proxy for practical data usability.

**H₂ (p = 0.517) — Not supported, and that's the point.** More complete columns contained fewer extreme outliers — but no significant difference in error *rates* was found between high- and low-consistency groups. This tells us something important: **DQ scores measure data structure, not sensor calibration quality.** A dataset can be structurally consistent and still produce bad sensor readings. These are different problems.

**H₃ (Low Cronbach's α) — Expected, not a failure.** Cronbach's α measures whether multiple items tap a single latent construct. Our four dimensions are *deliberately* measuring different things — completeness and timeliness are orthogonal by design. Low α confirms the framework is multidimensional, not internally incoherent.

**H₄ (7/8 columns significant)** — Temporal drift is real and pervasive. Seasonal variation, sensor degradation, and recording gaps all leave detectable fingerprints in the data distribution. A static DQ snapshot is insufficient; continuous monitoring is required.

---

<!-- ═══════════════════════════════════════════════════════════════
     KEY ANALYTICAL FINDINGS
═══════════════════════════════════════════════════════════════ -->

## 📈 Key Analytical Findings

### Sensor Cross-Correlations

The correlation structure (fig_04) reveals three distinct signal clusters — critical for understanding what these sensors are actually measuring:

| Pair | r | Interpretation |
|------|---|---------------|
| CO ↔ Benzene | **0.93** | Co-emitted by combustion — same source, same time |
| CO ↔ PT08.S1(CO) | **0.88** | Proxy sensor tracks certified reference closely |
| PT08.S3(NOx) ↔ pollutants | **Negative** | Sensor inversion — higher readings = lower pollution |
| Temperature ↔ Humidity | **−0.58** | Classic inverse relationship; seasonal confound |

**The NOx proxy inversion** (PT08.S3 negatively correlated with all pollutants) is not an error — it is how the tin oxide sensor chemistry works. It must be recoded before any predictive modelling.

### Regression: DQI → Sensor Error

DQ composite scores predict sensor measurement error with **R² = 0.013**.

That number is not a failure of the framework — it is confirmation that **data quality and measurement quality are different constructs**. The DQI captures whether data is structurally complete, consistent, and timely. Whether a sensor is *accurately calibrated* is a hardware problem, not a data problem. Using one to predict the other would be a conceptual error.

### Timeliness: The Only Failing Dimension

Score of **0.626** — below the 0.70 acceptability threshold — driven by two factors:
- **Dataset age:** The data is from 2004–2005. The exponential decay function `e^(−0.01 × age_days)` reflects this correctly.
- **Temporal recording gaps:** Periods of missing hourly observations within the recording window further reduce timeliness.

This is not a data cleaning failure. It is the timeliness dimension working correctly: flagging that a 20-year-old dataset should not be used for real-time environmental monitoring without freshness consideration.

---

<!-- ═══════════════════════════════════════════════════════════════
     FIGURES
═══════════════════════════════════════════════════════════════ -->

## 🖼️ Analysis Figures

All figures are generated reproducibly within the notebook. Each one addresses a specific analytical question.

| # | Figure | What It Shows |
|---|--------|--------------|
| 01 | `fig_01_missing_data.png` | Missing values by column + temporal heatmap — reveals *where* and *when* data is absent |
| 02 | `fig_02_dq_scores.png` | Four dimension scores plotted against the 0.7 threshold — the core result |
| 03 | `fig_03_distributions.png` | Completeness distribution + normalised boxplots per variable |
| 04 | `fig_04_correlation.png` | Pearson correlation matrix (lower triangle) — sensor cross-signal structure |
| 05 | `fig_05_h1.png` | H₁: Completeness vs usability scatter — r = 0.998 |
| 06 | `fig_06_h2.png` | H₂: Error rate distributions — high vs low consistency groups |
| 07 | `fig_07_h4_drift.png` | H₄: KS drift — CDF + density overlay for NOx |
| 08 | `fig_08_dqi.png` | Composite DQI distribution + 5-fold cross-validation bars |
| 09 | `fig_09_blue.png` | BLUE assumption checks — residuals, normality, homoscedasticity |
| 10 | `fig_10_regression.png` | Actual vs predicted sensor error + 5-fold CV results |
| 11 | `fig_11_validity.png` | Discriminant validity heatmap — dimension independence confirmed |

> All figures are saved at 300 DPI. Run the notebook end-to-end to regenerate them in your environment.

---

<!-- ═══════════════════════════════════════════════════════════════
     TECH STACK
═══════════════════════════════════════════════════════════════ -->

## 🛠️ Tech Stack

<div align="center">

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)](https://numpy.org)
[![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)](https://scipy.org)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)](https://scikit-learn.org)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white)](https://matplotlib.org)
[![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://seaborn.pydata.org)
[![Statsmodels](https://img.shields.io/badge/Statsmodels-3f3f3f?style=for-the-badge&logo=python&logoColor=white)](https://statsmodels.org)
[![Pingouin](https://img.shields.io/badge/Pingouin-Statistics-5c6bc0?style=for-the-badge&logo=python&logoColor=white)](https://pingouin-stats.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://kaggle.com)

</div>

---

<!-- ═══════════════════════════════════════════════════════════════
     PROJECT STRUCTURE
═══════════════════════════════════════════════════════════════ -->

## 🗂️ Repository Structure

```
quantifying-data-quality/
│
├── Data_Quality_Analysis.ipynb      ← Full analysis notebook (54 cells)
│                                      Sections: EDA → DQ Scoring → Hypotheses
│                                      → Regression → Validity → Interpretation
│
├── AirQualityUCI.csv                ← UCI Air Quality dataset (primary)
├── AirQualityUCI.xlsx               ← Excel version (identical data)
│
├── requirements.txt                 ← Pinned dependencies for full reproducibility
│
├── figures/
│   ├── fig_01_missing_data.png      ← Missing values analysis
│   ├── fig_02_dq_scores.png         ← Four dimension scores vs threshold
│   ├── fig_03_distributions.png     ← Completeness + boxplots
│   ├── fig_04_correlation.png       ← Pearson correlation matrix
│   ├── fig_05_h1.png                ← H₁: Completeness ↔ usability
│   ├── fig_06_h2.png                ← H₂: Error distributions
│   ├── fig_07_h4_drift.png          ← H₄: KS temporal drift
│   ├── fig_08_dqi.png               ← Composite DQI + CV bars
│   ├── fig_09_blue.png              ← BLUE assumption plots
│   ├── fig_10_regression.png        ← Actual vs predicted
│   └── fig_11_validity.png          ← Discriminant validity
│
└── README.md                        ← This file
```

---

<!-- ═══════════════════════════════════════════════════════════════
     REPRODUCE
═══════════════════════════════════════════════════════════════ -->

## ▶️ Reproduce the Analysis

**Option A — Run locally:**

```bash
# 1. Clone the repository
git clone https://github.com/nikhilvarmakandula/quantifying-data-quality.git
cd quantifying-data-quality

# 2. Install dependencies (Python 3.10+ required)
pip install pandas numpy matplotlib seaborn scipy pingouin statsmodels scikit-learn jupyter

# 3. Launch the notebook
jupyter notebook Data_Quality_Analysis.ipynb
```

**Option B — Run on Kaggle (no setup required):**

[![Open in Kaggle](https://img.shields.io/badge/Open_in_Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)

> The Kaggle notebook is pre-configured with all dependencies. Click "Copy & Edit" to run your own version.

**What to expect when you run it:**

The notebook runs end-to-end in approximately 3–5 minutes. All 11 figures are regenerated and saved to the local directory. Cell outputs match the results documented in this README. The only external dependency is the UCI Air Quality dataset — included in the repository.

---

<!-- ═══════════════════════════════════════════════════════════════
     FUTURE WORK
═══════════════════════════════════════════════════════════════ -->

## 🔭 Future Work

Four extensions would meaningfully advance this framework — ordered by research impact:

| Priority | Extension | Why It Matters |
|:--------:|-----------|---------------|
| 🔴 **High** | Cross-domain validation — healthcare, finance, climate datasets | A framework that only works on one dataset is not a framework |
| 🔴 **High** | ML-based dimension weighting (PCA or expert elicitation) vs equal weights | Equal weighting is conservative, not optimal — the right weights are domain-specific |
| 🟡 **Medium** | Real-time streaming pipeline with automated quality alerts | H₄ confirms drift exists; the logical response is continuous monitoring, not point-in-time scoring |
| 🟢 **Exploratory** | Random Forest / XGBoost for sensor error prediction | R² = 0.013 from DQ scores alone — feature engineering or sensor metadata may close the gap |

---

<!-- ═══════════════════════════════════════════════════════════════
     REFERENCES
═══════════════════════════════════════════════════════════════ -->

## 📚 References

<details>
<summary><strong>Click to expand full bibliography (8 sources)</strong></summary>

<br/>

**Batini, C., & Scannapieco, M.** (2016). *Data and information quality: Dimensions, principles and techniques*. Springer. — Foundation for Consistency and Timeliness dimension formulas.

**Cronbach, L. J.** (1951). Coefficient alpha and the internal structure of tests. *Psychometrika*, *16*(3), 297–334. — H₃ internal reliability test.

**De Vito, S.** (2016). *Air quality dataset*. UCI Machine Learning Repository. https://doi.org/10.24432/C59K5F — Primary dataset. Italian urban monitoring station, 2004–2005.

**Field, A.** (2018). *Discovering statistics using IBM SPSS statistics* (5th ed.). Sage Publications. — Statistical methodology reference for Welch t-test and Pearson correlation.

**Heinrich, B., Hristova, D., Klier, M., Schiller, A., & Szubartowicz, M.** (2018). Requirements for data quality metrics. *Journal of Data and Information Quality*, *9*(2), 1–32. — Accuracy dimension formula and normalisation approach.

**Massey, F. J.** (1951). The Kolmogorov-Smirnov test for goodness of fit. *Journal of the American Statistical Association*, *46*(253), 68–78. — H₄ temporal drift test.

**Pipino, L. L., Lee, Y. W., & Wang, R. Y.** (2002). Data quality assessment. *Communications of the ACM*, *45*(4), 211–218. — Completeness dimension formula.

**Wang, R. Y., & Strong, D. M.** (1996). Beyond accuracy: What data quality means to data consumers. *Journal of Management Information Systems*, *12*(4), 5–33. — Foundational DQ taxonomy; motivation for the four-dimension selection.

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     RELATED PROJECTS
═══════════════════════════════════════════════════════════════ -->

## 🔗 Related Projects

This project is part of a broader portfolio of data work — each one demonstrates a different analytical method at a different scale.

| Project | Core Method | Scale | Tools |
|---------|------------|-------|-------|
| 📡 [Rainfall Estimation via Heterogeneous Data Fusion](https://www.irjmets.com/paperdetail.php?paperId=b3d0de1ee3008bdcbdae1ccb72560041) | Ensemble ML · HPEC · Random Forest | Multi-source sensors | Python · scikit-learn |
| 🧠 [Skill Demand in German Tech Market — NLP Corpus](https://github.com/nikhilvarmakandula/skill-demand-german-tech-market) | TF-IDF + spaCy NER · K-Means (k=4) | 3,200 job postings · 156 skills | Python · spaCy · Pandas |
| 🚲 [Cyclistic Bike-Share — Member Conversion Analysis](https://github.com/nikhilvarmakandula/cyclistic-bikeshare-case-study) | SQL · Descriptive analytics · Tableau | 5,535,455 rides · 12 months | BigQuery · GCS · Tableau |

---

<!-- ═══════════════════════════════════════════════════════════════
     AUTHOR
═══════════════════════════════════════════════════════════════ -->

## 👤 Author

**Nikhilvarma Kandula**  
M.Sc. Big Data & Business Analytics · FOM University of Applied Sciences, Essen  
Data Analyst & Engineer · 1.5+ years Fintech · Peer-reviewed publication · Google Certified

<div align="center">

[![Portfolio](https://img.shields.io/badge/🌐_Portfolio-kandula.studio-0a0a0a?style=for-the-badge&logoColor=white)](https://kandula.studio)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nikhilvarmakandula-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nikhilvarmakandula)
[![Email](https://img.shields.io/badge/Email-kandulanikhilvarma@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kandulanikhilvarma@gmail.com)
[![Kaggle](https://img.shields.io/badge/Kaggle-nikhilvarmakandula-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)
[![Google Cert](https://img.shields.io/badge/Google_Data_Analytics-Certified-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://www.credly.com/badges/JO1A2NXM2RU9)

</div>

---

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER
═══════════════════════════════════════════════════════════════ -->

<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a237e,100:0d1117&height=120&section=footer&text=Data%20quality%20is%20not%20a%20property%20of%20datasets.%20It%E2%80%99s%20a%20property%20of%20decisions.&fontSize=13&fontColor=9fa8da&fontAlignY=65&animation=fadeIn" />

</div>
