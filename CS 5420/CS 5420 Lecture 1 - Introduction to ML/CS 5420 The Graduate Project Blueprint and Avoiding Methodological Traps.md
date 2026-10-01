## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Calculating Project Grade Needed for an "A":**
  * Grade formula: $\text{Final} = 0.40(92) + 0.40(81) + 0.20(S_{\text{project}}) = 36.8 + 32.4 + 0.20(S_{\text{project}}) = 69.2 + 0.20(S_{\text{project}})$.
  * Setting $\text{Final} \ge 90.0$:
   $$0.20(S_{\text{project}}) \ge 90.0 - 69.2 = 20.8 \implies S_{\text{project}} \ge \frac{20.8}{0.20} = \mathbf{104\%}$$
  * **Crucial Takeaway:** Because the project is capped at 100 points, this student **cannot reach a 90.0% through coursework alone**. Their only mathematical path to an "A" is having accumulated at least **4 to 5 raw attendance bonus points** across the semester. This reinforces the high sensitivity of exams and the protective value of attending unannounced sign-in lectures.
  2. **Attendance Bonus vs. Standard Curves:**
  * A standard curve is applied at the discretion of the instructor across the entire cohort.
  * Because Dr. Yu enforces a **strict non-rounding policy** ($89.99\% = \text{B}$), unannounced attendance points act as an absolute mathematical translation along the grade axis. Having even $+1.5$ bonus points converts an otherwise unroundable $88.8\%$ into an automatic $90.3\%$ "A".
  3. **Late Submission Decision Calculation:**
  * Current projected score: $60/100$ (submitted on time at $t = 0\text{h}$).
  * Score with 18 additional hours of engineering: $95/100$.
  * Since $0 < t \le 24\text{ hours}$, the penalty is strictly $-10$ points:
   $$\text{Effective Score} = 95 - 10 = \mathbf{85/100}$$
  * **Decision:** Submitting 18 hours late yields a net gain of $+25$ assignment points, which adds $+1.43\%$ directly to the overall course grade ($25 \times 0.057\%$). Taking the 10% penalty is mathematically optimal here.
  
  ---
## 2. Project Calendar & Critical Milestones
  
  The group project constitutes **20% of your total grade** (equivalent to 1.5 in-class exams) and spans the second half of the semester:
  
  ```
                            Project Milestone Roadmap
  ┌──────────────────────────────────────────────────────────────────────────────────┐
  │ Wed, Oct 21    Team Formation (Teams of 2–4; unassigned students placed by TA)   │
  │       ▼                                                                          │
  │ Fri, Nov 06    Formal Project Proposal (Problem, Dataset, Baseline, Protocol)    │
  │       ▼                                                                          │
  │ Mon, Dec 07    Final Deliverables: Comprehensive Report & Reproducible Notebook  │
  │       ▼                                                                          │
  │ Dec 09 & 11    In-Class Project Defense & Oral Presentations                     │
  └──────────────────────────────────────────────────────────────────────────────────┘
  ```
  
  * **Team Assembly Strategy (Oct 21):** Do not wait to be randomly assigned by the TA. Assemble a balanced team of 2–4 members covering:
  1. *Data Engineering & Preprocessing* (Pandas, missingness handling, pipeline design).
  2. *Model Optimization & Hyperparameter Tuning* (PyTorch training loops, cross-validation).
  3. *Scientific Writing & Visualization* (ROC/PR curves, confusion matrices, ablation tables).
  
  ---
## 3. Dataset Selection: The "Real-World" Mandate
  
  Slide 15 explicitly forbids toy datasets:
  $$\text{Prohibited: } \{\text{Iris}, \text{Digits}, \text{Make Moons}, \text{Titanic}, \text{Wine}, \text{Boston Housing}\}$$
### Why Toy Datasets Are Disqualified:
  * **Zero Structural Noise:** Features are clean, balanced, and artificially pre-conditioned.
  * **Trivial Geometry:** They are often linearly separable or have low dimensionality ($D < 10$), masking problems like multicollinearity or the curse of dimensionality.
  * **No Real-World Failure Modes:** They lack missing values, high-cardinality categorical variables, extreme class imbalances, and temporal dependencies.
### What Constitutes an Acceptable Graduate-Level Dataset:
  Your dataset must come from a public repository (e.g., **UCI Machine Learning Repository, Kaggle, Hugging Face Datasets, PhysioNet, Zenodo**, or government open-data portals) and meet these criteria:
  * **Sample Size ($N$):** Sufficiently large ($N \ge 2{,}000$ for classical ML; $N \ge 10{,}000$ for deep learning) to avoid statistical under-powering during cross-validation.
  * **Feature Complexity ($D$):** Heterogeneous feature types (continuous, ordinal, nominal text, or image metadata).
  * **Class Imbalance or Skew:** Realistic distributions requiring careful metric selection rather than default classification accuracy.
  
  ---
## 4. The Core Directive: "Rigor Beats Accuracy"
  
  Dr. Yu states: *"A careful simple model scores higher than an impressive model with a leaky pipeline."*
  
  In graduate grading, an AUC of $0.82$ obtained through a methodologically flawless, leak-free pipeline will receive an **A**, whereas an AUC of $0.99$ caused by data leakage will receive an **F**.
  
  ```
                           The Methodological Firewall
                                 Raw Dataset
                                      │
               ┌──────────────────────┴──────────────────────┐
               ▼                                             ▼
          Training Set                                   Test Set
  (80% of data: Folds 1 to K)                       (20% of data: Vaulted)
               │                                             │
               ▼                                             │
     fit_transform(X_train)                                  │
  • Learn Mean/Variance (μ, σ)                               │
  • Impute Medians                                           │
  • Fit Target Encodings                                     │
               │                                             │
               ▼                                             │
      Train Model Pipeline                                   │
               │                                             │
               ▼                                             ▼
       Validate & Tune                        transform(X_test)  [ONLY ONCE]
   (Grid Search / K-Fold CV)                 • Apply Frozen μ, σ from Train
                                             • Evaluate Generalization Error
  ```
### The Three Deadly Sins of Data Leakage:
#### 1. Preprocessing Leakage (Feature Distribution Snooping)
  * **The Error:** Computing global statistics (mean, variance, min-max bounds, PCA projections, or vocabulary tokenizers) across the entire dataset before splitting.
  * **The Consequence:** The model inadvertently learns information about the center and spread of the out-of-sample test distribution.
  * **The Solution:** Use `sklearn.pipeline.Pipeline` coupled with `ColumnTransformer`. Preprocessing operations must learn statistics strictly inside `.fit(X_train)` and apply them via `.transform(X_test)`.
#### 2. Target Leakage (Causal / Temporal Snooping)
  * **The Error:** Including a feature that is causally downstream of the target variable or recorded after the target event occurs in real life.
  * **Example:** Predicting whether a patient has pneumonia using an input feature named `prescribed_antibiotic_dose`. While highly predictive, the antibiotic was prescribed *because* the diagnosis was already made.
  * **The Solution:** Conduct temporal and causal audits of all features during Exploratory Data Analysis (EDA).
#### 3. Test-Set Snooping (Multiple Hypothesis Testing Degradation)
  * **The Error:** Evaluating your model against the test set repeatedly to decide whether to adjust learning rates, add layers, or modify feature sets.
  * **The Consequence:** The test set becomes an informal training set; you overfit to its specific noise profile.
  * **The Directive:** The test set must be vaulted and **evaluated exactly once** at the very end of the study. All model selection, feature pruning, and hyperparameter tuning must rely strictly on stratified $K$-Fold cross-validation on the training set.
  
  ---
## 5. Architectural Template: Building a Leak-Free Pipeline
  
  Below is the standard scikit-learn architectural design required to satisfy the project's structural constraints:
  
  ```python
  from sklearn.compose import ColumnTransformer
  from sklearn.pipeline import Pipeline
  from sklearn.impute import SimpleImputer
  from sklearn.preprocessing import StandardScaler, OneHotEncoder
  from sklearn.linear_model import LogisticRegression
  from sklearn.model_selection import train_test_split, cross_val_score
  
  # 1. Immediate Train/Test Separation (Vaulting the Test Set)
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
  )
  
  # 2. Define Sub-Pipelines for Heterogeneous Feature Types
  numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')), # Median learned on train fold
    ('scaler', StandardScaler())                    # μ and σ learned on train fold
  ])
  
  categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='most_frequent')),
    ('onehot', OneHotEncoder(handle_unknown='ignore')) # Handles unseen classes safely
  ])
  
  # 3. Encapsulate Feature Preprocessing inside ColumnTransformer
  preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)
    ]
  )
  
  # 4. Bind Preprocessor and Estimator into a Single Unified Execution Graph
  full_pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', LogisticRegression(solver='liblinear', penalty='l2'))
  ])
  
  # 5. Evaluate Strictly via Cross-Validation on the Training Split
  cv_scores = cross_val_score(full_pipeline, X_train, y_train, cv=5, scoring='roc_auc')
  
  # 6. Fit Once on Full Training Data, Evaluate Exactly Once on Vaulted Test Data
  full_pipeline.fit(X_train, y_train)
  final_test_auc = roc_auc_score(y_test, full_pipeline.predict_proba(X_test)[:, 1])
  ```
  
  ---
## 6. Model Family Comparisons & The Module 9–13 Requirement
  
  Slide 15 mandates two specific modeling rules:
  1. **Compare at least three distinct model families** under one identical evaluation protocol.
  2. **At least one method from Modules 9–13** must appear:
   * *Module 9:* Unsupervised Clustering ($k$-Means, DBSCAN) or PCA.
   * *Module 10–11:* Multi-Layer Perceptrons (MLPs), PyTorch Neural Architectures, GPU Deep Learning.
   * *Module 12–13:* NLP tokenization pipelines, Attention mechanisms, Pretrained Transformers (Hugging Face).
### What Qualifies as Distinct "Model Families"?
  Changing hyperparameters or tuning penalties within the same mathematical framework does **not** count as different families (e.g., Ridge Regression vs. Lasso Regression is still one family: Regularized Linear Models).
  
  ```
                            Valid Model Families
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Family 1: Generalized Linear Models (Logistic Regression, OLS)         │
  │ Family 2: Non-Parametric Ensembles (Random Forest, XGBoost, LightGBM)  │
  │ Family 3: Margin / Kernel Methods (Support Vector Machines with RBF)   │
  │ Family 4: Connectionist Neural Networks (Custom PyTorch MLP, CNN)       │
  │ Family 5: Attention / Transformers (Fine-tuned BERT, DistilBERT)        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### An Exemplary Project Experimental Design:
  
  ```
  [Problem: Predicting Sepsis from Clinical Records / Imbalanced Tabular Data]
  
  Baseline:
  └── DummyClassifier (stratified majority baseline) + Simple Logistic Regression
  
  Comparison Matrix (Identical 5-Fold Stratified CV, Scoring = PR-AUC):
  ├── Family A: Tuned XGBoost (Ensemble / Gradient Boosted Decision Trees)
  ├── Family B: Support Vector Machine with Gaussian RBF Kernel
  └── Family C (Mandatory Mod 9–13): 
    └── Deep PyTorch MLP with Dropout & BatchNorm (trained on GPU) 
        -- OR --
    └── Feature Extraction via PCA (Dimensionality Reduction) fed into an XGBoost
  ```
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why does using `RandomForestClassifier.fit()` inside a manual `for` loop that performs standard scaling outside the loop cause data leakage across cross-validation folds?*
  2. *If your baseline model achieves an accuracy of 96.2% on a fraud detection dataset, why might that number be completely meaningless without evaluating a naive dummy baseline?*
  3. *What is the difference between tuning a hyperparameter using $K$-Fold cross-validation on the training set versus tuning it by evaluating multiple iterations on the final test set?*
  
  ---