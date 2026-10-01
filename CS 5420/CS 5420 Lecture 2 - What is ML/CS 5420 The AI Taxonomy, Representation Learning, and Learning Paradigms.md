## 1. Classroom Survey & Cohort Demographics (Slide 2)
  
  Dr. Yu opens with the results of the student survey from Lecture 1:
  * **Real-world Applications:** $78.8\%$ (highest student demand)
  * **Live Coding Demos:** $59.6\%$
  * **Computer Vision & Deep Learning:** $57.7\%$
  * **Hands-on Labs / Code:** $57.7\%$
  * **Ethics & Safety:** $51.9\%$
  
  **Graduate Takeaway:** The course is explicitly oriented toward practical, high-stakes deployments rather than abstract blackboard proofs. This directly motivates why Lecture 2 balances foundational theory with catastrophic real-world failure analysis.
  
  ---
## 2. Set-Theoretic Hierarchy: AI vs. ML vs. DL (Slide 3)
  
  The nested Venn diagram illustrates that every deep learning model is a machine learning model, and every machine learning model is an artificial intelligence system, but the converses do not hold.
  
  ```
  ┌──────────────────────────────────────────────────────────────────┐
  │ Artificial Intelligence (AI)                                     │
  │  - Symbolic Logic, Expert Systems (MYCIN), Search (A*, Minimax)  │
  │  - Knowledge Graphs, Automated Theorem Provers                   │
  │                                                                  │
  │  ┌────────────────────────────────────────────────────────────┐  │
  │  │ Machine Learning (ML)                                      │  │
  │  │  - Statistical Learning, Optimization, Inductive Bias       │  │
  │  │  - Linear Models, Trees, SVMs, k-Means, Ensembles          │  │
  │  │                                                            │  │
  │  │  ┌──────────────────────────────────────────────────────┐  │  │
  │  │  │ Deep Learning (DL)                                   │  │  │
  │  │  │  - Multi-layer Neural Networks                       │  │  │
  │  │  │  - Representation Learning / End-to-End Gradients    │  │  │
  │  │  │  - CNNs, Transformers, Diffusion, ResNets            │  │  │
  │  │  └──────────────────────────────────────────────────────┘  │  │
  │  └────────────────────────────────────────────────────────────┘  │
  └──────────────────────────────────────────────────────────────────┘
  ```
### A. Artificial Intelligence (The Broadest Scope)
  * **Definition:** Computational systems capable of performing tasks typically requiring human biological intelligence (planning, reasoning, language understanding, symbolic manipulation).
  * **Non-ML Examples (Classical / GOFAI — "Good Old-Fashioned AI"):**
  * *Heuristic Graph Search:* $A^*$ pathfinding, Dijkstra's algorithm, Alpha-Beta pruning in chess engines.
  * *Symbolic / Rule-Based Expert Systems:* IF-THEN production rule inference engines (e.g., medical diagnostics in the 1970s).
  * *Constraint Satisfaction Problems (CSPs):* Backtracking solvers for Boolean satisfiability (SAT).
  * **Core Limitation:** Brittleness. Systems fail completely when encountering non-deterministic edge cases, sensor noise, or high-dimensional unstructured data.
### B. Machine Learning (Statistical Induction)
  * **Formal Definition (Tom Mitchell, 1997):**
  > *"A computer program is said to learn from experience $E$ with respect to some class of tasks $T$ and performance measure $P$, if its performance at tasks in $T$, as measured by $P$, improves with experience $E$."*
  * **Core Paradigm Shift:** Instead of a human programmer writing deterministic conditional branches:
  $$\text{Input} + \text{Algorithm (Rules)} \longrightarrow \text{Output}$$
  Machine learning inverts the control flow:
  $$\text{Input} + \text{Observed Outputs (Data)} \longrightarrow \text{Learned Function } f(x)$$
### C. Deep Learning (Hierarchical Representation Learning)
  * **Definition:** A subfield of ML parameterized by multi-layered artificial neural networks optimized via gradient descent and reverse-mode automatic differentiation.
  * **The "Deep" Criterion:** The compositional depth of parameterized transformations:
  $$f(x) = f^{(L)}\left(f^{(L-1)}\left(\dots f^{(1)}(x; \theta_1)\dots; \theta_{L-1}\right); \theta_L\right)$$
  where each hidden layer computes a non-linear continuous mapping: $h^{(l)} = \sigma\left(W^{(l)} h^{(l-1)} + b^{(l)}\right)$.
  
  ---
## 3. The Representation Crisis: Hand-Engineered vs. Learned Features (Slide 4)
  
  Slide 4 captures the fundamental operational divide between classical statistical ML and modern deep learning:
  
  $$\text{Classical ML: } \text{Input } x \xrightarrow{\text{Human Engineering}} \phi(x) \xrightarrow{\text{Shallow Estimator}} \hat{y}$$
  
  $$\text{Deep Learning: } \text{Input } x \xrightarrow{\text{Jointly Optimized Representation Layers } \phi(x; \theta_{\text{rep}})} h \xrightarrow{\text{Output Layer } g(h; \theta_{\text{out}})} \hat{y}$$
  
  ```
  Classical Machine Learning Pipeline (Decoupled, Human-in-the-Loop Feature Engineering):
  Raw Pixels ──▶ [Human Domain Expert: SIFT, HOG, Edge Filters] ──▶ Feature Vector φ(x) ──▶ [SVM / Random Forest] ──▶ Output
  
  Deep Learning Pipeline (End-to-End Differentiable Representation Learning):
  Raw Pixels ──▶ [Layer 1: Edges] ──▶ [Layer 2: Textures] ──▶ [Layer 3: Parts] ──▶ [Layer 4: Semantic Classes] ──▶ Output
               ▲                                                                                           │
               └────────────────────── Joint Backpropagation (Loss Gradient) ─────────────────────────────┘
  ```
### A. Classical Feature Engineering ("Rules Driven Learning")
  * **Process:** Domain experts manually invent mathematical transformations ($\phi: \mathcal{X} \to \mathbb{R}^d$) to extract salient signals:
  * *Computer Vision:* SIFT (Scale-Invariant Feature Transform), HOG (Histogram of Oriented Gradients), Haar wavelets.
  * *Audio:* Mel-Frequency Cepstral Coefficients (MFCCs).
  * *NLP:* $n$-grams, TF-IDF weights, syntactic parse trees.
  * **The Pathology:** The human feature extractor is **frozen and decoupled** from the final classifier. If the chosen feature representation discards critical information (e.g., phase information in audio or fine color gradations in vision), the downstream classifier cannot recover it.
### B. Deep Feature Learning ("Data Driven Learning")
  * **Process:** Raw, unstructured observations (pixel grids, raw PCM audio waveforms, token indices) are fed directly into the model. The feature extraction layers $\phi(\cdot; \theta)$ and decision boundary $g(\cdot; w)$ are **jointly optimized end-to-end**:
  $$\theta^* = \arg\min_\theta \frac{1}{N}\sum_{i=1}^N \mathcal{L}\left(g(\phi(x_i; \theta_{\text{rep}}); \theta_{\text{out}}), y_i\right)$$
  * **Hierarchical Abstraction:** Lower layers learn low-level primitives (Gabor-like edge detectors, frequency filters), intermediate layers assemble parts (corners, textures, phonemes), and higher layers encode abstract semantic concepts (object categories, sentiment).
### C. The Fundamental Trade-Off
  
  | Property | Classical Machine Learning (Engineered $\phi(x)$) | Deep Learning (Learned $\phi(x; \theta)$) |
  | :--- | :--- | :--- |
  | **Data Efficiency ($N$)** | High performance on small sample regimes ($N < 10{,}000$). | High sample complexity; underfits/overfits without massive data ($N > 10^5$). |
  | **Domain Inductive Bias** | Explicitly injected by expert human knowledge. | Weak inductive bias; must learn spatial/temporal invariances from data. |
  | **Compute Requirements** | Low; runs efficiently on standard multi-core CPUs. | Extreme; requires hardware accelerators (GPUs, TPUs) with high VRAM bandwidth. |
  | **Interpretability** | Moderate-to-high; features correspond to known physical units. | Black-box; distributed, unentangled latent representations. |
  | **Performance Ceiling** | Plateaus quickly as data scales; bounded by human feature quality. | Continues to scale with data and parameter capacity ("The Bitter Lesson"). |
  
  ---
## 4. The Three Core Learning Paradigms (Slides 5–6)
  
  Machine learning problems are categorized by the nature of the feedback signal available during optimization:
  
  ```
                            The Three Paradigms
  ┌────────────────────────────────────────────────────────────────────────┐
  │  1. Supervised Learning:    Pairs (x, y) ──▶ Learn Mapping f(x) ≈ y   │
  │  2. Unsupervised Learning:  Inputs {x}   ──▶ Discover Structure P(x)   │
  │  3. Reinforcement Learning: Environment  ──▶ Policy π(a|s) for Reward │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Paradigm 1: Supervised Learning (Slide 5)
  * **Data Formulation:** The training set consists of $N$ input-output tuples drawn i.i.d. from a joint distribution $P(X, Y)$:
  $$\mathcal{D}_{\text{train}} = \left\{ (x_1, y_1), (x_2, y_2), \dots, (x_N, y_N) \right\} \subset \mathcal{X} \times \mathcal{Y}$$
  * **Mathematical Objective:** Learn a parameterized function $f_\theta: \mathcal{X} \to \mathcal{Y}$ that minimizes the expected empirical risk:
  $$\hat{\theta} = \arg\min_\theta \frac{1}{N}\sum_{i=1}^N \mathcal{L}\left(y_i, f_\theta(x_i)\right)$$
  * **Sub-Tasks:**
  1. **Regression:** Target space is continuous: $\mathcal{Y} \subseteq \mathbb{R}$ or $\mathbb{R}^k$.
     * *Examples:* Predicting real estate valuations, continuous temperature, remaining useful equipment life.
     * *Standard Losses:* Mean Squared Error (MSE, $L_2$), Mean Absolute Error (MAE, $L_1$), Huber loss.
  2. **Classification:** Target space is categorical: $\mathcal{Y} = \{1, 2, \dots, C\}$.
     * *Examples:* Tumor detection (binary: $\{0, 1\}$), handwritten digit recognition (multiclass: $\{0, \dots, 9\}$).
     * *Standard Losses:* Binary Cross-Entropy (Log Loss), Categorical Cross-Entropy, Hinge Loss (SVM).
  
  ---
### Paradigm 2: Unsupervised Learning (Slides 5–6)
  * **Data Formulation:** Inputs contain no target supervision; there is **no ground-truth $y$ column**:
  $$\mathcal{D} = \left\{ x_1, x_2, \dots, x_N \right\}, \quad x_i \in \mathbb{R}^d$$
  * **Mathematical Objective:** Model the underlying data-generating distribution $P(X)$ or learn a low-dimensional manifold mapping $z = g(x)$ that preserves structural topology.
  * **Why "Everything Changes":**
  * Without $y$, there is no external error signal $(y - \hat{y})$ to compute direct gradients against.
  * Validation cannot rely on simple accuracy or cross-entropy against labels. Evaluation is often mathematically ill-posed, requiring proxy objectives (reconstruction error, silhouette coefficients, log-likelihood).
  * **Core Sub-Tasks:**
  1. **Clustering:** Partitioning $\mathcal{X}$ into $K$ disjoint subsets minimizing intra-cluster variance:
     $$\arg\min_S \sum_{k=1}^K \sum_{x \in S_k} \| x - \mu_k \|^2$$
  2. **Dimensionality Reduction & Manifold Learning:** Projecting high-dimensional data $x \in \mathbb{R}^d$ into a lower-dimensional latent space $z \in \mathbb{R}^k$ ($k \ll d$) while minimizing reconstruction distortion (e.g., PCA, t-SNE, UMAP, Autoencoders).
  3. **Density Estimation:** Inferring the continuous probability density function $p(x)$ that generated the samples (e.g., Gaussian Mixture Models, Kernel Density Estimation, Normalizing Flows).
  
  ---
### Paradigm 3: Reinforcement Learning (Slide 5)
  * **Data Formulation:** No static dataset of pairs or structures. The agent interacts dynamically with an environment modeled as a **Markov Decision Process (MDP)**:
  $$\mathcal{M} = \langle \mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R}, \gamma \rangle$$
  * **Feedback Mechanism:** The agent receives scalar rewards $r_t \in \mathbb{R}$ delayed over extended horizons. Actions change the future state of the world ($s_{t+1} \sim \mathcal{P}(\cdot \mid s_t, a_t)$).
  * **The Optimization Objective:** Learn a policy $\pi(a \mid s)$ that maximizes the expected discounted cumulative return:
  $$J(\pi) = \mathbb{E}_{\tau \sim \pi} \left[ \sum_{t=0}^T \gamma^t r(s_t, a_t) \right]$$
  * **Why It Is Out of Scope This Term:** RL introduces non-stationary distributions, active credit assignment problems, and the exploration-exploitation dilemma. CS 5420 concentrates on the core statistical foundations of supervised and unsupervised systems.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why is an $A^*$ pathfinding algorithm considered Artificial Intelligence, but definitively not Machine Learning?*
  2. *In terms of sample efficiency and optimization landscapes, what are the primary trade-offs of using end-to-end learned representations instead of hand-crafted domain features?*
  3. *Why does the complete absence of a ground-truth label $y$ in unsupervised learning make model evaluation fundamentally harder than in supervised learning?*
  
  ---