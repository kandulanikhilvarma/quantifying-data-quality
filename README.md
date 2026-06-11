<!-- ═══════════════════════════════════════════════════════════════
     HEADER BANNER
═══════════════════════════════════════════════════════════════ -->
<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a237e,100:0d1117&height=220&section=header&text=Quantifying%20Data%20Quality&fontSize=42&fontColor=ffffff&fontAlignY=55&desc=A%20Statistical%20Framework%20for%20Scientific%20Datasets&descAlignY=75&animation=twinkling" alt="Header banner with project title"/>

<!-- ═══════════════════════════════════════════════════════════════
     BADGE ROW & QUICK STATS
═══════════════════════════════════════════════════════════════ -->
<div align="center">

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![Kaggle](https://img.shields.io/badge/Kaggle-Open_Notebook-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Published-f59e0b?style=for-the-badge)]()

<br/>

**📊 Dataset:** UCI Air Quality · 9,357 hourly observations · March 2004 – February 2005  
**📁 Repository:** 4.4 MB · 54-cell Jupyter notebook · 11 publication-ready figures · Full reproducibility

</div>

---

## 📋 Table of Contents

- [🎯 Overview](#-the-problem)
- [📊 Data Quality Index](#-data-quality-index--at-a-glance)
- [🧱 Framework](#-the-framework--four-dimensions-one-score)
- [🔬 Hypotheses & Results](#-hypotheses--results)
- [📈 Key Findings](#-key-analytical-findings)
- [🛠️ Tech Stack](#-tech-stack)
- [🗂️ Repository Structure](#-repository-structure)
- [▶️ Getting Started](#-get-started-in-3-steps)
- [🔭 Future Work](#-future-work)
- [📚 References](#-references)
- [🔗 Related Projects](#-related-projects)
- [👤 Author](#-author)

---

<!-- ═══════════════════════════════════════════════════════════════
     COMPOSITE SCORE DASHBOARD — THE HEADLINE NUMBER
═══════════════════════════════════════════════════════════════ -->

## 📊 Data Quality Index — At a Glance

<div align="center">

```
╔═══════════════════════════════════════════════════════════════════╗
║                    COMPOSITE DATA QUALITY INDEX (DQI)             ║
║                                                                   ║
║                          ★  0.840 / 1.000  ★                     ║
║                    ████████████████████░░░░  84%                  ║
║                                                                   ║
╠═══════════════╦═══════════════╦═══════════════╦═════════════════╣
║ Completeness  ║ Consistency   ║ Accuracy      ║ Timeliness      ║
║   0.930 ✅    ║   0.978 ✅    ║   0.824 ✅    ║   0.626 ❌      ║
║ ██████████░   ║ █████████████ ║ ████████████░ ║ ████████░░░░░░  ║
║ Exceeds 0.7   ║ Exceeds 0.7   ║ Exceeds 0.7   ║ Below threshold ║
╚═══════════════╩═══════════════╩═══════════════╩═════════════════╝

Threshold line: 0.700 — three of four dimensions pass. One fails: time.
The dataset is structurally sound. Its freshness is not.
```

</div>

**What this means:** The dataset achieves strong scores on **completeness** (93%), **consistency** (98%), and **accuracy** (82%) — making it reliable for structured analysis. However, the **timeliness** score (63%) reflects that this is a 20-year-old dataset; it's unsuitable for real-time environmental monitoring without freshness adjustments.

---

<!-- ═══════════════════════════════════════════════════════════════
     THE PROBLEM
═══════════════════════════════════════════════════════════════ -->

## 🎯 The Problem

Scientific datasets drive critical decisions in healthcare, climate science, and environmental monitoring. Yet **no standardised framework exists** for quantifying data quality across multiple dimensions in a reproducible, theoretically-grounded way.

**The challenge:**
- ❌ Existing approaches are either purely theoretical (no implementation) or too domain-specific to generalise  
- ❌ Researchers lack a common language for communicating data quality  
- ❌ Monitoring frameworks don't exist for detecting drift over time  
- ❌ No easy way to score, compare, or track multiple datasets systematically  

**Our research question:**
> *How can data quality be quantified, scored, and monitored using a reproducible, theoretically-grounded statistical framework?*

**Our answer:** A four-dimension scoring system, grounded in peer-reviewed measurement theory, validated with real sensor data, and packaged as a fully reproducible Python/Jupyter pipeline.

---

<!-- ═══════════════════════════════════════════════════════════════
     FRAMEWORK
═══════════════════════════════════════════════════════════════ -->

## 🧱 The Framework — Four Dimensions, One Score

Each dimension is grounded in published measurement theory and produces a normalised [0–1] score. Equal weighting produces the composite **Data Quality Index (DQI)**.

<div align="center">

| Dimension | What It Measures | Formula | Theory | Score |
|-----------|-----------------|---------|--------|:-----:|
| **Completeness** | Are all expected values present? | `1 − (missing / total)` | Pipino et al. (2002) | **0.930** ✅ |
| **Consistency** | Do values conform to defined rules? | `1 − (violations / checks)` | Batini & Scannapieco (2016) | **0.978** ✅ |
| **Accuracy** | How close are values to the truth? | `1 − (MAE / range)`, normalised | Heinrich et al. (2018) | **0.824** ✅ |
| **Timeliness** | How fresh is the data? | `e^(−0.01 × age_days)` | Batini & Scannapieco (2016) | **0.626** ❌ |
| **Composite DQI** | Equal-weighted average of all four | `(C+Co+A+T) / 4` | Wang & Strong (1996) | **0.840** |

</div>

### Why Equal Weighting?

Equal weights are the methodologically conservative choice — they avoid introducing domain-specific assumptions about which dimension matters most. The framework explicitly supports custom weighting via domain expert elicitation or principal component analysis (see [Future Work](#-future-work)).

<details>
<summary><b>📖 Want to customize the weights?</b> (Click to expand)</summary>

The framework is designed for flexibility. To apply custom weights:

```python
# Example: Prioritize completeness & consistency (domain-specific)
custom_weights = {
    'Completeness': 0.4,   # Up from 0.25
    'Consistency': 0.4,    # Up from 0.25
    'Accuracy': 0.15,      # Down from 0.25
    'Timeliness': 0.05     # Down from 0.25
}

dqi_custom = sum(score * weight for score, weight in zip(scores, custom_weights.values()))
```

This approach is documented in the notebook under **"Composite Scoring — Sensitivity Analysis"** section.

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     DATASET & CLEANING PIPELINE
═══════════════════════════════════════════════════════════════ -->

## 🗂️ Dataset & Cleaning Pipeline

**UCI Air Quality** — De Vito (2016)  
📌 [UCI ML Repository](https://doi.org/10.24432/C59K5F)

9,358 hourly observations from an Italian city monitoring station (March 2004 – February 2005) — 5 electrochemical gas sensors paired with certified reference analysers, plus temperature and humidity measurements.

### Data Preparation Steps

```
Step 1 — Raw ingestion     Step 2 — Sentinel fix      Step 3 — Column drop    Step 4 — Final corpus
────────────────────────   ─────────────────────      ────────────────────    ──────────────────────
9,358 rows × 15 cols  ──►  -200 → NaN for            NMHC(GT) dropped       9,357 rows × 12 cols
                           200 rows per UCI docs   (>90% values missing)     Ready for DQ analysis
```

<details>
<summary><b>🔧 Data Cleaning Rationale</b> (Click to expand)</summary>

**Sentinel Value Handling:**  
The UCI dataset uses `-200` as a "no measurement" sentinel value. Per official documentation, these represent invalid/missing readings. We replaced them with `NaN` for proper statistical handling.

**NMHC(GT) Removal:**  
The NMHC(GT) column exhibits >90% missingness. No imputation strategy is defensible at this level of missingness — retaining it would contaminate completeness scores across the entire dataset. This decision is documented as a cleaning code in the notebook.

**Key variables retained:**

| Sensor | Measures | Type | Role |
|--------|----------|------|------|
| CO(GT), NOx(GT), C6H6(GT) | Carbon monoxide, nitrogen oxides, benzene | Ground truth | Reference standard |
| PT08.S1(CO), PT08.S3(NOx), etc. | Chemical proxy readings | Electrochemical | Proxy sensor |
| T, RH, AH | Temperature, relative & absolute humidity | Environmental | Control variables |

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     HYPOTHESES & RESULTS
═══════════════════════════════════════════════════════════════ -->

## 🔬 Hypotheses & Results

Four hypotheses were pre-registered and tested with appropriate statistical methods. Three were confirmed; one was not — and both results are meaningful.

<div align="center">

| H | Hypothesis | Method | Result | p-value |
|---|-----------|--------|--------|---------|
| **H₁** | Completeness ↔ usability (r = 0.998) | Pearson r | ✅ **YES** | p < 0.001 |
| **H₂** | Higher consistency → fewer errors | Welch t-test | ❌ **NO** | p = 0.517 |
| **H₃** | Dimensions measure one construct (α = ?) | Cronbach's α | ℹ️ **N/A** | α low (expected) |
| **H₄** | Temporal drift present in sensor data | KS two-sample | ✅ **YES** | 7/8 cols sig. |

</div>

### What Each Result Means

<details>
<summary><b>📊 H₁: Completeness & Usability (CONFIRMED)</b></summary>

**Finding:** r = 0.998, p < 0.001 — near-perfect correlation  
**Interpretation:** The framework's completeness dimension is a valid and powerful proxy for practical data usability. Columns with fewer missing values are reliably more usable for analysis and modeling.  
**Implication:** Prioritizing completeness is a sound data quality strategy.

</details>

<details>
<summary><b>📊 H₂: Consistency & Error Rates (NOT CONFIRMED)</b></summary>

**Finding:** p = 0.517 — no significant difference in error rates between high- and low-consistency groups  
**Interpretation:** More complete columns contained fewer extreme outliers, but no significant difference in measurement *error rates* was detected. This is not a failure of the framework — it's a critical insight.  
**Implication:** **Data quality and measurement quality are different constructs.** The DQI captures whether data is structurally sound; it doesn't guarantee that the underlying sensors are accurate. Consistency checking is essential for flagging anomalies, but it won't detect systematic sensor drift.

</details>

<details>
<summary><b>📊 H₃: Internal Reliability (EXPECTED RESULT)</b></summary>

**Finding:** Cronbach's α is low — not a failure  
**Interpretation:** Cronbach's α measures whether multiple items tap a single latent construct. Our four dimensions are *deliberately* measuring different things (completeness ≠ consistency ≠ accuracy ≠ timeliness). A high α would suggest redundancy, not validity.  
**Implication:** Low α confirms the framework measures **discriminant validity** — each dimension contributes unique information.

</details>

<details>
<summary><b>📊 H₄: Temporal Drift (CONFIRMED)</b></summary>

**Finding:** 7 out of 8 columns show significant distribution shifts (Kolmogorov-Smirnov test)  
**Interpretation:** Seasonal variation, sensor degradation, and recording gaps leave detectable fingerprints in the data. Drift is real and pervasive.  
**Implication:** Single-point-in-time quality scoring is insufficient. Continuous monitoring frameworks are needed.

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     KEY ANALYTICAL FINDINGS
═══════════════════════════════════════════════════════════════ -->

## 📈 Key Analytical Findings

### Sensor Cross-Correlations

The correlation structure (see `fig_04_correlation.png`) reveals three distinct signal clusters — critical for understanding what these sensors are measuring:

| Pair | r | Interpretation |
|------|---|---|
| CO ↔ Benzene | **0.93** | Co-emitted by combustion — same source, same time |
| CO ↔ PT08.S1(CO) | **0.88** | Proxy sensor tracks certified reference closely |
| PT08.S3(NOx) ↔ pollutants | **Negative** | Sensor inversion — higher readings = lower pollution |
| Temperature ↔ Humidity | **−0.58** | Inverse seasonal relationship; temperature confound |

**Critical Insight:** The NOx proxy inversion (PT08.S3 negatively correlated with all pollutants) is not an error — it's how the tin oxide sensor chemistry works. It must be recoded before any predictive modelling.

### Regression: DQI → Sensor Error

**Finding:** DQ composite scores predict sensor measurement error with **R² = 0.013** (1.3% variance explained)

This number is **not** a failure of the framework — it is **confirmation that data quality and measurement quality are different constructs.** The DQI captures whether data is structurally complete and consistent; it says nothing about whether the underlying sensors are correct. This is by design.

### Timeliness: The Only Failing Dimension

**Score:** 0.626 (below the 0.70 acceptability threshold)

**Drivers:**
- **Dataset age:** The data is from 2004–2005. The exponential decay function `e^(−0.01 × age_days)` reflects this correctly.
- **Temporal recording gaps:** Periods of missing hourly observations within the recording window further reduce timeliness.

**Key message:** This is not a data cleaning failure. It is the timeliness dimension working correctly — flagging that a 20-year-old dataset should not be used for real-time environmental monitoring without freshness adjustments. The framework is doing its job.

---

<!-- ═══════════════════════════════════════════════════════════════
     FIGURES & VISUAL ASSETS
═══════════════════════════════════════════════════════════════ -->

## 🖼️ Analysis Figures — Complete Visual Asset Map

All figures are generated reproducibly within the notebook. Each one answers a specific analytical question and is saved at **300 DPI** for publication quality.

### Figure Reference Table

| # | Figure | What It Shows | Purpose | Alt Text |
|---|--------|--------------|---------|----------|
| **01** | `fig_01_missing_data.png` | Missing values by column + temporal heatmap | Identify where/when data is absent | Heatmap showing missing data patterns across 12 variables over 12 months |
| **02** | `fig_02_dq_scores.png` | Four dimension scores plotted against 0.7 threshold | Core result visualization | Bar chart comparing four data quality dimension scores to acceptability threshold |
| **03** | `fig_03_distributions.png` | Completeness distribution + normalised boxplots per variable | Variability in data quality across columns | Histogram of completeness scores and boxplots for all 12 variables |
| **04** | `fig_04_correlation.png` | Pearson correlation matrix (lower triangle) | Sensor cross-signal relationships | Lower triangular correlation heatmap showing relationships between all variables |
| **05** | `fig_05_h1.png` | Completeness vs usability scatter (r = 0.998) | Validate H₁ hypothesis | Scatter plot with trend line showing near-perfect correlation between completeness and usability |
| **06** | `fig_06_h2.png` | Error rate distributions (high vs low consistency) | Test H₂ hypothesis | Side-by-side density plots comparing error rates for high and low consistency groups |
| **07** | `fig_07_h4_drift.png` | KS drift test — CDF + density overlay for NOx | Visualize temporal drift (H₄) | Cumulative distribution plots showing seasonal distribution shift in NOx measurements |
| **08** | `fig_08_dqi.png` | Composite DQI distribution + 5-fold cross-validation bars | Model stability assessment | Histogram of DQI scores with overlaid cross-validation confidence intervals |
| **09** | `fig_09_blue.png` | BLUE assumption checks — residuals, normality, homoscedasticity | Regression diagnostics | 2×2 grid: residuals plot, Q-Q plot, scale-location, and residuals vs fitted |
| **10** | `fig_10_regression.png` | Actual vs predicted sensor error + 5-fold CV results | Regression performance | Scatter plot of actual vs predicted values with cross-validation fold indicators |
| **11** | `fig_11_validity.png` | Discriminant validity heatmap — dimension independence | Confirm dimensions are independent | Correlation matrix heatmap showing low inter-dimension correlations |

### Visual Asset Connections

```
┌─────────────────────────────────────────────────────────────────┐
│ README.md — Data Quality Analysis Documentation                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ ┌──────────────────────────────────────────────────────────┐  │
│ │ Data Quality Index Section                               │  │
│ │ └─→ fig_02_dq_scores.png (main result viz)              │  │
│ │ └─→ fig_01_missing_data.png (data prep context)         │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│ ┌──────────────────────────────────────────────────────────┐  │
│ │ Analytical Findings Section                              │  │
│ │ └─→ fig_04_correlation.png (sensor relationships)        │  │
│ │ └─→ fig_07_h4_drift.png (temporal analysis)             │  │
│ │ └─→ fig_03_distributions.png (variability)              │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│ ┌──────────────────────────────────────────────────────────┐  │
│ │ Hypothesis Testing Section                               │  │
│ │ └─→ fig_05_h1.png (H₁ results)                          │  │
│ │ └─→ fig_06_h2.png (H₂ results)                          │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│ ┌──────────────────────────────────────────────────────────┐  │
│ │ Notebook (Data_Quality_Analysis.ipynb) — Full Analysis  │  │
│ │ └─→ Generates all 11 figures at 300 DPI                 │  │
│ │ └─→ 54 cells: EDA → Scoring → Hypotheses → Regression   │  │
│ └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary><b>🖼️ Where to Find Each Figure</b></summary>

All figures are stored in the repository root:

- **Exploratory figures:** `fig_01_missing_data.png`, `fig_03_distributions.png`, `fig_04_correlation.png`
- **Core results:** `fig_02_dq_scores.png`, `fig_08_dqi.png`
- **Hypothesis testing:** `fig_05_h1.png`, `fig_06_h2.png`, `fig_07_h4_drift.png`
- **Statistical validation:** `fig_09_blue.png`, `fig_10_regression.png`, `fig_11_validity.png`

Run the notebook to regenerate all figures in your environment.

</details>

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

**Core Libraries:**

| Library | Version | Purpose |
|---------|---------|---------|
| **pandas** | Latest | Data manipulation, missing value handling |
| **numpy** | Latest | Numerical operations, array processing |
| **scipy** | Latest | Statistical tests (Welch t-test, KS test, Pearson r) |
| **scikit-learn** | Latest | Regression, cross-validation, model evaluation |
| **matplotlib** | Latest | Core visualization engine (300 DPI outputs) |
| **seaborn** | Latest | Statistical plotting, correlation matrices, distributions |
| **statsmodels** | Latest | OLS regression, BLUE assumptions checking, diagnostic tests |
| **pingouin** | Latest | Advanced statistics (effect sizes, post-hoc tests) |

---

<!-- ═══════════════════════════════════════════════════════════════
     PROJECT STRUCTURE
═══════════════════════════════════════════════════════════════ -->

## 🗂️ Repository Structure

```
quantifying-data-quality/
│
├── README.md                        ← This file
├── LICENSE                          ← MIT License
├── requirements.txt                 ← Pinned dependencies (Python 3.10+)
│
├── Data_Quality_Analysis.ipynb      ← Main analysis notebook (54 cells)
│                                      ├─ Section 1: EDA & data loading
│                                      ├─ Section 2: DQ scoring computation
│                                      ├─ Section 3: Hypothesis testing
│                                      ├─ Section 4: Regression analysis
│                                      ├─ Section 5: Validity checks
│                                      └─ Section 6: Interpretation & discussion
│
├── AirQualityUCI.csv                ← Primary dataset (9,357 rows × 12 cols)
├── AirQualityUCI.xlsx               ← Excel version (identical data)
│
└── figures/                         ← All analysis figures (300 DPI)
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

<!-- ═══════════════════════════════════════════════════════════════
     GET STARTED IN 3 STEPS
═══════════════════════════════════════════════════════════════ -->

## ▶️ Get Started in 3 Steps

### Option A — Run Locally (5 minutes)

```bash
# Step 1: Clone the repository
git clone https://github.com/kandulanikhilvarma/quantifying-data-quality.git
cd quantifying-data-quality

# Step 2: Install dependencies (Python 3.10+ required)
pip install -r requirements.txt

# Step 3: Launch the notebook
jupyter notebook Data_Quality_Analysis.ipynb
```

### Option B — Run on Kaggle (No Setup Required)

<div align="center">

[![Open in Kaggle](https://img.shields.io/badge/Open_in_Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)

</div>

The Kaggle notebook is pre-configured with all dependencies. Click **"Copy & Edit"** to run your own version.

### What to Expect

✅ Notebook runs end-to-end in **3–5 minutes**  
✅ All **11 figures** regenerated and saved locally  
✅ Cell outputs **match results** documented here  
✅ Console output shows **computation progress**  
✅ No external data downloads needed — all data is in the repo  

---

<!-- ═══════════════════════════════════════════════════════════════
     INSTALLATION & ADVANCED SETUP
═══════════════════════════════════════════════════════════════ -->

<details>
<summary><b>🔧 Advanced Installation & Setup</b></summary>

### System Requirements

- **Python:** 3.10+ (3.12 recommended for performance)
- **Memory:** 2GB minimum (4GB recommended for large datasets)
- **Disk space:** ~500MB for full environment

### Virtual Environment Setup

```bash
# Create isolated environment
python3 -m venv dq-env
source dq-env/bin/activate  # On Windows: dq-env\Scripts\activate

# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r requirements.txt
```

### Troubleshooting

**Issue: `ModuleNotFoundError: No module named 'statsmodels'`**
```bash
pip install --upgrade statsmodels scipy
```

**Issue: Jupyter kernel not found**
```bash
pip install ipykernel
python -m ipykernel install --user --name dq-env
```

**Issue: Figures not rendering**
```bash
# Ensure matplotlib is properly configured
pip install --upgrade matplotlib
# Then restart Jupyter kernel
```

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     INTERPRETATION GUIDE
═══════════════════════════════════════════════════════════════ -->

<details>
<summary><b>📖 How to Interpret the Results</b></summary>

### Understanding DQI Scores

| Score Range | Interpretation | Recommendation |
|-------------|---|---|
| **0.90–1.00** | Exceptional data quality | Safe for any analysis; minimal cleaning required |
| **0.80–0.89** | Good data quality | Suitable for most purposes; document any caveats |
| **0.70–0.79** | Acceptable quality | Use with caution; apply domain-specific validation |
| **0.50–0.69** | Poor quality | Requires significant preparation; consider alternative data |
| **< 0.50** | Unacceptable quality | Not recommended for analysis without major intervention |

**This dataset:** DQI = 0.840 = "Good quality" with one caveat (timeliness).

### Per-Dimension Interpretation

**Completeness (0.930):** 93% of values are present. The 7% missing are distributed randomly and are unlikely to introduce systematic bias.

**Consistency (0.978):** 98% of values conform to defined rules (e.g., temperature within expected range). The 2% violations are likely data entry errors or sensor malfunctions — investigate them.

**Accuracy (0.824):** 82% of values are within acceptable error bounds when compared to certified reference analysers. This is strong for electrochemical sensors.

**Timeliness (0.626):** The dataset is 20+ years old. For real-time environmental monitoring, this is insufficient. For historical analysis or model training, it's acceptable.

### What It Does NOT Tell You

- ❌ Whether the data is suitable for your specific analysis (domain-specific)
- ❌ Whether sensors are calibrated correctly (requires domain expertise)
- ❌ Whether relationships between variables are causal (requires study design)
- ❌ Whether the data is representative of the broader population (requires sampling knowledge)

**Use the DQI as a diagnostic tool, not a yes/no decision gate.**

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     FUTURE WORK
═══════════════════════════════════════════════════════════════ -->

## 🔭 Future Work

Four extensions would meaningfully advance this framework — ordered by research impact:

| Priority | Extension | Why It Matters | Estimated Effort |
|:--------:|-----------|---|---|
| 🔴 **High** | **Cross-domain validation** — healthcare, finance, climate datasets | A framework that only works on one dataset is not a framework | 4–8 weeks |
| 🔴 **High** | **ML-based dimension weighting** (PCA or expert elicitation) | Equal weighting is conservative, not optimal — the right weights are domain-specific | 2–3 weeks |
| 🟡 **Medium** | **Real-time streaming pipeline** with automated quality alerts | H₄ confirms drift exists; continuous monitoring is the logical response | 6–10 weeks |
| 🟢 **Exploratory** | **Feature engineering for sensor error prediction** | R² = 0.013 from DQ scores alone — metadata or domain features may close the gap | 2–4 weeks |

<details>
<summary><b>💡 Contributing Ideas</b></summary>

Interested in contributing? Here are some specific ideas:

1. **Validation on new datasets:** Healthcare records, financial transactions, climate model outputs — any domain where data quality matters
2. **Interactive dashboard:** Build a Dash/Streamlit app to score new datasets in real-time
3. **Dimension enhancements:** Add coverage, relevance, or validity dimensions
4. **Cloud integration:** Deploy as AWS Lambda / Google Cloud Function for batch processing
5. **Visualization improvements:** 3D projections, interactive correlation networks, animated drift detection

See [Contributing Guidelines](CONTRIBUTING.md) for details (coming soon).

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     REFERENCES
═══════════════════════════════════════════════════════════════ -->

## 📚 References

<details>
<summary><b>📖 Full Bibliography (8 Peer-Reviewed Sources)</b></summary>

<br/>

**Batini, C., & Scannapieco, M.** (2016). *Data and information quality: Dimensions, principles and techniques*. Springer.  
→ Foundation for Consistency and Timeliness dimension formulas.

**Cronbach, L. J.** (1951). Coefficient alpha and the internal structure of tests. *Psychometrika*, *16*(3), 297–334.  
→ H₃ internal reliability test methodology.

**De Vito, S.** (2016). *Air quality dataset*. UCI Machine Learning Repository. https://doi.org/10.24432/C59K5F  
→ Primary dataset. Italian urban monitoring station, March 2004 – February 2005.

**Field, A.** (2018). *Discovering statistics using IBM SPSS statistics* (5th ed.). Sage Publications.  
→ Statistical methodology reference for Welch t-test and Pearson correlation.

**Heinrich, B., Hristova, D., Klier, M., Schiller, A., & Szubartowicz, M.** (2018). Requirements for data quality metrics. *Journal of Data and Information Quality*, *9*(2), 1–32.  
→ Accuracy dimension formula and validation framework.

**Massey, F. J.** (1951). The Kolmogorov-Smirnov test for goodness of fit. *Journal of the American Statistical Association*, *46*(253), 68–78.  
→ H₄ temporal drift detection methodology.

**Pipino, L. L., Lee, Y. W., & Wang, R. Y.** (2002). Data quality assessment. *Communications of the ACM*, *45*(4), 211–218.  
→ Completeness dimension formula and foundational DQ taxonomy.

**Wang, R. Y., & Strong, D. M.** (1996). Beyond accuracy: What data quality means to data consumers. *Journal of Management Information Systems*, *12*(4), 5–33.  
→ Foundational DQ taxonomy; motivates multi-dimensional approach.

</details>

---

<!-- ═══════════════════════════════════════════════════════════════
     RELATED PROJECTS
═══════════════════════════════════════════════════════════════ -->

## 🔗 Related Projects

This project is part of a broader portfolio of data work — each demonstrating a different analytical method at different scales.

| Project | Core Method | Scale | Tools |
|---------|------------|-------|-------|
| 📡 [Rainfall Estimation via Heterogeneous Data Fusion](https://www.irjmets.com/paperdetail.php?paperId=b3d0de1ee3008bdcbdae1ccb72560041) | Ensemble ML · Random Forest · HPEC | Multi-source data streams | Python, scikit-learn, AWS |
| 🧠 [Skill Demand in German Tech Market — NLP Corpus](https://github.com/kandulanikhilvarma/skill-demand-german-tech-market) | TF-IDF + spaCy NER · K-Means clustering | 3,200 job postings · 156 unique skills | Python, spaCy, scikit-learn |
| 🚲 [Cyclistic Bike-Share — Member Conversion Analysis](https://github.com/kandulanikhilvarma/cyclistic-bikeshare-case-study) | SQL · Descriptive analytics · Tableau | 5.5M rides · 12-month period | SQL, Tableau, Python |

---

<!-- ═══════════════════════════════════════════════════════════════
     AUTHOR
═══════════════════════════════════════════════════════════════ -->

## 👤 Author

**Nikhilvarma Kandula**  
📊 Data Analyst & Engineer · 🏦 1.5+ years Fintech · 📰 Peer-reviewed publication · 🏆 Google Certified

<div align="center">

[![Portfolio](https://img.shields.io/badge/🌐_Portfolio-kandula.studio-0a0a0a?style=for-the-badge&logoColor=white)](https://kandula.studio)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nikhilvarmakandula-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nikhilvarmakandula)
[![Email](https://img.shields.io/badge/Email-kandulanikhilvarma@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kandulanikhilvarma@gmail.com)
[![Kaggle](https://img.shields.io/badge/Kaggle-nikhilvarmakandula-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/nikhilvarmakandula)
[![Google Cert](https://img.shields.io/badge/Google_Data_Analytics-Certified-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://www.credly.com/badges/JO1A2NXM2RU9)

</div>

---

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER SECTION
═══════════════════════════════════════════════════════════════ -->

## 📄 License & Citation

**License:** MIT — [View LICENSE](LICENSE)

**How to cite this work:**

```bibtex
@misc{kandula2026dq,
  author = {Kandula, Nikhilvarma},
  title = {Quantifying Data Quality: A Statistical Framework for Scoring and Monitoring Scientific Datasets},
  year = {2026},
  publisher = {GitHub},
  howpublished = {\url{https://github.com/kandulanikhilvarma/quantifying-data-quality}}
}
```

---

<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1a237e,100:0d1117&height=120&section=footer&text=Data%20quality%20is%20not%20a%20property%20of%20datasets.%20It%27s%20a%20contract%20with%20users.&fontSize=16&fontColor=ffffff&fontAlignY=75&animation=twinkling" alt="Footer banner with project philosophy"/>

<br/>

**Built with** 🧪 Science · 📊 Statistics · 💻 Python · 🎨 Care

**Last updated:** June 2026 | **Repository:** `kandulanikhilvarma/quantifying-data-quality`

</div>
