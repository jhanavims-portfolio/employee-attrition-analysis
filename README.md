<h1 align="center">📉 Employee Attrition Analysis & Risk Scoring</h1>

<p align="center"><b>Who is most likely to leave, and why?</b><br>
Python · pandas · scikit-learn · Logistic Regression</p>

> ### 👀 Want to see the charts?
> **Open the `charts` folder in the `main` branch of this repository.** All 7 visuals are inside as PNG files.

---

## 🎯 At a glance

| 👥 Employees analysed | 📉 Overall attrition | ⏰ Overtime effect | 🤖 Model AUC |
|:---:|:---:|:---:|:---:|
| **1,470** | **16.1%** | **3x higher** | **0.81** |

---

## 🔑 Key findings

### 1. Sales loses 1 in 5 employees
```
Sales  (n=446)  ██████████  20.6%
HR     (n=63)   ██████████  19.0%
R&D    (n=961)  ███████     13.8%
```
HR's rate rests on only 63 employees, so treat it with care.

### 2. Overtime is the strongest signal
```
No overtime  (n=1,054)  █████            10.4%
Overtime     (n=416)    ███████████████  30.5%
```
Inside Sales alone the gap is wider: **37.5% with overtime vs 13.8% without.**

### 3. Lower pay, higher exits
```
Lowest-paid 25%   ███████████████  29.3%
25 to 50%         ███████          14.2%
50 to 75%         █████            10.6%
Top-paid 25%      █████            10.3%
```
Job satisfaction follows the same pattern: **22.8%** attrition at Low satisfaction vs **11.3%** at Very High.

### 4. The risk score separates leavers from stayers
Employees were scored High, Medium or Low risk. Here is how many in each group actually left:

| Risk level | Score range | Employees | Actually left |
|:---:|:---:|:---:|:---:|
| 🔴 High | above 0.50 | 155 | **67.1%** |
| 🟠 Medium | 0.30 to 0.50 | 125 | **30.4%** |
| 🟢 Low | 0.30 or below | 1,190 | **8.0%** |

Share of each department scored **High risk:** Sales 14.6% · HR 14.3% · R&D 8.4%

---

## 🤖 Model performance

Tested on 294 employees the model had never seen.

| Metric | Result |
|---|:---:|
| Baseline accuracy (always predict "stays") | 84.0% |
| ROC-AUC (held-out test) | **0.81** |
| ROC-AUC (5-fold cross-validation) | **0.83** |
| Recall at 0.30 cutoff (leavers caught) | 55.3% |
| Precision at 0.30 cutoff | 52.0% |
| Recall at 0.50 cutoff | 34.0% |
| Precision at 0.50 cutoff | 59.3% |

**Confusion matrix (0.30 cutoff)**

| | Model: stays | Model: leaves |
|---|:---:|:---:|
| **Actually stayed** | 223 | 24 |
| **Actually left** | 21 | 26 |

---

## 🧭 Top drivers of attrition risk

| Raises risk 🔺 | Lowers risk 🔻 |
|---|---|
| Working overtime | Better work-life balance |
| Frequent business travel | More total years of experience |
| Laboratory technician role | |
| Sales representative role | |
| Longer since last promotion | |

*These show association, not proof of cause.*

---

## 🛠️ Method

1. Cleaned the data and removed constant and ID columns
2. Standardised features and encoded categories
3. Stratified 80/20 train-test split
4. Trained a logistic regression model
5. Scored every employee using 5-fold cross-validation, so nobody is scored by a model that trained on them
6. Fixed one cutoff set for all results: **High above 0.50 · Medium 0.30 to 0.50 · Low 0.30 or below**

## ⚠️ Limitations

- The data is a snapshot, so scores are **risk scores**, not forecasts.
- At the 0.50 cutoff the model catches only 34% of leavers, so AUC is the fairer measure than accuracy.
- Small groups such as HR (n=63) give noisy percentages.

---

## 📁 What's in this repository

| File | Contents |
|---|---|
| `attrition_prediction_clean.ipynb` | Full analysis notebook |
| `cleaned_hr_data.csv` | Input dataset |
| `attrition_risk_scores.csv` | Risk score and level for every employee |
| `charts/` | 7 PNG charts: department, model performance, overtime, income and satisfaction, risk by department, drivers, ROC curve |

**To run it:** kept `cleaned_hr_data.csv` beside the notebook and ran all cells.
