## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why "Holding All Other Features Fixed" Is Mathematically Required:**
  * In multiple linear regression, the parameter weight is defined as a **partial derivative**:
   $$w_j = \frac{\partial \, \mathbb{E}[Y \mid X]}{\partial x_j}$$
  * By definition of a partial derivative, all other covariates ($x_k$ for $k \neq j$) are treated as mathematical constants. 
  * Dropping the conditional clause mistakenly implies a total derivative ($\frac{dY}{dx_j}$), falsely asserting that $x_j$ can change in the real world without any correlated co-features changing alongside it.
  2. **Dimensional Units and Interpretation of $w_1 = -0.04$:**
  * **Units:** $\frac{\text{Days of Recovery}}{\text{Milligrams of Dosage}}$ ($\text{days}/\text{mg}$).
  * **Physical Interpretation:** Comparing two patients who share the **exact same age, health metrics, and baseline covariates**, a patient administered one additional milligram of the drug is predicted to recover $0.04$ days faster.
  3. **The Invalidity of Sorting Raw Coefficients for Feature Importance:**
  * Raw coefficient magnitudes scale inversely with the measurement units of the input features:
   $$w_j \propto \frac{1}{\text{Scale}(X_j)}$$
  * Measuring a feature in small units (e.g., millimeters) makes $w_j$ tiny, while measuring in large units (e.g., kilometers) makes $w_j$ huge, despite representing the exact same physical relationship. 
  * **Standardization** ($z = \frac{x - \mu}{\sigma}$) converts features to unitless standard deviations, allowing standardized coefficients ($\beta_j^* = w_j \frac{s_{x_j}}{s_y}$) to be directly compared to quantify relative variance contributions.
  
  ---
## 2. The Live Coding Experiment: Breaking a Coefficient (Deck 2 Slide 5)
  
  Deck 2 Slide 5 executes an experiment on the Diabetes benchmark that exposes the fragility of observational regression coefficients:
  
  ```python
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import LinearRegression
  from sklearn.model_selection import train_test_split
  
  # Ingest unscaled data to observe raw clinical parameters
  X, y = load_diabetes(scaled=False, return_X_y=True, as_frame=True)
  Xtr, Xte, ytr, yte = train_test_split(X, y, test_size=0.2, random_state=42)
  
  # Progressively expand feature context
  for cols in [["s1"], ["s1", "s5"], list(X.columns)]:
    m = LinearRegression().fit(Xtr[cols], ytr)
    w_s1 = dict(zip(cols, m.coef_))["s1"]
    r2 = m.score(Xte[cols], yte)
    print(f"{len(cols):2d} features   s1 = {w_s1:+.3f}   R2 = {r2:.3f}")
  ```
  
  ```
                 The Coefficient Breakdown (Deck 2 Slide 5)
  ┌──────────────┬────────────────────────┬─────────────┬────────────────────────┐
  │ Model Scope  │ Features Included      │ Slope on s1 │ Test R² Score          │
  ├──────────────┼────────────────────────┼─────────────┼────────────────────────┤
  │ 1 Feature    │ ['s1']                 │ +0.449      │ R² = 0.056             │
  ├──────────────┼────────────────────────┼─────────────┼────────────────────────┤
  │ 2 Features   │ ['s1', 's5']           │ -0.288      │ R² = 0.376  <-- FLIPS! │
  ├──────────────┼────────────────────────┼─────────────┼────────────────────────┤
  │ 10 Features  │ All 10 Covariates      │ -1.282      │ R² = 0.453  <-- DEEPENS│
  └──────────────┴────────────────────────┴─────────────┴────────────────────────┘
                   Correlation Matrix Fact: Corr(s1, s5) = +0.516
  ```
  
  ---
## 3. "Three Different Questions": Understanding the Sign Flip (Deck 2 Slide 6)
  
  Slide 6 presents a bar chart and scatter plot explaining this phenomenon:
  
  $$\mathbf{\text{"No data changed. No bug. Each number is the correct answer to a different question."}}$$
  
  ```
                       The Visual Collapse of s1 (Slide 6)
    Coefficient on s1 (per mg/dL)                  Collinearity Scatter Plot
       ▲                                            s5 (Log Triglycerides)
  +0.5 │  ███ +0.449                                  ▲
       │  ███                                     6.0 │            .::'
   0.0 ┼──┴───────────┬─────────────┬──────▶          │        .::'  Corr(s1, s5) = +0.52
       │             ███           ███            5.0 │     .::'
  -0.5 │             ███ -0.288    ███                │  .::'
       │                           ███            4.0 │::'
  -1.0 │                           ███                │
       │                           ███ -1.282         └────────────────────────▶ s1
  -1.5 │                           ███                100    150    200    250
          s1 alone      s1 + s5     All 10 features          s1 = Total Cholesterol (mg/dL)
  ```
  
  ---
### Translating the Three Mathematical Questions:
  To understand why the sign flipped from positive ($+0.449$) to negative ($-0.288$), inspect the clinical variables:
  * **`s1`:** Total serum cholesterol in blood ($\text{mg}/\text{dL}$).
  * **`s5`:** Log of serum triglycerides level.
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Question 1: Unconditional Bivariate Regression (s1 alone)              │
  │ "Among all patients, what is the expected difference in disease        │
  │ progression per unit increase in total cholesterol, ignoring all other │
  │ blood biomarkers?"                                                     │
  │                                                                        │
  │ • Answer: +0.449.                                                      │
  │ • Clinical Reality: In the general population, patients with high      │
  │   total cholesterol also have elevated triglycerides and LDL. The      │
  │   coarse, unconditioned association with diabetes progression is       │
  │   positive.                                                            │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Question 2: Bivariate Multiple Regression (s1 and s5)                  │
  │ "Comparing two patients with the EXACT SAME triglyceride level (s5),   │
  │ what is the expected change in progression per unit increase in total  │
  │ cholesterol (s1)?"                                                     │
  │                                                                        │
  │ • Answer: -0.288 (THE SIGN FLIPS!).                                    │
  │ • Clinical Reality: Total cholesterol is the sum of fractions:         │
  │   Total = LDL ("bad") + HDL ("good") + VLDL (Triglycerides).           │
  │   If you hold triglycerides (s5) constant and increase total           │
  │   cholesterol (s1), the increase must come from protective HDL!        │
  │   Therefore, higher s1 at a fixed s5 indicates better health!          │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Question 3: Full Observational System (All 10 Features)                │
  │ "What is the expected change in progression per unit increase in s1,   │
  │ holding ALL OTHER nine clinical covariates fixed?"                     │
  │                                                                        │
  │ • Answer: -1.282. Conditioned on all other blood lipids and baseline   │
  │   vitals, the protective residual effect of s1 becomes even stronger. │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **Key Takeaway:** The coefficient does not belong to feature $s_1$ alone. A coefficient is a mathematical property of the **joint projection of $y$ onto $s_1$ orthogonal to all other features in that specific model**. Change the feature context, and the slope can reverse.
  
  ---
## 4. Formal Mathematical Derivation: Omitted Variable Bias (OVB)
  
  Why did the simple regression slope ($+0.449$) disagree with the multiple regression slope ($-0.288$)? We can prove this relationship analytically using the **Omitted Variable Bias Formula**.
### The Setup:
  Let the true data-generating relationship follow the **"Long Model"** containing two correlated features:
  
  $$y = \beta_1 x_1 + \beta_2 x_2 + \epsilon$$
  
  Suppose an engineer omits $x_2$ and fits the **"Short Model"** via univariate OLS:
  
  $$y = \tilde{\beta}_1 x_1 + u$$
  
  ---
### The Derivation:
  The OLS estimator for the univariate short regression is:
  
  $$\tilde{\beta}_1 = \frac{\widehat{\text{Cov}}(x_1, y)}{\widehat{\text{Var}}(x_1)}$$
  
  Substitute the true long model ($y = \beta_1 x_1 + \beta_2 x_2 + \epsilon$) into the covariance formula:
  
  $$\tilde{\beta}_1 = \frac{\widehat{\text{Cov}}(x_1, \; \beta_1 x_1 + \beta_2 x_2 + \epsilon)}{\widehat{\text{Var}}(x_1)}$$
  
  Applying the bilinearity of the covariance operator:
  
  $$\tilde{\beta}_1 = \beta_1 \frac{\widehat{\text{Cov}}(x_1, x_1)}{\widehat{\text{Var}}(x_1)} + \beta_2 \frac{\widehat{\text{Cov}}(x_1, x_2)}{\widehat{\text{Var}}(x_1)} + \frac{\widehat{\text{Cov}}(x_1, \epsilon)}{\widehat{\text{Var}}(x_1)}$$
  
  Since $\widehat{\text{Cov}}(x_1, x_1) = \widehat{\text{Var}}(x_1)$ and the exogenous noise $\epsilon$ is uncorrelated with $x_1$ ($\text{Cov}(x_1, \epsilon) = 0$):
  
  $$\mathbf{\tilde{\beta}_1 = \beta_1 + \beta_2 \cdot \gamma_{21}}$$
  
  where $\gamma_{21} = \frac{\widehat{\text{Cov}}(x_1, x_2)}{\widehat{\text{Var}}(x_1)}$ is the slope coefficient obtained by regressing the omitted variable $x_2$ on the included variable $x_1$.
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Omitted Variable Bias Theorem:                                     │
  │                                                                        │
  │        β̃₁ (Short Estimate) = β₁ (True Partial) + β₂ · γ₂₁ (Bias Term)  │
  │                                                                        │
  │ • β₁  : The direct effect of x₁ holding x₂ constant.                   │
  │ • β₂  : The direct effect of the omitted variable x₂ on y.             │
  │ • γ₂₁ : The auxiliary relationship between the omitted x₂ and x₁.      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Explaining the Diabetes Sign Flip via OVB:
  Now, substitute the empirical values from Deck 2 Slide 5 into the OVB equation:
  1. $\beta_1$ (True partial slope of $s_1$ holding $s_5$ fixed) is **negative** ($\beta_1 = -0.288$).
  2. $\beta_2$ (Direct effect of triglycerides $s_5$) is **strongly positive** ($\beta_2 > 0$).
  3. $\gamma_{21}$ (Correlation between $s_1$ and $s_5$) is **strongly positive** ($\text{Corr} = +0.516 > 0$).
  
  $$\tilde{\beta}_1 = \underbrace{\beta_1}_{(-0.288)} + \underbrace{\beta_2 \cdot \gamma_{21}}_{(\text{Large Positive Term})}$$
  
  The bias term $\beta_2 \cdot \gamma_{21}$ is large and positive, completely overpowering the negative partial slope $\beta_1$ and forcing the short regression slope to be positive:
  
  $$\tilde{\beta}_1 = +0.449$$
  
  Omitting $s_5$ does not merely weaken the estimate; it **inverts the algebraic sign**.
  
  ---
## 5. The Frisch-Waugh-Lovell (FWL) Theorem (The Graduate Fill-In)
  
  What is multiple regression actually computing when it fits a parameter $w_j$? 
  
  The **Frisch-Waugh-Lovell Theorem (1933, 1963)** proves that multiple regression is equivalent to a series of orthogonal univariate projections:
  
  ```
                      The Frisch-Waugh-Lovell Algorithm
  ┌────────────────────────────────────────────────────────────────────────┐
  │ To isolate the multiple regression coefficient w₁ on feature x₁:       │
  │                                                                        │
  │ Step 1: Regress x₁ on all other covariates X_(-1):                     │
  │         x₁ = X_(-1) γ + x̃₁                                             │
  │         Extract residual vector x̃₁ = (I - H_(-1)) x₁                   │
  │         (This is the variation in x₁ UNEXPLAINED by other features!)   │
  │                                                                        │
  │ Step 2: Regress target y on all other covariates X_(-1):               │
  │         y = X_(-1) δ + ỹ                                               │
  │         Extract residual vector ỹ = (I - H_(-1)) y                     │
  │         (This is the variation in y UNEXPLAINED by other features!)    │
  │                                                                        │
  │ Step 3: Run a simple univariate regression of ỹ on x̃₁:                 │
  │                                                                        │
  │                         w₁ = ⟨x̃₁, ỹ⟩ / ||x̃₁||₂²                        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The Geometric Insight:
  * The multiple regression coefficient $w_j$ does **not** evaluate the raw feature $x_j$ against $y$.
  * It evaluates the **residualized feature** $\tilde{x}_j$ (what remains of $x_j$ after projecting out all linear information contained in the other features) against the **residualized target** $\tilde{y}$.
  * **Why Collinearity Destabilizes Coefficients:** If $x_1$ and $x_2$ are highly collinear, the residual variation $\tilde{x}_1$ approaches zero ($\|\tilde{x}_1\|_2^2 \to 0$). The denominator in the FWL equation collapses, causing the parameter variance to explode.
  
  ---
## 6. Variance Inflation Factor (VIF) & Multicollinearity
  
  The variance of a fitted regression coefficient $\hat{w}_j$ can be decomposed analytically as:
  
  $$\text{Var}(\hat{w}_j) = \frac{\sigma^2}{(n-1) s_j^2} \cdot \mathbf{\text{VIF}_j}$$
  
  where $\sigma^2$ is the error variance, $s_j^2$ is the sample variance of feature $j$, and $\text{VIF}_j$ is the **Variance Inflation Factor**:
  
  $$\mathbf{\text{VIF}_j = \frac{1}{1 - R_j^2}}$$
  
  where $R_j^2$ is the coefficient of determination obtained from regressing feature $x_j$ on all other remaining features $X_{-j}$.
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ VIF Diagnostics:                                                       │
  │                                                                        │
  │ • When R_j² = 0.0 (Feature j is completely orthogonal to others):      │
  │   VIF_j = 1 / (1 - 0) = 1.0 (Zero variance inflation).                 │
  │                                                                        │
  │ • When R_j² = 0.90 (Feature j is highly collinear with others):        │
  │   VIF_j = 1 / (1 - 0.90) = 10.0 (Variance is inflated by 10×!).       │
  │                                                                        │
  │ • Rule of Thumb: VIF > 5.0 indicates severe multicollinearity;         │
  │   VIF > 10.0 proves parameter estimates are numerically unstable.     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 7. In-Class Activity: Interpretation & Causal Rewriting Drill (Deck 1 Slide 10)
  
  Deck 1 Slide 10 presents an interpretation drill on a fitted housing dataset:
  
  ```python
  # Fitted Model:
  Price = 50_000 + 120*(sqft) - 8_000*(bedrooms) + 15_000*(bathrooms) - 500*(age)
  ```
  
  Slide 10 tasks the class with three exercises:
### Drill 1: Interpret the `-8,000` Coefficient on `bedrooms`
  * **Incorrect Interpretation:** *"Adding a bedroom lowers the price of a house by $8,000."* (Asserts false causation and ignores the design matrix).
  * **Correct Statement:** *"Comparing two houses with the **exact same square footage, number of bathrooms, and age**, a house with one additional bedroom is predicted to sell for **$8,000 less**."*
  * **Why the Slope Is Negative:** If you hold square footage constant and add a bedroom, you are subdividing the same interior space into smaller rooms, decreasing open living area.
  
  ---
### Drill 2: Flagging and Rewriting Causal Claims (Slide 10)
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Flawed Causal Statement 1:                                             │
  │ "Installing a bathroom will increase your home's equity by $15,000."   │
  │                                                                        │
  │ Correct Observational Revision:                                        │
  │ "In this dataset, homes with an additional bathroom—holding square     │
  │ footage, bedrooms, and age constant—are associated with a $15,000      │
  │ higher predicted market price."                                        │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Flawed Causal Statement 2:                                             │
  │ "Aging your house by 10 years destroys $5,000 in property value."      │
  │                                                                        │
  │ Correct Observational Revision:                                        │
  │ "Comparing homes with identical square footage and room counts, homes  │
  │ that are 10 years older have a predicted sale price that is $5,000     │
  │ lower on average."                                                     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Drill 3: Predict What Happens When Adding a Strongly Correlated Feature
  Slide 10 asks: *"Finally: I add a strongly correlated feature and refit. Predict what happens to the coefficients before I run it."*
  * **The Action:** Add `finished_living_area` (which has a correlation of $\text{Corr} = +0.96$ with `sqft`).
  * **The Prediction:**
  1. **Standard Errors Explode:** The confidence intervals on both `sqft` and `finished_living_area` widen due to high VIF ($\text{VIF} \approx 12.8$).
  2. **Sign Flipping or Magnitude Collapse:** The original coefficient on `sqft` ($+120$) drops toward zero or flips negative, while `finished_living_area` absorbs the shared variance.
  3. **Prediction Stability:** Total model predictions ($\hat{y}$) and test $R^2$ remain largely unchanged, but individual parameter weights become uninterpretable.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Using the Omitted Variable Bias formula $\tilde{\beta}_1 = \beta_1 + \beta_2 \gamma_{21}$, prove that omitting a confounder $x_2$ will cause a simple regression slope to overestimate the true effect ($\tilde{\beta}_1 > \beta_1$) if both $\beta_2 > 0$ and $\text{Cov}(x_1, x_2) > 0$.*
  2. *In the Diabetes live-coding experiment, why did the slope on total cholesterol ($s_1$) flip from $+0.449$ to $-0.288$ when triglycerides ($s_5$) was introduced into the model?*
  3. *According to the Frisch-Waugh-Lovell Theorem, what residualized vectors are being evaluated when calculating the multiple regression coefficient $w_j$?*
  
  ---