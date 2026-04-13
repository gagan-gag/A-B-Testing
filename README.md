# A/B Test Analysis Engine

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat&logo=jupyter&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-8CAAE6?style=flat&logo=scipy&logoColor=white)
![Statsmodels](https://img.shields.io/badge/Statsmodels-Econometrics-4B8BBE?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

> **A full end-to-end A/B test analysis engine** — simulating how data scientists at product companies measure whether a new feature genuinely improves user behaviour. Built across 7 Jupyter notebooks: from synthetic data generation to automated stakeholder reporting.

---

![Banner](outputs/banner.png)

---

## The Problem

Every product team asks the same question after shipping a feature:

> *"Did it actually work — or did we just get lucky?"*

Answering that incorrectly misleads product decisions, wastes engineering effort, and costs revenue. This project builds a **complete, rigorous scientific framework** to answer that question properly — covering every stage a data scientist handles in a real A/B experiment.

---

## Experiment Scenario

A simulated rollout of a **new Jira-style dashboard feature** to 50% of users (treatment) while the remaining 50% see the existing experience (control).

| Group | Users | Conversion Rate | Avg Session | CTR |
|---|---|---|---|---|
| Control | 2,000 | 11.10% | 369.3s | 7.40% |
| Treatment | 2,000 | 16.55% | 382.3s | 10.70% |
| **Lift** | — | **+5.45%** | **+13.0s** | **+3.30%** |

---

## Notebook Breakdown

| # | Notebook | What It Does |
|---|---|---|
| 1 | `notebook1_data_generation.ipynb` | Generates 4,000 synthetic users with realistic noise, missing values and outliers |
| 2 | `notebook2_eda.ipynb` | EDA — data quality checks, SRM test, normality test, subgroup analysis |
| 3 | `notebook3_frequentist_testing.ipynb` | Z-test, Welch's t-test, Mann-Whitney U, Chi-square + confidence intervals |
| 4 | `notebook4_power_analysis.ipynb` | Pre/post-hoc power analysis, MDE calculator, power curves |
| 5 | `notebook5_bayesian_testing.ipynb` | Beta-Binomial model — P(Treatment > Control), Expected Loss |
| 6 | `notebook6_sequential_testing.ipynb` | Peeking problem demo, SPRT early stopping, Benjamini-Hochberg correction |
| 7 | `notebook7_report_generation.ipynb` | Auto-generates a self-contained HTML report for stakeholders |

---

## Key Results

```
Conversion Rate  : Control 11.10%  →  Treatment 16.55%  (+5.45%)  ✅ Significant
Session Duration : Control 369.3s  →  Treatment 382.3s  (+13.0s)  ✅ Significant
Click-Through    : Control  7.40%  →  Treatment 10.70%  (+3.30%)  ✅ Significant

P(Treatment > Control) [Bayesian] : ~95%+
Decision : SHIP TREATMENT
```

---

## Technical Highlights

### Frequentist Testing (Notebook 3)
- **Two-proportion Z-test** for conversion rate
- **Welch's t-test** (does not assume equal variance) for session duration
- **Mann-Whitney U** as a non-parametric robustness check
- **Chi-square** for click-through rate
- Reports **Cohen's d effect size** and **95% confidence intervals** — not just p-values

### Power Analysis (Notebook 4)
- Pre-experiment **sample size calculator** for any baseline rate and MDE
- **Power curves** across three lift scenarios (+1pp, +3pp, +5pp)
- Post-hoc achieved power verification
- Alpha spending trade-off visualisation

### Bayesian Testing (Notebook 5)
- **Beta-Binomial conjugate model** — analytically solved, no MCMC
- **Monte Carlo simulation** (100,000 samples) for P(Treatment > Control)
- **Expected Loss framework** for minimising decision risk
- Credible intervals vs confidence intervals — explained and compared

### Real-World Pitfalls (Notebook 6)
- **Peeking problem simulation** — shows false positive rate inflating from 5% to 30%+
- **SPRT (Sequential Probability Ratio Test)** — statistically valid early stopping
- **Benjamini-Hochberg correction** — controls False Discovery Rate across 3 metrics

### Automated Reporting (Notebook 7)
- Generates a **self-contained HTML report** with all charts embedded as base64
- Colour-coded significance table with BH-corrected p-values
- Plain-English recommendation section with next steps
- Methodology notes explaining every statistical choice

---

## Project Structure

```
AB-Test-Analysis-Engine/
│
├── data/
│   └── ab_experiment_raw.csv
│
├── outputs/
│   ├── banner.png
│   ├── nb1_group_comparison.png
│   ├── nb2_session_distribution.png
│   ├── nb2_subgroup_analysis.png
│   ├── nb2_daily_entry.png
│   ├── nb3_test_results.png
│   ├── nb4_power_curves.png
│   ├── nb4_alpha_tradeoff.png
│   ├── nb5_prior_posterior.png
│   ├── nb5_lift_posterior.png
│   ├── nb6_peeking_problem.png
│   ├── nb6_sprt.png
│   ├── nb6_multiple_comparisons.png
│   └── experiment_report.html
│
├── notebook1_data_generation.ipynb
├── notebook2_eda.ipynb
├── notebook3_frequentist_testing.ipynb
├── notebook4_power_analysis.ipynb
├── notebook5_bayesian_testing.ipynb
├── notebook6_sequential_testing.ipynb
├── notebook7_report_generation.ipynb
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/gagan-gag/AB-Test-Analysis-Engine.git
cd AB-Test-Analysis-Engine
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Launch Jupyter
```bash
jupyter notebook
```

### 4. Run notebooks in order
Start with `notebook1_data_generation.ipynb` — it creates the `data/` folder and CSV that all subsequent notebooks depend on.

> **Note:** Run all cells top to bottom in each notebook before moving to the next.

---

## Dataset Overview

| Column | Type | Description |
|---|---|---|
| `user_id` | string | Unique ID (C = control, T = treatment) |
| `group` | string | `control` or `treatment` |
| `entry_date` | date | Date user entered experiment |
| `converted` | int | 1 = converted, 0 = did not |
| `session_duration_sec` | float | Time in product (includes ~2% missing, ~1% outliers) |
| `clicked_prompt` | int | 1 = clicked in-product prompt |
| `plan_type` | string | `free` / `standard` / `premium` |
| `device` | string | `desktop` / `mobile` / `tablet` |

---

## Skills Demonstrated

| Skill | Notebook |
|---|---|
| Synthetic data engineering | 1 |
| Exploratory data analysis | 2 |
| Sample Ratio Mismatch (SRM) detection | 2 |
| Parametric & non-parametric hypothesis testing | 3 |
| Effect size reporting (Cohen's d, absolute/relative lift) | 3 |
| Experimental design & power analysis | 4 |
| Bayesian inference (Beta-Binomial) | 5 |
| Probabilistic decision making (Expected Loss) | 5 |
| Sequential testing (SPRT) | 6 |
| Multiple comparisons correction (BH / FDR) | 6 |
| Automated stakeholder reporting | 7 |

---

## Tech Stack

| Library | Purpose |
|---|---|
| `numpy` / `pandas` | Data manipulation |
| `scipy.stats` | Hypothesis testing, Beta distribution |
| `statsmodels` | Power analysis, proportion tests, BH correction |
| `matplotlib` / `seaborn` | Visualisation |
| `jupyter` | Interactive notebooks |

---

## Author

**Gagan R**
Information Science Engineering | Akash Institute of Engineering and Technology, Bengaluru
Graduating 2027

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/gaganr-15948b393)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/gagan-gag)

---

## License

This project is licensed under the MIT License — feel free to use, adapt, and build on it.
