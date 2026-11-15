# Employee Attrition Analysis & Risk Scoring

Which employees are most likely to leave, and why? Analysis of 1,470 employees (16.1% attrition) with a scaled logistic-regression risk model.

**Tools:** Python (pandas, scikit-learn, matplotlib) · **Notebook:** `attrition_prediction_clean.ipynb`

## Key findings
- **Sales loses 1 in 5 employees** (20.6%, n=446) vs 13.8% in R&D (n=961); HR is 19.0% (n=63, small sample).
- **Overtime is the strongest signal:** 30.5% attrition with overtime vs 10.4% without. In Sales it is 37.5% vs 13.8%.
- **Pay and satisfaction matter:** the lowest-paid quartile leaves at 29.3% vs 10.3% for the top quartile.
- **Model:** ROC-AUC 0.81 on a held-out test set (0.83 cross-validated). Employees scored High risk actually left 67% of the time, vs 8% for Low risk.

![Attrition rate by department](charts/01_attrition_by_department.png)
*Sales has the highest attrition rate among the large departments; HR's rate rests on only 63 employees.*

![Model performance](charts/02_model_performance.png)
*Evaluated on 294 held-out employees. Accuracy alone is misleading (84% is achievable by predicting nobody leaves), so recall, precision and AUC are reported.*

<details>
<summary><b>Overtime effect</b></summary>

![Overtime](charts/03_attrition_by_overtime.png)
*Employees working overtime leave at three times the rate of those who do not.*
</details>

<details>
<summary><b>Income and job satisfaction</b></summary>

![Income and satisfaction](charts/04_attrition_by_income_and_satisfaction.png)
*Attrition falls sharply as pay rises and is highest where job satisfaction is Low.*
</details>

<details>
<summary><b>Risk level by department</b></summary>

![Risk by department](charts/05_risk_level_by_department.png)
*Risk levels: High > 0.50, Medium 0.30-0.50, Low ≤ 0.30, from 5-fold cross-validated scores so no employee is scored by a model that trained on them.*
</details>

<details>
<summary><b>Top drivers of attrition risk</b></summary>

![Feature importance](charts/06_feature_importance.png)
*Standardised logistic-regression coefficients. Coefficients show association, not causation.*
</details>

<details>
<summary><b>ROC curve</b></summary>

![ROC curve](charts/07_roc_curve.png)
*AUC 0.81: a randomly chosen leaver is scored higher than a randomly chosen stayer about 81% of the time.*
</details>

## Method and limitations
- Features standardised (`StandardScaler`); constant/ID columns dropped; stratified 80/20 split.
- The data is a snapshot, so scores are **risk scores**, not forecasts of future attrition.
- At the 0.50 cutoff the model catches only 34% of leavers (precision 59%); at 0.30 it catches 55% (precision 52%). Accuracy at 0.50 (85.7%) is only slightly above the 84.0% baseline, so AUC is the fairer measure.
- Small groups (e.g. HR, n=63) give noisy percentages.

## Run it
Place `cleaned_hr_data.csv` beside the notebook and run all cells. Charts save to `charts/`.
