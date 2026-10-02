# Early Warning for Student Attrition

## An Explainable and Fair Machine Learning Approach

## Project Overview

Student dropout is a major challenge for higher education institutions because students who are at risk may not be identified early enough for timely support.

This capstone develops an explainable machine learning early-warning system to identify students at risk of dropping out at two intervention points:

- **Enrollment:** Using information available when a student first enrolls.
- **Semester 1:** Using enrollment information plus academic performance from the first semester.

The goal is not only to predict dropout risk, but to support earlier and more targeted student interventions while considering model interpretability and fairness.

## Objectives

This project aims to:

- Build an early-warning model using information available at enrollment.
- Build a second model using additional Semester 1 academic information.
- Prioritize identifying students who eventually drop out, making **dropout recall** a key model-selection metric.
- Compare multiple machine learning approaches using cross-validation.
- Evaluate how much predictive performance improves when Semester 1 information becomes available.
- Examine model interpretability and fairness to support responsible use in student intervention.

- ## Modeling Approach

The project follows a two-stage early-warning framework:

1. **Enrollment Model** — predicts dropout risk using information available at enrollment.
2. **Semester 1 Model** — predicts dropout risk after first-semester academic information becomes available.

Logistic Regression, Random Forest, and Gradient Boosting were compared. Feature-selection and PCA-based variants were also evaluated.

Hyperparameter tuning and model selection were performed using **stratified 5-fold cross-validation on the training data only**. The held-out test set was reserved exclusively for final evaluation.

## Final Model Performance

Tuned Logistic Regression was selected for both early-warning stages based on cross-validation performance.

| Model Stage | Recall | Precision | F1 Score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Enrollment | 72.2% | 52.6% | 60.8% | 78.6% | 64.5% |
| Semester 1 | 77.5% | 69.8% | 73.5% | 89.1% | 83.4% |

The **Enrollment model** provides earlier warning, identifying 72.2% of eventual dropouts before first-semester results are available.

The **Semester 1 model** provides stronger targeting, increasing dropout recall to 77.5% while substantially improving precision and reducing false positives.

## Limited-Capacity Intervention

To simulate a real-world scenario where an institution can support only the highest-risk **10% of students**, the models were also evaluated using a Top-10% intervention strategy.

| Model Stage | Students Flagged | Dropouts Captured | Recall@Top10 | Precision@Top10 |
|---|---:|---:|---:|---:|
| Enrollment | 89 | 73 | 25.7% | 82.0% |
| Semester 1 | 89 | 88 | 31.0% | 98.9% |

The results highlight the trade-off between **timing and targeting quality**. The Enrollment model allows earlier intervention, while the Semester 1 model identifies a more concentrated group of students who eventually drop out.

These results describe the current held-out test sample and should be validated on future student cohorts before deployment.

## Repository Structure

- `notebooks/` — Complete analysis workflow from problem definition through predictive modeling
- `data/` — Data used for the analysis
- `models/` — Saved final model artifacts and configuration
- `results/` — Model evaluation and intervention results
- `README.md` — Project overview, methodology, and key findings

- ## Key Takeaway

The project demonstrates that student dropout risk can be identified before the end of the first semester. The Enrollment model offers greater lead time for early support, while incorporating Semester 1 academic information improves both dropout detection and targeting precision.

The models are intended to support—not replace—human decision-making. Predictions should be used as one input for student support and should be validated on future cohorts before operational deployment.
