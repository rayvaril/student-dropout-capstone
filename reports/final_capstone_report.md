# Early Warning for Student Attrition

## An Explainable Machine Learning Approach with Fairness Evaluation

### Capstone Report

## Executive Summary

This capstone develops an early-warning machine learning system for identifying students at risk of dropping out at two intervention stages: **Enrollment** and **Semester1**.

The project compares multiple machine learning approaches and selects tuned Logistic Regression models based on cross-validation performance, with **Dropout Recall** as the primary model-selection metric. The held-out test set is reserved exclusively for final evaluation.

The Enrollment model achieves a Dropout Recall of **72.2%**, while the Semester 1 model improves recall to **77.5%** and substantially improves precision from **52.6% to 69.8%**.

Explainability analysis shows that the Enrollment model relies more heavily on background and enrollment-stage characteristics, while the Semester 1 model shifts toward observed academic-progress indicators such as approved units, grades, and completion-related features.

Fairness analysis identifies meaningful subgroup performance gaps across gender, age, and scholarship status. These gaps are smaller in the Semester 1 model but are not eliminated.

The findings support using the models as **decision-support tools for targeted student outreach**, rather than as automated decision systems. Further validation on future cohorts and continued fairness monitoring would be required before real-world deployment.

## 1. Problem Definition

Student dropout is a major challenge for higher education institutions because students who are at risk may not be identified early enough for timely support.

This capstone investigates whether machine learning can help identify students labeled as **Dropout** early enough to support targeted intervention while maintaining explainability and evaluating whether predictive performance is comparable across relevant student groups.

The project focuses on two intervention points:

- **Enrollment stage** — using information available at or near enrollment
- **Semester 1 stage** — using enrollment information plus first-semester academic performance

The central trade-off is between **earlier intervention** and **stronger prediction**. An Enrollment model provides more lead time but has less information available, while a Semester 1 model has access to stronger academic-progress signals but provides intervention later in the student journey.

## 2. Dataset and Target Definition

The project uses the **Predict Students' Dropout and Academic Success** dataset from the UCI Machine Learning Repository.

The dataset contains **4,424 student records and 37 variables**, including demographic information, application and enrollment characteristics, Semester 1 and Semester 2 academic performance, and macroeconomic context.

The original target contains three classes:

- `Dropout`
- `Enrolled`
- `Graduate`

For this capstone, the target is converted into a binary classification problem:

- `1` = Dropout
- `0` = Not labeled as Dropout (`Graduate` or `Enrolled`)

Because the `0` class includes students who are still labeled `Enrolled`, it should not be interpreted as confirmed eventual non-dropout.

The dataset contains no missing values and no exact duplicate records. Feature timing is treated carefully to prevent future information from leaking into earlier prediction stages.

## 3. Methodology

The project follows a staged machine learning workflow designed to preserve realistic prediction timing and avoid data leakage.

### 3.1 Prediction Windows

Two model stages are evaluated:

- **Enrollment model** — uses features available at or near enrollment
- **Semester 1 model** — uses the Enrollment features plus first-semester academic information

Semester 2 variables are excluded because they occur too late to support the intended early-warning use case.

### 3.2 Data Preparation

The Enrollment model uses **22 predictors**. The Semester 1 model uses those same 22 variables plus **6 raw Semester 1 variables and 5 engineered Semester 1 features**, for a total of **33 predictors**.

The engineered Semester 1 features include:

- approval rate
- evaluations per enrolled unit
- completion gap
- no-approved-units indicator
- no-enrolled-units indicator

Categorical variables are one-hot encoded, while numerical variables are standardized within the modeling pipeline.

### 3.3 Train/Test Strategy

The dataset is split using stratified sampling into:

- **3,539 training students**
- **885 held-out test students**

Hyperparameter tuning and model selection are performed using **stratified 5-fold cross-validation on the training data only**.

The held-out test set is reserved exclusively for final evaluation.

### 3.4 Candidate Models

Three main algorithms are compared:

- Logistic Regression
- Random Forest
- Gradient Boosting

L1-based feature selection and PCA-based dimensionality reduction are also evaluated.

Because identifying at-risk students is the main objective, **Dropout Recall** is used as the primary model-selection metric, supported by Precision, F1 Score, ROC-AUC, PR-AUC, and confusion-matrix analysis.

## 4. Final Model Results

Tuned Logistic Regression is selected for both prediction stages because it provides the strongest cross-validated Dropout Recall while maintaining competitive performance across the supporting metrics.

### 4.1 Held-Out Test Performance

| Model Stage | Recall | Precision | F1 Score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Enrollment | 72.2% | 52.6% | 60.8% | 78.6% | 64.5% |
| Semester 1 | 77.5% | 69.8% | 73.5% | 89.1% | 83.4% |

The **Enrollment model** identifies 72.2% of students labeled as Dropout, providing an opportunity for earlier intervention. However, its lower precision means that a substantial number of students flagged as high risk are not ultimately labeled as Dropout.

The **Semester 1 model** improves performance across all reported metrics. Dropout Recall increases to 77.5%, while Precision rises to 69.8%.

In the held-out test set:

- Enrollment model: **205 true positives, 79 false negatives, and 185 false positives**
- Semester 1 model: **220 true positives, 64 false negatives, and 95 false positives**

This demonstrates the project’s central trade-off: the Enrollment model provides more lead time, while the Semester 1 model provides stronger and more efficient targeting.

## 5. Explainability Findings

The explainability analysis shows that the two model stages rely on different types of information.

### 5.1 Enrollment Model

Permutation importance indicates that the Enrollment model relies most heavily on background and enrollment-stage characteristics, including:

- scholarship status
- course
- application mode
- gender
- admission grade
- age at enrollment

These features are useful for early prediction because they are available before Semester 1 academic results exist. However, the presence of demographic and socioeconomic variables among the stronger predictors also increases the importance of subgroup fairness evaluation.

### 5.2 Semester 1 Model

After Semester 1 data becomes available, the importance pattern changes substantially.

The strongest predictors include:

- curricular units approved
- Semester 1 grade
- curricular units enrolled
- approval rate
- no-approved-units indicator
- completion gap

This suggests that the Semester 1 model relies more strongly on observed academic progress rather than primarily on background characteristics.

### 5.3 Partial Dependence Analysis

Partial dependence analysis is used to examine how the Semester 1 model’s predicted dropout risk changes across two of its strongest academic predictors.

For **Curricular units 1st sem (approved)**, predicted dropout risk decreases sharply as the number of approved units increases. The relationship is strongest among students with very few approved units.

For **Curricular units 1st sem (grade)**, predicted dropout risk also decreases as grades increase.

These patterns reinforce the EDA and permutation-importance findings that stronger first-semester academic progress is associated with lower predicted dropout risk in the fitted model.

Partial dependence describes the model's average predictive behavior and should not be interpreted causally, particularly because several Semester 1 academic variables are correlated.

### 5.4 Individual Explanations

Because the final models use Logistic Regression, individual predictions can be decomposed into feature contributions.

These explanations show which model inputs push a prediction toward or away from the Dropout class. However, feature contributions should not be interpreted causally, especially when predictors are correlated.

A high predicted probability also does not guarantee that a student will be labeled as Dropout. The analysis includes a high-confidence false-positive example to demonstrate why risk scores should support human review rather than trigger automatic decisions.

## 6. Fairness Evaluation

Fairness is evaluated on the same held-out test set used for final model performance.

The analysis focuses on:

- gender
- age groups
- scholarship status

The main fairness-related metrics are:

- subgroup Dropout Recall
- false-negative rate
- selection rate
- demographic parity difference
- equal opportunity difference
- equalized-odds gap

### 6.1 Gender

For the Enrollment model, Dropout Recall is:

- Female: **57.2%**
- Male: **86.3%**

This produces a recall gap of **0.291**.

For the Semester 1 model:

- Female: **72.5%**
- Male: **82.2%**

The recall gap decreases to **0.097**.

The equalized-odds gap also decreases from **0.340** at Enrollment to **0.108** after Semester 1.

### 6.2 Age Groups

Three age groups are evaluated:

- 18–20
- 21–25
- 26+

For the Enrollment model, Dropout Recall ranges from **45.1%** for students aged 18–20 to **90.5%** for students aged 26+, producing a recall gap of **0.454**.

For the Semester 1 model, recall improves to:

- 18–20: **62.7%**
- 21–25: **75.0%**
- 26+: **90.5%**

The overall recall gap decreases to **0.277**.

The equalized-odds gap decreases from **0.544** to **0.277**.

### 6.3 Scholarship Status

For the Enrollment model, Dropout Recall is:

- No Scholarship: **78.8%**
- Scholarship: **13.8%**

This produces a recall gap of **0.650**.

For the Semester 1 model:

- No Scholarship: **81.6%**
- Scholarship: **41.4%**

The recall gap decreases to **0.402**.

The equalized-odds gap also decreases from **0.650** to **0.402**.

The scholarship-holder subgroup contains only **29 actual dropout cases** in the test set, so these results should be interpreted cautiously.

### 6.4 Fairness Interpretation

Across gender, age, and scholarship status, the Semester 1 model shows smaller observed subgroup gaps than the Enrollment model.

However, meaningful disparities remain, particularly across age groups and scholarship status.

These results do not by themselves prove that either model is fair or unfair. They describe subgroup differences observed in the held-out test sample and should be interpreted alongside group sizes, underlying dropout prevalence, model purpose, and the consequences of false positives and false negatives.

### 6.5 Fairness Mitigation Strategies

The fairness audit identifies meaningful subgroup performance gaps, particularly across age groups and scholarship status. Because these findings come from a single held-out test sample, mitigation strategies should be validated on additional cohorts before changing the operational decision rule.

Potential strategies include:

- **Reweighting during training** to place greater emphasis on underperforming subgroups or subgroup-positive cases.
- **Threshold review** to examine whether the current classification threshold produces unnecessarily high false-negative rates for particular groups.
- **Feature sensitivity analysis** by retraining models without selected demographic or socioeconomic variables and comparing both predictive performance and subgroup gaps.
- **Fairness-aware model selection** that considers subgroup Recall and false-negative-rate gaps alongside overall predictive metrics.
- **Ongoing subgroup monitoring** across future cohorts using Recall, false-negative rate, selection rate, demographic parity, and equalized-odds measures.
- **Human review and supportive outreach** rather than automated decisions, particularly for groups with higher observed error rates.

No mitigation technique is applied automatically in this capstone because mitigation can change both overall predictive performance and subgroup outcomes. Any strategy should first be tested on validation data and evaluated for both utility and fairness before operational use.

### Tested Feature-Sensitivity Mitigation

Because `Gender`, `Age at enrollment`, and `Scholarship holder` were both model inputs and variables used in the fairness audit, an additional sensitivity analysis was performed.

Both final Logistic Regression models were retrained after removing these three attributes.

| Metric | Enrollment Original | Enrollment Reduced | Semester 1 Original | Semester 1 Reduced |
|---|---:|---:|---:|---:|
| Recall | 72.2% | 66.9% | 77.5% | 77.1% |
| Precision | 52.6% | 52.6% | 69.8% | 70.9% |
| F1 | 60.8% | 58.9% | 73.5% | 73.9% |

Selected Equalized-Odds gaps changed as follows:

| Group | Enrollment Original | Enrollment Reduced | Semester 1 Original | Semester 1 Reduced |
|---|---:|---:|---:|---:|
| Gender | 0.340 | 0.174 | 0.108 | 0.119 |
| Age | 0.544 | 0.554 | 0.277 | 0.242 |
| Scholarship status | 0.650 | 0.160 | 0.402 | 0.321 |

The Enrollment model experienced a meaningful reduction in recall when these attributes were removed, suggesting greater dependence on enrollment-stage demographic or socioeconomic information.

In contrast, the Semester 1 model retained almost the same recall while slightly improving precision and F1. Several fairness gaps also improved, particularly for age and scholarship status.

These findings suggest that Semester 1 academic-progress information provides a less attribute-dependent basis for dropout prediction. However, removing sensitive attributes does not guarantee fairness because correlated proxy variables may remain. Fairness should therefore continue to be monitored across future cohorts.

## 7. Limited-Capacity Intervention Analysis

To simulate a realistic setting where an institution can support only the highest-risk **10% of students**, the models are evaluated using a Top-10% intervention strategy.

| Model Stage | Students Flagged | Dropouts Captured | Recall@Top10 | Precision@Top10 |
|---|---:|---:|---:|---:|
| Enrollment | 89 | 73 | 25.7% | 82.0% |
| Semester 1 | 89 | 88 | 31.0% | 98.9% |

The Enrollment model identifies **73 of the 284 actual dropout cases** within the highest-risk 10% of students.

The Semester 1 model identifies **88 of the 284 actual dropout cases** within the same intervention capacity.

This demonstrates the practical trade-off between timing and targeting quality:

- The **Enrollment model** provides more lead time for intervention.
- The **Semester 1 model** produces a more concentrated high-risk group and substantially higher precision.

These results describe the current held-out test sample and should be validated on future student cohorts before being used to determine real-world intervention capacity.

## 8. Limitations

Several limitations should be considered when interpreting the results of this capstone.

First, the dataset represents a specific institutional and historical context. Model performance may therefore differ when applied to future cohorts or students from other institutions.

Second, the binary target combines `Graduate` and `Enrolled` students into the class **Not labeled as Dropout**. Students who are still enrolled do not represent confirmed eventual non-dropout outcomes, so the target should be interpreted carefully.

Third, the analysis is predictive rather than causal. Features associated with dropout risk should not be assumed to cause dropout.

Fourth, some subgroup fairness estimates are based on smaller numbers of actual dropout cases. This is particularly important for scholarship holders, where the held-out test set contains only 29 dropout cases.

Fifth, fairness results are based on the current held-out test sample. Subgroup performance may change across future cohorts and should be monitored continuously.

Finally, the project does not implement a live intervention system. Operational decisions such as intervention thresholds, staffing capacity, outreach strategy, and fairness-mitigation policies would require additional institutional validation.
A high predicted probability also does not guarantee that a student will be labeled as Dropout. The analysis includes a high-confidence false-positive example to demonstrate why risk scores should support human review rather than trigger automatic decisions.

## 9. Conclusion and Recommendatins

This capstone demonstrates that student dropout risk can be identified at multiple points in the student journey, with a clear trade-off between intervention timing and predictive strength.

The Enrollment model provides an earlier warning signal and identifies 72.2% of students labeled as Dropout. However, it produces more false positives and shows larger subgroup performance gaps.

The Semester 1 model improves both predictive performance and targeting efficiency. It achieves 77.5% Dropout Recall, 69.8% Precision, and stronger ROC-AUC and PR-AUC performance. It also reduces several observed fairness gaps across gender, age, and scholarship status.

Explainability analysis shows that the Semester 1 model relies more heavily on direct academic-progress signals, while the Enrollment model depends more on background and enrollment-stage characteristics.

Based on these findings, the following recommendations are proposed:

- Use the **Enrollment model** for broad, low-cost early outreach where earlier intervention is the priority.
- Use the **Semester 1 model** for more targeted support when academic-progress information becomes available.
- Treat model predictions as **decision-support signals**, not automatic decisions.
- Continue monitoring subgroup Recall, false-negative rates, selection rates, and equalized-odds gaps.
- Validate the models on future student cohorts before operational deployment.
- Review intervention thresholds based on actual institutional capacity and the relative cost of false negatives and false positives.
- Investigate fairness-mitigation strategies if subgroup disparities remain large in future validation data.

Overall, the project supports a two-stage early-warning approach in which earlier predictions provide more lead time, while later predictions provide stronger targeting and more reliable academic signals.


## 10. Reproducibility and Project Artifacts

The complete analytical workflow is preserved in the project repository.

The notebook sequence is:

1. `00_problem_definition.ipynb` — problem framing, research questions, evaluation priorities, and project scope
2. `01_dataset_assessment.ipynb` — dataset integrity, semantic typing, feature timing, leakage review, and governance
3. `02_exploratory_analysis.ipynb` — exploratory analysis of enrollment and Semester 1 signals
4. `03_feature_engineering.ipynb` — construction of the two prediction windows and engineered academic-progress features
5. `04_predictive_modeling.ipynb` — model comparison, tuning, selection, held-out test evaluation, and intervention analysis
6. `05_explainability_fairness.ipynb` — feature importance, individual explanations, subgroup performance, and fairness evaluation

The repository also contains:

- processed modeling datasets
- saved final Logistic Regression pipelines
- model configuration
- cross-validation and held-out test results
- limited-capacity intervention results
- permutation-importance results
- subgroup fairness results
- formal fairness-gap summaries

Random seeds, train/test settings, and final model hyperparameters are preserved so that the modeling workflow can be reproduced consistently.

