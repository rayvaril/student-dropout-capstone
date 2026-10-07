# Early Warning for Student Attrition

## An Explainable Machine Learning Approach with Fairness Evaluation

## Project Overview

Student dropout is a major challenge for higher education institutions because students who are at risk may not be identified early enough for timely support.

This capstone develops an explainable machine learning early-warning system to identify students at risk of dropping out at two intervention points:

- **Enrollment:** Using information available when a student first enrolls.
- **Semester 1:** Using enrollment information plus academic performance from the first semester.

The goal is not only to predict dropout risk, but to support earlier and more targeted student interventions while considering model interpretability and fairness.

## Business Success Criteria

The model is intended to support limited student-intervention capacity rather than automate student decisions.

Key business-oriented measures include:

- **Students correctly identified for support**
- **Recall@Top10%** — proportion of actual Dropout cases captured when only the highest-risk 10% of students can be prioritized
- **Precision@Top10%** — proportion of prioritized students who are actually labeled as Dropout
- **False positives avoided** — reducing unnecessary outreach and use of limited support resources
- **Retention uplift** — to be measured in a future intervention pilot
- **Cost per successfully supported student** — to be measured once intervention costs are available

The current dataset does not contain intervention costs or experimentally observed retention gains, so no monetary ROI claim is made in this capstone.

## Data Source

This project uses the UCI Machine Learning Repository dataset:

**Predict Students' Dropout and Academic Success**

- **4,424 student records**
- **36 input features**
- Original target classes: `Dropout`, `Enrolled`, `Graduate`
- Includes enrollment information, demographic and socioeconomic variables, and first- and second-semester academic performance
- No missing values are reported by UCI

**Citation:**  
Realinho, V., Vieira Martins, M., Machado, J., & Baptista, L. (2021). *Predict Students' Dropout and Academic Success* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5MC89

For detailed feature assessment and variable definitions, see:

`notebooks/01_dataset_assessment.ipynb`

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

## Model Comparison

Models were compared using stratified 5-fold cross-validation on the training data only. Dropout Recall was the primary selection metric.

| Prediction Window | Model | Recall | Precision | F1 | ROC-AUC | PR-AUC |
|---|---|---:|---:|---:|---:|---:|
| Enrollment | Tuned Logistic Regression | **0.706** | 0.524 | 0.601 | 0.769 | 0.614 |
| Enrollment | Logistic + L1 Feature Selection | 0.700 | 0.513 | 0.592 | 0.764 | 0.604 |
| Enrollment | Logistic + PCA | 0.696 | 0.509 | 0.587 | 0.756 | 0.590 |
| Enrollment | Tuned Random Forest | 0.632 | 0.548 | 0.587 | 0.763 | 0.602 |
| Enrollment | Tuned Gradient Boosting | 0.431 | 0.629 | 0.512 | 0.767 | 0.611 |
| Semester 1 | Tuned Logistic Regression | **0.771** | 0.692 | 0.729 | 0.883 | 0.823 |
| Semester 1 | Logistic + L1 Feature Selection | 0.765 | 0.689 | 0.724 | 0.878 | 0.814 |
| Semester 1 | Logistic + PCA | 0.752 | 0.686 | 0.717 | 0.874 | 0.807 |
| Semester 1 | Tuned Random Forest | 0.727 | 0.728 | 0.727 | 0.879 | 0.815 |
| Semester 1 | Tuned Gradient Boosting | 0.669 | 0.799 | 0.728 | 0.881 | 0.823 |

**Why Logistic Regression?**  
Tuned Logistic Regression achieved the highest Dropout Recall for both prediction windows while maintaining competitive F1, ROC-AUC, and PR-AUC. It was also preferred because interpretability is important for an early-warning decision-support system.

## Final Model Performance

Tuned Logistic Regression was selected for both early-warning stages based on cross-validation performance.

| Model Stage | Recall | Precision | F1 Score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Enrollment | 72.2% | 52.6% | 60.8% | 78.6% | 64.5% |
| Semester 1 | 77.5% | 69.8% | 73.5% | 89.1% | 83.4% |

The **Enrollment model** provides earlier warning, identifying 72.2% of eventual dropouts before first-semester results are available.

## Explainability and Fairness Findings

Explainability analysis shows that the two prediction stages rely on different types of information.

- At **Enrollment**, the model relies more on background and enrollment-stage characteristics such as scholarship status, course, application mode, gender, admission grade, and age.
- After **Semester 1**, academic-progress variables become more influential, particularly approved units, Semester 1 grade, enrolled units, approval rate, and completion-related features.

Fairness analysis on the held-out test set identified meaningful subgroup performance gaps across gender, age, and scholarship status.

The Semester 1 model reduced the observed fairness gaps compared with the Enrollment model, but did not eliminate them. The largest remaining disparities were observed across age groups and scholarship status.

These findings reinforce that the models should be used as decision-support tools rather than as automated decision systems. Any real-world deployment would require future-cohort validation, continued subgroup monitoring, and consideration of fairness-mitigation strategies.

The **Semester 1 model** provides stronger targeting, increasing dropout recall to 77.5% while substantially improving precision and reducing false positives.

### Methods Used

**Explainability**
- Permutation importance was used to identify which original features most influenced predictive performance.
- Partial Dependence Plots (PDP) were used to examine how predicted dropout risk changes as key Semester 1 variables change.
- Logistic Regression coefficients were also used for individual-level contribution analysis.
- These explanations describe predictive relationships and should not be interpreted as causal effects.

**Fairness Metrics**
Model performance was audited across **gender, age groups, and scholarship status** using:

- **Selection-rate gap** — a demographic-parity-style comparison
- **Recall gap** — an equal-opportunity comparison
- **False-positive-rate gap**
- **Equalized-odds gap** — based on differences in true-positive and false-positive rates

Semester 1 reduced the observed gaps across all three audited groupings, but meaningful disparities remained, particularly for **age** and **scholarship status**.

### Tested Feature-Sensitivity Mitigation

A sensitivity analysis retrained both Logistic Regression models after removing:

- `Gender`
- `Age at enrollment`
- `Scholarship holder`

| Metric | Enrollment Original | Enrollment Reduced | Semester 1 Original | Semester 1 Reduced |
|---|---:|---:|---:|---:|
| Recall | 72.2% | 66.9% | 77.5% | 77.1% |
| Precision | 52.6% | 52.6% | 69.8% | 70.9% |
| F1 | 60.8% | 58.9% | 73.5% | 73.9% |

Selected Equalized-Odds gaps also changed:

| Group | Enrollment Original | Enrollment Reduced | Semester 1 Original | Semester 1 Reduced |
|---|---:|---:|---:|---:|
| Gender | 0.340 | **0.174** | 0.108 | 0.119 |
| Age | 0.544 | 0.554 | 0.277 | **0.242** |
| Scholarship status | 0.650 | **0.160** | 0.402 | **0.321** |

The Enrollment model loses meaningful recall when these attributes are removed. In contrast, the Semester 1 model retains almost the same recall while slightly improving Precision and F1, and reducing several observed fairness gaps.

This suggests that Semester 1 academic-progress information provides a less attribute-dependent basis for prediction. However, feature removal does not guarantee fairness because correlated proxy variables may remain.

Detailed results are available in:

- `results/feature_sensitivity_performance.csv`
- `results/feature_sensitivity_fairness_gaps.csv`

**Proposed Mitigation Strategies**
Before deployment, the project recommends:

- reweighting or resampling during training
- reviewing decision thresholds
- testing model performance with sensitive/proxy features removed
- including fairness metrics in model selection
- monitoring subgroup performance over time
- retaining human review for intervention decisions

No fairness mitigation was automatically applied to the final model because any intervention should first be validated on future cohorts and reviewed for institutional and ethical implications.

See:

`notebooks/05_explainability_fairness.ipynb`

and the fairness result files in:

`results/`

## Limited-Capacity Intervention

To simulate a real-world scenario where an institution can support only the highest-risk **10% of students**, the models were also evaluated using a Top-10% intervention strategy.

| Model Stage | Students Flagged | Dropouts Captured | Recall@Top10 | Precision@Top10 |
|---|---:|---:|---:|---:|
| Enrollment | 89 | 73 | 25.7% | 82.0% |
| Semester 1 | 89 | 88 | 31.0% | 98.9% |

The results highlight the trade-off between **timing and targeting quality**. The Enrollment model allows earlier intervention, while the Semester 1 model identifies a more concentrated group of students who eventually drop out.

These results describe the current held-out test sample and should be validated on future student cohorts before deployment.

### Repository Structure

- `notebooks/` — Complete analysis workflow from problem definition through predictive modeling, explainability, and fairness evaluation
- `data/` — Raw and processed data used for the analysis
- `models/` — Saved final model artifacts and configuration
- `results/` — Model evaluation and intervention results
- `reports/` — Final written reports and supporting documentation
- `presentations/` — Final capstone presentation materials
- `src/` — Reusable source code and utility functions
- `README.md` — Project overview, methodology, and key findings

## Limitations

This project should be interpreted as a decision-support prototype rather than a deployment-ready system.

Key limitations include:

- **Single institutional context:** The dataset represents one higher-education setting, so performance may not generalize to other institutions, countries, or student populations.
- **Target definition:** Students labeled `Enrolled` are grouped with `Graduate` as class `0`, even though their eventual outcome is not yet confirmed.
- **Future-cohort performance is unknown:** Results are based on one held-out test split and should be validated on later student cohorts.
- **Predictive, not causal:** Feature importance and Partial Dependence results show associations with predicted dropout risk, not proof that changing those variables would cause dropout risk to change.
- **Fairness estimates may be unstable for smaller subgroups:** Some audited groups contain relatively few Dropout cases.
- **Feature timing requires operational validation:** Variables such as `Debtor` and `Tuition fees up to date` were excluded from the Enrollment model because their exact availability at the prediction point could not be verified.
- **No intervention outcomes are available:** The dataset does not measure whether outreach changes retention, so retention uplift and monetary ROI cannot yet be estimated.

Before deployment, the models should be validated on future cohorts, re-audited for fairness, and tested within a real intervention workflow.

## How to Reproduce

1. Clone the repository.
2. Install dependencies from `requirements.txt`.
3. Run the notebooks in order:

   - `00_problem_definition.ipynb`
   - `01_dataset_assessment.ipynb`
   - `02_exploratory_analysis.ipynb`
   - `03_feature_engineering.ipynb`
   - `04_predictive_modeling.ipynb`
   - `05_explainability_fairness.ipynb`

4. Final model artifacts are saved in `models/`.
5. Evaluation and fairness outputs are saved in `results/`.
6. Final presentations are available in `presentations/`.

The workflow is designed so that preprocessing, model selection, evaluation, explainability, and fairness analysis can be reproduced from the notebooks and saved configuration files.

- ## Key Takeaway

The project demonstrates that student dropout risk can be identified before the end of the first semester. The Enrollment model offers greater lead time for early support, while incorporating Semester 1 academic information improves both dropout detection and targeting precision.

The models are intended to support—not replace—human decision-making. Predictions should be used as one input for student support and should be validated on future cohorts before operational deployment.
