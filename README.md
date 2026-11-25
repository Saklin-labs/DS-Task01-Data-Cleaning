[!NOTE]
IncodeVision Data Science Internship · Task 01 (Track Code: DS)A reusable end-to-end Python pipeline transforming the raw Titanic dataset (891 × 15) into a verified, analysis-ready (784 × 15) and model-ready (784 × 24) CSV.Results at a GlanceDataset: Titanic passengers (891 rows × 15 columns), sourced from the open-source seaborn-data repository.MetricRaw (titanic_raw.csv)Clean (titanic_clean.csv)ML-Ready (titanic_ml_ready.csv)Rows891784784Columns151524Exact Duplicate Rows10700Missing Cells86900[!TIP]
Why keep 7 identical-looking rows after imputation?All 107 exact duplicates were dropped on the raw dataset (Step 2). After imputing age by group median, 7 distinct passengers share identical feature values because the dataset has no Passenger ID. They are intentionally preserved to avoid losing real records.Pipeline ArchitectureEvery transformation is deterministic, logged to outputs/cleaning_log.txt, and reproducible via both the script and the notebook:StepStageWhat Happens1InspectLoads raw CSV, audits shape, counts duplicates (107) and missing cells (869), and saves the initial missing-value heatmap.2DeduplicateDrops 107 exact duplicate rows and resets index (891 → 784).3Standardize TextStrips whitespace and enforces consistent casing (sex, who, alive → lowercase; class, embark_town → Title Case; embarked → UPPERCASE).4Drop RedundanciesRemoves duplicate-meaning columns: alive (survived), class (pclass), and embark_town (embarked).5Impute Missing Values• deck (688 missing, ~77%) → filled with "Unknown" category.• embarked (2 missing) → filled with column mode.• age (177 missing) → filled with group median by pclass + sex.6Optimize Data TypesCasts survived & pclass to int8, adult_male & alone to bool, categorical columns to category, and rounds age to 1 decimal place.7Cap Outliers (IQR)Applies $1.5 \times \text{IQR}$ winsorization without dropping rows: fare (102 values outside [-31.0, 73.2]) and age (15 values outside [-2.0, 62.0]).8Feature Engineering & ScalingCreates family_size = sibsp + parch + 1, one-hot encodes categoricals (drop_first=True), and applies z-score standardization to numeric features.9Validate & ExportAsserts zero missing values, saves titanic_clean.csv & titanic_ml_ready.csv, and generates summary tables, logs, and diagnostic plots.Project StructureDS-Task01-Data-Cleaning/
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
Quick Start1. Clone the repositorygit clone https://github.com/Sohan186/DS-Task01-Data-Cleaning.git
cd DS-Task01-Data-Cleaning
2. Install dependenciespip install -r requirements.txt
3. Run the pipelineOption A — Run the Python script:python data_cleaning.py
Option B — Explore the interactive notebook:
Open data_cleaning.ipynb directly on GitHub or launch locally:jupyter notebook data_cleaning.ipynb
Tech StackLanguage: Python 3.9+Data Manipulation: pandas, NumPyVisualization: matplotlib, seabornEnvironment: Jupyter NotebookMade by Saklin Mostak Molla · Released under the MIT License
