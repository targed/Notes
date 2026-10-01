## 1. The Operational Reality: "Garbage In, Garbage Out" (Slide 2)
  
  Slide 2 presents a foundational truth of industrial and academic machine learning:
  
  $$\mathbf{\text{"Practitioners report 60–80\% of project time is spent on data preparation."}}$$
  
  ```
              Production Machine Learning Time Allocation
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Data Cleaning & Pipeline Preparation  ████████████████████ 60%        │
  │ Dataset Collection & Label Ingestion  ██████ 19%                       │
  │ Pattern Mining & Exploratory Analysis ███ 11%                          │
  │ Algorithm Tuning & Model Selection    ██ 10%                           │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 2 emphasizes two primary failure modes:
  1. **Model Dependency:** Statistical estimators and neural networks possess zero intrinsic common sense. An algorithm minimizes empirical risk strictly over the mathematical matrices fed into its optimization loop:
  $$\hat{\theta} = \arg\min_\theta \frac{1}{N}\sum_{i=1}^N \mathcal{L}(y_i, f_\theta(x_i))$$
  If input features $x_i$ encode unparsed string noise, corrupted sentinel values, or duplicated rows, the model learns those artifacts as valid predictive signals.
  2. **Systemic Bias & Silent Leakage:** Unprincipled data cleaning (such as dropping rows non-randomly or scaling features globally before splitting) systematically alters the underlying joint density $P(X, Y)$, creating optimistic offline metrics that collapse in production.
  
  ---
## 2. The Pathologies of Real-World Enterprise Data (Slide 3)
  
  Slide 3 illustrates an enterprise purchase order sheet to highlight why automated parsers fail when ingesting raw corporate records:
  
  ```
                      Anatomy of Corrupted Enterprise Ingestion
         Raw Spreadsheet Artifact                   Underlying Systems Defect
  ┌──────────────────────────────────────────┐    ┌──────────────────────────────────────┐
  │ "THIS IS A TEMPORARY PO..." (Header text)│ ──▶│ Multi-Index Unstructured Rows:       │
  │ Material Code | Description | Qty | Unit │    │ Metadata rows break CSV parsers.     │
  ├──────────────────────────────────────────┤    ├──────────────────────────────────────┤
  │ BH46 | 615 15MM METRIC | 267 lb          │ ──▶│ Numbers Stored as Text:              │
  │ BH48 | 1/2" X 15MM 310 | 126 each        │    │ Stray unit strings ("lb", "each")    │
  │ BH51 | 3/4" COPPER PIPE | $309           │    │ force entire column to object dtype. │
  ├──────────────────────────────────────────┤    ├──────────────────────────────────────┤
  │ Customer Id: 4    | PO Date: 01/01/2023  │ ──▶│ Side-Mounted Metadata:               │
  │ Delivery Time: between 2-3pm             │    │ Column definitions shifted sideways. │
  ├──────────────────────────────────────────┤    ├──────────────────────────────────────┤
  │ Sensor Reading: -999.0                   │ ──▶│ Hidden Sentinel Placeholders:        │
  │ Account Balance: 0.00                    │    │ Magic numbers silently mask missing  │
  │ Postal Code: 99999                       │    │ values without throwing NaNs.        │
  └──────────────────────────────────────────┘    └──────────────────────────────────────┘
  ```
### The Three Critical Data Ingestion Hazards:
#### 1. Unstructured Headers & Off-Grid Metadata
  * Enterprise Excel exports frequently include human-readable commentary banners, merged title blocks, or side-mounted transaction metadata.
  * Running a naive `pd.read_csv("data.csv")` mistakenly treats the first commentary string as the column header schema, shifting all feature mappings downward and populating the DataFrame with invalid `object` dtypes.
#### 2. Numerical Quantities Encoded as Text Objects
  * Ingested numerical variables often contain non-numeric characters: currency markers (`$`, `€`), thousands separators (`,`), or physical unit labels (`"267 lb"`, `"126 each"`).
  * **The Computational Consequence:** Pandas defaults the column to an unstructured Python pointer array (`dtype: object`). The CPU cannot execute SIMD vector math or matrix multiplications over string pointers, halting scikit-learn estimators with:
  $$\texttt{ValueError: could not convert string to float}$$
#### 3. Hidden Sentinel Placeholders (The Magic Number Trap)
  * Legacy database systems often disallow true null (`NULL`/`NaN`) representations. Instead, system architects rely on **sentinel values**:
  $$\text{Sentinels: } \{-999, -9999, 9999, 0.00, \text{"Unknown"}, \text{"N/A"}\}$$
  * **The Silent Catastrophe:** If a missing patient heart rate is recorded as `-999`, Pandas loads the column as a clean `float64`. Standard imputers and scalers see no missing values.
  * During optimization, gradient descent and linear models treat `-999` as an extreme negative measurement, corrupting the feature mean ($\mu$) and variance ($\sigma^2$) and introducing high-leverage distortions into the parameter weights.
  
  ---
## 3. The Three-Tier Pre-Modeling Audit Framework (Slide 4)
  
  Slide 4 formalizes the diagnostic checklist that must precede any pipeline transformations:
  
  $$\mathbf{\text{"Audit Before You Touch Anything"}}$$
  
  ```
                       The 3-Tier Pre-Modeling Audit
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Tier 1: Per-Column Schema & Distribution Stats                         │
  │  • Memory type verification (verify numericals are not 'object').      │
  │  • Total absolute missing count (df.isna().sum()).                     │
  │  • Proportional missingness ratio (df.isna().mean()).                  │
  │  • Cardinality count: number of distinct categories (df.nunique()).    │
  │  • Parametric boundaries: physical plausibility of min/max values.     │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Tier 2: Row-Level Entity Integrity                                     │
  │  • Exact duplicate rows (df.duplicated().sum()).                       │
  │  • Sub-key duplicates: identical entity IDs with conflicting records.  │
  │  • Relational inconsistencies across subsets (e.g., Age < 18 with       │
  │    YearsEmployed = 25).                                                │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Tier 3: Domain Semantic Validation                                     │
  │  • Biological & physical sanity boundaries (e.g., Human Age = 250?).   │
  │  • Sentinel code isolation (Is 0.00 a free product, or an unrecorded  │
  │    missing price?).                                                    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Building the Automated Audit Engine in Pandas (Slides 5–6)
  
  Slides 5 and 6 implement an automated audit function to inspect a dataset before modeling:
  
  ```python
  import pandas as pd
  from sklearn.datasets import fetch_openml
  
  def audit_dataset(df: pd.DataFrame) -> pd.DataFrame:
    """
    Executes a vectorized Tier-1 and Tier-2 audit across all columns.
    Returns a unified diagnostic summary sorted by missingness proportion.
    """
    audit = pd.DataFrame({
        'dtype': df.dtypes,
        'missing_count': df.isna().sum(),
        'missing_pct': (df.isna().mean() * 100).round(2),
        'unique_vals': df.nunique(),
    })
    
    # Check for row-level duplication across all feature coordinates
    exact_dups = df.duplicated().sum()
    print(f"=== ROW-LEVEL INTEGRITY ===")
    print(f"Total Observations (N) : {len(df)}")
    print(f"Exact Duplicate Rows   : {exact_dups} ({(exact_dups/len(df)*100):.2f}%)")
    
    return audit.sort_values('missing_pct', ascending=False)
  ```
  
  ---
### Step-by-Step Code Deconstruction:
  1. **Vectorized Missing Ratio:**
   `df.isna().mean() * 100` computes the empirical null fraction directly. In NumPy/Pandas, boolean arrays treat `True` as $1$ and `False` as $0$. The arithmetic mean of a boolean mask is mathematically identical to the sample proportion:
   $$\text{Missing Pct} = \left(\frac{1}{N} \sum_{i=1}^N \mathbf{1}_{\{x_i \text{ is NaN}\}}\right) \times 100$$
  2. **Cardinality Verification (`df.nunique()`):**
   * If a column exhibits `unique_vals == 1`, the feature has **zero variance** and provides no predictive signal; it should be pruned.
   * If a non-target column exhibits `unique_vals == N`, the feature is a **unique identifier** (e.g., `ticket_id`, `uuid`), which will cause high-capacity models to overfit via memorization.
  3. **Exact Duplication Audit (`df.duplicated().sum()`):**
   Scans the design matrix for rows with identical values across every feature column.
  
  ---
### Executing the Audit on OpenML Titanic ($N = 1{,}309$) (Slide 6)
  
  Slide 6 executes the audit against the complete OpenML Titanic dataset:
  
  ```python
  # Load raw benchmark directly from OpenML repository
  titanic_bunch = fetch_openml('titanic', version=1, as_frame=True)
  df_titanic = titanic_bunch.frame
  
  # Execute diagnostic engine
  audit_result = audit_dataset(df_titanic)
  print(audit_result)
  ```
  
  ```
                   Empirical OpenML Titanic Audit Profile
  === ROW-LEVEL INTEGRITY ===
  Total Observations (N) : 1309
  Exact Duplicate Rows   : 1 (0.08%)
  
                      dtype  missing_count  missing_pct  unique_vals
  body                float64           1188        90.76          121
  cabin                object           1014        77.46          186
  boat                 object            823        62.87           27
  home.dest            object            564        43.09          369
  age                 float64            263        20.09           98
  embarked           category              2         0.15            3
  fare                float64              1         0.08          281
  survived           category              0         0.00            2
  pclass                int64              0         0.00            3
  sex                category              0         0.00            2
  ```
  
  ---
### Graduate Context: OpenML Titanic ($N=1{,}309$) vs. Seaborn Titanic ($N=891$)
  Why did Seaborn's dataset in Lecture 5 report **$891$ rows**, while OpenML in Lecture 7 reports **$1{,}309$ rows**?
  
  ```
                       The Dataset Lineage Divergence
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Historical Reality: The RMS Titanic carried 1,309 passengers          │
  │                      (excluding crew members).                         │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Seaborn Titanic (N = 891):                                             │
  │   • Represents only the Kaggle competition TRAINING partition.         │
  │   • Sliced down by 418 rows (which were vaulted into Kaggle's private  │
  │     evaluation test set).                                              │
  ├────────────────────────────────────────────────────────────────────────┤
  │ OpenML Titanic v1 (N = 1,309):                                         │
  │   • Represents the complete, unpartitioned historical manifest.        │
  │   • Contains the true population distribution, including additional    │
  │     features (body identification numbers, lifeboat assignment tags).  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **Evaluation Trap:** Evaluating models on Seaborn's 891 rows vs. OpenML's 1,309 rows tests two different population samples. OpenML's inclusion of variables like `boat` (lifeboat identifier) and `body` (post-mortem body identification tag) introduces **catastrophic target leakage** if not removed prior to training. A passenger with a non-null `boat` survived with near $100\%$ certainty; a passenger with a non-null `body` number perished with $100\%$ certainty.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why does leaving sentinel missingness codes (such as `-999` or `9999`) in a continuous numerical feature cause severe distortions in linear models and neural networks, even if the model compiles and runs without raising errors?*
  2. *In the OpenML Titanic dataset ($N = 1{,}309$), why does the feature `body` (which records post-mortem body recovery identification tags) constitute catastrophic target leakage for predicting passenger survival?*
  3. *What is the difference between an exact duplicate row (`df.duplicated()`) and a sub-key entity duplicate, and why does leaving duplicate samples in a dataset violate cross-validation independence?*
  
  ---