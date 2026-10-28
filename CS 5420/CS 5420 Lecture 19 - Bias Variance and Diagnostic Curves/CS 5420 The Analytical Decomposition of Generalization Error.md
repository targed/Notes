## 1. The Core Phenomenon: Training Error Is Not Test Error (Slide 2)
  
  Slide 2 demonstrates the disconnect between empirical training performance and true generalization error using an explicit target function corrupted by additive Gaussian noise:
  
  $$y = f(x) + \epsilon = \cos(1.5\pi x) + \epsilon, \quad \text{where } \epsilon \sim \mathcal{N}(0, \sigma^2)$$
  
  Fitting polynomial models of varying capacity across $N = 15$ noisy training points reveals three distinct structural regimes:
  
  ```
                  Polynomial Fits on 15 Noisy Points (Slide 2)
      Degree 1: Underfit            Degree 4: About Right           Degree 15: Overfit
    y                              y                              y
   2 ┼    --- true f(x)           2 ┼    --- true f(x)           2 ┼ /  --- true f(x)
     │   ●        \                 │   ●  \                      │/   \
   0 ┼────\────────●──────        0 ┼───●───\───────●─────       0 ┼●───\───────●───\─
     │     \                      │          \─●───/              │      \─●───/     \
  -2 ┼──────●─────────────       -2 ┼─────────────────────      -2 ┼──────────────────● (Explodes!)
     └────────────────────▶ x       └────────────────────▶ x       └────────────────────▶ x
     Train MSE: 0.138               Train MSE: 0.008               Train MSE: 0.006 (Lowest!)
     Test MSE:  0.143               Test MSE:  0.013 (Optimal)     Test MSE:  0.798 (Catastrophic)
  ```
  
  Slide 2 emphasizes the central conclusion:
  $$\mathbf{\text{"Degree 15 has the lowest training MSE and by far the highest test MSE. Fitting the training set is not the goal."}}$$
  
  * **Degree 1 (Linear Hyperplane):** The hypothesis space $\mathcal{H}_1$ is too rigid to capture the curvature of $\cos(1.5\pi x)$. It exhibits high error on both training and test data (**Underfitting**).
  * **Degree 4 (Quartic Polynomial):** Matches the underlying complexity of the cosine arc while ignoring sample-specific noise. It achieves optimal test generalization.
  * **Degree 15 (High-Degree Polynomial):** The hypothesis space has $16$ parameters for only $15$ data points ($d > N$). It interpolates the training points perfectly ($\text{Train MSE} \to 0$), but oscillates wildly between samples (Runge’s phenomenon), causing test error to explode by a factor of **$60\times$** (**Overfitting**).
  
  ---
## 2. Flexibility and the Error Trajectory (Slide 3)
  
  Slide 3 plots the empirical Mean Squared Error averaged across **500 independent training draws of 30 points each**:
  
  ```
                 Flexibility vs. Expected Error Curves (Slide 3)
      MSE
       ▲
  0.15 ┼   ● Test MSE (High Bias)                           ● Test MSE (High Variance)
       │    \                                              /
  0.10 ┼     \                                            /
       │      \             Lowest Test MSE: Degree 4    /
  0.05 ┼       \                     │                  /
       │        \                    ▼                 /
  0.01 ┼ - - - - ●─────────●─────────●─────────●──────● - - - Var(ε) = 0.01 (Noise Floor)
       │          \         \                 /      /
  0.00 ┴───────────●─────────●───────────────────────●───────▶ Polynomial Degree (Flexibility)
       1           2         3       4         5    12
                                     ▲
                           Training MSE (Keeps Falling Monotonically)
  ```
  
  $$\mathbf{\text{"Never choose flexibility by training error. No model gets below } \text{Var}(\epsilon) = 0.01\text{."}}$$
  
  1. **Training MSE is strictly monotonically non-increasing** as polynomial degree expands:
   $$\text{Degree}_A < \text{Degree}_B \implies \min_w \hat{R}_{\text{train}}(w_{\text{Degree}_B}) \le \min_w \hat{R}_{\text{train}}(w_{\text{Degree}_A})$$
   Higher-degree polynomials contain all lower-degree polynomials as nested sub-spaces. Training error evaluates pure empirical fitting capacity; it can never detect when capacity becomes excessive.
  2. **Test MSE follows an asymmetric U-shape.** It declines rapidly as structural bias is eliminated, reaches a unique global minimum at degree 4, and then rises monotonically as parameter variance destabilizes the predictions.
  
  ---
## 3. Formal Mathematical Derivation: The Bias-Variance Decomposition (Slide 4)
  
  Slide 4 defines the three constituent components of generalization error:
  
  $$\mathbf{\text{"Two sources of test error you can change, plus one you cannot."}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Bias:             Error from approximating a complex reality with   │
  │                      a too-simple model class.                         │
  │                                                                        │
  │ 2. Variance:         Sensitivity of the fitted model f̂ to the random   │
  │                      fluctuations of the training dataset 𝒟.           │
  │                                                                        │
  │ 3. Irreducible Noise Randomness in the universe (ε).                   │
  │    Var(ε) = σ²:      No algorithm can eliminate it; it forms the floor.│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Step-by-Step Analytical Derivation:
  Assume a continuous data-generating process:
  $$y = f(x) + \epsilon, \quad \text{where } \mathbb{E}[\epsilon] = 0 \text{ and } \text{Var}(\epsilon) = \mathbb{E}[\epsilon^2] = \sigma^2$$
  
  Suppose we sample a random training dataset $\mathcal{D} = \{(x_i, y_i)\}_{i=1}^N$ from the true population distribution and train an estimator $\hat{f}(x; \mathcal{D})$. 
  Because $\mathcal{D}$ is stochastic, the fitted model $\hat{f}(x; \mathcal{D})$ is itself a random variable.
  
  We evaluate the **Expected Generalization Error** at an arbitrary query point $x$, taking expectations over both the training dataset draws $\mathcal{D}$ and the test label noise $\epsilon$:
  
  $$\mathbb{E}_{\mathcal{D}, \epsilon} \Big[ (y - \hat{f}(x))^2 \Big]$$
  
  To simplify notation, let $\hat{f} = \hat{f}(x; \mathcal{D})$, $\mathbb{E}[\hat{f}] = \mathbb{E}_{\mathcal{D}}[\hat{f}(x; \mathcal{D})]$, and $f = f(x)$.
#### Step 1: Substitute the Data-Generating Model
  Substitute $y = f + \epsilon$:
  $$\mathbb{E}_{\mathcal{D}, \epsilon} \Big[ (f + \epsilon - \hat{f})^2 \Big] = \mathbb{E}_{\mathcal{D}, \epsilon} \Big[ ((f - \hat{f}) + \epsilon)^2 \Big]$$
  
  Expand the quadratic polynomial:
  $$\mathbb{E}_{\mathcal{D}, \epsilon} \Big[ (f - \hat{f})^2 + 2\epsilon(f - \hat{f}) + \epsilon^2 \Big]$$
#### Step 2: Exploit Noise Independence
  Because the measurement noise $\epsilon$ on the test sample is independent of the training data $\mathcal{D}$ used to fit $\hat{f}$, and because $\mathbb{E}[\epsilon] = 0$, the cross-term vanishes:
  $$\mathbb{E}_{\mathcal{D}, \epsilon}[2\epsilon(f - \hat{f})] = 2 \, \mathbb{E}[\epsilon] \cdot \mathbb{E}_{\mathcal{D}}[f - \hat{f}] = 2(0) \cdot \mathbb{E}_{\mathcal{D}}[f - \hat{f}] = 0$$
  
  Since $\mathbb{E}[\epsilon^2] = \sigma^2$, the expectation reduces to:
  $$\mathbb{E}_{\mathcal{D}} \Big[ (f - \hat{f})^2 \Big] + \sigma^2$$
#### Step 3: Add and Subtract the Expected Model Prediction $\mathbb{E}[\hat{f}]$
  Inside the squared term, introduce the expected model output across datasets:
  $$f - \hat{f} = \Big(f - \mathbb{E}[\hat{f}]\Big) + \Big(\mathbb{E}[\hat{f}] - \hat{f}\Big)$$
  
  Square this binomial:
  $$(f - \hat{f})^2 = \Big(f - \mathbb{E}[\hat{f}]\Big)^2 + 2\Big(f - \mathbb{E}[\hat{f}]\Big)\Big(\mathbb{E}[\hat{f}] - \hat{f}\Big) + \Big(\mathbb{E}[\hat{f}] - \hat{f}\Big)^2$$
  
  Now, take the expectation $\mathbb{E}_{\mathcal{D}}$ across all three terms:
  1. **The First Term:** $\Big(f - \mathbb{E}[\hat{f}]\Big)^2$ is deterministic with respect to $\mathcal{D}$ (neither $f$ nor $\mathbb{E}[\hat{f}]$ depends on a specific dataset draw). Its expectation is itself:
   $$\mathbb{E}_{\mathcal{D}}\left[ \Big(f - \mathbb{E}[\hat{f}]\Big)^2 \right] = \mathbf{\Big( f(x) - \mathbb{E}[\hat{f}(x)] \Big)^2 \equiv \text{Bias}^2(\hat{f}(x))}$$
  2. **The Cross-Term:** Notice that $\Big(f - \mathbb{E}[\hat{f}]\Big)$ is a constant scalar. Passing the expectation through:
   $$\mathbb{E}_{\mathcal{D}}\left[ 2\Big(f - \mathbb{E}[\hat{f}]\Big)\Big(\mathbb{E}[\hat{f}] - \hat{f}\Big) \right] = 2\Big(f - \mathbb{E}[\hat{f}]\Big) \cdot \mathbb{E}_{\mathcal{D}}\Big[ \mathbb{E}[\hat{f}] - \hat{f} \Big]$$
   Since $\mathbb{E}_{\mathcal{D}}[\mathbb{E}[\hat{f}] - \hat{f}] = \mathbb{E}[\hat{f}] - \mathbb{E}[\hat{f}] = 0$, the cross-term vanishes completely:
   $$2\Big(f - \mathbb{E}[\hat{f}]\Big) \cdot (0) = \mathbf{0}$$
  3. **The Third Term:** Evaluates the definition of statistical variance across training set draws:
   $$\mathbb{E}_{\mathcal{D}}\left[ \Big(\hat{f} - \mathbb{E}[\hat{f}]\Big)^2 \right] = \mathbf{\text{Var}(\hat{f}(x))}$$
#### The Final Identity:
  $$\mathbf{\mathbb{E}_{\mathcal{D}, \epsilon} \Big[ (y - \hat{f}(x))^2 \Big] = \underbrace{\Big( f(x) - \mathbb{E}[\hat{f}(x)] \Big)^2}_{\text{Bias}^2} \;+\; \underbrace{\mathbb{E}_{\mathcal{D}}\Big[ (\hat{f}(x) - \mathbb{E}[\hat{f}(x)])^2 \Big]}_{\text{Variance}} \;+\; \underbrace{\sigma^2}_{\text{Irreducible Noise}}}$$
  
  ---
## 4. Visualizing the Ensemble: Consensus vs. Spread (Slide 5)
  
  Slide 5 illustrates the physical meaning of bias and variance by overlaying **20 distinct models fitted to 20 independent draws of 30 training points**:
  
  $$\mathbf{\text{"Bias: the average fit misses } f\text{. Variance: the fits spread apart from one training set to the next."}}$$
  
  ```
                     Overlaying 20 Independent Model Fits (Slide 5)
       Degree 1: High Bias, Low Variance              Degree 12: Low Bias, High Variance
    y                                              y
    2 ┼    --- true f(x)                           2 ┼    --- true f(x)
      │   \                                          │   \
    0 ┼────\─────────/─────────────                0 ┼────\─────────/─────────────
      │     \───●───/                                │     \───●───/
   -2 ┼────────────────────────────               -2 ┼────────────────────────────
      └────────────────────────────▶ x               └────────────────────────────▶ x
      • The fits AGREE with each other               • The fits DISAGREE violently
      • All 20 lines tightly clustered               • Wild spread across datasets
      • But the AVERAGE fit misses f(x)              • Average fit tracks f(x) closely,
      • High structural error                        • but individual fits are chaotic
  ```
  
  * **Degree 1 (High Bias, Low Variance):**
  * Every single one of the 20 models outputs nearly the identical flat line. The variance across fits is tiny ($\text{Var}(\hat{f}) \approx 0$).
  * However, the expected prediction $\mathbb{E}[\hat{f}(x)]$ completely misses the true curve $f(x)$. The model makes rigid, incorrect structural assumptions.
  * **Degree 12 (Low Bias, High Variance):**
  * If you compute the arithmetic average across all 20 erratic curves ($\mathbb{E}[\hat{f}(x)]$), the oscillations cancel out, and the mean curve tracks $\cos(1.5\pi x)$ accurately ($\text{Bias}^2 \approx 0$).
  * However, any *single* model trained on a specific training set deviates wildly from the mean. The model is hypersensitive to the specific noise realization of the dataset.
  
  ---
## 5. The Optimal Trade-Off Frontier (Slide 6)
  
  Slide 6 plots the three decomposed mathematical components together:
  
  $$\mathbf{\text{"As flexibility grows, squared bias falls and variance rises. Test MSE is lowest where both are small."}}$$
  
  ```
                       The Bias-Variance Trade-Off (Slide 6)
      Error
        ▲
   0.15 ┼   ● Squared Bias: (f(x) - E[f̂])² (Falls monotonically as capacity grows)
        │    \                                              / ● Variance: E[(f̂ - E[f̂])²]
   0.10 ┼     \                                            /    (Rises monotonically)
        │      \             Optimal Degree: 4            /
   0.05 ┼       \                    │                   /
        │        \                   ▼                  /
   0.01 ┼ - - - - ●──────────────────●─────────────────● - - - Var(ε) = 0.01 (Noise Floor)
        │          \                / \               /
   0.00 ┴───────────●──────────────●───●─────────────●────────▶ Model Flexibility
                    1              3   4             12
                                       ▲
                            Total Test MSE = Bias² + Variance + σ²
  ```
  
  Slide 6 cites a classic principle from *An Introduction to Statistical Learning* (ISLR):
  $$\mathbf{\text{"Degree 4 gives the lowest test MSE. ISLR: the challenge is a method for which both are low."}}$$
  
  * In low-capacity regimes, test error is dominated by **squared bias**.
  * In high-capacity regimes, test error is dominated by **parameter variance**.
  * The global optimization objective of machine learning engineering is identifying the regularization penalty, tree depth, or parameter dimension where the sum $\text{Bias}^2 + \text{Variance}$ is minimized.
  
  ---
## 6. Graduate Context: Classical U-Curve vs. Modern Double Descent
  
  Slide 6 illustrates the classical statistical paradigm: increasing model parameters beyond the sample size causes test error to diverge toward infinity. 
  
  In contemporary deep learning and overparameterized neural networks, researchers observed that the curve does not always diverge indefinitely. This led to the discovery of **Deep Double Descent** (Belkin et al., PNAS 2019):
  
  ```
                        The Double Descent Phenomenon
      Test Error
          ▲
          │   Classical U-Curve            Interpolation Peak (d ≈ N)
          │         \                            /▲\
          │          \                          / │ \   Modern Overparameterized Regime
          │           \   Underparameterized   /  │  \  (Test error DECREASES again!)
          │            \      Regime          /   │   \───────────────
          │             \───●────────────────/    │                   \──────
          │                 Optimal Capacity      │
          └───────────────────────────────────────┼────────────────────────▶ Model Capacity
                                                  d = N
  ```
  
  1. **The Classical Regime ($d < N$):** Test error follows the U-shape derived in Slide 6.
  2. **The Interpolation Threshold ($d \approx N$):** The number of parameters equals the number of training samples. The model is forced to fit every data point and its noise exactly without regularization; the Gram matrix $X^T X$ becomes ill-conditioned, and variance peaks catastrophically.
  3. **The Overparameterized Regime ($d \gg N$):** In modern architectures (Transformers, ResNets), parameters vastly exceed sample size. Because models are trained using stochastic gradient descent with implicit inductive regularization, the optimizer selects the **minimum-norm interpolating solution**, smoothing out the function and causing test error to decrease a second time.
  
  *Course Context:* For standard linear models, polynomial expansions, and classical estimators in Module 7, the classical U-curve strictly holds.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why is training error mathematically guaranteed to be a monotonically non-increasing function of polynomial degree, while test error follows a U-shaped trajectory?*
  2. *In the mathematical derivation of $\mathbb{E}[(y - \hat{f})^2]$, why does the cross-term $\mathbb{E}[2\epsilon(f - \hat{f})]$ evaluate strictly to zero? State the independence assumption required.*
  3. *If a model has zero bias ($\text{Bias}^2 = 0$) and zero variance ($\text{Variance} = 0$), can its expected test Mean Squared Error ever drop to $0.000$ on real-world data? Explain why or why not using Slide 4's noise floor.*
  
  ---