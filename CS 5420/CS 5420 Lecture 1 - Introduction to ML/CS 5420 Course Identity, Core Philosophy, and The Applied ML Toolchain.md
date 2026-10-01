## 1. Course Metadata & Academic Framing
  * **Course:** CS 5420 — Introduction to Machine Learning (Graduate Level)
  * **Institution:** Missouri University of Science and Technology (Missouri S&T)
  * **Instructor:** Dr. Xiaowei Yu (CSB 304 | `xych8@mst.edu`)
  * **Schedule:** MWF 1:00–1:50 PM | Room: CS 220
  
  At the 5000-level, "Introduction to Machine Learning" is distinct from an undergraduate survey. It serves as a dual-track course: providing theoretical literacy (why algorithms behave the way they do) alongside rigorous engineering discipline (how to design, validate, debug, and deploy pipelines without methodological errors).
  
  ---
## 2. The Pedagogical Philosophy: What Graduate ML Is vs. Is Not
  
  The opening slides establish four core themes regarding how this course approaches machine learning:
  
  ```
                      Core Theme of CS 5420
  ┌─────────────────────────────────────────────────────────────────┐
  │  "What models assume  ──▶  Where they fail  ──▶  How to evaluate│
  │   (Inductive Bias)         (Pathologies)         honestly"      │
  └─────────────────────────────────────────────────────────────────┘
  ```
### A. What the Course Is
  1. **A Working Foundation in Methodology and Algorithms:**
   * It is not enough to import a class and call `.fit()`. You must understand the underlying objective function, the optimization method used to solve it, and the data distribution assumptions required for the model to generalize.
  2. **Engineering Discipline:**
   * Modern ML engineering requires fluency in environment management, vectorized programming, automatic differentiation, and framework interoperability.
  3. **Honest Evaluation:**
   * The primary failure mode in applied ML is not poor accuracy; it is **unrealistically high accuracy** caused by data leakage, uncalibrated evaluation metrics, or overfitting to benchmark artifacts.
### B. What the Course Is Not
  * **Not an exhaustive algorithm catalog:** Focus is placed on representative model families (linear models, margin-based classifiers, tree/ensemble paradigms, connectionist/neural architectures, and attention mechanisms) rather than minor variants of the same concepts.
  * **Not accuracy chasing:** In research and production, an interpretable or simple model with well-calibrated confidence bounds often outperforms an uncalibrated, over-parameterized black box.
  * **Not copy-paste engineering:** If you cannot explain the mathematical operations occurring within a pipeline or debug numerical instabilities in your training loop, the pipeline is practically useless.
  * **Not pure detached theory:** You will not spend weeks proving uniform convergence bounds or VC-dimension theorems that do not translate to actionable modeling decisions.
  
  ---
## 3. Deconstructing the Teaching Principles
### 1. "Build First"
  * **Concept:** Implement, observe behavior, run into runtime errors or convergence issues, and then deconstruct the internal mechanics.
  * **Graduate Value:** Real ML systems break at integration points (data ingestion, dynamic shapes, device placement, precision loss). Building first grounds theoretical abstractions in computational reality.
### 2. "Math on Demand"
  * **Concept:** Derivations are introduced when they directly inform practical implementation:
  * *Loss Functions:* Why Cross-Entropy is used over Mean Squared Error for classification (information theory, gradient behavior under sigmoid/softmax saturations).
  * *Gradients & Jacobians:* How backpropagation calculates vector-Jacobian products through the chain rule.
  * *Margins & Dual Formulations:* Why Support Vector Machines depend only on dot products of support vectors, motivating the Kernel Trick.
  * **Omission of Non-Actionable Proofs:** You will skip long existence proofs that provide no leverage for training, tuning, or debugging models.
### 3. "Real Data, Real Pitfalls"
  * **Data Leakage:** When information from outside the training dataset influences the model during training (e.g., standardizing data *before* train-test splitting, temporal leakage in time-series, or duplicate samples across splits).
  * **Distribution Shift:** When the training distribution $P_{\text{train}}(X, Y)$ differs from the test/production distribution $P_{\text{test}}(X, Y)$.
  
  ---
## 4. The Applied Toolchain Ecosystem
  
  The course uses a specific progression of Python data science and machine learning libraries. Each tool occupies a distinct layer in the computing stack:
  
  ```
  ┌──────────────────────────────────────────────────────────────────┐
  │                      Hugging Face Ecosystem                      │
  │      (Tokenizers, Datasets, Pretrained Transformers, Accelerate) │
  ├──────────────────────────────────────────────────────────────────┤
  │                             PyTorch                              │
  │       (Dynamic Autograd Engine, Tensor Ops, GPU Acceleration)     │
  ├──────────────────────────────────────────────────────────────────┤
  │                          scikit-learn                            │
  │ (Classical Algorithms, Preprocessing Transformers, Pipelines, CV)│
  ├──────────────────────────────────────────────────────────────────┤
  │                         NumPy & Pandas                           │
  │   (Contiguous Arrays, Vectorized Linear Algebra, Tabular Data)   │
  ├──────────────────────────────────────────────────────────────────┤
  │                      Google Colab / Jupyter                      │
  │       (Interactive Prototyping, Cloud Hardware: T4/A100 GPUs)    │
  └──────────────────────────────────────────────────────────────────┘
  ```
### Detailed Breakdown of the Stack:
#### 1. Google Colaboratory (Execution Environment)
  * **Role:** Cloud-hosted Jupyter notebook environment providing managed hardware access (CPUs, GPUs, and TPUs).
  * **Under the Hood:** Runs on Ubuntu Linux virtual machines. Allows immediate testing with pre-installed CUDA drivers without requiring local GPU configuration.
#### 2. NumPy & Pandas (Data Foundations & Numerical Computing)
  * **NumPy (`ndarray`):**
  * Provides $N$-dimensional homogeneous arrays stored in contiguous memory buffers (row-major C-style or column-major Fortran-style).
  * Employs vectorization and SIMD (Single Instruction, Multiple Data) execution, avoiding Python's GIL (Global Interpreter Lock) and pointer overhead for numerical operations.
  * **Pandas (`DataFrame`, `Series`):**
  * Built on top of NumPy arrays; designed for heterogeneous, labeled tabular data.
  * Handles missing values (`NaN`), structural aggregations (`groupby`), schema alignments, and metadata manipulation prior to tensor conversion.
#### 3. scikit-learn (Classical Machine Learning & Pipeline Infrastructure)
  * **Role:** The standard library for non-deep learning models and preprocessing pipelines.
  * **Design Pattern:** Strictly object-oriented around three core interfaces:
  1. **Estimators:** `fit(X, y)` trains the model parameters from data.
  2. **Transformers:** `transform(X)` or `fit_transform(X)` performs stateful feature transformations (e.g., imputation, scaling, one-hot encoding).
  3. **Predictors:** `predict(X)` or `predict_proba(X)` outputs inferences on unseen data.
  * **Core Abstraction (`sklearn.pipeline.Pipeline`):**
  * Chains feature transformers and estimators into a single execution graph.
  * Ensures that preprocessing steps (like mean and variance calculation) learn parameters *only* from the training fold, systematically preventing data leakage during cross-validation.
#### 4. PyTorch (Deep Learning & Automatic Differentiation)
  * **Role:** The leading research framework for neural networks and tensor computations.
  * **Key Components:**
  * **`torch.Tensor`:** Similar to NumPy's `ndarray`, but with built-in support for accelerator memory allocation (CUDA/ROCm/MPS) and automatic gradient tracking.
  * **Autograd:** A reverse-mode automatic differentiation engine that constructs a dynamic Directed Acyclic Graph (DAG) during the forward pass to compute $\nabla_\theta \mathcal{L}$ during `.backward()`.
  * **`torch.nn.Module`:** Base class for all neural network architectures, encapsulating parameter state, layer registration, and execution logic.
#### 5. Hugging Face (Transformer & Foundation Model Infrastructure)
  * **Role:** Simplifies the usage, fine-tuning, and sharing of large pretrained language, vision, and multimodal models.
  * **Key Sub-Libraries:**
  * **`transformers`:** Provides standardized implementations of modern architectures (BERT, RoBERTa, LLaMA, Whisper) with pretrained weights.
  * **`tokenizers`:** High-performance, Rust-backed text tokenization algorithms (Byte-Pair Encoding, WordPiece, Unigram).
  * **`datasets`:** Memory-mapped streaming of massive datasets via Apache Arrow to eliminate RAM bottlenecks.
  
  ---
## Summary Review Questions for Section 1
  
  To test your conceptual understanding before moving forward:
  1. *Why does performing a standard scaling operation (`StandardScaler`) on an entire dataset prior to running train-test split constitute data leakage?*
  2. *How does NumPy achieve performance orders of magnitude faster than standard Python loops when operating over floating-point arrays?*
  3. *Why is PyTorch preferred over scikit-learn when designing deep learning models, while scikit-learn is preferred for classical tabular baselines?*