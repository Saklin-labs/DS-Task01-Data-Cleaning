[readme_md.md](https://github.com/user-attachments/files/35402753/readme_md.md)
> [!NOTE]
> **IncodeVision Data Science Internship · Task 01 (Track Code: DS)**  
> A reusable end-to-end Python pipeline transforming the raw Titanic dataset (`891 × 15`) into a verified, analysis-ready (`784 × 15`) and model-ready (`784 × 24`) CSV.

<div align="center">

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/pandas/pandas-original.svg" alt="Data Cleaning Pipeline Logo" width="84" height="84" />

<h1>DS-Task01-Data-Cleaning</h1>

---

<p><strong>Feed it a messy CSV. Get a clean, model-ready dataset.</strong></p>

<p>Open-source Python data cleaning &amp; preprocessing pipeline with automated diagnostics, IQR winsorization, feature engineering, and visual reports.</p>

<p>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-5C6BC0?style=flat" alt="License: MIT" /></a>
  <img src="https://img.shields.io/badge/python-3.9%2B-5C6BC0?style=flat" alt="Python 3.9+" />
  <img src="https://img.shields.io/badge/pandas-supported-BA68C8?style=flat" alt="pandas supported" />
  <img src="https://img.shields.io/badge/seaborn-visuals-E59866?style=flat" alt="seaborn visuals" />
  <a href="data_cleaning.ipynb"><img src="https://img.shields.io/badge/jupyter-notebook-5C6BC0?style=flat" alt="Jupyter Notebook" /></a>
</p>

<p>
  <a href="#results-at-a-glance">Results</a> ·
  <a href="#pipeline-architecture">Pipeline</a> ·
  <a href="#project-structure">Structure</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#tech-stack">Tech Stack</a>
</p>

</div>

<table>
  <tr>
    <td align="center" width="33.3%">
      <img src="outputs/01_missing_before.png" alt="Missing Values Before Cleaning" width="100%" />
    </td>
    <td align="center" width="33.3%">
      <img src="outputs/02_outliers_fare.png" alt="Fare Outliers Before and After Capping" width="100%" />
    </td>
    <td align="center" width="33.3%">
      <img src="outputs/03_missing_after.png" alt="Missing Values After Cleaning" width="100%" />
    </td>
  </tr>
  <tr>
    <td align="center">
      <strong>Missing Values (Before)</strong><br />
      <sub>raw dataset · 869 missing cells · 891 rows</sub>
    </td>
    <td align="center">
      <strong>IQR Outlier Capping</strong><br />
      <sub>winsorization · 102 fare &amp; 15 age capped</sub>
    </td>
    <td align="center">
      <strong>Missing Values (After)</strong><br />
      <sub>verified clean · 0 missing cells · 784 rows</sub>
    </td>
  </tr>
</table>

---

## Results at a Glance

**Dataset:** Titanic passengers (`891 rows × 15 columns`), sourced from the open-source [seaborn-data](https://github.com/mwaskom/seaborn-data) repository.

| Metric | Raw (`titanic_raw.csv`) | Clean (`titanic_clean.csv`) | ML-Ready (`titanic_ml_ready.csv`) |
| :--- | :---: | :---: | :---: |
| **Rows** | 891 | 784 | 784 |
| **Columns** | 15 | 15 | 24 |
| **Exact Duplicate Rows** | 107 | 0 | 0 |
| **Missing Cells** | 869 | 0 | 0 |

> [!TIP]
> **Why keep 7 identical-looking rows after imputation?**  
> All `107` exact duplicates were dropped on the raw dataset (Step 2). After imputing `age` by group median, `7` distinct passengers share identical feature values because the dataset has no Passenger ID. They are intentionally preserved to avoid losing real records.

---

## Pipeline Architecture

Every transformation is deterministic, logged to `outputs/cleaning_log.txt`, and reproducible via both the script and the notebook:

| Step | Stage | What Happens |
| :---: | :--- | :--- |
| **1** | **Inspect** | Loads raw CSV, audits shape, counts duplicates (`107`) and missing cells (`869`), and saves the initial missing-value heatmap. |
| **2** | **Deduplicate** | Drops `107` exact duplicate rows and resets index (`891 → 784`). |
| **3** | **Standardize Text** | Strips whitespace and enforces consistent casing (`sex`, `who`, `alive` → lowercase; `class`, `embark_town` → Title Case; `embarked` → UPPERCASE). |
| **4** | **Drop Redundancies** | Removes duplicate-meaning columns: `alive` (`survived`), `class` (`pclass`), and `embark_town` (`embarked`). |
| **5** | **Impute Missing Values** | • `deck` (688 missing, ~77%) → filled with `"Unknown"` category.<br />• `embarked` (2 missing) → filled with column mode.<br />• `age` (177 missing) → filled with group median by `pclass` + `sex`. |
| **6** | **Optimize Data Types** | Casts `survived` & `pclass` to `int8`, `adult_male` & `alone` to `bool`, categorical columns to `category`, and rounds `age` to 1 decimal place. |
| **7** | **Cap Outliers (IQR)** | Applies $1.5 \times \text{IQR}$ winsorization without dropping rows: `fare` (`102` values outside `[-31.0, 73.2]`) and `age` (`15` values outside `[-2.0, 62.0]`). |
| **8** | **Feature Engineering & Scaling** | Creates `family_size = sibsp + parch + 1`, one-hot encodes categoricals (`drop_first=True`), and applies z-score standardization to numeric features. |
| **9** | **Validate & Export** | Asserts zero missing values, saves `titanic_clean.csv` & `titanic_ml_ready.csv`, and generates summary tables, logs, and diagnostic plots. |

---

## Project Structure

```text
DS-Task01-Data-Cleaning/
├── data/
│   ├── titanic_raw.csv          # Original raw dataset (891 × 15)
│   ├── titanic_clean.csv        # Analysis-ready cleaned dataset (784 × 15)
│   └── titanic_ml_ready.csv     # One-hot encoded & z-score scaled (784 × 24)
├── outputs/
│   ├── 01_missing_before.png    # Heatmap of missing values before cleaning
│   ├── 02_outliers_fare.png     # Boxplot comparison before & after IQR capping
│   ├── 03_missing_after.png     # Heatmap verifying zero missing values
│   ├── cleaning_summary.csv     # Before/after metrics table
│   └── cleaning_log.txt         # Step-by-step execution log
├── data_cleaning.ipynb          # Interactive walkthrough notebook with outputs
├── data_cleaning.py             # Standalone executable pipeline script
├── requirements.txt             # Python dependencies
├── .gitignore
├── LICENSE
└── README.md
```

---

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/Sohan186/DS-Task01-Data-Cleaning.git
cd DS-Task01-Data-Cleaning
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the pipeline

**Option A — Run the Python script:**
```bash
python data_cleaning.py
```

**Option B — Explore the interactive notebook:**
Open [`data_cleaning.ipynb`](data_cleaning.ipynb) directly on GitHub or launch locally:
```bash
jupyter notebook data_cleaning.ipynb
```

---

## Tech Stack

- **Language:** Python 3.9+
- **Data Manipulation:** `pandas`, `NumPy`
- **Visualization:** `matplotlib`, `seaborn`
- **Environment:** Jupyter Notebook

---

<div align="center">

Made by **Saklin Mostak Molla** · Released under the [MIT License](LICENSE)

</div>
