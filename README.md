# 🎓 Anticipating Future College Admission Cutoffs

> A machine learning framework that predicts future college admission cutoff ranks by combining student reviews, historical NIRF scores, and JOSAA cutoff data.

Published in **IFIP AICT 717, ICCIDS 2024** — Springer Nature Switzerland AG 2024  
DOI: [10.1007/978-3-031-69982-5_13](https://doi.org/10.1007/978-3-031-69982-5_13)

**Authors:** Chikkam Girish, Tata Umesh, N. Karthik, V. Vani  
Department of Computer Science and Engineering, NIT Puducherry

---

## 📌 Overview

This project addresses the challenge of predicting future admission cutoff ranks for engineering colleges in India (admissible via JOSAA). Unlike prior work, this framework uniquely incorporates **student and alumni reviews** as a key feature alongside historical data, resulting in improved prediction accuracy.

The model has two primary outputs:
- Predicted **NIRF factor scores** for the upcoming year
- Predicted **admission cutoff ranks** per college and seat category

Given a student's JEE rank and category, the system returns a **list of admissible colleges** based on predicted cutoffs.

---

## 🏗️ System Architecture

The pipeline consists of three stages chained together:

```
Student Reviews  ──►  Random Forest Classifier  ──►  Sentiment Score
                                                           │
NIRF Scores (2020–23) ──────────────────────────►  Final Rating
JOSAA Cutoff Ranks (2020–23) ──────────────────►         │
                                                          ▼
                                               Moving Average (MA) Model
                                                          │
                                                          ▼
                                           Gradient Boosting Regressor
                                                          │
                                          ┌───────────────┴────────────────┐
                                          ▼                                ▼
                               Predicted NIRF Scores            Predicted Cutoff Ranks
                                                                           │
                                                                           ▼
                                                              College Predictor (User Input)
```

---

## 📂 Repository Structure

```
├── data/
│   ├── student_reviews/        # Review + rating data (2020–2023)
│   ├── nirf_scores/            # NIRF factor scores from nirfindia.org
│   └── josaa_cutoffs/          # Closing ranks from JOSAA (2020–2023)
│
├── preprocessing/
│   ├── data_cleaning.py        # Null handling, deduplication, abnormal data removal
│   └── data_imputation.py      # Mean imputation for missing NIRF/cutoff values
│
├── models/
│   ├── sentiment_analysis.py   # Random Forest Classifier for review → sentiment score
│   ├── time_series.py          # Moving Average model for trend forecasting
│   └── regression.py           # Gradient Boosting Regressor for final prediction
│
├── college_predictor/
│   └── predictor.py            # User-facing college list generator (rank + category input)
│
├── results/
│   ├── confusion_matrix.png
│   ├── accuracy_vs_trees.png
│   └── rmse_comparison.png
│
├── notebooks/
│   └── full_pipeline.ipynb     # End-to-end walkthrough on Google Colab
│
├── requirements.txt
└── README.md
```

---

## 🤖 Models Used

| Stage | Task | Model | Performance |
|---|---|---|---|
| 1 | Sentiment Analysis | Random Forest Classifier (50 trees) | Accuracy: **86.61%** |
| 2 | Time Series Forecasting | Moving Average (MA) | MSE: **10.84** |
| 3 | Regression | Gradient Boosting Regressor | MSE: **25.62** |

**NIRF Score Prediction (2023):** RMSE = **5.78**

### Why these models?
- **Random Forest Classifier** — Ensemble approach that handles high-dimensional text features (unique words) effectively; outperformed Naive Bayes, SVM, Logistic Regression, and KNN.
- **Moving Average** — Best classical time series model for this data; accounts for unexpected fluctuations using past errors. Outperformed AR, ARMA, and ARIMA.
- **Gradient Boosting Regressor** — Iteratively corrects weak learners to minimize MSE; outperformed Linear Regression, SVR, Random Forest Regressor, and KNN.

---

## 📊 Dataset

| Source | Description | Years | Size |
|---|---|---|---|
| Kaggle + Surveys | Student & alumni reviews with ratings (/10) | 2020–2023 | ~48,500 reviews |
| [nirfindia.org](https://www.nirfindia.org) | TLR, RPC, GO, OI, Perception scores | 2020–2023 | — |
| [josaa.admissions.nic.in](https://josaa.admissions.nic.in) | College-wise closing ranks by category | 2020–2023 | — |

**NIRF factors and their weights used in this framework:**

| Factor | Weight |
|---|---|
| Teaching, Learning & Resources (TLR) | 30% |
| Research & Professional Practice (RPC) | 30% |
| Graduation Outcome (GO) | 20% |
| Outreach & Inclusivity (OI) | 10% |
| Perception | 10% |

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.8+
- Google Colab (recommended) or local Jupyter environment

### Install dependencies

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

### requirements.txt includes
```
scikit-learn
pandas
numpy
matplotlib
statsmodels
xgboost
nltk
```

---

## 🚀 Usage

### 1. Run the full pipeline (recommended)
Open `notebooks/full_pipeline.ipynb` in Google Colab and run all cells sequentially.

### 2. Generate sentiment scores from reviews
```python
from models.sentiment_analysis import train_sentiment_model, predict_sentiment

model = train_sentiment_model("data/student_reviews/reviews_2020_2022.csv")
scores = predict_sentiment(model, "data/student_reviews/reviews_2023.csv")
```

### 3. Forecast cutoff ranks and NIRF scores
```python
from models.time_series import moving_average_forecast

predicted = moving_average_forecast(historical_data, window=3)
```

### 4. Predict admissible colleges for a student
```python
from college_predictor.predictor import get_admissible_colleges

colleges = get_admissible_colleges(rank=150000, category="OPEN")
print(colleges)
```

**Sample output:**
```
Assam University, Silchar — Cutoff Rank: 226733
NIT Arunachal Pradesh — Cutoff Rank: 326527
NIT Meghalaya — Cutoff Rank: 358752
NIT Manipur — Cutoff Rank: 658533
...
```

---

## 📈 Results

The proposed model (with student reviews) outperforms the baseline (without reviews) on NIRF score prediction:

| System | RMSE |
|---|---|
| Existing system (no reviews) | ~9.5 |
| **Proposed system (with reviews)** | **5.78** |

---

## 🔭 Future Scope

- Extend to non-engineering domains: medical, law, administration
- Add department/course-level predictions
- Incorporate NIRF rank ↔ cutoff rank correlation
- Extend forecasting beyond one year
- Account for annual seat intake changes
- Add an oversight mechanism for review quality control

---

## 📄 Citation

If you use this work, please cite:

```bibtex
@inproceedings{girish2024anticipating,
  title     = {Anticipating Future College Admission Cutoffs: An Innovative Predictive Model
               Incorporating Student Reviews and Historical Admissions Cutoff Data
               Using Machine Learning},
  author    = {Girish, Chikkam and Umesh, Tata and Karthik, N. and Vani, V.},
  booktitle = {IFIP International Federation for Information Processing, ICCIDS 2024},
  series    = {IFIP AICT},
  volume    = {717},
  pages     = {167--178},
  year      = {2024},
  publisher = {Springer Nature Switzerland AG},
  doi       = {10.1007/978-3-031-69982-5_13}
}
```

---
*© IFIP International Federation for Information Processing 2024. Published by Springer Nature Switzerland AG.*
