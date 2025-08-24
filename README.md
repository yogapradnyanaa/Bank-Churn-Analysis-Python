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


## 🔍 Analyze

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


