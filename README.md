# 🏥 Data Cleanser — Healthcare Patient Data Preprocessing

## 🧹 Missing Values & Outlier Handling

A complete healthcare data-cleaning and preprocessing project using Python.

---

## 🎥 Project Video

▶️ **[Watch Project Explanation Video](https://drive.google.com/file/d/1WTzt21a59AQPrQ0SNECbcdCrHaXNvWvP/view?usp=drive_link)**

---

## 📌 Project Overview

This project focuses on cleaning and preprocessing a healthcare patient dataset containing missing values, duplicate records, and potential outliers.

The main objective is to transform the raw dataset into a clean and reliable dataset that can be used for further statistical analysis and machine-learning applications.

---

## 🎯 Project Objectives

- Load and understand the dataset
- Inspect dataset structure
- Check data types
- Check duplicate records
- Identify missing values
- Calculate missing-value percentages
- Apply different missing-value imputation techniques
- Compare imputation methods
- Identify potential outliers
- Apply different outlier-treatment methods
- Compare outlier-treatment methods
- Validate the final cleaned dataset
- Export the final cleaned CSV file

---

## 📊 Dataset

The project uses a healthcare patient dataset containing patient-level health information.

### Main Features

| Column | Description |
|---|---|
| `patient_id` | Unique patient identifier |
| `age` | Patient age |
| `gender` | Patient gender |
| `region` | Patient region |
| `bmi` | Body Mass Index |
| `blood_pressure` | Blood pressure measurement |
| `cholesterol` | Cholesterol level |
| `glucose` | Glucose level |
| `disease_risk` | Disease-risk target variable |

### Target Variable

`disease_risk`

- `0` → Low Risk
- `1` → High Risk

---

# 🔄 Project Workflow

```text
Raw Healthcare Dataset
        ↓
Dataset Inspection
        ↓
Duplicate Check
        ↓
Missing Value Analysis
        ↓
Missing Value Imputation
        ↓
Imputation Comparison
        ↓
Outlier Identification
        ↓
Outlier Treatment
        ↓
Method Comparison
        ↓
Final Dataset Validation
        ↓
Clean CSV Dataset
```

---

# 🔍 1. Dataset Inspection

The dataset is loaded using Pandas.

```python
import pandas as pd

df = pd.read_csv("patient_health_records_dirty.csv")
```

Initial inspection is performed using:

```python
df.head()
df.shape
df.columns
df.info()
df.describe()
```

---

# 🔁 2. Duplicate Data Check

Duplicate rows are checked using:

```python
df.duplicated().sum()
```

Duplicate patient IDs are also checked:

```python
df["patient_id"].duplicated().sum()
```

---

# ❓ 3. Missing Value Analysis

Missing values are identified using:

```python
df.isnull().sum()
```

A heatmap can be used for visual analysis:

```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(10, 6))
sns.heatmap(df.isnull(), cbar=False, yticklabels=False)
plt.title("Missing Values Heatmap")
plt.show()
```

---

# 🧩 4. Missing Value Imputation

Different methods are applied to handle missing values.

## Simple Imputation

```python
from sklearn.impute import SimpleImputer

median_imputer = SimpleImputer(strategy="median")
mode_imputer = SimpleImputer(strategy="most_frequent")
```

## Missing Indicator

```python
df["Age_missing"] = df["age"].isnull().astype(int)
```

## KNN Imputation

```python
from sklearn.impute import KNNImputer

knn_imputer = KNNImputer(n_neighbors=3)
df_knn = knn_imputer.fit_transform(df_numeric)
```

Different values of `k` can be experimented with:

```text
k = 2
k = 3
k = 5
k = 7
```

## MICE-Style Iterative Imputation

MICE stands for **Multiple Imputation by Chained Equations**.

```python
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

mice_imputer = IterativeImputer(random_state=42)
df_mice = mice_imputer.fit_transform(df_numeric)
```

---

# 📊 5. Imputation Comparison

| Method | Approach |
|---|---|
| Simple Imputation | Median / Most Frequent |
| Most Frequent | Most Common Category |
| Random Sample | Random Observed Value |
| KNN | Similar Observations |
| MICE | Iterative Estimation |

---

# 📈 6. Outlier Detection

Potential outliers are investigated across relevant numerical columns.

```python
numerical_columns = [
    "age",
    "bmi",
    "blood_pressure",
    "cholesterol",
    "glucose"
]
```

Boxplots are used for visual analysis:

```python
plt.figure(figsize=(10, 6))
sns.boxplot(data=df[numerical_columns])
plt.title("Numerical Columns — Outlier Analysis")
plt.show()
```

---

# 📐 7. Z-Score Method

```python
from scipy.stats import zscore

z_scores = df[zscore_columns].apply(zscore)
```

Extreme observations are identified using:

```text
|Z| > 3
```

---

# 📦 8. IQR Method

```text
IQR = Q3 − Q1
```

```text
Lower Bound = Q1 − 1.5 × IQR
Upper Bound = Q3 + 1.5 × IQR
```

Example:

```python
Q1 = df[column].quantile(0.25)
Q3 = df[column].quantile(0.75)

IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
```

---

# 📊 9. Percentile Method

For this project:

```text
Lower Limit = 1st Percentile
Upper Limit = 99th Percentile
```

```python
lower = df[column].quantile(0.01)
upper = df[column].quantile(0.99)
```

---

# ✂️ 10. Winsorization

Winsorization reduces the influence of extreme observations without deleting complete patient records.

```text
1st Percentile  → Lower Cap
99th Percentile → Upper Cap
```

---

# 📊 11. Outlier Treatment Comparison

| Method | Treatment |
|---|---|
| Z-Score | Detect and remove extreme rows |
| IQR | Detect and remove values outside bounds |
| Percentile | Detect extreme percentile values |
| Winsorization | Cap extreme values |

---

# 🧪 12. Final Dataset

After missing-value treatment and outlier treatment, the final cleaned dataset is prepared.

```python
final_df
```

---

# ✅ 13. Final Validation

### Missing Values

```python
final_df.isnull().sum()
```

### Duplicate Rows

```python
final_df.duplicated().sum()
```

### Duplicate Patient IDs

```python
final_df["patient_id"].duplicated().sum()
```

### Data Types

```python
final_df.dtypes
```

### Dataset Shape

```python
final_df.shape
```

### Target Distribution

```python
final_df["disease_risk"].value_counts()
```

---

# 💾 14. Export Final Clean Dataset

```python
final_df.to_csv(
    "patient_health_records_clean.csv",
    index=False
)
```

---

# 🛠️ Technologies Used

- 🐍 Python
- 🐼 Pandas
- 🔢 NumPy
- 📊 Matplotlib
- 📈 Seaborn
- 🤖 Scikit-learn
- 📐 SciPy
- 📓 Jupyter Notebook

---

# 📁 Project Structure

```text
Data-Cleanser/
│
├── Data_Cleanser_Final.ipynb
├── patient_health_records_dirty.csv
├── patient_health_records_clean.csv
├── requirements.txt
└── README.md
```

---

# 🚀 How to Run the Project

### 1. Clone the Repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
```

### 2. Open the Project Folder

```bash
cd Data-Cleanser
```

### 3. Install Required Libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter
```

### 4. Start Jupyter Notebook

```bash
jupyter notebook
```

### 5. Open the Notebook

```text
Data_Cleanser_Final.ipynb
```

---

# 🎯 Final Outcome

The project demonstrates a complete healthcare data-preprocessing workflow.

### Missing Values

Handled using:

- Simple Imputation
- Most Frequent Imputation
- Random Sample Imputation
- KNN Imputation
- MICE-Style Iterative Imputation

### Outliers

Analyzed and treated using:

- Z-Score
- IQR
- Percentile
- Winsorization

### Final Result

A cleaned healthcare dataset is generated and exported as:

```text
patient_health_records_clean.csv
```

---

# 🎥 Project Demonstration

👉 **[Watch Project Video](https://drive.google.com/file/d/1g7JMIvaslyv5zHR1pPWz3B6KY2qL4Px7/view?usp=drive_link)**

---

# 👨‍💻 Author

## Mayur Makvana

Data Science & Machine Learning Learner

---

## ⭐ Project Highlights

- 🧹 Complete Data Cleaning Pipeline
- 📊 Missing Value Analysis
- 🧩 Multiple Imputation Techniques
- 🤖 KNN Imputation
- 🔄 MICE-Style Iterative Imputation
- 📈 Outlier Detection
- 📐 Z-Score Analysis
- 📦 IQR Method
- 📊 Percentile Method
- ✂️ Winsorization
- 🔍 Method Comparison
- ✅ Final Dataset Validation
- 💾 Clean CSV Export

---

## 📜 Conclusion

This project demonstrates how raw healthcare data can be systematically cleaned and prepared for downstream data analysis and machine-learning tasks.

The project focuses on understanding the problem, applying multiple preprocessing techniques, comparing their results, and validating the final cleaned dataset before exporting it.
