# Credit Risk Modelling

## Project Objective

The primary objective of this project is to develop a robust credit risk model that can be utilized by the Risk Unit to accurately measure credit risk.

## Success Criteria

The model's success is evaluated based on the following key metrics:

1. **AUC (Area Under the Curve) and Gini Coefficient:** Both metrics should be greater than $85\%$.

2. **KS Statistic (Kolmogorov-Smirnov Statistic):** The KS statistic should be greater than $40\%$.

3. **Max KS Stat in First 3 Deciles:** The maximum KS statistic should occur within the first three deciles.

4. **Model Interpretation:** The model should be interpretable, allowing for clear understanding of the factors contributing to credit risk.

## Dataset

The model was developed using two years of historical loan data from Lauki Finance, specifically from February 2022 to February 2024.

* **Training and Validation Data:** February 2022 to February 2024

* **Out-of-Time Data:** March 2024 to May 2024

### Features

The dataset includes features categorized into three main areas:

#### Customer Features

| Variable | Description |
| ----- | ----- |
| `cust_id` | Customer ID |
| `age` | Age of the customer |
| `gender` | Gender |
| `marital_status` | Marital Status |
| `employment_status` | Employment Status |
| `income` | Income of the Customer |
| `number_of_dependents` | Number of dependents |
| `residence_type` | Residence Type |
| `years_at_current_address` | Years at present address |
| `city` | City |
| `state` | State |
| `zipcode` | Zipcode/Pincode |

#### Loan Features

| Variable | Description |
| ----- | ----- |
| `loan_id` | Loan ID |
| `cust_id` | Customer ID |
| `loan_purpose` | Loan Purpose |
| `loan_type` | Loan Type |
| `sanction_amount` | Sanction Amount |
| `loan_amount` | Loan Amount |
| `net_disbursement` | Amount Disbursed in Customer's Account |
| `loan_tenure_months` | Loan Tenure in Months |
| `principal_outstanding` | POS (Principal Outstanding)/BookSize of Customer |
| `bank_balance_at_application` | Bank Balance at application |
| `disbursal_date` | Disbursed Date |
| `installment_start_dt` | Installment start date |
| `default` | Default / No Default (Target Variable) |

#### Bureau Data Features

| Variable | Description |
| ----- | ----- |
| `cust_id` | Customer ID |
| `no_of_open_accounts` | Total Number of open accounts till date |
| `no_of_closed_accounts` | Total Number of closed accounts till date |
| `total_loan_months` | Total Loan in months |
| `delinquent_months` | Total delinquent in months |
| `total_dpd` | Total Due passed day |
| `enquiry_count` | Total Enquiry count |
| `credit_utilization_ratio` | Credit Utilization Ratio |

### Significant Variables (by Information Value - IV)

| Variable | IV | Inference |
| ----- | ----- | ----- |
| `credit_utilization_ratio` | $2.35$ | Higher usage of available credit significantly increases default risk. |
| `delinquency_ratio` | $0.71$ | Higher delinquency rates are strongly linked to increased default risk. |
| `loan_to_income` | $0.47$ | Higher loan amounts relative to income increase the likelihood of default. |
| `avg_dpd_per_delinquency` | $0.40$ | Higher days past due per delinquency correlates with higher default risk. |
| `loan_purpose` | $0.36$ | Certain loan purposes are more likely to be associated with default. |
| `residence_type` | $0.24$ | Residence type has a moderate impact on default risk. |
| `loan_tenure_months` | $0.21$ | Longer loan tenures increases default risk. |
| `loan_type` | $0.16$ | Different loan types have a minor influence on default risk. |
| `age` | $0.08$ | Younger or older age has a minimal effect on default risk. |
| `number_of_open_accounts` | $0.08$ | More open accounts can lead to default risk. |

## Modeling Steps

The credit risk modeling process involved the following stages:

1. **Data Preprocessing:**
    * Invalid `loan_purpose` values were replaced with the mode.
    * Feature selection was performed using Information Value (IV), Variance Inflation Factor (VIF), and domain knowledge.
    * Min-max scaling was applied to numeric features.

2. **Train, Test Split:**
    * The dataset was split into $75\%$ for training and $25\%$ for testing.

3. **Model Training:**
    * Three different models were trained:
        * Logistic Regression
        * XGBoost
        * Random Forest

4. **Fine-Tuning:**
    * Hyperparameter optimization was performed using RandomizedSearchCV and Optuna.

5. **Model Evaluation:**
    * The models were evaluated based on AUC, KS, Gini Coefficient, and a Classification Report.

## Trials and Performance

The performance of the trained models is summarized below:

| Models | AUC | Gini |
| ----- | ----- | ----- |
| Logistic Regression | $98\%$ | $96\%$ |
| XGBoost | $99\%$ | $96\%$ |
| Random Forest | $97\%$ | $95\%$ |

## Model Evaluation Summary

The final model evaluation metrics are as follows:

| Metric | Value |
| ----- | ----- |
| AUC | $98\%$ |
| Gini | $96\%$ |
| Top 3 Decile Capture Rate | $99.53\%$ |

The KS statistic table is provided below:

| Decile | Minimum Probability | Maximum Probability | Events | Non-events | Event Rate | Non-event Rate | Cum Events | Cum Non-events | Cum Event Rate | Cum Non-event Rate | KS |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 0 | 9 | 0.812 | 1.000 | 900.000 | 350.000 | 72.000 | 28.000 | 900.000 | 350.000 | 83.799 | 3.064 | 80.735 |
| 1 | 8 | 0.198 | 0.812 | 159.000 | 1091.000 | 12.720 | 87.280 | 1059.000 | 1441.000 | 98.603 | 12.615 | 85.887 |
| 2 | 7 | 0.026 | 0.198 | 10.000 | 1239.000 | 0.801 | 99.199 | 1069.000 | 2680.000 | 99.534 | 23.461 | 76.073 |
| 3 | 6 | 0.004 | 0.026 | 5.000 | 1245.000 | 0.400 | 99.600 | 1074.000 | 3925.000 | 100.000 | 34.361 | 65.639 |
| 4 | 5 | 0.001 | 0.004 | 0.000 | 1249.000 | 0.000 | 100.000 | 1074.000 | 5174.000 | 100.000 | 45.295 | 54.705 |
| 5 | 4 | 0.000 | 0.001 | 0.000 | 1250.000 | 0.000 | 100.000 | 1074.000 | 6424.000 | 100.000 | 56.237 | 43.763 |
| 6 | 3 | 0.000 | 0.000 | 0.000 | 1250.000 | 0.000 | 100.000 | 1074.000 | 7674.000 | 100.000 | 67.180 | 32.820 |
| 7 | 2 | 0.000 | 0.000 | 0.000 | 1249.000 | 0.000 | 100.000 | 1074.000 | 8923.000 | 100.000 | 78.114 | 21.886 |
| 8 | 1 | 0.000 | 0.000 | 0.000 | 1250.000 | 0.000 | 100.000 | 1074.000 | 10173.000 | 100.000 | 89.057 | 10.943 |
| 9 | 0 | 0.000 | 0.000 | 0.000 | 1250.000 | 0.000 | 100.000 | 1074.000 | 11423.000 | 100.000 | 100.000 | 0.000 |
