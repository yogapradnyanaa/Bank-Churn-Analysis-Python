# 🏦 Bank Customer Churn Study Case

## 📌 Introduction
The **Bank Customer Churn dataset** contains information about customers, including age, gender, location, tenure, credit card ownership, number of bank products used, account balance, and whether they exited (churned) or remained with the bank.  

Bank management aims to reduce churn rates, since retaining existing customers is generally more cost-effective than acquiring new ones.  

By analyzing this dataset, the bank can identify patterns and key factors influencing churn, enabling them to take strategic actions to improve customer retention.

## 1. 🔎 Ask

In this phase, the main goal is to define the **business task**. The steps include:

### A. Identify the Business Task  

1. **What is the problem?**  
   - The bank is losing customers, leading to decreased revenue and higher costs of acquiring new customers.  

2. **Understand the Context**  
   - **Objective:** Identify the factors and patterns influencing customer churn.  
   - **Stakeholder:** Marketing Manager.  

3. **Formulate Business Questions**  
   - What factors drive customers to churn?  
   - Which customer segments have the highest churn risk?  
   - Do variables such as age, balance, number of products used, or activity status significantly affect churn?  

---

**The Business Task:** Analyze customer data to identify the factors and patterns that influence churn, and build a predictive model to help the bank design effective strategies to minimize customer churn.

---

### B. Main Questions to Explore  

1. What factors most influence customers to churn?  
2. What are the profiles or patterns of high-risk churn customers compared to loyal ones?  
3. How can the bank leverage these insights to design effective retention strategies or programs?

## 2. 🗂 Prepare

In this phase, data collection, understanding, and feasibility assessment are conducted before proceeding to analysis.  

- **IsActiveMember** → `1 = Active`, `0 = Not Active`  
- **Exited** → `1 = Churn`, `0 = Loyal`  

---

### 📊 Dataset Overview
The dataset comes from historical customer data of a bank, containing demographic details, membership status, and financial activity.  
This **tabular dataset** is used to analyze the factors influencing customer churn (i.e., customers leaving the bank).  

- **Size:** 10,000 rows, each representing a unique customer  
- **Features:**  
  - **Demographics:** Age, Gender, Country of residence  
  - **Account Information:** Tenure (years as a customer), Balance, Number of products used, Credit card status, Activity status  
  - **Target Variable:** `Exited` → indicates whether the customer churned (`1`) or not (`0`)  

This dataset enables identification of patterns and characteristics of high-risk churn customers, helping the bank design more effective retention strategies.  

---

### ✅ Data Credibility Analysis – *ROCCC Approach*  
*(Reliable, Original, Comprehensive, Current, Cited)*  

- 📉 **Reliable (Medium):** The dataset contains 10,000 customers with variables such as age, balance, number of products, credit card ownership, and churn status. The sample size is large enough to support stable statistical analysis. However, the absence of detailed collection methodology introduces potential bias (e.g., geographic or customer-type bias).  

- 📉 **Original (Low):** This is an open dataset likely sourced from third parties, not directly from a specific bank. Therefore, its accuracy may differ from real-world customer data of an actual institution.  

- 📈 **Comprehensive (Medium):** The dataset includes relevant variables for churn analysis (demographics, account activity, product ownership, activity status, etc.). However, it lacks additional data such as service interaction history, reasons for churn, or detailed transaction behavior, which could enrich the analysis.  

- 📈 **Current (Medium):** The year of data collection is not specified, making it difficult to confirm temporal relevance. If the dataset is outdated, customer behavior may have shifted due to banking technology advancements and digital finance trends.  

- 📉 **Cited (Low):** The dataset lacks formal documentation or verifiable publication sources. No information is provided regarding who collected the data, when it was gathered, or how it was validated. This makes it difficult to fully assess validity and replicability of analysis results.  


## 3. ⚙️ Process

In this phase, the dataset is cleaned and prepared for analysis. This includes removing duplicates, handling missing values, and fixing formatting or consistency issues.

---

### A. Importing the Dataset
After uploading the dataset, the first step is to **explore its structure** and identify potential issues that may affect analysis.

---

### B. Exploring the Dataset  

1. **Data Structure**  
- The dataset contains **10,000 rows** and **18 columns**.  
- Data types: 12 numeric columns, 4 categorical columns (object), 2 float columns.  
- No missing values (`missing values = 0` across all columns).  
- No duplicate records (`duplicates = 0`).  

2. **Identifier Variables**  
- `RowNumber` and `CustomerId` are unique identifiers for each row → not useful for prediction.  
- `Surname` has **2,932 unique values** (likely family names) → not relevant for churn prediction.  

3. **Numeric Variables**  
- **CreditScore:** Range 350–850, mean ~650. Distribution similar to typical credit scoring (higher = better).  
- **Age:** Range 18–92, mean ~38, with many clustered between ages 30–50.  
- **Tenure:** Range 0–10 years, mean ~5 years → most customers have moderate length of stay.  
- **Balance:** Many customers with zero balance; maximum ~250k, mean ~76k.  
- **EstimatedSalary:** Even distribution between 0–200k → likely synthetic or scaled.  
- **Point Earned:** Range 119–1000, mean ~606.  

4. **Categorical Variables**  
- **Geography:** 3 countries represented → limited geographic scope.  
- **Gender:** 2 categories, relatively balanced distribution.  
- **HasCrCard, IsActiveMember, Exited, Complain:** Binary (0/1).  
- **Card Type:** 4 card types (e.g., Diamond, Gold, etc.).  
- **Satisfaction Score:** Ordinal scale from 1–5.  

---

### C. Cleaning the Dataset  

1. **Duplicate Records**  
- Checked using duplication test → **no duplicates found**. ✅  

2. **Missing Data**  
- Checked for null values → **no missing values** in the dataset. ✅  

3. **Incorrect Data**  
- **Mistyped Numbers:** Summary statistics reviewed, no unusual values detected.  
- **Misspelled Words (Categorical Variables):**  
  - Checked unique values in `Geography`, `Gender`, `Card Type`.  
  - Found irregular characters (`?`) in `Surname` → cleaned by removing them.  

4. **Inconsistent Data**  
- **Extra Spaces:** Verified across all string columns → no extra spaces detected. ✅  
- **Mismatched Data Types:** All variables are already in the correct format. ✅  
- **Column Names:** All column names are consistent and properly labeled. ✅  


## 4. 🔍 Analyze

In this phase, **Exploratory Data Analysis (EDA)** is performed to better understand the dataset before building predictive models.  
EDA is crucial because:  
- It helps validate assumptions about the data.  
- It identifies patterns and anomalies that may affect the analysis.  
- It provides early business insights to guide decision-making.  
- It ensures data is well-understood before applying statistical or machine learning models.  

---

### A. Univariate Analysis
Focuses on analyzing each variable individually to understand its distribution, frequency, and summary statistics.  

```python
customer_churn.info()
```

<img width="563" height="619" alt="image" src="https://github.com/user-attachments/assets/c105fd6c-a14e-4bf7-88ab-c95a9c25a97b" />

#### 🔹 Numerical Variables

There are **6 numerical variables** in the dataset:

1. **CreditScore** → Customer’s credit score (range 350–850).  
2. **Age** → Age of the customer (range 18–92 years).  
3. **Tenure** → Number of years the customer has been with the bank (0–10 years).  
4. **Balance** → Account balance of the customer (0 – ~250,000).  
5. **EstimatedSalary** → Estimated annual salary of the customer (0 – 200,000).  
6. **Point Earned** → Loyalty points earned by the customer (119 – 1000). 

#### For numerical variables, I will check whether their values follow a **normal distribution** or not.

<img width="915" height="773" alt="image" src="https://github.com/user-attachments/assets/a6ab2c0e-8585-41a1-ae4b-9c2e6bca5d8a" />

#### 🔹 Numerical Variables Distribution  

Based on the histogram analysis:  

- **CreditScore** → Distribution is close to normal, with most customers in the 600–700 range. A few outliers exist below 400 and above 850, indicating generally good credit profiles.  
- **Age** → Right-skewed distribution, peaking at ages 30–40. The number of customers declines after age 50, showing dominance of the productive age group.  
- **Tenure** → Relatively evenly distributed between 1–10 years, with a slight drop in the first year.  
- **Balance** → Highly right-skewed, showing two distinct groups: customers with zero balance and customers with large balances.  
- **EstimatedSalary** → Almost evenly distributed across the income range, reflecting diversity in customer income levels.  
- **Point Earned** → Relatively uniform distribution with a slight peak between 300–800 points, suggesting common target or achievement thresholds among customers.  


#### 🔹 Categorical Variables Distribution  

<img width="645" height="557" alt="image" src="https://github.com/user-attachments/assets/d50526e7-5612-49b0-8255-5581cee9f619" />

Based on the analysis of categorical variables:  

- **Geography** → The majority of customers come from **France**.  
- **Gender** → Distribution between male and female customers is nearly balanced.  
- **Number of Products** → Most customers own **1–2 products**, while ownership of **3–4 products** is very rare.  
- **HasCrCard** → Credit card ownership is relatively high among customers.  
- **IsActiveMember** → The proportion of active vs non-active members is almost equal.  
- **Exited** → The majority of customers did **not churn (Exited = 0)**.  
- **Complain** → Most customers have **never filed a complaint (Complain = 0)**.  
- **Satisfaction Score** → Evenly distributed across the **1–5 scale**.  
- **Card Type** → Distribution is relatively balanced among **Diamond, Gold, Silver, and Platinum** categories.


### B. Bivariate Analysis  

In this step, the focus is on examining the relationship between **two variables** usually comparing independent variables with the target variable (`Exited`).  

Since the target variable is binary (0 = loyal, 1 = churn), the following visualization techniques are used:

- **Bar Charts** → to compare the proportion of churned vs loyal customers across categorical variables (e.g., Gender, Geography, Card Type, IsActiveMember).  
- **Box Plots** → to compare the distribution of numerical variables (e.g., Age, Balance, EstimatedSalary, CreditScore) between churned and loyal customers.  

These visualizations make it easier to detect patterns and highlight factors that may influence customer churn.  

#### 🔹 Bivariate Analysis – Numerical Variables vs Churn 

<img width="995" height="452" alt="image" src="https://github.com/user-attachments/assets/91ee6153-4d3d-4962-a63a-ec38c33a6023" />

Based on the boxplot analysis between numerical variables and the target (`Exited`):  

- **CreditScore** → Customers who churn (`Exited = 1`) tend to have slightly lower credit scores compared to loyal customers.  
- **Age** → Shows a clear difference: churned customers are generally older (median above 40), while loyal customers are around their mid-30s.  
- **Tenure** → No significant difference in distribution between churned and loyal customers.  
- **Balance** → Median values are similar for both groups, though churned customers show slightly higher variability.  
- **EstimatedSalary** → Distribution is almost identical between churned and non-churned groups, suggesting limited influence on churn.  
- **Point Earned** → Both groups show similar patterns, but churned customers tend to have a slightly higher median of points.  

📌 **Key Insight:** Among the numerical variables, **Age** appears to be the most distinguishing factor influencing customer churn.

#### 🔹 Bivariate Analysis – Categorical Variables vs Churn 

<img width="940" height="427" alt="image" src="https://github.com/user-attachments/assets/47d58cdc-22c3-45b2-821b-5a9844f79c77" />

Based on the visualization between categorical variables and the target (`Exited`):  

- **Geography** → Churn is more frequent among customers from **Germany**, while customers from **France** show the lowest churn proportion.  
- **Gender** → Female customers show a slightly higher churn rate compared to males.  
- **NumOfProducts** → Customers with **1 product** have the highest churn, while those with 3–4 products are fewer in number but still experience churn.  
- **HasCrCard** → Customers who own a credit card churn less frequently compared to those without a card.  
- **IsActiveMember** → Inactive customers are significantly more likely to churn than active ones.  
- **Complain** → Customers who have submitted complaints exhibit higher churn rates.  
- **Satisfaction Score** → Churn occurs at all satisfaction levels (1–5) with relatively similar proportions, showing no strong pattern.  
- **Card Type** → Churn is relatively evenly distributed across **Diamond, Gold, Silver, and Platinum** cardholders, with no significant differences.  

📌 **Key Insight:** Categorical variables such as **Geography** and **IsActiveMember** appear to be strong indicators of churn behavior, while variables like **Satisfaction Score** and **Card Type** show weaker influence. 


#### 🔹 Correlation Analysis  

Correlation analysis is part of **bivariate analysis**, as it examines the relationship between **two numerical variables**.  
This step is important because: 

- It helps identify whether variables move together (positive correlation) or in opposite directions (negative correlation).
- It provides early insights into which variables are potentially influential or irrelevant in explaining churn.

##### 1. Continuous Numerical Variables vs Exited  
<img width="663" height="223" alt="image" src="https://github.com/user-attachments/assets/881d06ea-6638-4721-a323-690ea75258c2" />

##### 2. Categorical Variables vs Exited  
<img width="577" height="255" alt="image" src="https://github.com/user-attachments/assets/bf7736e2-5a20-4f3d-8194-b975bf2d9c93" />


Correlation analysis was conducted to examine how both **numerical** and **categorical** variables relate to customer churn (`Exited`).  

- From the **numerical variables**, **Age** shows the strongest correlation with churn (**r = 0.285**), indicating that older customers are more likely to leave. **Balance** also has a weak positive correlation (**r = 0.119**). Other variables such as **EstimatedSalary, Point Earned, Tenure, and CreditScore** show very weak relationships, suggesting minimal impact on churn.  

- From the **categorical variables**, **Complain** has an extremely strong association with churn (**Cramér’s V = 0.995**), making it the most dominant factor. **NumOfProducts** has a moderate relationship (**0.387**), while **Geography** shows a weak one (**0.173**). Other features such as **IsActiveMember** and **Gender** have very weak influence, and **Card Type, HasCrCard, and Satisfaction Score** show almost no meaningful effect.  

📌 **Key Insight:**  
The most influential factors for churn are **Complain**, followed by **NumOfProducts** and **Age**. These variables should be prioritized in further analysis and predictive modeling to improve customer retention strategies.


### C. Multivariate Analysis  

Multivariate analysis is performed to examine the interaction between **three or more variables simultaneously**.  

### 🔧 Data Encoding for Logistic Regression  

Before fitting the logistic regression model, categorical variables must be encoded into numerical format.  
Steps performed:  

1. **One-Hot Encoding** applied to `Geography`, `Gender`, and `Card Type`.  
2. **Identifier columns** (`RowNumber`, `CustomerId`, `Surname`) dropped since they do not contribute to prediction.  
3. **Boolean columns** converted into integers (`0/1`) to ensure all predictors are numeric.  

This prepares the dataset for logistic regression modeling.  

```python
# One-Hot Encoding categorical variables
df_encoded = pd.get_dummies(
    customer_churn,
    columns=['Geography', 'Gender', 'Card Type'],
    drop_first=False,
    dtype=bool
)

print(df_encoded.head())
print(df_encoded.dtypes.head(15))

# Drop non-predictor identifier columns
df_encoded = df_encoded.drop(columns=["RowNumber", "CustomerId", "Surname"], errors="ignore")

# Convert boolean columns into integers (0/1)
bool_cols = df_encoded.select_dtypes(include="bool").columns
df_encoded[bool_cols] = df_encoded[bool_cols].astype(int)

# Check data types after conversion
print(df_encoded.dtypes.tail(10))
```

```python
# === LOGIT Multivariate (robust-ready) ===
import pandas as pd
import numpy as np
import statsmodels.api as sm

# >>> Assumes `df` contains the raw or semi-encoded data (e.g., Churn_Modelling.csv)
# If needed: df = pd.read_csv("Churn_Modelling.csv")

# 1) Drop non-informative identifiers
df = df.drop(columns=["RowNumber", "CustomerId", "Surname"], errors="ignore")

# 2) Ensure target exists
if "Exited" not in df.columns:
    raise ValueError("Target column 'Exited' not found in df.")

# 3) One-Hot Encode available categoricals (exclude target)
cat_cols = df.select_dtypes(include=["object", "category"]).columns.tolist()
cat_cols = [c for c in cat_cols if c != "Exited"]
if len(cat_cols) > 0:
    df = pd.get_dummies(df, columns=cat_cols, drop_first=True)

# 4) Split X, y
y = df["Exited"].astype(int)
X = df.drop(columns=["Exited"]).copy()

# 5) Type fixes:
#    - bool -> int (0/1)
#    - force all to numeric (weird strings -> NaN), then median-impute
bool_cols = X.select_dtypes(include="bool").columns
if len(bool_cols) > 0:
    X[bool_cols] = X[bool_cols].astype(int)

X = X.apply(pd.to_numeric, errors="coerce")
X = X.fillna(X.median(numeric_only=True))

# 6) Remove zero-variance columns (constant) to avoid singular matrix
zero_var = [c for c in X.columns if X[c].nunique() <= 1]
if zero_var:
    print("Dropping zero-variance columns:", zero_var)
    X = X.drop(columns=zero_var)

# (Optional) remove perfectly duplicated columns
dup_cols = X.T[X.T.duplicated()].index.tolist()
if dup_cols:
    print("Dropping duplicate columns:", dup_cols)
    X = X.drop(columns=dup_cols)

# 7) Fit Logit
X = sm.add_constant(X, has_constant="add")
model = sm.Logit(y, X)
result = model.fit(disp=False)

print(result.summary())

# 8) Odds Ratio + p-value
odds = pd.DataFrame({
    "Variable": result.params.index,
    "Coef": result.params.values,
    "OddsRatio": np.exp(result.params.values),
    "p_value": result.pvalues.values
}).sort_values("p_value")

print("\n=== Odds Ratio & Significance (sorted by p-value) ===")
print(odds.to_string(index=False))
```

<img width="627" height="489" alt="image" src="https://github.com/user-attachments/assets/21cda3e9-2b9a-424a-ae33-af8d9e83b3e0" />

<img width="417" height="318" alt="image" src="https://github.com/user-attachments/assets/2810aa28-65f9-4dd1-8e4e-a9ecd74dea38" />

### Key Findings from Logistic Regression  

Based on the logistic regression analysis, there are three main factors that strongly influence customer churn:  

1. **Complain History**  
   - Customers who have submitted complaints are **far more likely to churn** compared to those who have not.  
   - This is the single most dominant driver of churn.  

2. **Age**  
   - Older customers show a significantly higher probability of leaving.  
   - The likelihood of churn increases by roughly **10% for every additional year of age**.  

3. **Active Member Status**  
   - Active customers are much more loyal.  
   - Being an active member greatly reduces the chance of churn.  

Other variables such as **CreditScore, Tenure, Balance, Number of Products, Card Type, EstimatedSalary, Gender, and Geography** show no significant impact on churn.  

📌 **Strategic Implication:**  
Churn management strategies should prioritize:  
- Reducing and addressing **customer complaints**.  
- Providing targeted retention programs for **older customers**.  
- Encouraging customers to remain **active users** of banking services.


## 📢 5. Share  

At this stage, the goal is to **communicate findings clearly** to stakeholders using accessible visualizations.  
While the detailed statistical analysis (correlation, logistic regression) was done in the **Analyze** step, here we focus on creating **simple, descriptive graphics** to help non-technical audiences understand the customer profile.  

### [Bank Churn Analysis – Tableau Public](https://public.tableau.com/views/BankChurnAnalysis_17556692933930/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

<img width="957" height="637" alt="image" src="https://github.com/user-attachments/assets/aee6a3a9-7c30-481a-be37-51b74c386129" />


## ✅ 6. Act  

The logistic regression analysis revealed that there are **three main factors** most strongly associated with customer churn.  
First, **complain history** has an extremely large impact: customers who have submitted complaints are far more likely to churn than those who have not.  
Second, **age** is also significant: the older the customer, the higher the probability of churn, with an increase of about 10% for every additional year.  
Third, **activity status** plays a decisive role: active members are much more loyal, while inactive ones are at far greater risk of leaving.  

Other variables such as credit score, tenure, balance, number of products, card type, estimated salary, gender, and even geography did not show strong statistical significance in the model.  
However, exploratory analysis still pointed out some relevant patterns — for instance, customers in **Germany** exhibited higher churn rates compared to France and Spain, and customers holding **three or more products** tended to churn more, which may reflect negative effects of over-cross-selling.  

📌 **Strategic Recommendations**  
- **Address complaints promptly**: treat them as the clearest early warning signal for churn.  
- **Focus on older customers**: design retention programs, loyalty rewards, or dedicated services to reduce their churn risk.  
- **Encourage active usage**: launch re-engagement campaigns, targeted promotions, or usage-based incentives to keep members active.  
- **Strengthen service in Germany**: implement more proactive and localized customer support.  
- **Be selective with cross-selling**: avoid overwhelming customers with too many products at once.  

By acting on these findings, the bank can directly tackle the strongest churn drivers while also addressing secondary risk factors, leading to reduced churn rates, higher customer lifetime value, and stronger long-term loyalty.  
