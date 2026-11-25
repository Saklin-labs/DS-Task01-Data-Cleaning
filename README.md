# Titanic Dataset — Data Cleaning & Preprocessing

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue?logo=python" />
  <img src="https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas" />
  <img src="https://img.shields.io/badge/NumPy-Computing-013243?logo=numpy" />
  <img src="https://img.shields.io/badge/Status-Completed-success" />
</p>

<p align="center">
  <b>A complete data preprocessing workflow that transforms raw Titanic passenger data into clean, structured, and machine-learning-ready datasets.</b>
</p>

---

## Project Overview

Real-world datasets often contain missing values, duplicate records, inconsistent information, and outliers. These issues can negatively affect data analysis and machine learning performance.

This project demonstrates an end-to-end data cleaning and preprocessing workflow using the Titanic dataset. It focuses on improving data quality, creating meaningful features, and preparing structured datasets for further analysis and machine learning applications.

## Key Results

| Metric | Result |
|---|---:|
| Original records | 891 |
| Duplicate records removed | 107 |
| Final cleaned records | 784 |
| Missing cells before cleaning | 869 |
| Missing cells after cleaning | 0 |

## Technical Workflow

1. **Data Inspection**
   - Loaded and examined the raw dataset.
   - Checked data types, dimensions, missing values, and duplicates.

2. **Data Cleaning**
   - Removed duplicate records.
   - Handled missing values using suitable imputation strategies.
   - Removed redundant columns.

3. **Missing Value Treatment**
   - `Age`: Median imputation grouped by passenger class and sex.
   - `Embarked`: Mode imputation.
   - `Deck`: Missing values represented as `Unknown`.

4. **Outlier Treatment**
   - Applied IQR-based capping to `Fare` and `Age`.

5. **Feature Engineering**
   - Created `family_size`.
   - Generated capped numerical features.

6. **Machine Learning Preparation**
   - Applied one-hot encoding.
   - Applied z-score standardization.
   - Exported processed datasets.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Structure

```text
DS-Task01-Data-Cleaning/
│
├── data/
│   ├── titanic_raw.csv
│   ├── titanic_clean.csv
│   └── titanic_ml_ready.csv
│
├── outputs/
│   ├── Visualizations
│   ├── cleaning_summary.csv
│   └── cleaning_log.txt
│
├── data_cleaning.ipynb
├── data_cleaning.py
├── requirements.txt
├── .gitignore
└── README.md
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Saklin-labs/DS-Task01-Data-Cleaning.git
cd DS-Task01-Data-Cleaning
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

Activate it:

**Windows**
```bash
venv\Scripts\activate
```

**macOS / Linux**
```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the project

```bash
python data_cleaning.py
```

Alternatively, open `data_cleaning.ipynb` in Jupyter Notebook.

## Skills Demonstrated

- Data Cleaning and Data Quality Assessment
- Missing Value Analysis and Imputation
- Duplicate Detection and Removal
- Outlier Detection and Treatment
- Feature Engineering
- Categorical Encoding
- Numerical Standardization
- Exploratory Data Analysis

## Important Note

Some records may become identical after missing-value imputation. They were not automatically removed because the dataset does not provide a unique passenger identifier that would establish them as duplicate passengers.

## Author

**Saklin Mostak Molla**

B.Sc. Data Science Student | Aspiring Data Scientist

- GitHub: [@Saklin-labs](https://github.com/Saklin-labs)
- LinkedIn: [Saklin Molla](https://www.linkedin.com/in/saklin-molla-824b292b3/)

---

<p align="center">
  <i>Turning raw data into reliable, analysis-ready information.</i>
</p>
