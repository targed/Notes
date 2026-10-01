### Question 1: Leak Detective — E-Commerce Target Leakage
  > **Scenario:**  
  > An engineering team builds a machine learning system to predict whether an online transaction is fraudulent ($\text{Fraud} = 1$) at the moment checkout occurs. The feature store includes a boolean column: `chargeback_dispute_filed` (indicating whether a credit card dispute was logged by the payment processor). In offline cross-validation, an XGBoost model scores a **$0.998$ ROC-AUC**. When deployed into live production traffic, the system flags almost zero fraud cases, and fraudulent transactions surge.
  
  * **(a)** Is there data leakage? If so, identify the specific leakage family.
  * **(b)** Explain the causal and temporal mechanism that caused the offline metric ($0.998$ AUC) to collapse in production.
  * **(c)** Provide the exact one-line fix to eliminate this defect.
  
  ---
#### Worked Solution:
  * **(a) Verdict & Category:**  
  **LEAK.** This is **Target Leakage** (specifically, a **Post-Hoc Causal Proxy**).
  * **(b) Mechanism of Collapse:**  
  * Let $T_{\text{checkout}}$ be the decision epoch (when the customer clicks "Submit Order"). 
  * A credit card chargeback dispute is initiated by the real cardholder days or weeks *after* the fraudulent purchase has occurred ($T_{\text{chargeback}} \gg T_{\text{checkout}}$). 
  * In the historical database, `chargeback_dispute_filed = 1` had a near $100\%$ correlation with fraud. The model learned to rely almost exclusively on this feature as a decision shortcut.
  * In live production at $T_{\text{checkout}}$, a transaction has just been submitted; no chargeback could possibly have been filed yet. Therefore, `chargeback_dispute_filed` is **strictly $0$ (or null) for $100\%$ of incoming production transactions**. 
  * The model evaluates its primary shortcut, finds it set to zero, and predicts that zero transactions are fraudulent.
  * **(c) The One-Line Fix:**
  ```python
  X = df.drop(columns=['chargeback_dispute_filed'])
  ```
  
  ---
### Question 2: Leak Detective — Longitudinal Clinical Cohort Splitting
  > **Scenario:**  
  > A hospital research group trains a model to predict $30$-day hospital readmission using electronic health records. The dataset contains $10{,}000$ admission records collected from $2{,}500$ unique chronic disease patients (averaging $4$ admissions per patient over five years). The data scientist partitions the data using:
  > ```python
  > X_train, X_test, y_train, y_test = train_test_split(
  >     X, y, test_size=0.2, random_state=42
  > )
  > ```
  > The model achieves an $88\%$ test accuracy. However, when tested at an external partner clinic, the model drops to $61\%$ accuracy.
  
  * **(a)** Identify the specific category of data leakage present.
  * **(b)** Explain why standard uniform random splitting violates the fundamental independence assumption across the train and test partitions.
  * **(c)** State the scikit-learn cross-validation splitter that mathematically eliminates this leakage.
  
  ---
#### Worked Solution:
  * **(a) Verdict & Category:**  
  **LEAK.** This is **Group / Entity Leakage** (Correlated Cluster Splitting).
  * **(b) Mechanism of Violation:**  
  * Classical generalization theory assumes instances are **independent and identically distributed (i.i.d.)**:
    $$(x_i, y_i) \overset{\text{i.i.d.}}{\sim} \mathcal{D}$$
  * Multiple hospitalizations from the *same patient* are heavily correlated: they share invariant physiological baselines, chronic genetic predispositions, socioeconomic factors, and clinician idiosyncrasies.
  * Random splitting places past or future hospital visits from **Patient #412 into the training set, while placing another visit from Patient #412 into the test set**.
  * The model learns patient-specific idiosyncrasies (effectively memorizing that Patient #412 has high baseline readmission risk) rather than learning general disease progression rules. The test set evaluates memorized patients, falsely inflating offline metrics.
  * **(c) Production Fix:**
  ```python
  from sklearn.model_selection import GroupKFold, GroupShuffleSplit
# Enforce that ALL records from a given patient remain in a single partition
  splitter = GroupShuffleSplit(n_splits=1, test_size=0.2, random_state=42)
  train_idx, test_idx = next(splitter.split(X, y, groups=df['patient_id']))
  X_train, X_test = X.iloc[train_idx], X.iloc[test_idx]
  y_train, y_test = y.iloc[train_idx], y.iloc[test_idx]
  ```
  
  ---
  
  ### Question 3: Leak Detective — NLP Vocabulary Snooping
  > **Scenario:**  
  > An NLP sentiment classification pipeline executes the following text preprocessing routine:
  > ```python
  > from sklearn.feature_extraction.text import TfidfVectorizer
  > from sklearn.model_selection import train_test_split
  > from sklearn.naive_bayes import MultinomialNB
  >
  > # 50,000 raw customer text reviews
  > vectorizer = TfidfVectorizer(max_features=5000)
  > X_tfidf = vectorizer.fit_transform(raw_text_corpus)  # Fit on all 50,000 reviews!
  >
  > X_tr, X_te, y_tr, y_te = train_test_split(
  >     X_tfidf, y, test_size=0.2, random_state=42
  > )
  > model = MultinomialNB().fit(X_tr, y_tr)
  > ```
  
  * **(a)** Identify the leakage category.
  * **(b)** Which specific statistical parameters learned by `TfidfVectorizer` leaked from the test partition into the training features?
  * **(c)** Provide the idiomatic scikit-learn fix using `make_pipeline`.
  
  ---
  
  #### Worked Solution:
  * **(a) Verdict & Category:**  
  **LEAK.** This is **Preprocessing Leakage** (Feature Representation / Inverse Document Frequency Snooping).
  * **(b) Mechanism of Parameter Contamination:**  
  * The term frequency-inverse document frequency formula is:
    $$\text{TF-IDF}(t, d, \mathcal{D}) = \text{TF}(t, d) \times \ln\left( \frac{1 + N_{\text{total}}}{1 + \text{DF}(t)} \right) + 1$$
  * By calling `.fit_transform()` on `raw_text_corpus` before splitting:
    1. **Vocabulary Construction:** The top $5{,}000$ vocabulary tokens were selected using word frequencies across the test set.
    2. **Document Frequency ($\text{DF}(t)$):** The denominator of the IDF weights directly incorporates how many times each word appeared in the held-out test reviews. Words unique to test documents leaked into the training vocabulary.
  * **(c) Production Fix:**
  ```python
  from sklearn.pipeline import make_pipeline
  
  X_tr, X_te, y_tr, y_te = train_test_split(
      raw_text_corpus, y, test_size=0.2, random_state=42
  )
  pipeline = make_pipeline(
      TfidfVectorizer(max_features=5000), MultinomialNB()
  )
  pipeline.fit(X_tr, y_tr)
  ```
  
  ---
  
  ### Question 4: Leak Detective — Out-of-Fold Contamination in Cross-Validation
  > **Scenario:**  
  > A student writes the following cross-validation script for an assignment:
  > ```python
  > # 1. Clean split of final test set
  > X_train, X_test, y_train, y_test = train_test_split(
  >     X, y, test_size=0.2, random_state=42
  > )
  >
  > # 2. Impute missing values
  > imputer = SimpleImputer(strategy='median')
  > X_train_imp = imputer.fit_transform(X_train)  # Impute X_train
  >
  > # 3. Cross-validate
  > model = LogisticRegression(max_iter=1000)
  > cv_scores = cross_val_score(model, X_train_imp, y_train, cv=5)
  > ```
  
  * **(a)** The student claims: *"I split the test set first, so my cross-validation score is completely leak-free."* Are they correct? Explain why or why not.
  * **(b)** Identify which specific validation records contaminate the training folds during each cross-validation iteration.
  * **(c)** Provide the refactored execution graph that enforces leak-free cross-validation.
  
  ---
  
  #### Worked Solution:
  * **(a) Verdict:**  
  **INCORRECT.** The student has eliminated test-set leakage, but their cross-validation estimate suffers from **Internal Preprocessing Fold Contamination**.
  * **(b) Mechanism of Contamination:**  
  * In 5-fold cross-validation, `X_train_imp` is partitioned into 5 folds ($F_1, F_2, F_3, F_4, F_5$).
  * In the first iteration, the model trains on $F_2 \cup F_3 \cup F_4 \cup F_5$ and validates on $F_1$.
  * However, the median values used to impute missing entries across the training folds were calculated over all five folds:
    $$M = \text{Median}(F_1 \cup F_2 \cup F_3 \cup F_4 \cup F_5)$$
  * The hold-out validation fold ($F_1$) directly informed the imputed values of the training folds. The reported `cv_scores` are overly optimistic.
  * **(c) Production Fix:**
  ```python
  from sklearn.pipeline import make_pipeline
# Encapsulate the imputer INSIDE the pipeline so it re-fits strictly on the 4 training folds during every CV split!
  clean_pipe = make_pipeline(
      SimpleImputer(strategy='median'), LogisticRegression(max_iter=1000)
  )
  cv_scores = cross_val_score(clean_pipe, X_train, y_train, cv=5)
  ```
  
  ---
  
  ### Question 5: Gradient Descent — Epoch & Update Algebra
  > **Problem Statement:**  
  > A convolutional neural network is trained on a high-throughput image dataset containing $N = 120{,}000$ training images. The optimization logs report that across $5$ complete epochs, the model executed exactly **$1{,}875$ parameter update steps**.
  >
  > *(a) What was the exact mini-batch size ($B$) configured in the data loader? Show all mathematical steps.*  
  > *(b) How many parameter updates would have occurred over those 5 epochs if the model were trained using pure Stochastic Gradient Descent (SGD)?*  
  > *(c) How many parameter updates would have occurred over those 5 epochs under Batch Gradient Descent (BGD)?*
  
  ---
  
  #### Worked Solution:
  * **(a) Compute Mini-Batch Size ($B$):**
  1. Calculate updates executed per single epoch:
     $$\text{Updates per Epoch} = \frac{\text{Total Updates}}{\text{Total Epochs}} = \frac{1{,}875}{5} = \mathbf{375 \text{ updates/epoch}}$$
  2. By definition, the number of updates per epoch equals the number of mini-batches:
     $$\text{Updates per Epoch} = \left\lceil \frac{N}{B} \right\rceil = 375$$
  3. Solving for $B$:
     $$B = \frac{N}{375} = \frac{120{,}000}{375} = \mathbf{320}$$
  * **Answer:** $\mathbf{B = 320 \text{ samples per batch}}$
  
  * **(b) Pure Stochastic Gradient Descent ($B = 1$):**
  * Under SGD, parameters are updated after every single sample:
    $$\text{Updates per Epoch} = N = 120{,}000$$
    $$\text{Total Updates (5 Epochs)} = 5 \times 120{,}000 = \mathbf{600{,}000 \text{ parameter updates}}$$
  
  * **(c) Batch Gradient Descent ($B = N$):**
  * Under Batch GD, the entire dataset is evaluated to execute a single step:
    $$\text{Updates per Epoch} = 1$$
    $$\text{Total Updates (5 Epochs)} = 5 \times 1 = \mathbf{5 \text{ parameter updates}}$$
  
  ---
  
  ### Question 6: Optimization Code Forensics
  > **Problem Statement:**  
  > An ML engineer inspects the following training script:
  > ```python
  > def custom_optimizer(X, y, lr=0.01, epochs=10):
  >     n, d = X.shape
  >     w = np.zeros(d)
  >     for epoch in range(epochs):
  >         indices = np.random.permutation(n)
  >         for idx in indices:
  >             x_i = X[idx : idx + 1]  # Shape: (1, d)
  >             y_i = y[idx : idx + 1]  # Shape: (1,)
  >             grad = -2 * x_i.T @ (y_i - x_i @ w)
  >             w = w - lr * grad
  >     return w
  > ```
  
  * **(a)** Which gradient descent regime does this script implement: Batch, Mini-Batch, or Stochastic? Justify based on matrix indexing.
  * **(b)** Identify the primary computational bottleneck of this Python script when trained on a GPU.
  * **(c)** How many parameter updates does this script execute if $X$ contains $n = 50{,}000$ samples and `epochs=10`?
  
  ---
  
  #### Worked Solution:
  * **(a) Gradient Regime:**  
  **Stochastic Gradient Descent (Online SGD)**.  
  *Justification:* The inner loop slices a single row at a time (`X[idx : idx + 1]` has shape $(1, d)$) and immediately executes a weight update (`w = w - lr * grad`). The batch size is strictly $B = 1$.
  * **(b) GPU Computational Bottleneck:**  
  * **Memory Bandwidth & Under-Utilization:** Running a Python `for` loop over individual samples forces $50{,}000$ sequential kernel launches per epoch.
  * GPUs require dense matrix-matrix multiplications (GEMM) across wide thread warps ($32\text{ to }512$ threads) to saturate tensor cores. 
  * In this script, the latency of dispatching kernel calls and transferring 1D vectors between host memory and device registers vastly exceeds the arithmetic computation time, stalling the GPU pipeline.
  * **(c) Total Updates:**  
  $$\text{Total Updates} = n \times \text{epochs} = 50{,}000 \times 10 = \mathbf{500{,}000 \text{ parameter updates}}$$
  
  ---
  
  ### Question 7: Loss Curve Forensics & Optimization Pathologies
  > **Problem Statement:**  
  > A machine learning platform logs the training loss trajectories of three independent linear regression models trained on the same data:
  >
  > ```
  > Trajectory A: The loss decreases rapidly for 5 steps, then oscillates between 10² and 10⁶,
  >               eventually printing "NaN" on iteration 28.
  > Trajectory B: The loss decreases smoothly, but after 5,000 iterations, the loss has only
  >               dropped from 10,000 to 9,850, forming an almost horizontal line.
  > Trajectory C: The loss fluctuates in a noisy band between 40.0 and 55.0, exhibiting
  >               continuous erratic jumps without settling into a stationary point.
  > ```
  
  * **(a)** For Trajectory A: Diagnose the learning rate $\eta$ relative to the Hessian curvature $\lambda_{\max}(H)$, and explain the occurrence of `NaN`.
  * **(b)** For Trajectory B: Diagnose the learning rate $\eta$, and state the required engineering correction.
  * **(c)** For Trajectory C: Identify which gradient descent regime produced this behavior under a constant learning rate, and explain the "Limit Cycle" phenomenon.
  
  ---
  
  #### Worked Solution:
  * **(a) Trajectory A Diagnosis:**
  * **Pathology:** The learning rate is **far too large** and violates the stability bound:
    $$\eta \ge \frac{2}{\lambda_{\max}(H)}$$
  * **Mechanics:** Each parameter update overshoots the opposite canyon wall of the quadratic loss bowl at a higher altitude than where it started. The updates diverge exponentially until floating-point limits are exceeded, producing numerical overflow (`NaN`).
  * **(b) Trajectory B Diagnosis:**
  * **Pathology:** The learning rate is **excessively small** ($\eta \approx 0$).
  * **Engineering Correction:** The optimizer is taking infinitesimal steps along a flat loss surface. Increase the learning rate by several orders of magnitude (e.g., multiply $\eta$ by $10\times$ or $100\times$) or standardize the feature matrix to eliminate ill-conditioned eigenvalues.
  * **(c) Trajectory C Diagnosis:**
  * **Pathology:** **Stochastic Gradient Descent (SGD, $B=1$) with a constant learning rate**.
  * **The Limit Cycle Phenomenon:** Because each update evaluates a single noisy sample, the variance of the gradient estimator does not vanish near the global minimum ($\text{Var}(\nabla \mathcal{L}_i) > 0$). With a fixed step size $\eta$, the updates jump randomly around the minimum basin without ever converging to the center. Exact convergence requires decaying $\eta$ according to the **Robbins-Monro conditions** ($\sum \eta_t = \infty, \sum \eta_t^2 < \infty$).
  
  ---
  
  ### Question 8: Leak Detective — Spatial-Temporal Sensor Autocorrelation
  > **Scenario:**  
  > An environmental agency trains a predictive model to forecast urban particulate pollution ($\text{PM}_{2.5}$) using $50$ stationary monitoring towers across a metropolitan area. Each tower logs hourly temperature, humidity, wind velocity, and air quality over $365$ consecutive days ($N = 50 \times 8{,}760 = 438{,}000$ rows).  
  > The engineer executes:
  > ```python
  > X_train, X_test, y_train, y_test = train_test_split(
  >     X, y, test_size=0.2, shuffle=True, random_state=42
  > )
  > ```
  > The model achieves a test $R^2 = 0.96$. Deployed in real time to forecast next week's pollution, the model fails with $R^2 = 0.22$.
  
  * **(a)** Identify the two distinct forms of data leakage occurring simultaneously.
  * **(b)** Explain how spatial and temporal autocorrelation allowed the model to cheat during offline testing.
  * **(c)** Describe the blocking strategy required to evaluate real-world forecasting validity.
  
  ---
  
  #### Worked Solution:
  * **(a) Leakage Categories:**  
  1. **Temporal Leakage** (Time-Series Autocorrelation).
  2. **Spatial / Group Leakage** (Sensor Location Autocorrelation).
  * **(b) Mechanism of Autocorrelation Cheating:**  
  * **Temporal Cheating:** Air pollution is continuous in time. The weather at Tower #12 on Tuesday at 2:00 PM is nearly identical to Tuesday at 1:00 PM and 3:00 PM. By shuffling randomly, the test set evaluates hour $t$, while the training set contains hour $t-1$ and hour $t+1$ from the same sensor! The model performs trivial local interpolation.
  * **Spatial Cheating:** Neighboring towers $500\text{ meters}$ apart share microclimate conditions. Shuffling places concurrent observations from neighboring towers across both train and test partitions.
  * **(c) Methodological Blocking Strategy:**  
  * **Temporal Blocking:** Split chronologically using a hard date threshold (e.g., train on January–October; evaluate on November–December).
  * **Spatial Blocking:** Quarantine entire monitoring towers (Leave-One-Location-Out). Train on 40 towers; test generalizability on 10 unobserved geographical towers.
  
  ---
  
  ### Question 9: PyTorch DataLoader Systems Configurations
  > **Problem Statement:**  
  > An engineer configures four different PyTorch `DataLoader` pipelines to train a neural network on an NVIDIA Tesla T4 GPU ($16\text{ GB}$ VRAM) using a dataset of $N = 2{,}000{,}000$ tabular rows ($d = 500$, `float32`):
  >
  > ```python
  > Config 1: DataLoader(dataset, batch_size=1, shuffle=True)
  > Config 2: DataLoader(dataset, batch_size=128, shuffle=True)
  > Config 3: DataLoader(dataset, batch_size=2000000, shuffle=False)
  > Config 4: DataLoader(dataset, batch_size=16384, shuffle=True)
  > ```
  
  * **(a)** Which configuration executes pure Online Stochastic Gradient Descent?
  * **(b)** Which configuration is guaranteed to crash the runtime with an unrecoverable `torch.cuda.OutOfMemoryError (OOM)`? Show the memory calculation.
  * **(c)** Between Config 1 and Config 2, explain why Config 2 achieves vastly higher computational throughput (samples processed per second) on GPU hardware.
  
  ---
  
  #### Worked Solution:
  * **(a) Online SGD:**  
  **Config 1** (`batch_size=1`).
  * **(b) OOM Crash Analysis (Config 3):**  
  * **Config 3** crashes with an out-of-memory error.
  * **Memory Calculation:**
    The design matrix contains $N = 2{,}000{,}000$ samples and $d = 500$ features in `float32` (4 bytes per scalar):
    $$\text{Raw Matrix Size} = 2{,}000{,}000 \times 500 \times 4\text{ bytes} = 4{,}000{,}000{,}000\text{ bytes} \approx \mathbf{4.0\text{ GB}}$$
    During Batch GD on a neural network, backpropagation requires storing the forward-pass intermediate activation tensors, weight gradient matrices, optimizer momentum buffers, and CUDA memory workspace.
    $$\text{Total Memory Peak} \approx 4\times \text{ to } 6\times \text{ input size} \approx \mathbf{16\text{ to } 24\text{ GB}}$$
    This exceeds the $15.0\text{ GB}$ usable limit of an NVIDIA T4 GPU, triggering an unrecoverable CUDA OOM exception.
  * **(c) Throughput Advantage of Config 2 ($B = 128$):**  
  * Config 1 executes $2$ million individual kernel launches per epoch. The CPU spends most of its time dispatching CUDA instructions over the PCIe bus, and the thousands of GPU ALUs sit idle between sample steps.
  * Config 2 groups rows into $(128 \times 500)$ contiguous tensors. This allows the GPU's streaming multiprocessors to execute optimized cuBLAS **GEMM (General Matrix Multiply)** instructions in parallel, maximizing ALU occupancy and processing orders of magnitude more samples per second.
  
  ---
  
  ### Question 10: Leak Detective — Out-of-Fold Target Encoding Failure
  > **Scenario:**  
  > A machine learning practitioner handles a high-cardinality nominal feature `zip_code` ($K = 2{,}500$ unique categories) in an insurance fraud dataset. To prevent dimensionality explosion from One-Hot Encoding, the practitioner implements **Target Encoding**:
  > ```python
  > # 1. Replace every zip code with its mean fraud rate across the ENTIRE dataset
  > zip_fraud_map = df.groupby('zip_code')['fraud_label'].mean()
  > df['zip_code_encoded'] = df['zip_code'].map(zip_fraud_map)
  >
  > # 2. Partition into train and test
  > X_tr, X_te, y_tr, y_te = train_test_split(
  >     df[['zip_code_encoded']], df['fraud_label'], test_size=0.2, random_state=42
  > )
  >
  > # 3. Fit model
  > clf = DecisionTreeClassifier().fit(X_tr, y_tr)
  > ```
  > The decision tree scores **$98.4\%$ training accuracy**, but drops to **$51.2\%$ test accuracy**.
  
  * **(a)** Identify the specific category of data leakage present.
  * **(b)** Mathematically prove how target encoding across the full dataset causes the model to memorize the target labels of rare categories.
  * **(c)** Provide the production scikit-learn fix that prevents this leakage.
  
  ---
  
  #### Worked Solution:
  * **(a) Verdict & Category:**  
  **LEAK.** This is **Preprocessing / Target Encoding Leakage**.
  * **(b) Mathematical Proof of Memorization:**  
  * Consider a rare zip code (`zip_code = 90210`) that appears exactly **once** in the entire dataset, and that individual happened to commit fraud ($y_i = 1$).
  * The global target encoding evaluates:
    $$\hat{S}_{\text{90210}} = \frac{1}{N_{\text{90210}}} \sum_{k \in \text{90210}} y_k = \frac{1}{1}(1) = \mathbf{1.0}$$
  * Now, suppose this observation is assigned to the **training partition**, or to the **test partition**.
  * If assigned to the test partition, the test instance's feature value $x_{\text{test}}$ literally equals its ground-truth target label: $x_{\text{test}} = y_{\text{test}} = 1.0$!
  * If assigned to training, any observation with encoded value $1.0$ is classified as fraud with $100\%$ confidence. The model does not learn demographic regional risk; it directly learns a proxy encoding of the target labels of the dataset, producing extreme overfitting.
  * **(c) Production Fix:**  
  Target encoding must be computed strictly inside training folds using out-of-fold cross-validation with additive Bayesian smoothing:
  ```python
  from sklearn.preprocessing import TargetEncoder
# Scikit-learn's native TargetEncoder enforces out-of-fold computation
# and applies Bayesian smoothing to prevent target memorization!
  X_tr, X_te, y_tr, y_te = train_test_split(
      df[['zip_code']], df['fraud_label'], test_size=0.2, random_state=42
  )
  encoder = TargetEncoder(cv=5, smooth='auto')
  X_tr_encoded = encoder.fit_transform(X_tr, y_tr)
  X_te_encoded = encoder.transform(X_te)
  ```
  
  ---
  
  ## Master Checkpoint Summary: Section 3 Concepts
  
  ```
  ┌──────┬───────────────────────┬────────────────────────────────────────────────────────┐
  │ Item │ Diagnostic Focus      │ Core Systems / Methodological Rule of Thumb            │
  ├──────┼───────────────────────┼────────────────────────────────────────────────────────┤
  │ Q1   │ Target Leakage        │ Post-hoc features do not exist at decision epoch T₀.   │
  │ Q2   │ Group / Entity Leak   │ Same entity in train & test shatters i.i.d. cluster.  │
  │ Q3   │ NLP Preprocessing     │ TfidfVectorizer must learn IDF strictly on train text. │
  │ Q4   │ CV Fold Contamination │ Imputers must sit INSIDE Pipeline during cross-val.    │
  │ Q5   │ Batch Step Algebra    │ Updates = (N / B) × Epochs ──▶ B = N / (Updates/Epoch) │
  │ Q6   │ SGD Code Forensics    │ Slicing X[i : i+1] executes Online SGD (B = 1).        │
  │ Q7   │ Loss Surface Curves   │ η > 2/λ_max diverges to NaN; small η crawls linearly.   │
  │ Q8   │ Spatial-Temporal Leak │ Autocorrelation requires temporal & spatial blocking.  │
  │ Q9   │ GPU Memory Scaling    │ Batch GD (B = n) on millions of rows triggers OOM.     │
  │ Q10  │ Target Encoding Leak  │ Global mean encoding memorizes labels; use TargetEncoder│
  └──────┴───────────────────────┴────────────────────────────────────────────────────────┘
  ```
  
  ---