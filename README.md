# Marketing Mix Modelling (MMM) — Thesis & Internship Project
**Student:** Tanushree  
**Programme:** MSc Data Analytics & AI — EDHEC Business School  
**Company:** The Data Story, Amsterdam  
**Thesis Deadline:** 15 June 2026  

---

## What This Project Does

This project builds a Marketing Mix Model (MMM) — a statistical tool that answers one business question:

> **"Of all the revenue we made, how much came from each marketing channel — and how much would have happened anyway without any advertising?"**

For example, after running the model, you get an output like:
- Paid Search drove **18%** of revenue
- Paid Social drove **25%** of revenue  
- Display drove **12%** of revenue
- **45%** would have happened with zero advertising (loyal customers, seasonal demand, word of mouth)

This project also investigates a key problem in MMM: when marketing channels all scale up together (for example during Black Friday), it becomes very hard to separate their individual contributions. This is called **multicollinearity**. The project tests whether a more advanced modelling approach (Bayesian) handles this better than the standard approach (OLS).

---

## Repository Structure

```
mmm_thesis/
│
├── README.md                    ← You are here
├── requirements.txt             ← Python packages needed
├── .gitignore                   ← Files not tracked by git
│
├── src/                         ← All reusable Python code
│   ├── __init__.py
│   ├── data_quality.py          ← Data quality checks before modelling
│   ├── transformations.py       ← Adstock and saturation functions
│   ├── ols_model.py             ← OLS and Ridge regression models
│   ├── bayesian_model.py        ← Bayesian MMM using PyMC-Marketing
│   └── simulate.py              ← Synthetic data generator
│
├── notebooks/                   ← Step-by-step analysis notebooks
│   ├── 01_data_exploration.ipynb
│   ├── 02_baseline_model.ipynb
│   ├── 03_bayesian_model.ipynb
│   └── 04_experiment.ipynb
│
├── data/
│   ├── raw/                     ← Original datasets (not modified)
│   └── synthetic/               ← Generated experimental datasets
│
├── results/
│   ├── figures/                 ← All charts and plots
│   └── tables/                  ← All results tables
│
├── docs/
│   ├── model_overview.md        ← Plain-language model explanation
│   └── decisions.md             ← Why each modelling decision was made
│
└── tests/
    └── test_transformations.py  ← Basic unit tests
```

---

## How to Run This Project

### Step 1 — Install Python packages
```bash
pip install -r requirements.txt
```

### Step 2 — Run the baseline model
```bash
python src/ols_model.py
```
This produces channel contribution estimates and saves a results chart to `results/figures/`.

### Step 3 — Run the full experiment
```bash
python src/simulate.py        # generates synthetic datasets
python src/ols_model.py       # fits OLS and Ridge on all datasets
python src/bayesian_model.py  # fits Bayesian on all datasets
```

### Step 4 — View results
Open `results/figures/experiment_results.png` to see the main finding.

---

## The Research Question

> *How does inter-channel multicollinearity affect the reliability of channel contribution estimates in a Marketing Mix Model — and does a Bayesian framework with informative priors produce more robust estimates than OLS and Ridge under high channel correlation conditions?*

### The Experiment Design

| Condition | Channel Correlation | What It Represents |
|-----------|--------------------|--------------------|
| Low | 0.0 | Channels move independently |
| High | 0.7 | Channels move together (realistic e-commerce) |

Both OLS, Ridge, and Bayesian are tested under both conditions. Parameter recovery error measures how close each model gets to the known true answer.

---

## Key Modelling Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| Time granularity | Weekly | Standard for MMM — right balance of variation and noise |
| Adstock | Geometric decay | Standard formulation from Broadbent (1979) |
| TV decay | 0.6 | Literature benchmark — Jin et al. (2017) |
| Search decay | 0.1 | Immediate response channel — minimal carryover |
| Saturation | Michaelis-Menten | One parameter, interpretable, standard for OLS baseline |
| Seasonality | Monthly dummies | Simple, interpretable, no jargon |
| Multicollinearity check | VIF | Standard diagnostic — Marquardt (1970) |

Full decision log: `docs/decisions.md`

---

## Results Summary

*(Updated after experiment runs)*

| Model | Low Correlation Recovery Error | High Correlation Recovery Error |
|-------|-------------------------------|--------------------------------|
| OLS | TBD | TBD |
| Ridge | TBD | TBD |
| Bayesian | TBD | TBD |

---

## References

- Broadbent, S. (1979). One way TV advertisements work. *Journal of the Market Research Society*
- Jin et al. (2017). Bayesian methods for media mix modeling. *Google Research*
- Marquardt, D.W. (1970). Generalised inverses, ridge regression. *Technometrics*
- PyMC Labs (2023). PyMC-Marketing. https://www.pymc-marketing.io
- Runge et al. (2024). Packaging up media mix modeling: Robyn. *arXiv:2403.14674*
