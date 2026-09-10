# SmartKart Customer Retention Intelligence Platform

An end-to-end **Machine Learning solution for customer churn prediction and retention prioritization** built for SmartKart, a retail business.

The project transforms a real-world-style customer dataset containing data-quality issues into a clean, model-ready dataset, trains a **Logistic Regression** model, evaluates its predictive performance, interprets the key churn drivers, and produces a business-ready **customer churn risk report**.

---

## 📌 Project Overview

Customer churn is a major business challenge for retail organizations. Identifying customers who are likely to leave allows businesses to intervene proactively through targeted retention campaigns, customer support improvements, and personalized offers.

This project develops a predictive churn intelligence pipeline that:

* Cleans and validates raw customer data
* Handles duplicates and invalid values
* Treats statistical outliers using the IQR method
* Selects meaningful business features
* Standardizes numerical variables
* Trains a Logistic Regression classification model
* Predicts churn probability for unseen customers
* Evaluates model performance using classification metrics
* Interprets the factors influencing churn
* Generates a ranked customer-risk report for retention teams

---

## 🎯 Business Objective

**Primary objective:**
Predict which SmartKart customers are most likely to churn and provide the business with a prioritized list of customers requiring retention attention.

### Key Business Questions

1. Which customers are most likely to churn?
2. What customer characteristics are associated with higher churn risk?
3. Can churn risk be quantified using a probability score?
4. Which customers should the retention team prioritize?
5. What actionable insights can management derive from the model?

---

## 📊 Dataset

The project uses a **100-record SmartKart customer dataset** containing intentionally introduced real-world data-quality issues.

### Key Variables

| Variable        | Description                   | Role       |
| --------------- | ----------------------------- | ---------- |
| `Customer_ID`   | Unique customer identifier    | Identifier |
| `Age`           | Customer age                  | Feature    |
| `Monthly_Spend` | Customer's monthly spending   | Feature    |
| `Complaints`    | Number of customer complaints | Feature    |
| `Churn`         | Whether the customer churned  | Target     |

### Target Variable

`Churn`

* `0` = No Churn
* `1` = Churn

After data cleaning, the dataset contains **95 usable customer records**.

---

## 🔄 Machine Learning Pipeline

The project follows a structured **15-step ML workflow**:

```text
Raw Customer Data
       ↓
Data Inspection
       ↓
Data Quality Checks
       ↓
Duplicate Removal
       ↓
Invalid Value Handling
       ↓
Missing Value Imputation
       ↓
Outlier Detection & Treatment
       ↓
Feature Selection
       ↓
Target Definition
       ↓
Train-Test Split
       ↓
Feature Standardization
       ↓
Logistic Regression
       ↓
Churn Prediction
       ↓
Model Evaluation
       ↓
Business Interpretation
       ↓
Customer Risk Report
```

---

## 🧹 Data Cleaning

The raw dataset contains several data-quality problems that are addressed before modelling.

### Duplicate Records

Five duplicate records are removed.

```text
100 raw records → 95 records
```

### Invalid Values

Examples include:

* Negative `Monthly_Spend`
* Invalid `Age` values
* Whitespace-only values
* Text values such as `"thirty"`
* Unrealistic ages such as `-5` and `150`

Invalid values are converted to missing values and subsequently handled using median imputation.

### Missing Value Treatment

Median imputation is used for:

* `Age`
* `Monthly_Spend`
* `Complaints`

Median is preferred because it is more robust to extreme values than the mean.

---

## 📈 Outlier Treatment

Outliers are detected using the **Interquartile Range (IQR)** method.

The project applies IQR-based capping rather than deleting affected customers.

This preserves the customer's other information while preventing extreme values from disproportionately influencing the Logistic Regression model.

Examples identified in the dataset include:

* Extremely high `Monthly_Spend`
* Unusually high `Complaints`

---

## 🧠 Feature Selection

The model uses three business-relevant predictive features:

```python
[
    "Age",
    "Monthly_Spend",
    "Complaints"
]
```

`Customer_ID` is deliberately excluded because it is an identifier rather than a meaningful predictive variable.

---

## ⚙️ Model

### Algorithm

**Logistic Regression**

Logistic Regression is appropriate because churn is a **binary classification problem**.

The model also provides churn probabilities, which are particularly useful for business applications because customers can be ranked according to their estimated risk.

### Train-Test Split

The cleaned dataset is divided into:

* **80% Training Data**
* **20% Testing Data**

Stratified sampling is used to maintain a similar churn distribution across the training and testing datasets.

```text
Training customers: 76
Testing customers: 19
```

---

## 📏 Feature Standardization

`StandardScaler` is used to standardize:

* Age
* Monthly Spend
* Complaints

The scaler is fitted **only on the training data** and then applied to the test data.

This prevents test-set information from leaking into the training process.

---

## 📊 Model Evaluation

The model is evaluated using standard classification metrics:

* Accuracy
* Precision
* Recall
* F1-Score
* Confusion Matrix

The project prioritizes **Recall** because missing a genuine churner can represent a lost customer, whereas incorrectly flagging a loyal customer can generally be addressed through a retention campaign.

The notebook reports approximately:

* **Accuracy:** 89–95%
* **Recall:** ~100%
* **Precision:** ~83–91%

Exact results may vary depending on the execution environment and dataset state.

---

## 🔍 Model Interpretation

One of the main objectives of this project is not only to predict churn but also to understand **why customers may churn**.

The Logistic Regression coefficients provide directional insights.

### Monthly Spend

`Monthly_Spend` has a strong **negative relationship** with churn risk.

> Higher monthly spending is associated with lower predicted churn risk in this dataset.

### Complaints

`Complaints` has a strong **positive relationship** with churn risk.

> More customer complaints are associated with higher predicted churn risk.

This represents an important operational insight for SmartKart.

### Age

`Age` has a relatively smaller positive relationship with churn risk compared with the other features.

---

## 💡 Key Business Insight

The analysis suggests that **customer complaints and customer spending behavior are important retention signals**.

### Recommended Business Actions

**1. Prioritize complaint resolution**

Customers with repeated complaints should receive proactive attention before dissatisfaction leads to churn.

**2. Protect high-value customers**

High-spending customers should be monitored and provided with personalized engagement and retention strategies.

**3. Use predictive risk scoring**

Instead of treating every customer equally, retention teams can focus resources on customers with the highest predicted churn probability.

---

## 🚨 Customer Risk Scoring

The model generates a probability of churn for every test customer.

Example:

```text
Customer_ID | Churn Probability | Risk
------------|-------------------|------------------
CUST_001    | 0.91              | Likely to Churn
CUST_002    | 0.76              | Likely to Churn
CUST_003    | 0.34              | Not Likely to Churn
```

Customers are sorted from **highest to lowest churn probability**.

The resulting report contains:

* Customer ID
* Age
* Monthly Spend
* Complaints
* Actual Churn
* Predicted Churn
* Churn Probability
* Risk Label

---

## 📁 Project Deliverables

```text
SmartKart/
│
├── SmartKart_Churn_Prediction_ML_Pipeline.ipynb
├── SmartKart_dirty_100_rows.csv
├── smartkart_churn_risk_report.csv
└── README.md
```

### Main Outputs

**Notebook**

Complete end-to-end machine learning pipeline with explanations, modelling, evaluation, and business interpretation.

**Churn Risk Report**

A business-ready CSV containing customer-level churn predictions and risk scores.

---

## 🛠️ Technology Stack

* **Python**
* **Pandas**
* **NumPy**
* **Scikit-learn**
* **Matplotlib**
* **Seaborn**
* **Google Colab / Jupyter Notebook**

### Machine Learning

* Logistic Regression
* StandardScaler
* Train-Test Split
* IQR Outlier Treatment
* Classification Metrics

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone <repository-url>
cd SmartKart
```

### 2. Install dependencies

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

### 3. Open the notebook

```bash
jupyter notebook SmartKart_Churn_Prediction_ML_Pipeline.ipynb
```

### 4. Run all cells

The notebook performs the complete pipeline from raw data preparation through customer-risk reporting.

---

## 📌 Limitations

This project is a prototype based on a relatively small dataset of 100 raw records.

Therefore:

* Model performance should not be interpreted as production-level performance.
* Additional customer attributes could improve predictive power.
* A larger historical dataset would provide a more reliable evaluation.
* Production deployment would require monitoring for data drift and model performance.
* Business intervention thresholds should be determined based on retention campaign costs and customer value.

---

## 🚀 Future Enhancements

Potential improvements for a production-grade SmartKart solution include:

* Larger historical customer datasets
* Additional behavioral and transactional features
* Random Forest / Gradient Boosting / XGBoost comparison
* Hyperparameter optimization
* Cross-validation
* ROC-AUC and PR-AUC analysis
* Explainable AI using SHAP
* Automated retention recommendations
* Customer segmentation
* Interactive Power BI/Tableau dashboard
* Model monitoring and drift detection
* Automated scoring pipeline

---

## 🏆 Business Value

The project demonstrates how machine learning can move beyond prediction and become a **decision-support system**.

Instead of simply answering:

> **"Who will churn?"**

the solution enables SmartKart to answer:

> **"Who is most likely to churn, how likely are they to churn, what factors are associated with their risk, and who should the retention team prioritize?"**

This makes the project relevant to **Customer Analytics, CRM, Marketing Analytics, Business Intelligence, and Predictive Analytics**.

---

## 👤 Project Type

**Domain:** Retail / E-commerce
**Use Case:** Customer Retention & Churn Prediction
**ML Type:** Supervised Learning
**Problem Type:** Binary Classification
**Primary Algorithm:** Logistic Regression
**Output:** Customer Churn Risk Report

---

## 📜 Resume-Ready Project Description

> **SmartKart Customer Retention Intelligence Platform** — Built an end-to-end supervised machine learning pipeline to predict customer churn using Logistic Regression. Performed data cleaning, duplicate removal, missing-value treatment, IQR-based outlier capping, feature standardization, model evaluation, and coefficient-based interpretation. Generated customer-level churn probabilities and a prioritized risk report to support proactive retention decisions.
