# Anticipating Future College Admission Cutoffs : An Innovative Predictive Model Incorporating Student Reviews and Historical Admissions Cutoff Data Using Machine Learning

A machine learning framework that predicts future college admission cutoff ranks by combining student reviews, historical NIRF scores and JOSAA cutoff data.

The algorithm implemented by this codebase is described in the following peer-reviewed publication. Please cite this paper when using our code for academic purposes:

> Girish, C., Umesh, T., Karthik, N., Vani, V. (2024). Anticipating Future College Admission Cutoffs: An Innovative Predictive Model Incorporating Student Reviews and Historical Admissions Cutoff Data Using Machine Learning. In: Owoc, M.L., Varghese Sicily, F.E., Rajaram, K., Balasundaram, P. (eds) Computational Intelligence in Data Science. ICCIDS 2024. IFIP Advances in Information and Communication Technology, vol 717. Springer, Cham. [![DOI](https://img.shields.io/badge/doi-10.1007%2F978--3--031--69982--5__13-blue)](https://doi.org/10.1007/978-3-031-69982-5_13)

---

## Overview

This project addresses the challenge of predicting future admission cutoff ranks for engineering colleges in India (admissible via JOSAA). Unlike prior work, this framework uniquely incorporates **student and alumni reviews** as a key feature alongside historical data, resulting in improved prediction accuracy.

The model has two primary outputs:
- Predicted **NIRF factor scores** for the upcoming year.
- Predicted **admission cutoff ranks** per college and seat category.

Given a student's JEE rank and category, the system returns a **list of admissible colleges** based on predicted cutoffs.

---

## Models Used

| Stage | Task | Model | Performance |
|---|---|---|---|
| 1 | Sentiment Analysis | Random Forest Classifier (50 trees) | Accuracy: **86.61%** |
| 2 | Time Series Forecasting | Moving Average (MA) | MSE: **10.84** |
| 3 | Regression | Gradient Boosting Regressor | MSE: **25.62** |

**NIRF Score Prediction (2023):** RMSE = **5.78**

---

## Data Sources

| Data | Source | 
|---|---|
| Student Reviews | Combination of existing Kaggle datasets and manually collected surveys from students and alumni |
| NIRF Scores | [nirfindia.org](https://www.nirfindia.org) — Engineering category, 2020–2023 |
| JOSAA Cutoffs | [josaa.admissions.nic.in](https://josaa.admissions.nic.in) — 2020–2023 |

---

## Results

| System | RMSE |
|---|---|
| Existing system (without reviews) | ~9.5 |
| **Proposed system (with reviews)** | **5.78** |

---

## Future Scope

- Extend to non-engineering domains: medical, law, administration
- Add department/course-level predictions
- Incorporate NIRF rank - cutoff rank correlation
- Extend forecasting beyond one year
- Account for annual seat intake changes
- Add an oversight mechanism for review quality control
