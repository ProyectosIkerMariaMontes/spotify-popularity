# spotify-popularity
Kaggle competition | Predicting Spotify song popularity from audio features using KNN, SVM, Random Forest, XGBoost and Bayesian models in R. Ranked 1st on the leaderboard.

# Spotify Song Popularity — Kaggle Competition

Predicting the popularity of Spotify songs from audio features using multiple regression and classification models in R.

> **🏆 Ranked 1st on the Kaggle leaderboard at time of submission.**

> **Note:** Code and internal comments are in Spanish/Catalan, as required by the course. This README is in English.

---

## Overview

This project was developed as part of the *Data Mining* course at UB-UPC (2025), framed as a Kaggle competition. The goal was to predict `song_popularity` (0–100) from 14 audio and metadata features extracted from the Spotify API.

- **Dataset:** 13,186 songs × 15 variables (12 numeric, 3 categorical)
- **KPI:** MAPE (Mean Absolute Percentage Error)
- **Split:** 70% training / 30% test, stratified by `time_signature`

---

## Pipeline

### 1. Preprocessing
- **Missing values** — Two-step imputation: modal logic (`key → prob_major → audio_mode`) followed by MICE (PMM, Random Forest, Lasso)
- **Outlier detection** — ALSO (Attribute-Wise Learning Score for Outliers) with k-fold cross-validation + bivariate detection on `instrumentalness × speechiness`. Outliers converted into observation weights for downstream models
- **Oversampling** — SMOTE-inspired technique to balance the heavily skewed `time_signature` variable (3/4, 4/4, 5/4)

### 2. EDA & Feature Engineering
- Correlation analysis across all numeric variables
- Two variables dropped based on EDA
- New variables created: `key_signature`, `prob_major`, `outlier_weight`

### 3. Models

| Model | Notes |
|-------|-------|
| **KNN** (k=1) | Best MAPE on Kaggle; custom feature weighting |
| **SVM** | Regression with radial kernel |
| **Random Forest** | With cross-validation and hyperparameter tuning |
| **XGBoost** | Gradient boosting with feature selection |
| **GLM** | Multiple variants (featured, RF-informed) |
| **Naive Bayes** | Applied to discretized popularity; poor accuracy (0.33) |
| **Censored Bayesian** | Stan-based model via `censored_bayesian.stan` |

### 4. Association Rules
- Transactional transformation of numeric variables into 3 bins
- Two rule sets analyzed (support/confidence: 0.05/0.7 and 0.35/0.9)
- Popularity-focused rules identified with low support (~0.02)

---

## Repository Structure
spotify-popularity/
├── README.md
├── data/
│   ├── train.csv
│   ├── test.csv
│   ├── sample_submission.csv
│   └── (intermediate .RDS files)
├── preprocessing/
│   ├── missings.R
│   ├── outliers.R
│   ├── also.R
│   └── oversampling.R
├── eda/
│   ├── exploration.R
│   └── descriptive_analysis.Rmd
├── models/
│   ├── knn.R
│   ├── svm.R
│   ├── random_forest.R
│   ├── xgboost.R
│   ├── glm.R
│   ├── naive_bayes.R
│   └── ...
├── predictions/
│   └── (model predictions as .csv)
└── report/
└── Report_definitivo.pptx

---

## Results

| Model | Kaggle MAPE |
|-------|-------------|
| KNN (k=1) | **Best** (1st place) |
| Random Forest | — |
| XGBoost | — |
| SVM | — |
| GLM | — |
| Naive Bayes | Not competitive |

---

## Technologies

- **Language:** R
- **Key libraries:** `mice`, `isotree`, `caret`, `xgboost`, `e1071`, `randomForest`, `arules`, `rstan`, `ggplot2`, `dplyr`

---

## Team

| Member | Contributions |
|--------|--------------|
| Iker Maria Montes | Business understanding, missing values, outlier treatment, SVM |
| Bernat Garcia Garcia | ALSO outlier scoring, feature engineering, KNN weighting, XGBoost, oversampling, Bayesian model |
| Davide Rota | KNN, GLM variants, Naive Bayes, XGBoost (featured), KPI definition |
| Pol Tobella Jacomet | Descriptive analysis, Random Forest, Regression trees, cross-validation & tuning |

> Kaggle competition: [Predicción de la Popularidad de Canciones](https://www.kaggle.com/competitions/prediccion-de-la-popularidad-de-canciones/overview)
