# Technical Presentation Outline

## Slide 1 — Early Warning for Student Attrition
**Subtitle:** An Explainable Machine Learning Approach with Fairness Evaluation

- Student dropout early-warning system
- Two prediction stages: Enrollment and Semester 1
- Classification + explainability + fairness evaluation

**Key message:** Can we identify dropout risk early enough to support intervention without treating the model as an automated decision-maker?

---

## Slide 2 — Problem, Target, and Success Criteria

- Dataset: UCI Predict Students' Dropout and Academic Success
- 4,424 students, 37 original variables
- Binary target:
  - 1 = Dropout
  - 0 = Not labeled as Dropout (Graduate + Enrolled)
- Primary technical metric: Dropout Recall
- Supporting metrics: Precision, F1, ROC-AUC, PR-AUC
- Business scenario: limited intervention capacity

**Key message:** Missing an at-risk student is especially costly, so Recall is prioritized.

---

## Slide 3 — Data & Leakage-Safe Prediction Design

### Enrollment Model
- 22 predictors available at or near enrollment
- Excludes Semester 1 and Semester 2 academic information
- Debtor and Tuition fees up to date excluded pending timing verification

### Semester 1 Model
- Enrollment predictors
- 6 raw Semester 1 variables
- 5 engineered academic-progress features
- 33 predictors total

**Engineered features:**
- approval rate
- evaluations per enrolled unit
- completion gap
- no-approved-units indicator
- no-enrolled-units indicator

**Key message:** Feature availability is matched to the point in time when the prediction would actually be made.

---

## Slide 4 — Experimental Design

- Stratified 80/20 train-test split
- Training: 3,539 students
- Held-out test: 885 students
- Stratified 5-fold cross-validation on training data only
- Test set reserved exclusively for final evaluation

### Models compared
- Logistic Regression
- Random Forest
- Gradient Boosting

### Additional methods
- L1 feature selection
- PCA dimensionality reduction
- Hyperparameter tuning

**Key message:** Model selection is based on cross-validation—not test-set performance.

---

## Slide 5 — Model Selection Results

### Best cross-validated model
**Tuned Logistic Regression**

Enrollment CV Recall: **70.6%**

Semester 1 CV Recall: **77.1%**

Why selected:
- highest Dropout Recall
- competitive F1, ROC-AUC, and PR-AUC
- interpretable coefficients
- simpler than ensemble alternatives

**Key message:** Logistic Regression offered the best balance of recall, interpretability, and reproducibility.

---

## Slide 6 — Held-Out Test Performance

| Stage | Recall | Precision | F1 | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Enrollment | 72.2% | 52.6% | 60.8% | 78.6% | 64.5% |
| Semester 1 | 77.5% | 69.8% | 73.5% | 89.1% | 83.4% |

### Confusion matrix outcomes
Enrollment:
- TP 205
- FN 79
- FP 185
- TN 416

Semester 1:
- TP 220
- FN 64
- FP 95
- TN 506

**Key message:** Semester 1 improves both dropout detection and targeting efficiency.

---

## Slide 7 — Explainability: What Drives Predictions?

### Enrollment model
Strongest permutation-importance signals include:
- scholarship status
- course
- application mode
- gender
- admission grade
- age at enrollment

### Semester 1 model
Academic-progress variables dominate:
- approved units
- Semester 1 grade
- enrolled units
- approval rate
- no-approved-units indicator
- completion gap

### PDP finding
Predicted dropout risk falls as:
- approved units increase
- Semester 1 grades improve

**Key message:** The model shifts from background characteristics to direct academic-progress signals once Semester 1 data becomes available.

---

## Slide 8 — Fairness Audit

### Gender Recall Gap
Enrollment: **0.291**
Semester 1: **0.097**

### Age Recall Gap
Enrollment: **0.454**
Semester 1: **0.277**

### Scholarship Recall Gap
Enrollment: **0.650**
Semester 1: **0.402**

### Equalized-Odds Gap
| Group | Enrollment | Semester 1 |
|---|---:|---:|
| Gender | 0.340 | 0.108 |
| Age | 0.544 | 0.277 |
| Scholarship | 0.650 | 0.402 |

**Key message:** Semester 1 reduces observed subgroup disparities, but meaningful gaps remain.

---

## Slide 9 — Limited-Capacity Intervention

Assume the institution can support only the highest-risk **10% of students**.

| Stage | Students Flagged | Dropouts Captured | Recall@Top10 | Precision@Top10 |
|---|---:|---:|---:|---:|
| Enrollment | 89 | 73 | 25.7% | 82.0% |
| Semester 1 | 89 | 88 | 31.0% | 98.9% |

**Trade-off:**
- Enrollment = earlier intervention
- Semester 1 = stronger targeting

**Key message:** The system can prioritize limited support resources rather than simply producing predictions.

---

## Slide 10 — Conclusions, Limitations & Next Steps

### Conclusions
- Dropout risk can be detected at Enrollment
- Semester 1 materially improves predictive performance
- Later predictions rely more on observed academic progress
- Semester 1 also reduces several fairness gaps

### Limitations
- Single dataset / institutional context
- Enrolled students are included in the 0 class
- Predictive, not causal
- Some fairness subgroups have small positive-case counts
- Future-cohort performance is unknown

### Next Steps
- validate on future cohorts
- test fairness-mitigation strategies
- monitor subgroup Recall and false-negative rates
- evaluate thresholds against intervention capacity
- maintain human review

**Final takeaway:** Use the model to support outreach—not to automate decisions about students.
