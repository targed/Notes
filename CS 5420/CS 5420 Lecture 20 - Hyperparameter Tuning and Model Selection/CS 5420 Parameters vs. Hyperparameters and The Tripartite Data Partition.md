## 1. Parameters vs. Hyperparameters: The Optimization Hierarchy (Slide 2)
  
  Slide 2 establishes the fundamental division of quantities within machine learning models:
  
  $$\mathbf{\text{"Every model has two kinds of numbers. Only one kind is learned from the training data."}}$$
  
  ```
                      The Two Tiers of Machine Learning Quantities
  ┌───────────────────────────────────────────────┬───────────────────────────────────────────────┐
  │ Parameters (θ)                                │ Hyperparameters (λ)                           │
  ├───────────────────────────────────────────────┼───────────────────────────────────────────────┤
  │ • Learned by .fit()                           │ • Set BEFORE .fit()                           │
  │ • Selected automatically by the training data │ • Chosen by the engineer using held-out data  │
  │ • Optimized via empirical risk minimization   │ • Governs the flexibility / capacity of ℋ     │
  │ • Examples: OLS weights w, logistic weights,  │ • Examples: k in k-NN, regularization α (λ),  │
  │   decision tree split thresholds              │   polynomial degree d, learning rate η        │
  └───────────────────────────────────────────────┴───────────────────────────────────────────────┘
  ```
  
  Slide 2 delivers the governing theoretical principle:
  $$\mathbf{\text{"Hyperparameters set flexibility. Training error always picks the most flexible setting."}}$$
  
  ---
### Mathematical Formulation of the Bilevel Optimization Problem:
  Model fitting and hyperparameter selection form a **bilevel optimization problem**:
  
  1. **Lower-Level Problem (Parameter Estimation):**
   Given a fixed hyperparameter configuration $\lambda \in \Lambda$ (which defines the hypothesis space $\mathcal{H}_\lambda$), the parameter vector $\theta^*(\lambda)$ is learned by minimizing the empirical training loss:
   $$\theta^*(\lambda) = \arg\min_{\theta} \mathcal{L}_{\text{train}}(\theta; \lambda)$$
   * *Mechanism:* Computed internally by closed-form equations (e.g., Normal Equations $(X^T X)^{-1} X^T y$) or iterative numerical solvers (e.g., L-BFGS, SGD).
  2. **Upper-Level Problem (Hyperparameter Selection):**
   The optimal hyperparameter $\lambda^*$ is selected by minimizing validation error evaluated on **held-out data** unobserved during the parameter fitting phase:
   $$\lambda^* = \arg\min_{\lambda \in \Lambda} \mathcal{L}_{\text{val}}\left(\theta^*(\lambda)\right)$$
   * *Why Training Error Fails:* If we attempted to optimize $\lambda$ on the training set:
     $$\min_{\lambda} \mathcal{L}_{\text{train}}(\theta^*(\lambda))$$
     the optimization would trivially select the maximal capacity setting (e.g., polynomial degree $d \to \infty$, regularization $\lambda \to 0$, or $k$-NN neighborhood $k = 1$), yielding zero training error while causing test variance to explode.
  
  ---
## 2. Inventory of Hyperparameters Across Model Families (Slide 3)
  
  Slide 3 catalogs the primary hyperparameters encountered across standard algorithms, detailing how each knob navigates the **bias-variance spectrum**:
  
  ```
  ┌───────────────────────┬───────────────────────────┬──────────────────────────────────────────┐
  │ Model Family          │ Hyperparameters (Scikit)  │ Directional Effect on Capacity           │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ k-Nearest Neighbors   │ n_neighbors (k)           │ Larger k ──▶ Smoother, simpler boundary  │
  │                       │ weights ('uniform'/'dist')│ (High Bias, Low Variance).               │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ Logistic Regression   │ C (Inverse penalty = 1/λ) │ Smaller C (larger λ) ──▶ Stronger penalty│
  │                       │ penalty ('l1', 'l2')      │ (High Bias, Low Variance).               │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ Ridge / Lasso         │ alpha (λ)                 │ Larger alpha ──▶ Shrinks weights to zero │
  │                       │                           │ (High Bias, Low Variance).               │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ Polynomial Features   │ degree (d)                │ Higher degree ──▶ More flexible manifold │
  │                       │                           │ (Low Bias, High Variance).               │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ Gaussian Naive Bayes  │ var_smoothing             │ Adds variance floor to prevent zero-div; │
  │                       │                           │ usually left at default (1e-9).          │
  ├───────────────────────┼───────────────────────────┼──────────────────────────────────────────┤
  │ Gradient Descent      │ learning_rate (η)         │ Too small ──▶ Stalls in slow crawl;      │
  │                       │ max_iter / n_iter         │ Too large (η ≥ 2/λ_max) ──▶ Diverges!    │
  └───────────────────────┴───────────────────────────┴──────────────────────────────────────────┘
  ```
  
  Slide 3 provides a rule of thumb for applied ML engineering:
  $$\mathbf{\text{"Tune the few knobs that set flexibility. Leave the rest at their defaults."}}$$
  
  Trying to tune non-capacity hyperparameters (such as tolerance `tol`, solver algorithms like `'lbfgs'` vs. `'saga'`, or numerical smoothing floors) wastes computational budgets without improving model generalization.
  
  ---
## 3. Dissecting the Parameter vs. Hyperparameter Quizzes (Slides 4–6)
  
  Slides 4, 5, and 6 test the distinction between parameters and hyperparameters through diagnostic questions:
  
  ```
  ┌─────────────────────────────────────────────────────────────┬──────────────────┬─────────────┐
  │ Diagnostic Item (Slides 4–5)                                │ Classification   │ Mechanism   │
  ├─────────────────────────────────────────────────────────────┼──────────────────┼─────────────┤
  │ Q1: The coefficients of a fitted linear regression          │ PARAMETER        │ Learned     │
  │ Q2: k in k-NN                                               │ HYPERPARAMETER   │ Pre-set     │
  │ Q3: alpha (λ) in Lasso                                      │ HYPERPARAMETER   │ Pre-set     │
  │ Q4: The per-class feature means in Gaussian Naive Bayes     │ PARAMETER        │ Learned     │
  │ Q5: The learning rate of gradient descent                   │ HYPERPARAMETER   │ Pre-set     │
  │ Q6: Step size in iterative weight updates                   │ HYPERPARAMETER   │ Pre-set     │
  │ Q7: Why not choose polynomial degree by training error?     │ B (Highest deg)  │ Overfitting │
  └─────────────────────────────────────────────────────────────┴──────────────────┴─────────────┘
  ```
  
  ---
### In-Depth Justifications:
  
  * **Q1 (Linear Regression Coefficients $\implies$ Parameter):**
  * Derived from data via the Normal Equations: $\hat{w} = (X^T X)^{-1} X^T y$. The user does not specify $w$; the data determines its values through least-squares optimization.
  * **Q2 ($k$ in $k$-NN $\implies$ Hyperparameter):**
  * Dictates the neighborhood size. Set by the user before inference begins; cannot be learned by fitting because $k=1$ always achieves zero training error.
  * **Q3 ($\alpha$ in Lasso $\implies$ Hyperparameter):**
  * Controls the strength of the $L_1$ penalty $\|w\|_1$. Must be tuned via cross-validation; training loss minimization would trivially set $\alpha = 0$.
  * **Q4 (Feature Means $\mu_{jc}$ in Gaussian Naive Bayes $\implies$ Parameter):**
  * Computed directly from the training data via maximum likelihood estimation:
    $$\hat{\mu}_{jc} = \frac{1}{N_c} \sum_{i \in \text{Class } c} x_{ij}$$
    These values are learned parameters stored after `.fit()`.
  * **Q5 & Q6 (Learning Rate $\eta$ in Gradient Descent $\implies$ Hyperparameter):**
  * While the weights change at every iteration, the learning rate $\eta$ is an algorithmic step-size multiplier configured prior to training.
  * **Q7 (Why Not Choose Polynomial Degree by Training Error?):**
  * Polynomial models are **nested**: $\mathcal{H}_1 \subset \mathcal{H}_2 \subset \dots \subset \mathcal{H}_d$. 
  * A degree-15 polynomial has all the expressive capacity of a degree-2 polynomial plus 13 additional degrees of freedom. Training error is monotonically non-increasing in degree, so evaluating on the training set will **always pick the highest degree available**, leading to extreme overfitting.
  
  ---
### The Scikit-Learn Systems Design Invariant (Slide 6)
  Slide 6 highlights the software architecture convention used across `scikit-learn`:
  
  $$\mathbf{\text{"In scikit-learn, learned values end in an underscore: } \texttt{coef\_}, \; \texttt{intercept\_}, \; \texttt{theta\_}, \; \texttt{var\_}\text{."}}$$
  
  ```python
  # Before calling .fit(): ONLY hyperparameters exist
  model = LinearRegression(fit_intercept=True)
  hasattr(model, "coef_")  # Returns False!
  
  # After calling .fit(X_train, y_train): LEARNED PARAMETERS POPULATE
  model.fit(X_train, y_train)
  hasattr(model, "coef_")  # Returns True!
  print(model.coef_)  # Trailing underscore denotes a learned parameter θ
  print(model.intercept_)  # Trailing underscore denotes a learned parameter θ
  ```
  
  Any attribute ending with an underscore (`estimator.attribute_`) was computed from the training data during `.fit()`. Attributes passed as constructor arguments (`estimator(param=val)`) are user-specified hyperparameters.
  
  ---
## 4. The Tripartite Data Partition (Slide 7)
  
  Slide 7 outlines the data partitioning scheme required when models must be tuned *and* evaluated honestly:
  
  $$\mathbf{\text{"Three Sets: Each job gets its own data: fit the parameters, choose the hyperparameters, report the result."}}$$
  
  ```
                       The Tripartite Data Split (Slide 7)
    ┌──────────────────────────────┬──────────────────┬──────────────────┐
    │     Training Set (60%)       │ Validation (20%) │    Test (20%)    │
    ├──────────────────────────────┼──────────────────┼──────────────────┤
    │ Fit the parameters           │ Choose the       │ Report ONCE,     │
    │ (weights w, decision splits) │ hyperparameters  │ at the very end. │
    │                              │ (k, λ, degree)   │                  │
    └──────────────────────────────┴──────────────────┴──────────────────┘
  ```
  
  Slide 7 illustrates the alternative production standard using cross-validation:
  
  $$\mathbf{\text{"k-fold CV on the training part (L18): every row takes a turn as validation. Test: still untouched."}}$$
  
  ```
               k-Fold Cross-Validation on Training Portion (Slide 7)
    Fold 1: [ VAL ] [ TRAIN ] [ TRAIN ] [ TRAIN ] [ TRAIN ] ──▶ Score 1  │
    Fold 2: [ TRAIN ] [ VAL ] [ TRAIN ] [ TRAIN ] [ TRAIN ] ──▶ Score 2  │ Held-Out Test Set
    Fold 3: [ TRAIN ] [ TRAIN ] [ VAL ] [ TRAIN ] [ TRAIN ] ──▶ Score 3  │ (20% Vaulted)
    Fold 4: [ TRAIN ] [ TRAIN ] [ TRAIN ] [ VAL ] [ TRAIN ] ──▶ Score 4  │ NEVER TOUCHED
    Fold 5: [ TRAIN ] [ TRAIN ] [ TRAIN ] [ TRAIN ] [ VAL ] ──▶ Score 5  │ DURING TUNING!
  ```
  
  Slide 7 concludes with the core rule of model evaluation:
  $$\mathbf{\text{"Choose on the test set and it becomes a validation set. Then nothing is left for an honest final number."}}$$
  
  ---
### The Operational Separation of Roles:
  1. **The Training Partition:**
   * Used strictly by the numerical optimizer to learn parameter weights $\theta$ (solving $\min_\theta \mathcal{L}_{\text{train}}(\theta)$).
  2. **The Validation Partition (or $k$-Fold CV on Training):**
   * Used strictly by the engineer or search wrapper (`GridSearchCV`) to compare different models and select optimal hyperparameters $\lambda^*$.
  3. **The Test Partition:**
   * Used **strictly once** to evaluate the final, frozen model.
   * **The Degradation Hazard:** If an engineer evaluates multiple hyperparameters on the test set and selects the configuration that achieves the highest test score, the test set has been used to make an optimization decision. It is no longer an independent proxy for future data; it has been converted into a validation set, and the reported score becomes overly optimistic.
  
  ---
## Summary Review Questions for Section 1
  
  1. *In your own words, what is the fundamental mathematical difference between how a parameter (such as $w_1$) is determined versus how a hyperparameter (such as $\alpha$ in Ridge) is determined?*
  2. *In `scikit-learn`, how can you immediately determine whether an attribute of a model object is a learned parameter or a pre-set hyperparameter simply by inspecting its variable name?*
  3. *Why does choosing a hyperparameter (such as polynomial degree $d$) based on training set error always lead to selecting the most complex model available?*
  
  ---