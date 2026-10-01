# Part C1 — Leak Detective Practice Scenarios
#### Scenario (a)
  To predict whether an enterprise customer will churn next quarter, the feature engineering pipeline includes `exit_survey_feedback_score` (a 1–5 customer satisfaction rating collected by account managers).  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (b)
  An analyst handles missing values using:
  ```python
  X_imp = SimpleImputer(strategy="mean").fit_transform(X)
  X_train, X_test, y_train, y_test = train_test_split(
    X_imp, y, test_size=0.2, random_state=42
  )
  ```
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (c)
  Hourly wind farm power generation logs from 2018–2024 are split using:
  ```python
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, shuffle=True, random_state=42
  )
  ```
  to train a model that forecasts tomorrow's peak energy generation.  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (d)
  A dermatology imaging dataset contains $400$ dermoscopic lesion photos collected from $80$ unique patients ($5$ skin lesions photographed per patient). The data scientist partitions the images using:
  ```python
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42
  )
  ```
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (e)
  An engineer tunes a binary classifier by testing decision thresholds $\tau \in [0.10, 0.90]$ directly against the held-out test set predictions, selects the threshold $\tau^* = 0.28$ that maximizes the test $F_1$-score, and reports that test $F_1$-score in the final report.  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (f)
  The raw dataset is split first into `X_train, X_test, y_train, y_test`. An NLP text vectorizer `TfidfVectorizer(max_features=2000)` is fitted strictly on `X_train`, and then applied via `.transform()` to both `X_train` and `X_test`.  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (g)
  A hospital sepsis screening model deployed in the emergency department includes the input feature: `iv_antibiotic_order_placed_timestamp`.  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (h)
  In a high-dimensional tabular dataset ($N = 5{,}000, d = 8{,}000$), the researcher runs:
  ```python
  selector = SelectKBest(score_func=mutual_info_classif, k=25)
  X_selected = selector.fit_transform(X, y)
  X_train, X_test, y_train, y_test = train_test_split(
    X_selected, y, test_size=0.2, random_state=42
  )
  ```
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (i)
  To prevent dimensionality explosion on a high-cardinality feature `zip_code` ($K = 1{,}800$ unique codes), an engineer replaces each zip code with the empirical mean of the target variable ($\bar{y}_{\text{zip}}$) computed across all rows in the dataset prior to model training.  
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
#### Scenario (j)
  The data science team writes the following cross-validation workflow:
  ```python
  pipe = Pipeline([
    ("scaler", RobustScaler()),
    ("classifier", LogisticRegression(max_iter=1000)),
  ])
  cv_scores = cross_val_score(pipe, X_train, y_train, cv=5)
  ```
  * **Verdict:** ___________________________  
  * **Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C1
### Scenario (a)
  * **Verdict:** **LEAK**
  * **Category:** **Target Leakage** (Post-Hoc Causal Proxy)
  * **Why:** An exit survey score is collected *after* a customer has already made the decision to terminate their contract. At live prediction time $T_0$, active customers have not churned yet, so this feature is null/unpopulated. The model learns a shortcut that does not exist during real-world inference. `[L02, L09]`
  * **One-Line Fix:**
  ```python
  df = df.drop(columns=["exit_survey_feedback_score"])
  ```
  
  ---
### Scenario (b)
  * **Verdict:** **LEAK**
  * **Category:** **Preprocessing Leakage** (Distributional Snooping)
  * **Why:** `SimpleImputer.fit()` computes the column mean across all rows, baking the test partition's central tendency into the imputed training coordinates before splitting. `[L07, L09]`
  * **One-Line Fix:**
  ```python
  X_train, X_test, y_train, y_test = train_test_split(
      X, y, test_size=0.2, random_state=42
  )
  X_train = imputer.fit_transform(X_train)
  X_test = imputer.transform(X_test)
  ```
  *(Or encapsulate via `make_pipeline(SimpleImputer(), model)`).*
  
  ---
### Scenario (c)
  * **Verdict:** **LEAK**
  * **Category:** **Temporal Leakage** (Arrow-of-Time Inversion)
  * **Why:** Setting `shuffle=True` on time-series data destroys chronological order. The training set is populated with future weather and energy states ($t+1$) used to predict historical past events ($t$). In real-world forecasting, future macro-trends are unobservable. `[L09]`
  * **One-Line Fix:**
  ```python
  # Split chronologically without shuffling using TimeSeriesSplit or a date mask
  X_train, X_test = X[date < "2024-01-01"], X[date >= "2024-01-01"]
  ```
  
  ---
### Scenario (d)
  * **Verdict:** **LEAK**
  * **Category:** **Group / Entity Leakage** (Correlated Cluster Splitting)
  * **Why:** Multiple lesion photos from the *same patient* are heavily correlated (sharing identical skin type, genetics, lighting, and camera artifacts). Random splitting places three photos of Patient #12 into the training set and two photos of Patient #12 into the test set. The model memorizes patient-specific idiosyncrasies rather than general oncological features. `[L02, L12]`
  * **One-Line Fix:**
  ```python
  from sklearn.model_selection import GroupShuffleSplit
  
  splitter = GroupShuffleSplit(n_splits=1, test_size=0.25, random_state=42)
  tr_idx, te_idx = next(splitter.split(X, y, groups=df["patient_id"]))
  X_train, X_test = X.iloc[tr_idx], X.iloc[te_idx]
  ```
  
  ---
### Scenario (e)
  * **Verdict:** **LEAK**
  * **Category:** **Test-Set Reuse / Meta-Overfitting** (Data Snooping)
  * **Why:** The test set must be vaulted in cold storage and evaluated **strictly once**. Iterating over thresholds on the test set turns it into an informal validation set. The reported $F_1$-score overfits the specific sample noise of that test split. `[L02, L13]`
  * **One-Line Fix:**
  ```python
  # Tune the threshold using cross-validation on X_train; evaluate on X_test once
  tau_opt = tune_threshold_cv(model, X_train, y_train)
  test_f1 = f1_score(y_test, (model.predict_proba(X_test)[:, 1] >= tau_opt))
  ```
  
  ---
### Scenario (f)
  * **Verdict:** **NO LEAK**
  * **Category:** None (Methodologically Sound Workflow)
  * **Why:** The raw text corpus is partitioned first. The vocabulary and inverse document frequency (IDF) statistics are derived strictly from `X_train` via `.fit()`. Unseen tokens in `X_test` are safely ignored by `.transform()`. Zero test information enters the pipeline. `[L04, L09]`
  * **One-Line Fix:**
  ```python
  # N/A: The workflow is already leak-free
  ```
  
  ---
### Scenario (g)
  * **Verdict:** **LEAK**
  * **Category:** **Target Leakage** (Downstream Clinical Intervention / Proxy Trap)
  * **Why:** Clinicians order IV antibiotics *because* they already suspect or have confirmed severe bacterial sepsis. The feature is born downstream of clinical judgment. In an emergency screening deployment, undiagnosed patients will not have an antibiotic order placed yet, leaving the feature unpopulated. `[L02, L09]`
  * **One-Line Fix:**
  ```python
  df = df.drop(columns=["iv_antibiotic_order_placed_timestamp"])
  ```
  
  ---
### Scenario (h)
  * **Verdict:** **LEAK**
  * **Category:** **Preprocessing / Feature Selection Leakage** (Label Snooping)
  * **Why:** `SelectKBest` computes supervised statistical dependencies against the target vector across all 5,000 samples ($y_{\text{train}} \cup y_{\text{test}}$). In high dimensions ($d = 8{,}000$), it selects random noise features that happen to correlate with the test labels by chance, fabricating an illusion of high predictive accuracy that collapses in production. `[L09]`
  * **One-Line Fix:**
  ```python
  # Encapsulate the feature selector inside a Pipeline or split before fitting
  pipeline = Pipeline(
      [("selector", SelectKBest(mutual_info_classif, k=25)), ("model", clf)]
  )
  ```
  
  ---
### Scenario (i)
  * **Verdict:** **LEAK**
  * **Category:** **Preprocessing / Target Encoding Leakage**
  * **Why:** Calculating conditional target means ($\bar{y}_{\text{zip}}$) over the full dataset directly injects the ground-truth labels ($y_{\text{test}}$) of the test set into the feature values of the training instances. For rare zip codes, the model memorizes the target labels directly. `[L08, L09]`
  * **One-Line Fix:**
  ```python
  from sklearn.preprocessing import TargetEncoder
  # TargetEncoder uses out-of-fold cross-validation inside the training split
  encoder = TargetEncoder(cv=5, smooth="auto")
  X_train["zip_enc"] = encoder.fit_transform(
      X_train[["zip_code"]], y_train
  )
  X_test["zip_enc"] = encoder.transform(X_test[["zip_code"]])
  ```
  
  ---
### Scenario (j)
  * **Verdict:** **NO LEAK**
  * **Category:** None (Methodologically Sound Workflow)
  * **Why:** Wrapping `RobustScaler` and the estimator inside an `sklearn.pipeline.Pipeline` guarantees that `cross_val_score` re-fits the scaler strictly on the $4$ training folds during each split, transforming the $5^{\text{th}}$ validation fold using frozen fold-specific medians and IQRs. Zero validation data leaks into training. `[L08, L09, L10]`
  * **One-Line Fix:**
  ```python
  # N/A: The workflow is already leak-free
  ```
  
  ---