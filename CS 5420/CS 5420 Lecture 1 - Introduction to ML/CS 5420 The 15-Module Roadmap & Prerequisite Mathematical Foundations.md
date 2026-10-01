## 1. Quick Review: Answers to Section 1 Checkpoints
  
  Before diving into the curriculum, here are the technical answers to the three questions from Section 1:
  
  1. **Why `StandardScaler` before splitting causes data leakage:**
  Standard scaling computes the empirical mean $\mu = \frac{1}{N}\sum x_i$ and standard deviation $\sigma = \sqrt{\frac{1}{N}\sum (x_i - \mu)^2}$. If computed across the entire dataset, information about the distribution of the test set leaks into the training pipeline. The test set is no longer a true out-of-sample proxy, yielding overly optimistic evaluation metrics.
  2. **How NumPy outperforms native Python loops:**
  Python lists store pointers to heap-allocated `PyObject` wrappers, requiring type inspection and pointer indirection on every element access. NumPy allocates a contiguous block of homogeneous memory in C and executes compiled C/Fortran routines that leverage CPU L1/L2 cache locality and SIMD (Single Instruction, Multiple Data) vector registers.
  3. **Why PyTorch vs. scikit-learn:**
  `scikit-learn` relies on CPU-bound, fixed-algorithm implementations with standard tabular structures. It does not provide dynamic automatic differentiation or GPU tensor primitives. `PyTorch` is an autograd engine optimized for massively parallel accelerator hardware (GPUs/TPUs), making it essential for non-linear, high-dimensional neural computations where gradient graphs must be dynamically constructed.
  
  ---
## 2. Deconstructing the 15-Module Curriculum
  
  The course curriculum forms a coherent theoretical and systems trajectory over 42 lecture meetings. It progresses from foundational data wrangling to classical convex optimization, non-convex deep learning, and finally modern sequence transduction and autonomous agentic loops.
  
  ```
                  Curricular Trajectory (42 Lectures)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Modules 1–4   Foundations: Vectorization, Preprocessing, Pipelines     │
  │       ▼                                                                │
  │ Modules 5–8   Parametric & Kernel Methods: Regression, SVMs, Metrics   │
  │       ▼                                                                │
  │ Modules 9–11  Unsupervised Learning & Deep Connectionist Models (GPUs) │
  │       ▼                                                                │
  │ Modules 12–15 NLP, Transformers, Autonomous Agents & Applied Ethics   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Module Block 1: Foundations, Environment, Python & Data Preprocessing (Modules 1–4)
  * **Underlying Goal:** Establish baseline computational literacy and eliminate common pipeline bugs.
  * **Topics & Unspoken Realities:**
  * **Memory Layouts & Vectorized Operations:** Strides, views vs. copies in NumPy, broadcasting semantics, and contiguous memory buffers (`order='C'` vs. `order='F'`).
  * **Exploratory Data Analysis (EDA) & Data Cleaning:** Identifying structural missingness (MCAR, MAR, MNAR—*Missing Completely at Random, Missing at Random, Missing Not at Random*), outlier detection via IQR/z-score, and distribution skewness.
  * **Feature Engineering & Transformation:** Imputation strategies, categorical encoding (One-Hot, Target/Bayesian, Ordinal), log/Power transformations (Box-Cox, Yeo-Johnson) for variance stabilization.
  * **The Leakage Firewall:** Formalizing strict data hygiene using `sklearn.pipeline.Pipeline` and `ColumnTransformer`.
  
  ---
### Module Block 2: Parametric Models, Classification, Evaluation & Kernels (Modules 5–8)
  * **Underlying Goal:** Transition from deterministic math to statistical inference, convex optimization, and margin maximization.
  * **Topics & Unspoken Realities:**
  * **Linear & Polynomial Regression:** Ordinary Least Squares (OLS), the Normal Equation $\hat{\beta} = (X^T X)^{-1} X^T y$, multicollinearity, condition numbers, and regularized variants:
    * *Ridge ($L_2$):* Shrinks coefficients via weight decay; smooth convex loss.
    * *Lasso ($L_1$):* Promotes sparsity via non-smooth optimization at the axes.
    * *ElasticNet:* Convex combination of $L_1$ and $L_2$ penalties.
  * **Parametric Classification:** Logistic regression, log-odds/logit link functions, Maximum Likelihood Estimation (MLE), and binary cross-entropy loss landscapes.
  * **Rigorous Evaluation Metrics:** Moving beyond accuracy. ROC-AUC, Precision-Recall curves (PR-AUC for imbalanced data), Brier score for calibration, Cost-Matrix evaluations, and stratified cross-validation.
  * **Support Vector Machines (SVM) & Kernel Methods:**
    * Primal formulation: Margin maximization subject to inequality constraints.
    * Dual formulation: Solved using quadratic programming via Karush-Kuhn-Tucker (KKT) conditions.
    * The **Kernel Trick**: Projecting input space $\mathcal{X}$ into an infinite-dimensional Reproducing Kernel Hilbert Space (RKHS) without explicitly computing coordinates, using functions satisfying Mercer's condition (e.g., RBF/Gaussian kernel $K(x, x') = \exp(-\gamma \|x - x'\|^2)$).
  
  ---
### Module Block 3: Unsupervised Learning, Neural Architectures & GPU Acceleration (Modules 9–11)
  * **Underlying Goal:** Moving into latent space discovery and non-convex gradient-based optimization.
  * **Topics & Unspoken Realities:**
  * **Unsupervised Learning & Dimensionality Reduction:**
    * *Clustering:* $k$-Means (Lloyd’s algorithm, Voronoi tessellations), DBSCAN (density-based reachability), Hierarchical Agglomerative Clustering.
    * *Decomposition:* Principal Component Analysis (PCA) derived via variance maximization and reconstruction error minimization; solved via Singular Value Decomposition (SVD) of the centered covariance matrix.
  * **Connectionist Models (Multi-Layer Perceptrons):**
    * Universal Approximation Theorem: Limitations and practical significance.
    * Activation functions: Sigmoid/Tanh (vanishing gradient problem) vs. ReLU, LeakyReLU, GeLU (dying ReLU problem and modern smooth variants).
    * Forward pass, loss computation, and backpropagation via the vector-chain rule.
  * **Deep Learning on Hardware Accelerators:**
    * CUDA cores, Tensor cores, memory hierarchies (Host RAM $\to$ PCIe $\to$ VRAM $\to$ SRAM / L1/L2 cache).
    * Mini-batch stochastic gradient descent (SGD), momentum, RMSprop, Adam, and AdamW (decoupled weight decay).
  
  ---
### Module Block 4: NLP, Transformers, Autonomous Agents & System Ethics (Modules 12–15)
  * **Underlying Goal:** Moving from static prediction to sequential context processing, generative foundation models, and tool-augmented agent loops.
  * **Topics & Unspoken Realities:**
  * **Natural Language Processing (NLP) Foundations:** Bag-of-Words, TF-IDF, distributed semantic representations (Word2Vec, GloVe), and limitations of recurrent topologies (RNNs, LSTMs, GRUs) regarding sequential bottlenecks.
  * **The Transformer Architecture:**
    * Scaled Dot-Product Attention:
      $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
    * Multi-Head Attention, positional encodings (sinusoidal, RoPE), encoder-only (BERT), decoder-only (GPT), and encoder-decoder (T5) paradigms.
  * **Autonomous AI Agents:**
    * Moving from passive conditional sampling $P(y \mid x)$ to active closed-loop policies.
    * Orchestration, Model Context Protocol (MCP), constrained decoding (GBNF grammars), tool calling, and environment sandboxing (Docker, gVisor, Firecracker).
  * **Ethics, Fairness, Privacy & Robustness:**
    * Demographic parity, equalized odds, differential privacy, membership inference attacks, adversarial robustness, and environmental compute costs.
  
  ---
## 3. The Dual Textbook Strategy: Bishop vs. Raschka
  
  Slide 8 lists two textbooks. In a graduate course, they serve complementary purposes:
  
  ```
                            Theoretical Axis
                 (Bishop: PRML, 2006 — Bayesian & Probabilistic)
                                      ▲
                                      │   [The Sweet Spot]
                                      │   Rigorous math grounded
                                      │   in production code
                                      │
                                      ▼
                  (Raschka et al., 2022 — PyTorch & scikit-learn)
                            Engineering Axis
  ```
  
  | Dimension | **Christopher M. Bishop (PRML, 2006)** | **Sebastian Raschka et al. (2022)** |
  | :--- | :--- | :--- |
  | **Primary Lens** | Probabilistic, Bayesian, Analytical | Empirical, Algorithmic, Applied Systems |
  | **Mathematical Style** | Derivations from first principles; probability densities; expectations | Algorithmic implementations; vectorized linear algebra; computational graphs |
  | **Programming Language** | Language-agnostic (pseudocode & math notation) | Pure Python: `numpy`, `scikit-learn`, `torch` |
  | **Core Strengths** | Generative models, Gaussian processes, EM algorithm, graphical models | PyTorch idiom, idiomatic scikit-learn design, transformer fine-tuning |
  | **How You Should Use It** | Consult for theoretical justification on **why** a loss function or kernel works mathematically. | Consult for **how** to implement training routines, data loaders, and model evaluation code. |
  
  ---
## 4. The "Math on Demand" Prerequisite Toolkit
  
  The syllabus states that mathematics will be derived "on demand." Below is the reference map connecting underlying math concepts to their corresponding course modules.
### A. Multivariable Calculus & Continuous Optimization
  * **Gradient Vectors ($\nabla f$) & Directional Derivatives:**
  * *Where it appears:* Gradient descent across all modules (Linear models, Neural Nets).
  * *Key concept:* The gradient points in the direction of steepest *ascent*. We step in the direction of $-\nabla_\theta \mathcal{L}(\theta)$.
  * **The Jacobian Matrix ($J$):**
  * *Where it appears:* Backpropagation in deep networks (Module 10).
  * *Definition:* Matrix of all first-order partial derivatives of a vector-valued function:
    $$J_{ij} = \frac{\partial f_i}{\partial x_j}$$
  * *Crucial computational trick:* In deep learning frameworks, we rarely materialize full Jacobian matrices. We compute **Vector-Jacobian Products (VJPs)** to accumulate gradients efficiently in reverse mode:
    $$v^T J$$
  * **The Hessian Matrix ($H$) & Curvature:**
  * *Where it appears:* Newton-Raphson optimization in Logistic Regression, loss surface analysis.
  * *Definition:* Symmetric matrix of second-order partial derivatives ($H_{ij} = \frac{\partial^2 f}{\partial x_i \partial x_j}$). Its eigenvalues indicate local curvature: positive definite means locally strictly convex (a valley), negative definite means a peak, and indefinite eigenvalues indicate a saddle point.
  
  ---
### B. Linear Algebra & Matrix Decompositions
  * **Matrix-Vector Products as Linear Transformations:**
  * *Where it appears:* Linear layers ($y = Wx + b$), PCA, coordinate projections.
  * *Intuition:* A matrix does not simply hold numbers; it stretches, rotates, and projects vector spaces.
  * **Positive Semi-Definite (PSD) Matrices:**
  * *Where it appears:* Covariance matrices in PCA (Module 9), Kernel Gram matrices in SVMs (Module 8).
  * *Condition:* A symmetric matrix $A \in \mathbb{R}^{n \times n}$ is PSD if $x^T A x \ge 0$ for all non-zero $x \in \mathbb{R}^n$. Guarantees that all eigenvalues are real and non-negative, ensuring optimization problems remain convex.
  * **Singular Value Decomposition (SVD):**
  * *Where it appears:* Dimensionality reduction (PCA), low-rank matrix approximation (LoRA fine-tuning for LLMs).
  * *Factorization:* Any real matrix $X \in \mathbb{R}^{n \times d}$ can be decomposed as:
    $$X = U \Sigma V^T$$
    where $U$ and $V$ contain the orthonormal left- and right-singular vectors, and $\Sigma$ contains the singular values ordered by magnitude.
  
  ---
### C. Probability, Mathematical Statistics & Information Theory
  * **Maximum Likelihood Estimation (MLE) vs. Maximum A Posteriori (MAP):**
  * *Where it appears:* Deriving loss functions from probabilistic assumptions.
  * *MLE:* Maximizes the likelihood of the observed data:
    $$\theta_{\text{MLE}} = \arg\max_\theta \sum_{i=1}^N \log P(y_i \mid x_i; \theta)$$
    *(Minimizing negative log-likelihood directly yields Mean Squared Error for Gaussian noise, and Cross-Entropy for Bernoulli/Categorical distributions).*
  * *MAP:* Introduces a prior distribution $P(\theta)$ over parameters (Bayesian framing):
    $$\theta_{\text{MAP}} = \arg\max_\theta \left[ \sum_{i=1}^N \log P(y_i \mid x_i; \theta) + \log P(\theta) \right]$$
    *(A Gaussian prior $\mathcal{N}(0, \sigma^2)$ yields $L_2$ Ridge regularization; a Laplace prior yields $L_1$ Lasso regularization).*
  * **Entropy, Cross-Entropy & Kullback-Leibler (KL) Divergence:**
  * *Shannon Entropy:* Average uncertainty in a distribution:
    $$H(P) = -\sum_{x} P(x) \log P(x)$$
  * *KL Divergence:* Relative entropy measuring distance between true distribution $P$ and approximation $Q$:
    $$D_{\text{KL}}(P \parallel Q) = \sum_{x} P(x) \log\left(\frac{P(x)}{Q(x)}\right)$$
  * *Cross-Entropy:* $H(P, Q) = H(P) + D_{\text{KL}}(P \parallel Q)$. When $P$ is fixed (ground-truth labels), minimizing cross-entropy is mathematically identical to minimizing KL divergence to the true distribution.
  
  ---
## 5. Deconstructing the 6 Expected Learning Outcomes
  
  Slide 6 lists explicit learning outcomes. Here is what Dr. Yu is assessing from an academic standpoint:
  
  ```
  Outcome 1: Core ML Concepts & Taxonomy
   └─▶ Explain trade-offs between generative vs. discriminative, parametric vs. non-parametric.
  
  Outcome 2: Implementation in Standard Libraries
   └─▶ Write clean, vectorized Python pipelines using scikit-learn and PyTorch without loops over samples.
  
  Outcome 3: Hardware Acceleration & GPU Training
   └─▶ Manage device transfers (.to(device)), balance batch sizes against VRAM limits, and avoid host-device synchronization bottlenecks.
  
  Outcome 4: Empirical Model Selection
   └─▶ Justify choosing an SVM vs. an XGBoost vs. a Deep Network based on data volume (N), feature dimension (D), and inference latency.
  
  Outcome 5: Rigorous Validation & Pathologies
   └─▶ Detect, isolate, and eliminate data leakage, target leakage, metric gaming, and catastrophic distribution shift.
  
  Outcome 6: Systemic Ethics, Fairness & Privacy
   └─▶ Quantify algorithmic bias using statistical fairness criteria and reason about data governance and compute costs.
  ```
  
  ---
## Summary Review Questions for Section 2
  
  1. *How does framing regularized regression as Maximum A Posteriori (MAP) estimation mathematically explain why Ridge ($L_2$) and Lasso ($L_1$) handle weights differently?*
  2. *Why does the kernel trick in SVM allow us to classify non-linearly separable data without suffering the exponential memory cost of calculating explicit high-dimensional feature vectors?*
  3. *Why is the singular value decomposition (SVD) of a centered data matrix mathematically equivalent to finding the eigenvectors of the sample covariance matrix in PCA?*
  
  ---