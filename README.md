# Neurocognitive & Psychophysiological Data Science Pipeline: Acute HIFT / CrossFit® Crossover Trial

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![pandas](https://img.shields.io/badge/ETL-pandas%20%7C%20NumPy-150458)
![statsmodels](https://img.shields.io/badge/Statistics-statsmodels%20%7C%20SciPy-orange)
![scikit-learn](https://img.shields.io/badge/Machine%20Learning-scikit--learn-F7931E)
![Domain](https://img.shields.io/badge/Domain-Neurocognition%20%26%20Human%20Performance-success)

## Executive Overview
An end-to-end Python **Extract, Transform, and Load (ETL)**, **psychometric feature engineering**, **within-subject causal mediation (`MEMORE`)**, and **supervised Machine Learning (`LOSO-CV`)** pipeline analyzing **18,622 formal behavioral trials** and **60 psychometric survey records** from a 30-participant randomized, counterbalanced crossover study (*Acute CrossFit® / High-Intensity Functional Training [HIFT] vs. Rest*).

This project bridges **cognitive neuroscience**, **exercise physiology**, and **applied machine learning**:
1. **Automated Data Governance & Reconciliation:** Programmatically resolves multi-visit file collisions, administrator re-attempts, and cross-project survey logs into a validated Single Source of Truth (`360` standardized session datasets).
2. **Repeated-Measures Statistical Modeling & ANCOVA:** Demonstrates a disproportionate, dose-dependent acceleration in executive processing speed ($p = .0017$) and single-task response variability stabilization ($p = .0211$) without speed-accuracy trade-offs.
3. **Psychophysiological Mediation & Predictive ML:** Couples exercise-induced shifts in interoceptive/cognitive mindfulness (`SMS-PA-2`) with dual-task working memory stability ($r = .54, p = .0035$) and predicts individual neurocognitive responders out-of-sample (`75.0% LOSO-CV accuracy`, `F1 = 0.837`).

---

## Pipeline Architecture

```text
Raw Data Sources
 ├── /Switch (363 raw PsychoPy task-switching CSV logs; unlabelled Visit 1/2 collisions)
 └── SMS-PA-2 Master Survey (.xlsx; multi-project Likert text responses)
       │
       ▼
[Phase 0] Ground-Truth Reconciliation & Automated QC
 ├── Regex cohort isolation (CT01–CT30) & timestamp-based counterbalance matching
 ├── Programmatic audit & exclusion of 3 aborted/superseded 1st attempts (_1.csv)
 └── Standardized 360-file export: Subject_Time_Condition_Task.csv
       │
       ▼
[Phases 1–3] Trial Consolidation, Outlier Trimming & Feature Engineering
 ├── Sparse PsychoPy loop consolidation (.bfill) across 18,622 formal trials
 ├── Physiological fast-guess filter (RT >= 150 ms) & within-cell ±3 SD trimming (1.14% removed)
 └── Aggregation of Central Tendency (Mean RT, Acc%, IES), IIV (RTSD, CV_RT, IQR_RT) & Executive Costs
       │
       ▼
[Phases 4–6] Inferential Modeling, ANCOVA & Within-Subject Mediation (MEMORE)
 ├── 2 × 2 × 3 Repeated-Measures ANOVA & planned Δ (POST − PRE) contrasts
 ├── Baseline-adjusted MixedLM ANCOVA on Global Mixing, Local Switching & Dual-Task Costs
 └── Montoya & Hayes (2017) within-subject mediation (2,000-iteration bootstrap 95% CIs)
       │
       ▼
[Phase 7] Supervised Machine Learning (Leave-One-Subject-Out Cross-Validation)
 ├── Pruned Random Forest Classifier (max_depth=2): 75.0% Accuracy | 0.837 F1 | 0.625 ROC-AUC
 └── Regularized RidgeCV Regressor: Out-of-sample IIV prediction (r = 0.428, p = .0231)
