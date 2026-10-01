## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **First-Order Taylor Derivation of Steepest Descent:**
  * Expanding the cost function $J(w + \Delta w)$ around $w$ using a first-order Taylor series:
   $$J(w + \Delta w) \approx J(w) + \nabla J(w)^T \Delta w$$
  * For a step of fixed Euclidean length $\alpha = \|\Delta w\|_2$, define the directional unit vector $u = \frac{\Delta w}{\|\Delta w\|_2}$. The directional change in loss is:
   $$\Delta J \approx \alpha \Big( \nabla J(w)^T u \Big) = \alpha \|\nabla J(w)\|_2 \|u\|_2 \cos(\theta) = \alpha \|\nabla J(w)\|_2 \cos(\theta)$$
   where $\theta$ is the angle between the gradient vector $\nabla J(w)$ and the step direction $u$.
  * To achieve the steepest instantaneous decrease ($\Delta J \ll 0$), we minimize $\cos(\theta)$. The minimum occurs at $\theta = \pi$ ($180^\circ$), where $\cos(\theta) = -1$.
  * Therefore, the update direction must point **directly opposite to the gradient**:
   $$u^* = -\frac{\nabla J(w)}{\|\nabla J(w)\|_2} \implies \mathbf{\Delta w = -\eta \nabla J(w)}$$
  2. **Maximum Stable Learning Rate for $\lambda_{\max} = 500$:**
  * For quadratic loss functions, gradient descent is mathematically guaranteed to diverge if the step size exceeds the reciprocal of the Lipschitz constant of the gradient ($L = \lambda_{\max}$):
   $$\eta_{\text{max}} = \frac{2}{\lambda_{\max}(H)} = \frac{2}{500} = \mathbf{0.004}$$
  * Any learning rate $\eta \ge 0.004$ will cause updates to bounce across the valley walls with expanding amplitude, leading to numerical overflow (`NaN`).
  3. **Why Disparate Feature Variances Constrain the Learning Rate:**
  * In unscaled datasets, the maximum eigenvalue of the Hessian $\lambda_{\max}(X^T X)$ is dominated by the feature with the largest variance ($\lambda_{\max} \approx \sigma_{\max}^2$).
  * Because stability requires $\eta < \frac{2}{\lambda_{\max}}$, a single high-variance feature forces the global learning rate $\eta$ down to a tiny fraction.
  * However, progress along low-variance feature dimensions is governed by $\lambda_{\min}$. At this tiny learning rate, parameter updates along the low-variance directions ($\Delta w_{\text{flat}} \approx -\eta \lambda_{\min} w$) crawl, requiring tens of thousands of redundant iterations to reach the minimum.
  
  ---
## 2. The Three Gradient Regimes (Slides 6–7)
  
  Slide 6 formalizes the three operational paradigms of gradient-based optimization:
  
  $$\mathbf{\text{"Batch: all } n \text{ samples per update. Stable, expensive."}}$$
  $$\mathbf{\text{"Stochastic: one sample per update. Noisy, fast, can escape shallow minima."}}$$
  $$\mathbf{\text{"Mini-batch: 32 to 512 samples. What everyone actually uses."}}$$
  
  ```
                     The Avichawla Substack Diagram (Slide 7)
    #1 Stochastic Gradient Descent     #2 Mini-Batch Gradient Descent    #3 Batch Gradient Descent
    ┌────────────────────────────┐    ┌────────────────────────────┐    ┌────────────────────────────┐
    │     [One Data Point]       │    │   [Mini-Batch: 32 to 512]  │    │      [Entire Dataset]      │
    │            │               │    │              │             │    │              │             │
    │            ▼               │    │              ▼             │    │              ▼             │
    │     Update Weights         │    │       Update Weights       │    │       Update Weights       │
    │            │               │    │              │             │    │              │             │
    │  Repeat for ALL points     │    │  Repeat for ALL batches    │    │   Repeat for ALL epochs    │
    │  and all epochs            │    │  and all epochs            │    │                            │
    └────────────────────────────┘    └────────────────────────────┘    └────────────────────────────┘
  ```
  
  ---
### Mathematical Formulation of Empirical Risk
  In supervised learning, the objective function is the empirical expectation of the loss over $n$ training instances:
  
  $$J(w) = \frac{1}{n} \sum_{i=1}^n \mathcal{L}(y_i, f(x_i; w))$$
  
  The true population gradient is the arithmetic mean of all $n$ individual sample gradients:
  
  $$\nabla J(w) = \frac{1}{n} \sum_{i=1}^n \nabla_w \mathcal{L}(y_i, f(x_i; w))$$
  
  The three regimes differ solely in **how many samples are evaluated to approximate this gradient** before executing a parameter update.
  
  ---
## 3. Comparative Mechanics of the Three Paradigms
  
  ---
### Regime 1: Batch Gradient Descent (BGD)
  BGD computes the exact gradient over the **entire dataset of $n$ samples** before taking a single step:
  
  $$w^{(t+1)} = w^{(t)} - \eta \cdot \frac{1}{n} \sum_{i=1}^n \nabla_w \mathcal{L}(y_i, f(x_i; w^{(t)}))$$
  
  * **Update Frequency:** Exactly **$1$ update per epoch**.
  * **Computational Cost per Step:** $\mathcal{O}(n \cdot d)$ floating-point operations.
  * **Trajectory Properties:**
  * Deterministic and smooth. On convex surfaces, the loss is guaranteed to decrease monotonically at every step (under an appropriate $\eta$).
  * **The Systems Bottleneck:**
  * If a production dataset contains $n = 10{,}000{,}000$ samples, computing a single gradient update requires streaming $10$ million instances through memory. 
  * If the entire dataset cannot fit into host RAM or GPU VRAM, BGD requires continuous out-of-core disk I/O, making it impractical for large-scale training.
  
  ---
### Regime 2: Stochastic Gradient Descent (Online SGD)
  SGD approximates the full gradient using a **single randomly sampled observation** ($i_t \sim \text{Uniform}(\{1, \dots, n\})$):
  
  $$w^{(t+1)} = w^{(t)} - \eta \cdot \nabla_w \mathcal{L}(y_{i_t}, f(x_{i_t}; w^{(t)}))$$
  
  * **Update Frequency:** Exactly **$n$ updates per epoch**.
  * **Computational Cost per Step:** $\mathcal{O}(d)$ floating-point operations.
  * **Statistical Properties:**
  * The single-sample gradient is an **unbiased estimator** of the true gradient:
    $$\mathbb{E}_{i_t \sim \mathcal{U}(1, n)} \left[ \nabla_w \mathcal{L}(y_{i_t}, f(x_{i_t}; w)) \right] = \frac{1}{n}\sum_{i=1}^n \nabla_w \mathcal{L}(y_i, f(x_i; w)) \equiv \nabla J(w)$$
  * **Gradient Variance:** Extremely high. Because individual samples contain idiosyncratic measurement noise, the instantaneous gradient vector fluctuates wildly from step to step.
  
  ```
                    Optimization Trajectories in Loss Space
         Batch GD (Smooth & Direct)                   Stochastic GD (Noisy Random Walk)
    w_2                                          w_2
      ▲                                            ▲
      │       ( ( ( ● ) ) )                        │       ( ( ( ● ) ) )
      │      /   min                               │      /   min
      │     /                                      │     /\/\/\/\
      │    /                                       │    /  \/\  /\ (High variance walk)
      │   /  Direct monotonic path                 │   /    \/\/
      │  ● w⁽⁰⁾                                    │  ● w⁽⁰⁾
      └────────────────────────▶ w_1               └────────────────────────▶ w_1
  ```
#### The Dual Nature of Stochastic Noise:
  1. **The Advantage (Escaping Saddle Points & Local Minima):** Slide 6 notes: *"Noisy, fast, can escape shallow minima."* In non-convex loss landscapes (e.g., deep neural networks), the stochastic fluctuations inject kinetic noise, allowing the parameters to bounce out of poor, sharp local minima into broader, flatter basins that generalize better.
  2. **The Disadvantage (The Limit Cycle Trap):** Because the variance of the gradient estimator does not diminish near the minimum ($\text{Var}(\nabla \mathcal{L}_i) > 0$), standard SGD **never settles into the exact global minimum** under a fixed learning rate $\eta$. Instead, it oscillates in a bounded limit cycle around the minimum. To achieve exact asymptotic convergence, the learning rate must be decayed over time following the **Robbins-Monro Conditions**:
   $$\sum_{t=1}^\infty \eta_t = \infty \quad \text{and} \quad \sum_{t=1}^\infty \eta_t^2 < \infty$$
  
  ---
### Regime 3: Mini-Batch Gradient Descent (MBGD)
  Slide 6 states:
  $$\mathbf{\text{"Mini-batch: 32 to 512 samples. What everyone actually uses."}}$$
  $$\mathbf{\text{"Mini-batch is what PyTorch's DataLoader gives you."}}$$
  
  MBGD strikes a balance by partitioning the dataset into random subsets $\mathcal{B}_t \subset \{1, \dots, n\}$ of size $B$ (where $B \ll n$):
  
  $$w^{(t+1)} = w^{(t)} - \eta \cdot \frac{1}{B} \sum_{i \in \mathcal{B}_t} \nabla_w \mathcal{L}(y_i, f(x_i; w^{(t)}))$$
  
  * **Update Frequency:** Exactly **$\lceil n / B \rceil$ updates per epoch**.
  * **Computational Cost per Step:** $\mathcal{O}(B \cdot d)$ floating-point operations.
  
  ---
## 4. The Systems & Hardware Justification for Mini-Batching
  
  Why has the machine learning community standardized on mini-batch sizes of $B \in [32, 512]$ rather than pure SGD ($B = 1$) or full batch ($B = n$)?
  
  ```
                       The Statistical & Hardware Trade-Off
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Statistical Variance Reduction (The Law of Large Numbers):          │
  │    Let Σ denote the covariance matrix of individual sample gradients.  │
  │    The variance of the mini-batch sample mean gradient scales as:      │
  │                                                                        │
  │                 Var( 1/B ∑ ∇ℒ_i ) = (1 / B) · Σ                        │
  │                                                                        │
  │    • Moving from B = 1 to B = 64 reduces gradient variance by 98.4%!   │
  │    • Trajectory stabilizes significantly; allows 10x larger step sizes.│
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Hardware GPU Saturation (SIMT Vectorization):                       │
  │    • B = 1: Dispatches vector-vector operations. CUDA cores sit idle;  │
  │      memory bandwidth overhead dominates compute time.                 │
  │    • B = 64 to 512: Turns computation into dense matrix-matrix math    │
  │      (GEMM: X_batch ∈ ℝ^(B × d)). Tensor cores run at peak throughput. │
  │    • A GPU computes a batch of B = 64 in roughly the SAME physical     │
  │      wall-clock time as a single sample B = 1!                         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ```
                   The Law of Diminishing Returns in Batch Scaling
  Batch Size Scale Step:     B = 1 ──▶ B = 64              B = 512 ──▶ B = 1024
  Variance Reduction:        64× Drop (Massive win!)       2× Drop (Marginal win)
  Wall-Clock Compute Cost:   ~Identical (GPU saturated)    2× Compute Cost Increase
  Engineering Verdict:       HIGHLY EFFICIENT              INEFFICIENT
  ```
  
  ---
## 5. Comparative Summary Matrix (Slides 6–7)
  
  ```
  ┌─────────────────────┬───────────────────┬───────────────────┬────────────────────────┐
  │ Metric / Property   │ Batch GD (B = n)  │ Stochastic (B = 1)│ Mini-Batch (B = 32–512)│
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Samples per Step    │ n (Full dataset)  │ 1 single sample   │ B samples              │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Updates per Epoch   │ 1 update          │ n updates         │ ⌈n / B⌉ updates        │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Compute per Step    │ 𝒪(n · d) (Heavy)  │ 𝒪(d) (Minimal)    │ 𝒪(B · d) (Balanced)    │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Gradient Variance   │ Zero (Exact ∇J)   │ Extreme (Noisy)   │ Controlled (Σ / B)     │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Trajectory in Loss  │ Smooth, direct    │ Jagged, random    │ Moderately smooth,     │
  │                     │ monotonic path    │ walk              │ guided corridor        │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ GPU Utilization     │ Memory bound      │ Poor (Under-util) │ Optimal (SIMT / GEMM)  │
  │                     │ (OOM on large n)  │                   │                        │
  ├─────────────────────┼───────────────────┼───────────────────┼────────────────────────┤
  │ Non-Convex Minima   │ Trapped in local  │ Bounces out of    │ Escapes shallow basins,│
  │ Escapability        │ saddle points     │ shallow basins    │ converges to flat mins │
  └─────────────────────┴───────────────────┴───────────────────┴────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why does single-sample Stochastic Gradient Descent ($B = 1$) fail to utilize the parallel compute capability of modern GPUs compared to Mini-Batch Gradient Descent ($B = 64$)?*
  2. *Prove mathematically that the sample gradient calculated on a random mini-batch $\mathcal{B}$ of size $B$ is an unbiased estimator of the full-batch population gradient $\nabla J(w)$.*
  3. *Why is pure Batch Gradient Descent ($B = n$) more prone to getting trapped in poor, sharp local minima or saddle points in deep non-convex loss surfaces than Mini-Batch Gradient Descent?*
  
  ---