## 1. The Epistemology of Unsupervised Learning (Slides 2–3)
  
  Slide 2 opens by prompting the class to visually group unlabeled observations:
  
  $$\mathbf{\text{"150 points, 2 features, no } y\text{. How many groups do you see? Sketch where the borders would go."}}$$
  $$\mathbf{\text{"Your eyes just clustered the data. But how does a computer do it?"}}$$
  
  Slide 3 formalizes the structural divide between supervised and unsupervised paradigms:
  
  $$\mathbf{\text{"Clustering puts similar points in the same group. No } y\text{, so there is no accuracy to check it against."}}$$
  
  ```
                      Supervised vs. Unsupervised Learning (Slide 3)
         Supervised Setting (Given Labels y)            Unsupervised Setting (Only Inputs X)
    x₂                                             x₂
     ▲                                              ▲
     │    ● ● ●             ■ ■ ■                   │    ● ● ●             ● ● ●
     │  ● ● ● ●           ■ ■ ■ ■ ■                 │  ● ● ● ●           ● ● ● ● ●
     │   ● ● ●             ■ ■ ■ ■                  │   ● ● ●             ● ● ● ●
     │                                              │
     │            ▲ ▲ ▲                             │            ● ● ●
     │          ▲ ▲ ▲ ▲ ▲                           │          ● ● ● ● ●
     │           ▲ ▲ ▲                              │           ● ● ●
   0 ┴──────────────┬───────────────────▶ x₁      0 ┴──────────────┬───────────────────▶ x₁
     Ground-truth labels y ∈ {●, ■, ▲} given.       No target labels y exist!
     Model learns conditional mapping P(Y | X).     Algorithm must discover intrinsic structure.
  ```
  
  ---
### The Fundamental Mathematical Shift:
  * **In Supervised Learning:** 
  The training dataset consists of input-output pairs:
  $$\mathcal{D}_{\text{sup}} = \{(x_1, y_1), (x_2, y_2), \dots, (x_N, y_N)\} \subset \mathcal{X} \times \mathcal{Y}$$
  The objective function minimizes empirical loss against a known ground truth:
  $$\min_\theta \frac{1}{N}\sum_{i=1}^N \mathcal{L}(y_i, f_\theta(x_i))$$
  Generalization is measured directly via objective metrics: Classification Accuracy, ROC-AUC, or Mean Squared Error.
  * **In Unsupervised Clustering:** 
  The dataset contains no target labels:
  $$\mathcal{D}_{\text{unsup}} = \{x_1, x_2, \dots, x_N\} \subset \mathbb{R}^d$$
  * **The Problem:** There is no target vector $y$ to compute residuals $(y_i - \hat{y}_i)$ against.
  * **The Consequence:** **Classification accuracy does not exist.** Model evaluation becomes ill-posed; the algorithm must rely on heuristic geometric proxies (such as within-cluster cohesion and between-cluster separation) rather than ground-truth error minimization.
  
  ---
## 2. What Makes a "Good" Cluster? (Slide 4)
  
  Slide 4 defines the geometric dual objectives that govern clustering:
  
  $$\mathbf{\text{"Within a cluster: Small distances. Points sit close to their own center."}}$$
  $$\mathbf{\text{"Between clusters: Large distances. The centers sit far apart."}}$$
  $$\mathbf{\text{"'Similar' means 'close'. So the distance you choose decides the clusters."}}$$
  
  ```
                       The Dual Clustering Criteria (Slide 4)
  Feature x₂
      ▲
      │             ▲ ▲ ▲                       ■ ■ ■
      │           ▲ ★_C₁▲ ▲ ─── between: ───▶ ■ ★_C₂■ ■   (Large Inter-Cluster Distance:
      │            ▲ ▲ ▲         LARGE          ■ ■ ■      ||μ₁ - μ₂|| ──▶ Maximized)
      │               \                          /
      │                \                        /
      │                 \                      /
      │                  ▼                    ▼
      │                     ●   ●   ●
      │                   ●   ★_C₃●   ●  (Small Intra-Cluster Distance:
      │                     ●   ●   ●     ||xᵢ - μ₃|| ──▶ Minimized)
      └────────────────────────────────────────────────────────▶ Feature x₁
         ★ = Cluster Centroids (μⱼ)
  ```
  
  ---
### The Mathematical Formulation of Quality:
  Let $\mathcal{C} = \{C_1, C_2, \dots, C_K\}$ be a partition of $N$ observations into $K$ disjoint subsets. A clustering algorithm balances two competing forces:
  
  1. **Intra-Cluster Cohesion (Compactness):**
   The variance within each cluster $C_k$ around its centroid $\mu_k$ must be minimized:
   $$\text{Within}(C_k) = \sum_{x_i \in C_k} \|x_i - \mu_k\|_2^2 \longrightarrow \mathbf{\text{Minimize}}$$
  2. **Inter-Cluster Separation (Isolation):**
   The pairwise distance between distinct cluster centroids must be maximized:
   $$\text{Between}(C_j, C_k) = \|\mu_j - \mu_k\|_2^2 \longrightarrow \mathbf{\text{Maximize}}$$
  
  Because computers have no intrinsic common sense, **"similarity" is defined entirely by spatial proximity in a metric space $(\mathcal{X}, d)$**. The choice of metric function $d(x, z)$ dictates which points are considered neighbors.
  
  ---
## 3. Measuring Distance: The Metric Space Toolkit (Slide 5)
  
  Slide 5 catalogs the three primary distance functions used in unsupervised learning:
  
  $$\mathbf{\text{"Every clustering method starts from a distance between two points } x \text{ and } z\text{."}}$$
  
  ```
                      The Distance Metric Spectrum (Slide 5)
  ┌───────────────────────┬──────────────────────────┬──────────────────────────────────────────┐
  │ Metric Family         │ Mathematical Formula     │ Geometric Behavior & Use Case            │
  ├───────────────────────┼──────────────────────────┼──────────────────────────────────────────┤
  │ Euclidean (L₂)        │ d(x, z) = √(∑ (xⱼ - zⱼ)²)│ Straight-line physical distance.         │
  │                       │                          │ Rotational invariant; default in k-means.│
  ├───────────────────────┼──────────────────────────┼──────────────────────────────────────────┤
  │ Manhattan (L₁)        │ d(x, z) = ∑ |xⱼ - zⱼ|    │ Orthogonal grid / taxicab distance.      │
  │                       │                          │ Robust to single-coordinate outliers.    │
  ├───────────────────────┼──────────────────────────┼──────────────────────────────────────────┤
  │ Cosine Distance       │ 1 - (xᵀz / (||x|| ||z||))│ Evaluates angular difference in [0, 2].  │
  │                       │                          │ Strictly scale-invariant; text & embeds. │
  └───────────────────────┴──────────────────────────┴──────────────────────────────────────────┘
  ```
  
  Slide 5 issues a foundational warning:
  $$\mathbf{\text{"Distances add up feature by feature. A feature measured in large units takes over."}}$$
  
  ---
### In-Depth Mathematical Comparison:
#### 1. Euclidean Distance ($L_2$ Norm):
  $$d_2(x, z) = \sqrt{\sum_{j=1}^d (x_j - z_j)^2} = \|x - z\|_2$$
  * **Underlying Assumption:** The feature space is **isotropic** (variance is equal in all directions).
  * **The Optimization Tie-In:** The sample mean $\mu = \frac{1}{N}\sum x_i$ is the unique mathematical point that minimizes the sum of squared Euclidean distances. This mathematical alignment is why standard $k$-means relies exclusively on Euclidean distance.
#### 2. Manhattan Distance ($L_1$ Norm / Taxicab):
  $$d_1(x, z) = \sum_{j=1}^d |x_j - z_j| = \|x - z\|_1$$
  * **Outlier Resilience:** Slide 5 notes: *"One big gap counts less."* 
  * Under Euclidean distance, an error of $\Delta = 10$ along a single feature contributes $10^2 = \mathbf{100}$ to the sum.
  * Under Manhattan distance, that same error contributes linearly: $|\Delta| = \mathbf{10}$.
  * The point that minimizes the sum of absolute Manhattan distances is the **component-wise median**. Replacing Euclidean distance with Manhattan distance in $k$-means yields the **$k$-Medians algorithm**.
#### 3. Cosine Distance (Angular Metric):
  $$d_{\cos}(x, z) = 1 - \cos(\theta) = 1 - \frac{x^T z}{\|x\|_2 \|z\|_2}$$
  * **Length Invariance:** Evaluates strictly the angle $\theta$ between two vectors, completely ignoring their Euclidean magnitudes ($\|x\|_2$ and $\|z\|_2$).
  * **Text Mining & Embeddings:** In natural language processing, Document A might be a short 100-word article about machine learning, while Document B is a 5,000-word textbook chapter on the same topic. Their Euclidean distance is massive because word counts differ by $50\times$. However, their normalized word frequency vectors point in the identical direction ($\theta \approx 0 \implies \cos(\theta) \approx 1 \implies d_{\cos} \approx 0$). Cosine distance recognizes them as belonging to the same semantic cluster.
  
  ---
## 4. The Scale Disparity Collapse: Scale Before You Cluster (Slide 6)
  
  Slide 6 presents an empirical case study demonstrating why unscaled data breaks distance-based clustering:
  
  $$\mathbf{\text{"120 customers: age (20–70 years) and income (\$20k–\$100k). } k\text{-means with } k = 2\text{."}}$$
  $$\mathbf{\text{"Standardize first. Raw income gaps are }\sim 1{,}000\times \text{ age gaps, so raw } k\text{-means never sees age."}}$$
  
  ```
                       The Scaling Collapse in k-Means (Slide 6)
       Raw Features: Split by Income ONLY               Standardized Features: Split by Age Group
    Income ($)                                       Income ($)
     100k ┼  ■ ■■ ■        ■ ■■ ■                     100k ┼  ■ ■■ ■        ● ●● ●
          │  ■ ■■■ ■       ■ ■■■ ■                         │  ■ ■■■ ■       ● ●●● ●
      80k ┼  ■ ■■ ■        ■ ■■ ■                      80k ┼  ■ ■■ ■        ● ●● ●
          │  ────────────  ──────────── Cluster Cut        │         ■              ●
      60k ┼  ● ●● ●        ● ●● ●                      60k ┼  ■ ■■ ■        ● ●● ●
          │  ● ●●● ●       ● ●●● ●                         │  ■ ■■■ ■       ● ●●● ●
      40k ┼  ● ●● ●        ● ●● ●                      40k ┼  ■ ■■ ■        ● ●● ●
          │                                                │
      20k ┼                                            20k ┼
          └───────┬────────────┬────────▶ Age              └───────┬────────────┬────────▶ Age
                 25           60                                  25           60
          Two vertical age clusters IGNORED!               Correct demographic segmentation:
          Horizontal boundary cuts income!                 Young Customers (■) vs. Seniors (●)
  ```
  
  ---
### The Mathematical Proof of Dominance:
  Consider two customer observations evaluated under raw units:
  * Customer 1: $\text{Age} = 25\text{ years}, \quad \text{Income} = \$30{,}000$
  * Customer 2: $\text{Age} = 65\text{ years}, \quad \text{Income} = \$70{,}000$
  
  Compute the squared Euclidean distance between them:
  $$\Delta_{\text{age}} = 65 - 25 = 40 \implies \Delta_{\text{age}}^2 = (40)^2 = \mathbf{1{,}600}$$
  $$\Delta_{\text{income}} = 70{,}000 - 30{,}000 = 40{,}000 \implies \Delta_{\text{income}}^2 = (40{,}000)^2 = \mathbf{1{,}600{,}000{,}000}$$
  
  $$\|x_1 - x_2\|_2^2 = 1{,}600{,}000{,}000 + 1{,}600 = \mathbf{1{,}600{,}001{,}600}$$
  
  $$\text{Percentage Contribution of Age} = \frac{1{,}600}{1{,}600{,}001{,}600} \times 100\% = \mathbf{0.0001\%}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Systems Pathology:                                                 │
  │                                                                        │
  │ • Income variation contributes 99.9999% of the distance metric!        │
  │ • Age contributes one part in a million.                               │
  │ • Even though the true natural segmentation in the population is       │
  │   demographic (Young vs. Senior), raw k-means is completely blind to   │
  │   the age axis. It clusters strictly by high-income vs. low-income.    │
  │                                                                        │
  │ • The Fix: StandardScaler transforms both variables to zero mean and   │
  │   unit variance (σ = 1.0), restoring equal geometric leverage.         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why is it mathematically impossible to calculate standard classification accuracy, Precision, or Recall on an unsupervised clustering model? What geometric proxies are used instead?*
  2. *In text classification or document clustering, why is Cosine distance preferred over Euclidean distance when comparing documents of vastly different word lengths?*
  3. *If a dataset contains `vehicle_weight` in pounds ($\sigma \approx 800$) and `engine_cylinders` ($\sigma \approx 1.5$), prove why unstandardized $k$-means clustering will produce clusters that ignore cylinder count.*
  
  ---