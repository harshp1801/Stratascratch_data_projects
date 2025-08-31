# 📊 Marketing Campaign Response Prediction

This repository contains a data science project completed as part of a **take-home assignment** for a Data Scientist position at **SparkCognition**.  
The objective is to **predict whether a customer will respond to a marketing campaign** using demographic and campaign-related features.

---

## 📝 Problem Statement
An insurance company launched an email marketing campaign and provided customer information along with whether the customer **responded**.  
We are tasked with building a model that can accurately **predict campaign responses**, helping the company optimize future marketing strategies.

---

## 📂 Repository Structure

- **dataset/**: Directory containing the data files.
  - **marketing_training.csv**: Training dataset for model development.
  - **marketing_testing.csv**: Testing dataset for model evaluation.
- **solution.ipynb**: Jupyter notebook with the project code for analysis and modeling.
- **requirements.txt**: List of Python dependencies required to run the project.
- **README.md**: This file, providing project overview and setup instructions.
   
---

## 📝 Data Description
- **dataset/marketing_training.csv** – Training dataset including the target column `responded`.  
- **dataset/marketing_testing.csv** – Test dataset (without the `responded` column).  

### Key Columns
- **Demographics:** age, schooling, job, marital status, etc.  
- **Campaign interactions:** month, previous outcomes, pdays, pmonths, pastEmail, etc.  
- **Target:** `responded` (1 = customer responded, 0 = did not respond).  

---

## ⚙️ Approach

### 1. **Data Preprocessing**
- Instead of dropping the rows with null values, filled the numeric column that is **`'custAge'`** with **median** as it's more robust to skewed data and categorical columns **`'schooling'`** and **`'day_of_week'`** with **mode**.
- For columns where **`unknown`** appeared less number of time, those were imputed using statistical measures like **mode**.
- Columns where the number of **`unknown`** entries were **high**, those were treated as a separate category and were encoded to preserve information.
- Dropped columns **`schooling_illiterate`** and **`default_yes`** as doesn't adds much value to the overall analysis.
- Transforming the columns **`pdays`** and **`pmonths`** into binary to preserve underlying information.

### 2. **Handling Class Imbalance**
- Used **SMOTE** to oversample minority class in training set only to avoid data leakage. 
- Performed **threshold tuning** on predicted probabilities to optimize precision vs recall trade-off.  

### 3. **Models Tried**
- **Logistic Regression**: When combined with SMOTE Logistic Regression achieved high accuracy, but it failed to address the minority class.The best threshold achieved was 0.40 with average F1_score = 0.83.
- **AdaBoost**: With SMOTE it Performed slightly better than Logistic Regression but is sensitive to noise introduced by SMOTE, Best threshold achieved was 0.50 with average F1_score = 0.843
- **Decision Tree**: With SMOTE Decision Tree performed better than both Logistic Regression and AdaBoost, Best threshold achieved was 0.50 with average F1_score = 0.8915. But it is prone to overfitting 
- **Random forest**: Combined with SMOTE and threshold tuning it Performed better than all the previous models as it combines many decision trees, reduces chance of overfitting and captures the complex patterns in the data that was balanced after SMOTE, Best average F1 score observed is 0.936 at threshold 0.55.

**Final Choice: Random Forest** – Best trade-off between accuracy, recall, and F1.  

### 4. **Model Testing**
- Split train data into **train/test** using stratified sampling.  
- Evaluated using **Accuracy, Precision, Recall, and F1 score**.  
- Threshold tuned to **0.55**, improving minority recall while keeping precision high.  

---

## 📈 Final Results (Random Forest, Threshold = 0.55)
- **Accuracy:** ~0.94  
- **Precision:** ~0.96  
- **Recall:** ~0.94  
- **F1 Score:** ~0.94  

---

## 🔑 Key Insights
- Imbalance handling (SMOTE) was crucial to avoid biased predictions.  
- Threshold tuning improved F1 by balancing recall & precision.  
- Random Forest generalized better than single trees or boosting models.  
- Feature importance revealed campaign history & prior contact outcomes as top predictors.  

---

