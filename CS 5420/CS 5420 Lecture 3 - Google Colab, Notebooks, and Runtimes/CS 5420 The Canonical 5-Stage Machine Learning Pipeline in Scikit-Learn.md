## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Why `!cd sample_data` Fails While `%cd sample_data` Succeeds:**
  * The exclamation mark (`!`) instructs the IPython kernel to spawn an isolated child subshell (`/bin/bash -c ...`). The child process changes directory, immediately reaches its end-of-life, and exits. The parent Python process never modified its current working directory.
  * The percent sign (`%`) denotes an **IPython Magic Command**, which executes directly within the active kernel process by invoking `os.chdir('sample_data')`, altering the working directory for all subsequent cells.
  2. **Why Random Forests Run Inefficiently on GPUs Compared to CNNs:**
  * GPUs achieve acceleration through **Single Instruction, Multiple Threads (SIMT)** over dense, uniform contiguous matrix blocks (GEMM operations).
  * Decision trees rely on recursive, conditional branching logic:
   $$\text{if } x_j \le \tau \text{ goto LeftChild else goto RightChild}$$
  * Samples in a batch traverse different paths down the tree, leading to **warp divergence** on a GPU (threads in a warp must execute different execution paths serially). CPUs, with their sophisticated branch predictors and large L1/L2 caches, handle tree traversals much more efficiently.
  3. **Behavior of PyTorch's Allocator When Exceeding VRAM:**
  * If $14.8\text{ GB}$ of an available $15.0\text{ GB}$ VRAM pool is allocated, requesting an additional $2.0\text{ GB}$ causes PyTorch's caching allocator to search for free contiguous memory blocks. 
  * When physical memory limits are exhausted, PyTorch cannot dynamically page memory to disk like an OS virtual memory swap. It immediately throws an unrecoverable exception:
   $$\texttt{torch.cuda.OutOfMemoryError: CUDA out of memory.}$$
   This halts script execution and requires restarting the runtime or running `torch.cuda.empty_cache()`.
  
  ---
## 2. Deconstructing the Canonical 5-Stage ML Pipeline (Slide 13)
  
  Slide 13 presents the fundamental lifecycle of supervised machine learning engineering:
  
  ```
                      The 5-Stage Machine Learning Lifecycle
  ┌────────────────────────────────────────────────────────────────────────┐
  │  Stage 1: Ingestion           Load structured feature & target vectors │
  │         │                                                              │
  │         ▼                                                              │
  │  Stage 2: Exploration (EDA)   Audit dimensions, datatypes, missingness │
  │         │                                                              │
  │         ▼                                                              │
  │  Stage 3: Partitioning        Vault the test set via pseudo-randomness │
  │         │                                                              │
  │         ▼                                                              │
  │  Stage 4: Optimization        Minimize empirical risk on train data    │
  │         │                     via Estimator.fit(X_train, y_train)      │
  │         │                                                              │
  │         ▼                                                              │
  │  Stage 5: Evaluation          Compute generalization score on          │
  │                               unseen data via Estimator.score(X, y)    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **Graduate Takeaway:** While models evolve from linear regressors to multi-billion-parameter transformers, this 5-stage abstraction remains invariant. In professional workflows, bugs almost never occur in the mathematical formulation of Stage 4; they occur at the interfaces between Stages 2, 3, and 5 (leakage, shape misalignment, uncalibrated metrics).
  
  ---
## 3. Data Ingestion: Arrays, Shapes, and Memory Buffers (Slide 14)
  
  Slide 14 introduces programmatic dataset loading via `scikit-learn`:
  
  ```python
  from sklearn.datasets import load_iris
  
  X, y = load_iris(return_X_y=True)
  
  print("X shape:", X.shape)
  print("y shape:", y.shape)
  ```
  
  ```
                          Data Matrix Topologies
            Feature Matrix X                          Target Vector y
        (N = 150, d = 4 Features)                   (N = 150 Labels)
    ┌──────────────────────────────┐              ┌──────────────────┐
    │  x_11   x_12   x_13   x_14   │              │       y_1        │
    │  x_21   x_22   x_23   x_24   │              │       y_2        │
    │    :      :      :      :    │              │        :         │
    │  x_150  x_150  x_150  x_150  │              │      y_150       │
    └──────────────────────────────┘              └──────────────────┘
       shape: (150, 4) [float64]                     shape: (150,) [int64]
  ```
### Architectural Realities of `return_X_y=True`:
  1. **Separation of Concerns:** By passing `return_X_y=True`, `scikit-learn` bypasses returning its custom `Bunch` dictionary object (which bundles metadata, feature names, and descriptions) and directly returns two pure **NumPy `ndarray` instances**.
  2. **Rank & Dimensions:**
   * $X \in \mathbb{R}^{150 \times 4}$ is a **2-dimensional array** (Rank-2 tensor).
   * $y \in \{0, 1, 2\}^{150}$ is a **1-dimensional array** with shape `(150,)`, not `(150, 1)`. In Python, failing to recognize the difference between a 1D vector `(N,)` and a 2D column vector `(N, 1)` is a frequent source of broadcasting bugs in NumPy loss computations.
  
  ---
## 4. Exploratory Data Analysis: Auditing Matrix Geometry (Slide 15)
  
  Slide 15 prompts the class with three foundational structural questions:
  
  ```
  ┌─────────────────────────────────┬──────────┬─────────────────────────────────────────────────┐
  │ Structural Question             │ Value    │ Mathematical & Empirical Interpretation         │
  ├─────────────────────────────────┼──────────┼─────────────────────────────────────────────────┤
  │ How many samples in X?          │ N = 150  │ Evaluated via X.shape[0] or len(X).             │
  │                                 │          │ Number of independent observations (rows).      │
  │                                 │          │                                                 │
  │ How many features per sample?   │ d = 4    │ Evaluated via X.shape[1].                       │
  │                                 │          │ Dimensionality of the input space ℝ^4:          │
  │                                 │          │ {sepal length, sepal width,                     │
  │                                 │          │  petal length, petal width} (all continuous cm).│
  │                                 │          │                                                 │
  │ How many classes?               │ C = 3    │ Evaluated via len(np.unique(y)).                │
  │                                 │          │ Discrete label space {0, 1, 2} representing:    │
  │                                 │          │ {Iris Setosa, Iris Versicolour, Iris Virginica} │
  └─────────────────────────────────┴──────────┴─────────────────────────────────────────────────┘
  ```
### Class Distribution Balance
  Evaluating `np.bincount(y)` reveals that the dataset contains exactly **50 samples per class**:
  $$P(Y = 0) = P(Y = 1) = P(Y = 2) = \frac{1}{3}$$
  Because the class balance is uniform ($33.3\%$ per class), standard classification accuracy serves as a reliable performance metric here—unlike the imbalanced fraud or clinical detection scenarios examined in Lecture 2.
  
  ---
## 5. Dataset Partitioning & Pseudorandom Seeds (Slide 16)
  
  Slide 16 establishes the hold-out validation partition:
  
  ```python
  from sklearn.model_selection import train_test_split
  
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
  )
  ```
  
  ```
                        Hold-Out Dataset Partitioning
                        Full Dataset: N = 150 Samples
  ┌────────────────────────────────────────────────────────┬───────────────────┐
  │                 Training Split (80%)                   │  Test Split (20%) │
  │                N_train = 120 Samples                   │ N_test = 30 Sample│
  └────────────────────────────────────────────────────────┴───────────────────┘
   X_train: (120, 4)                                        X_test: (30, 4)
   y_train: (120,)                                          y_test: (30,)
  ```
### Mechanics of the Split:
  1. **The Partition Fraction:** Setting `test_size=0.2` allocates $20\%$ of the observations to evaluation ($150 \times 0.20 = 30$ samples) and reserves the remaining $80\%$ for model parameter estimation ($150 \times 0.80 = 120$ samples).
  2. **The Role of `random_state=42`:**
   * Partitioning requires shuffling the indices before splitting. If data were sliced sequentially without shuffling, the test set would contain only class `2` (since the Iris dataset is ordered by class label).
   * Setting a fixed seed initializes a **deterministic Pseudorandom Number Generator (PRNG)**. Every student running this code on any computer in the world will generate the exact same permutation of row indices, guaranteeing scientific reproducibility.
### The Missing Parameter: Stratified Sampling
  Notice that Slide 16 does **not** include the `stratify` argument:
  * Under pure uniform random splitting on small sample sizes, random sampling can yield an unrepresentative distribution (e.g., drawing 14 Setosa, 10 Versicolour, and only 6 Virginica in the test fold).
  * **Graduate Best Practice:** In classification problems, always enforce proportional representation by passing `stratify=y`:
  ```python
  X_train, X_test, y_train, y_test = train_test_split(
      X, y, test_size=0.2, random_state=42, stratify=y
  )
  ```
  This guarantees that both the training and test splits retain the exact $1:1:1$ class balance of the parent population.
  
  ---
## 6. Model Training & The Estimator API (Slide 17)
  
  Slide 17 instantiates and trains an ensemble model:
  
  ```python
  from sklearn.ensemble import RandomForestClassifier
  
  model = RandomForestClassifier(random_state=42)
  model.fit(X_train, y_train)
  ```
  
  ```
                     The Scikit-Learn Estimator Pattern
  ┌──────────────────────────────┐
  │ Model Initialization         │  Hyperparameters bound (n_estimators=100,
  │ model = Estimator(params)    │  criterion='gini', max_depth=None)
  └──────────────┬───────────────┘
               │ Invocation of .fit()
               ▼
  ┌──────────────────────────────┐
  │ Empirical Risk Minimization  │  Algorithms optimize parameters internally
  │ model.fit(X_train, y_train)  │  (Builds 100 bootstrapped decision trees)
  └──────────────┬───────────────┘
               │ Training completes; model state mutated
               ▼
  ┌──────────────────────────────┐
  │ Fitted Estimator             │  Internal attributes populated with trailing
  │ (Ready for Inference)        │  underscores: model.estimators_, model.classes_
  └──────────────────────────────┘
  ```
### What Happens During `RandomForestClassifier.fit()`?
  1. **Bootstrap Aggregating (Bagging):** The algorithm draws $B$ bootstrap samples (random samples with replacement of size $N_{\text{train}}$) from the training set.
  2. **Random Subspace Projection:** At each node in each decision tree, a random subset of features (typically $\sqrt{d} = \sqrt{4} = 2$) is evaluated to determine the optimal split point that maximizes label purity (minimizing Gini Impurity or Shannon Entropy).
  3. **Parameter Mutation:** The internal state of the `model` object changes. It populates fitted attributes (denoted in scikit-learn conventions by a trailing underscore, e.g., `model.estimators_`, `model.n_classes_`).
  
  ---
## 7. Model Evaluation & Generalization Scoring (Slide 18)
  
  Slide 18 computes out-of-sample performance:
  
  ```python
  accuracy = model.score(X_test, y_test)
  print("Test accuracy:", accuracy)
  ```
### Under the Hood of `model.score()`:
  The `.score()` method encapsulates a two-step operational sequence:
  
  ```
  Test Features (X_test) ──▶ model.predict(X_test) ──▶ Predictions (ŷ_test)
                                                            │
                                                            ▼
  True Targets  (y_test)  ──────────────────────────▶ Metric Engine:
                                                      Accuracy = 1/N ∑ 𝟙{y_i == ŷ_i}
  ```
  
  1. **Inference Generation:**
   * It passes $X_{\text{test}} \in \mathbb{R}^{30 \times 4}$ through all 100 trees in the ensemble.
   * Each tree casts a categorical vote; majority voting yields the predicted class vector $\hat{y}_{\text{test}} \in \{0, 1, 2\}^{30}$.
  2. **Metric Evaluation:**
   * For classification estimators, `.score()` computes **Mean Classification Accuracy**:
     $$\text{Accuracy} = \frac{1}{N_{\text{test}}} \sum_{i=1}^{N_{\text{test}}} \mathbf{1}_{\{y_i = \hat{y}_i\}}$$
   * On this split, a standard Random Forest achieves a test accuracy of **$1.00$ ($100\%$)**, correctly predicting all 30 out-of-sample instances.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does passing `return_X_y=True` to `load_iris` return $X$ as a 2D array of shape `(150, 4)` while $y$ is returned as a 1D array of shape `(150,)` instead of a 2D column vector `(150, 1)`?*
  2. *If you run `train_test_split` without specifying `random_state`, what happens to your experimental reproducibility across repeated executions, and why is this problematic for grading?*
  3. *What is the difference between a model's hyperparameters (passed during `RandomForestClassifier(...)`) and its fitted parameters (computed during `.fit(...)`)?*
  
  ---