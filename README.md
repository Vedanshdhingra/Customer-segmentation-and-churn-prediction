# Customer Churn Prediction and Segmentation

A machine learning project that analyzes telecom customer behavior, predicts customer churn, and segments customers based on churn risk and value-related attributes. The project uses exploratory data analysis, Random Forest classification, model evaluation, ROC-AUC analysis, and K-Means clustering to identify high-risk customer groups and support data-driven retention strategies.

## Project Overview

Customer churn is a major business problem for subscription-based telecom companies. This project focuses on:

- Understanding customer churn patterns through exploratory data analysis.
- Building a churn prediction model using supervised machine learning.
- Handling class imbalance using weighted classification.
- Evaluating model performance using accuracy, precision, recall, F1-score, confusion matrix, cross-validation, and ROC-AUC.
- Segmenting customers into meaningful business groups using K-Means clustering.

## Dataset

The project uses a Telco Customer Churn dataset loaded from:

```text
Telco_customer_churn.xlsx
```

Dataset summary observed in the notebook:

| Property | Value |
|---|---:|
| Rows | 7,043 |
| Columns | 33 |
| Non-churn customers | 5,174 |
| Churn customers | 1,869 |
| Target column | `Churn Value` |

Important features used in analysis include:

- `Tenure Months`
- `Monthly Charges`
- `Total Charges`
- `Contract`
- `Internet Service`
- `Payment Method`
- `Tech Support`
- `Churn Label`
- `Churn Value`
- `Churn Score`
- `CLTV`

## Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Google Colab / Jupyter Notebook

## Project Workflow

### 1. Data Loading

The dataset is loaded using Pandas:

```python
df = pd.read_excel("Telco_customer_churn.xlsx")
```

### 2. Exploratory Data Analysis

The notebook explores:

- Dataset shape and data types
- Churn distribution
- Tenure distribution
- Monthly charges distribution
- Contract-wise churn behavior
- Internet service and payment method impact
- Tech support impact
- Correlation between numerical variables

Key insight from contract analysis:

| Contract Type | Churn Rate |
|---|---:|
| Month-to-month | 42.71% |
| One year | 11.27% |
| Two year | 2.83% |

This shows that customers on month-to-month contracts are much more likely to churn.

### 3. Data Preprocessing

Preprocessing steps include:

- Handling missing values in `Total Charges`
- Dropping unnecessary columns such as location fields and direct churn explanation fields
- Removing high-cardinality columns such as `City`
- Encoding categorical variables using one-hot encoding
- Splitting the dataset into training and testing sets

### 4. Churn Prediction Model

A Random Forest Classifier is used for churn prediction.

Initial model:

```python
RandomForestClassifier(n_estimators=100, random_state=42)
```

Balanced model:

```python
RandomForestClassifier(
    n_estimators=100,
    random_state=42,
    class_weight="balanced"
)
```

Tuned model:

```python
RandomForestClassifier(
    n_estimators=300,
    max_depth=10,
    random_state=42,
    class_weight="balanced"
)
```

### 5. Model Evaluation

The tuned Random Forest model improves churn recall, which is important because correctly identifying customers likely to churn is more valuable than only maximizing accuracy.

Tuned model performance:

| Metric | Value |
|---|---:|
| Accuracy | 78% |
| Churn Precision | 59% |
| Churn Recall | 75% |
| Churn F1-score | 66% |
| ROC-AUC | 0.8571 |
| Mean Cross-validation Accuracy | 0.7794 |

The model gives a practical balance between overall accuracy and churn detection ability.

### 6. Customer Segmentation

After churn probability prediction, customers are segmented using K-Means clustering based on:

- `Tenure Months`
- `Monthly Charges`
- `Total Charges`
- `Churn Probability`

The final clustering uses:

```python
KMeans(n_clusters=3, random_state=42)
```

Cluster summary:

| Cluster | Segment Name | Avg Tenure | Avg Monthly Charges | Avg Total Charges | Avg Churn Probability |
|---|---|---:|---:|---:|---:|
| 0 | Budget Loyal Customers | 32.05 | 32.85 | 1047.70 | 0.1206 |
| 1 | High Risk New Customers | 10.96 | 71.96 | 884.07 | 0.6914 |
| 2 | Loyal Premium Customers | 58.40 | 90.43 | 5278.00 | 0.2306 |

The most important segment is **High Risk New Customers**, because this group has low tenure, relatively high monthly charges, and the highest churn probability.

## Key Business Insights

- Customers with shorter tenure are more likely to churn.
- Month-to-month contract customers show the highest churn risk.
- Higher monthly charges are associated with increased churn probability.
- Customers without long-term commitment should be targeted with retention offers.
- High-risk new customers should receive early engagement, discounts, onboarding support, or personalized plans.
- Premium loyal customers can be retained through loyalty benefits and service upgrades.

## How to Run the Project

### Option 1: Run on Google Colab

1. Open the notebook in Google Colab.
2. Upload the dataset file:
   ```text
   Telco_customer_churn.xlsx
   ```
3. Run all cells from top to bottom.

### Option 2: Run Locally

1. Clone the repository:

```bash
git clone <repository-url>
cd <repository-folder>
```

2. Install required dependencies:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl
```

3. Add the dataset file to the project directory:

```text
Telco_customer_churn.xlsx
```

4. Open the notebook:

```bash
jupyter notebook CBSOT_project1.ipynb
```

5. Run all cells.

## Suggested Project Structure

```text
customer-churn-prediction-segmentation/
│
├── CBSOT_project1.ipynb
├── Telco_customer_churn.xlsx
├── README.md
└── requirements.txt
```

## Requirements

Create a `requirements.txt` file with:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
openpyxl
jupyter
```

Install using:

```bash
pip install -r requirements.txt
```

## Results Summary

This project successfully builds a churn prediction and customer segmentation pipeline. The Random Forest model achieves a ROC-AUC score of **0.8571**, showing good ability to distinguish between churn and non-churn customers. K-Means clustering further divides customers into actionable business segments, especially identifying high-risk new customers who need immediate retention focus.

## Future Improvements

- Add advanced models such as XGBoost, LightGBM, or CatBoost.
- Use SMOTE or other resampling methods for better class imbalance handling.
- Perform deeper hyperparameter tuning using GridSearchCV or RandomizedSearchCV.
- Deploy the model using Streamlit or Flask.
- Add automated customer risk scoring dashboard.
- Save the trained model using Joblib or Pickle.
- Track model performance over time using new customer data.

## Conclusion

The project demonstrates how machine learning can help telecom businesses predict customer churn and create meaningful customer segments. These insights can support better marketing decisions, improve customer retention, and reduce revenue loss from churn.
