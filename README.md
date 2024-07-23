# Lending Club Case Study

## Table of Contents

- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Dataset Analysis](#dataset-analysis)
- [Approach](#approach)
- [Case Study Analysis](#case-study-analysis)
- [Conclusions](#conclusions)
- [Technologies Used](#technologies-used)
- [Acknowledgements](#acknowledgements)
- [Glossary](#glossary)
- [Team](#team)

## Problem Statement

Lending Club is a consumer finance marketplace for personal loans that matches borrowers who are seeking a loan with investors looking to lend money and make a return.

It specialises in lending various types of loans to urban customers. When the company receives a loan application, the company has to make a decision for loan approval based on the applicant's profile.

Like most other lending companies, _lending loans to ‘risky’_ applicants is the largest source of financial loss _(called credit loss)_. The credit loss is the amount of money lost by the lender when the borrower refuses to pay or runs away with the money owed.

In other words, **borrowers** who **default** cause the largest amount of **loss to the lenders**. In this case, the customers labelled as _'charged-off' are the 'defaulters'_.

The core objective of the excercise is to **help the company minimise the credit loss**. There are two potential sources of **credit loss** are:

- Applicant **likely to repay the loan**, such an applicant will bring in profit to the company with interest rates.** Rejecting such applicants will result in loss of business for the bank**.
- Applicant **not likely to repay** the loan, i.e. and will potentially default, then approving the loan may lead to a financial loss\* for the company

## Objectives

The goal is to find out risky loan applicants, then such loans can be reduced thereby reducing down the amount of credit loss. Identification of such applicants using EDA using the given [dataset](./loan.csv), is the aim of this case study.

If one is able to _identify these risky loan applicants_, then such loans can be reduced thereby reducing down the amount of credit loss. Identification of such applicants using EDA is the aim of this case study.

In other words, **the company wants to understand the driving variables** behind loan default, i.e. the variables which are strong indicators of default. The company can utilise this knowledge for its portfolio and risk assessment.

## DataSet Analysis

The data given in the csv is the past track record of all the customer who were either paid off all the amount or were charged off (as in defaulted)

Our focus is to identify such customers so that lending company reduce the loan and thus reducing their risk of getting into losses

- The dataset reflects loans post approval, thus does not represent any information on the rejection criteria process
  - Overall objective will be to observe key leading indicaters (driver variables) in the dataset, which contribute to defaulters
  - Use the analysis as a the foundation of the hypothesis
- The overall loan process is represented by three steps
  - Potential borrower requests for loan amount (loan_amnt)
  - The approver approves/rejects an amount based on past history/risk (funded_amnt)
  - The final amount offered as loan by the investor (funded_amnt_inv)

### Analysis based on Domain Understanding

#### Leading Attribute (loan_status)

- _Loan Status_ - It is a very important attribute around which all the EDA will be done The column has three distinct values
  - Fully-Paid - The customer has successfuly paid the loan
  - Charged-Off - The customer is "Charged-Off" or has "Defaulted"
  - Current - These customers, the loan is currently in progress and cannot contribute to our EDA analysis
    - For the given case study, "Current" status rows will be dropped/deleted

#### Decision Matrix (loan_status)

- _Loan Accepted_ - Three Scenarios
  - _Fully Paid_ - Applicant has fully paid the loan (the principal and the interest rate)
  - _Current_ - Applicant is still paying the EMI's, i.e. the tenure of the loan is not yet completed. These candidates are not considered as 'defaulted'.
  - _Charged-off_ - Applicant has not paid the instalments in due time for a long period of time, i.e. he/she has _defaulted_ on the loan
- _Loan Rejected_ - The company had rejected the loan (because the candidate does not meet their requirements etc.). Since the loan was rejected, there is no transactional history of those applicants with the company and so this data is not available with the company

#### Important Columns

The given columns are leading attributes, or **predictors**. These attributes are available at the time of the loan application and strongly helps in **prediction** of loan pass or rejection. Key attributes _Some of these columns may get dropped due to empty data in the dataset_

- **Customer Demographics**
  - Annual Income (annual_inc) - Annual income of the customer. Generally higher the income, more chances of loan pass
  - Home Ownership (home_ownership) - Wether the customer owns a home or stays rented. Owning a home adds a collateral which increases the chances of loan pass.
  - Employment Length (emp_length) - Employment tenure of a customer (this is overall tenure). Higher the tenure, more financial stablity, thus higher chances of loan pass
  - Debt to Income (dti) - The percentage of the salary which goes towards paying loan. Lower DTI, higher the chances of a loan pass.
  - State (addr_state) - Location of the customer. Can be used to create a generic demographic analysis. There could be higher delinquency or defaulters demographicaly.
- **Loan Attributes**
  - Loan Ammount (loan_amt)
  - Grade (grade)
  - Term (term)
  - Loan Date (issue_date)
  - Purpose of Loan (purpose)
  - Verification Status (verification_status)
  - Interest Rate (int_rate)
  - Installment (installment)
  - Public Records (public_rec) - Derogatory Public Records. The value adds to the risk to the loan. Higher the value, lower the success rate.
  - Public Records Bankruptcy (public_rec_bankruptcy) - Number of bankruptcy records publocally available for the customer. Higher the value, lower is the success rate.

#### Ignored Columns

- The following types of columns will be ignored in the analysis. This is a generic categorization of the columns which will be ignored in our approach and not the full list.
  - **Customer Behaviour Columns** - Columns which describes customer behaviour will not contribute to the analysis. The current analysis is at the time of loan application but the customer behaviour variables generate post the approval of loan applications. Thus these attributes wil not be considered towards the loan approval/rejection process.
  - Granular Data - Columns which describe next level of details which may not be required for the analysis. For example grade may be relevant for creating business outcomes and visualizations, sub grade is be very granular and will not be used in the analysis

### Data Set Analysis based on understanding of EDA

#### Rows Analysis

- Summary Rows: No summary rows were there in the dataset
- Header & Footer Rows - No header or footer rows in the dataset
- Extra Rows - No column number, indicators etc. found in the dataset
- Rows where the **loan_status = CURRENT will be dropped** as CURRENT loans are in progress and will not contribute in the decision making of pass or fail of the loan. The rows are dropped before the column analysis as it also cleans up unecessary column related to CURRENT early and columns with NA values can be cleaned in one go
- Find duplicate rows in the dataset and drop if there are

### Columns Analysis of the Dataset

#### Drop Columns

- There are multiple columns with **NA values** only. The **columns will be dropped**.
  - _This is evaluated after dropping rows with `loan_status = Current`_
  - `(next_pymnt_d, mths_since_last_major_derog, annual_inc_joint, dti_joint, verification_status_joint, tot_coll_amt, tot_cur_bal, open_acc_6m, open_il_6m, open_il_12m, open_il_24m, mths_since_rcnt_il, total_bal_il, il_util, open_rv_12m, open_rv_24m, max_bal_bc, all_util, total_rev_hi_lim, inq_fi, total_cu_tl, inq_last_12m, acc_open_past_24mths, avg_cur_bal, bc_open_to_buy, bc_util, mo_sin_old_il_acct, mo_sin_old_rev_tl_op, mo_sin_rcnt_rev_tl_op, mo_sin_rcnt_tl, mort_acc, mths_since_recent_bc, mths_since_recent_bc_dlq, mths_since_recent_inq, mths_since_recent_revol_delinq, num_accts_ever_120_pd, num_actv_bc_tl, num_actv_rev_tl, num_bc_sats, num_bc_tl, num_il_tl, num_op_rev_tl, num_rev_accts, num_rev_tl_bal_gt_0, num_sats, num_tl_120dpd_2m, num_tl_30dpd, num_tl_90g_dpd_24m, num_tl_op_past_12m, pct_tl_nvr_dlq, percent_bc_gt_75, tot_hi_cred_lim, total_bal_ex_mort, total_bc_limit, total_il_high_credit_limit)`
- There are multiple columns where the **values are only zero**, the **columns will be dropped**
- There are columns where the **values are constant**. They dont contribute to the analysis, **columns will be dropped**
- There are columns where the **value is constant but the other values are NA**. The column will be considered as constant. **columns will be dropped**
- There are columns where **more than 65% of data is empty** `(mths_since_last_delinq, mths_since_last_record)` - **columns will be dropped**
- **Drop columns `(id, member_id)`** as they are **index variables and have unique values** and dont contribute to the analysis
- **Drop columns `(emp_title, desc, title)`** as they are **discriptive and text (nouns) and dont contribute to analysis**
- **Drop redundant columns `(url)`**. On closer analysis url is a static path with the loan id appended as query. It's a redundant column to `(id)` column
- **Drop customer behaviour columns which represent data post the approval of loan**
  - They contribute to the behaviour of the customer. Behaviour of the customer is recorded post approval of loan and not available at the time of loan approval. Thus these variables will not be considered in analysis and thus dropped
  - `(delinq_2yrs, earliest_cr_line, inq_last_6mths, open_acc, pub_rec, revol_bal, revol_util, total_acc, out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee, last_pymnt_d, last_pymnt_amnt, last_credit_pull_d, application_type)`

#### Convert Column Format

- `(loan_amnt, funded_amnt, funded_amnt_inv)` columns are Object and will be converted to float
- `(int_rate, installment, dti)` columns are Object and will be converted to float
- **strip "month"** text from `term` column and convert to integer
- Percentage columns `(int_rate)` are object. **Strip "%"** characters and convert column to float
- `issue_d` column **converted to datetime format**

#### Standardise Values

- All currency columns are rounded off to 2 decimal places as currency are limited to cents/paise etc only.

#### Convert Column Values

- `loan_status` column converted to boolean **Charged Off = False and Fully Paid = True**. This converts the column into ordinal values
- `emp_length` converted to integer with following logic. Note < 1 year is converted to zero and 10+ converted to 10.
  - < 1 year: 0,
  - 2 years: 2,
  - 3 years: 3,
  - 7 years: 7,
  - 4 years: 4,
  - 5 years: 5,
  - 1 year: 1,
  - 6 years: 6,
  - 8 years: 8,
  - 9 years: 9,
  - 10+ years: 10

#### Added new columns

- verification_status_n added. Considering domain knowledge of lending = Verified > Source Verified > Not Verified. verification_status_n correspond to {Verified: 3, Source Verified: 2. Not Verified: 1} for better analysis
- issue_y is year extracted from issue_d
- issue_m is month extracted from issue_d

### Ignored Rows and Columns because of missing data

- Columns with high percentage of missing values will be dropped **(65% above for this case study)**
- Columns with less percentage of missing value will be imputed
- Rows with high percentage of missing values will be removed **(65% above for this case study)**

## Approach

- Step 0 - Data Cleaning & Manipulation Checklist
- Step 1 - Dropping Rows - where loan_status = "Current"
- Step 2 - Dropping Columns based on EDA and Domain Knowledge
- Step 3 - Convert the data types
- Step 4 - Identify columns with blank values which need to be imputed
- Step 5 - Analysis of the dataset post cleanup
- Step 6 - Outlier Treatment
- Step 7 - **Analysis** - Univariate, Bivariate and Derived Metrics Analysis
- Step 8 - **Conclusions** Inferences and Recommendations

## Case Study Analysis

## Conclusions

## Technologies Used

- [Python - Version 3.9.12](https://www.python.org/download/releases/3.0/)
- [numpy - Version 1.21.5](https://github.com/numpy)
- [pandas - Version 1.4.2](https://github.com/pandas-dev/pandas)
- [matplotlib - Version 3.5.1](https://github.com/matplotlib)
- [seaborn - Version 0.11.2](https://github.com/seaborn)
- [Jupyter Notebook - Version 3.3.2]()
- [JupyterLab - Version 6.4.11]()
- [Anaconda - Version 2.1.4]()

## Glossary

- EDA - Exploratory Data Analytics

## Team

- [Ganesh Kumbhar](<[url](https://course.upgrad.com/v/course/5802/profile/5889465/about)>)
- [Ashutosh Kumar Chandel](<[url](https://learn.upgrad.com/my-profile)>)
