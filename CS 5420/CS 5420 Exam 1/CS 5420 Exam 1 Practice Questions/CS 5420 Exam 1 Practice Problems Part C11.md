# Part C11 — Interpreting Coefficients Practice Problems
#### Problem 1 (Automotive Valuation & Scale Multipliers)
  A fitted multiple linear regression model predicts the market price of used passenger cars:
  $$\widehat{\text{price}} \; (\$1{,}000\text{s}) = 28.0 - 1.2 \cdot \text{age} - 0.15 \cdot \text{mileage\_1k} + 4.5 \cdot \text{is\_leather}$$
  where `age` is in years, `mileage_1k` is odometer miles divided by $1{,}000$, and `is_leather` is a binary indicator ($1 = \text{leather seats}, 0 = \text{cloth seats}$).
  * **(a)** Predict the selling price of a $5$-year-old car with $60{,}000$ miles and leather seats.
  * **(b)** State the exact observational interpretation of the $-0.15$ coefficient in the units of the data.
  * **(c)** A dealership manager states: *"If we spend $\$800$ to install aftermarket leather seats in an old base-model car, it will raise its market selling price by $\$4{,}500$."* Is this conclusion scientifically legitimate? Explain why or why not.
  
  ---
#### Problem 2 (Interaction Terms & Context-Dependent Slopes)
  A real estate model incorporates an interaction term between interior square footage and location:
  $$\widehat{\text{price}} \; (\$1{,}000\text{s}) = 80.0 + 0.12 \cdot \text{sqft} + 150.0 \cdot \text{is\_downtown} + 0.18 \cdot (\text{sqft} \times \text{is\_downtown})$$
  where `is_downtown` is a binary indicator ($1 = \text{Downtown}, 0 = \text{Suburbs}$).
  * **(a)** Predict the price of a $2{,}000\text{ sq ft}$ home in the suburbs vs. a $2{,}000\text{ sq ft}$ home downtown.
  * **(b)** Derive the marginal rate of price increase per additional square foot ($\frac{\partial \widehat{\text{price}}}{\partial \text{sqft}}$) for suburban homes.
  * **(c)** Derive the marginal rate of price increase per additional square foot for downtown homes. Why was an additive model without an interaction term insufficient here?
  
  ---
#### Problem 3 (Polynomial Trajectories & Peak Experience Returns)
  An econometric wage study fits a quadratic experience model across $10{,}000$ corporate professionals:
  $$\widehat{\text{salary}} \; (\$1{,}000\text{s}) = 45.0 + 3.2 \cdot \text{tenure} - 0.08 \cdot (\text{tenure}^2) + 12.0 \cdot \text{has\_masters}$$
  where `tenure` is years of corporate tenure, and `has_masters` is a binary indicator ($1 = \text{Master's Degree}, 0 = \text{Bachelor's}$).
  * **(a)** Predict the annual salary for a professional with a Master's degree and $10$ years of tenure.
  * **(b)** Calculate the marginal return to an additional year of tenure ($\frac{\partial \widehat{\text{salary}}}{\partial \text{tenure}}$) at $\text{tenure} = 5\text{ years}$ vs. at $\text{tenure} = 25\text{ years}$.
  * **(c)** At how many years of tenure does predicted corporate salary reach its peak?
  
  ---
#### Problem 4 (Healthcare Expenditure & Reverse Causation)
  An insurer fits a multiple linear regression model predicting annual medical claim costs ($Y$ in dollars):
  $$\widehat{\text{annual\_claims}} \; (\$) = 1{,}200 + 850 \cdot \text{doctor\_visits} + 40 \cdot \text{bmi} - 250 \cdot \text{is\_active}$$
  * **(a)** State the exact observational interpretation of the $+850$ coefficient.
  * **(b)** A health policy analyst recommends: *"Visiting the doctor causes patients to incur $\$850$ more in claims. We should restrict allowable annual doctor visits to reduce overall healthcare expenditures."* Explain the causal fallacy undermining this proposal.
  
  ---
#### Problem 5 (Semi-Logarithmic Wage Models / Elasticity)
  A labor economics model evaluates the natural logarithm of hourly wage:
  $$\ln(\widehat{\text{wage}}) = 1.60 + 0.08 \cdot \text{education} + 0.03 \cdot \text{experience}$$
  where `education` and `experience` are measured in years.
  * **(a)** Predict the actual hourly wage in dollars ($\text{wage} = e^{\ln(\text{wage})}$) for an employee with $16$ years of education and $10$ years of experience.
  * **(b)** In a semi-log regression $\ln(y) = \beta_0 + \beta_1 x_1$, how is the coefficient $\beta_1$ interpreted mathematically in terms of percentage change? What does $+0.08$ physically claim?
  * **(c)** Why is it mathematically incorrect to say: *"Each year of education increases hourly wages by $\$0.08$"?*
  
  ---
#### Problem 6 (Standardized vs. Raw Coefficients: The Scale Illusion)
  Two models are fitted on the exact same dataset predicting concrete compressive strength ($Y$ in MPa):
  * **Raw Model:** $\hat{y} = 15.0 + 0.08 \cdot \text{cement\_kg} + 120.0 \cdot \text{superplasticizer\_m3}$
  * **Standardized Model:** $\hat{z}_y = 0.58 \cdot z_{\text{cement}} + 0.18 \cdot z_{\text{superplasticizer}}$
  * **(a)** An engineer inspecting the raw model claims: *"Superplasticizer is $1{,}500\times$ more important than cement because $120.0 / 0.08 = 1{,}500$."* Why is this claim a statistical illusion?
  * **(b)** Using the standardized model, explain which feature actually accounts for more variance in concrete strength.
  
  ---
#### Problem 7 (Collinear Feature Addition & Sign Inversion)
  In a clinical study tracking diabetes disease progression ($Y$):
  * **Model 1 (Simple Regression):** $\widehat{\text{progression}} = 150.0 + 0.45 \cdot \text{total\_cholesterol}$
  * **Model 2 (Multiple Regression):** $\widehat{\text{progression}} = 150.0 - 0.29 \cdot \text{total\_cholesterol} + 1.15 \cdot \text{triglycerides}$
  * **(a)** Explain the clinical and mathematical reason why the coefficient on total cholesterol flipped from positive ($+0.45$) to negative ($-0.29$).
  * **(b)** What theorem/formula proves that the simple regression slope in Model 1 absorbed bias from the omitted triglycerides variable?
  * **(c)** Does the negative coefficient in Model 2 prove that consuming more cholesterol cures diabetes? Explain.
  
  ---
#### Problem 8 (Digital Marketing & Seasonal Confounding)
  An e-commerce company fits an observational advertising model across $52$ weeks:
  $$\widehat{\text{revenue}} \; (\$1{,}000\text{s}) = 100.0 + 4.2 \cdot \text{ad\_spend\_1k} + 0.8 \cdot \text{email\_subscribers\_1k}$$
  where `ad_spend_1k` is digital advertising expenditure in thousands of dollars.
  * **(a)** Predict weekly revenue when ad spend is $\$10{,}000$ and email subscribers are $50{,}000$.
  * **(b)** The marketing director argues: *"Each $\$1{,}000$ in ad spend generates $\$4{,}200$ in revenue. We should take out a $\$1{,}000{,}000$ bank loan to run ads because it is guaranteed to return $\$4{,}200{,}000$ in revenue."* Identify the unobserved confounder $Z$ that invalidates this causal projection.
  
  ---
#### Problem 9 (One-Hot Dummy Encodings & Reference Baselines)
  An educational research team studies undergraduate GPA scores ($Y \in [0.0, 4.0]$) across academic divisions:
  $$\widehat{\text{GPA}} = 2.80 + 0.40 \cdot \text{major\_STEM} + 0.25 \cdot \text{major\_Business} - 0.15 \cdot \text{major\_Arts}$$
  where `major_Humanities` was dropped as the baseline reference category.
  * **(a)** What is the predicted GPA for a Humanities student?
  * **(b)** What is the predicted GPA for a STEM student?
  * **(c)** State the exact comparative interpretation of the $+0.40$ coefficient on `major_STEM`.
  * **(d)** What mathematical error would occur if the researcher also added a column for `major_Humanities` to this regression?
  
  ---
#### Problem 10 (Rewriting Flawed Causal Assertions)
  Rewrite the following three conclusions from student research drafts so that they adhere to rigorous scientific and observational reporting standards:
  * **Statement A:** *"Mandating that remote employees return to the office will cause company-wide productivity to rise by $14\%$."*
  * **Statement B:** *"Our regression proves that upgrading cloud server RAM from 16GB to 64GB cuts system crash frequency in half."*
  * **Statement C:** *"Lowering software subscription pricing by $\$10$ forces customer lifetime retention to expand by 8 months."*
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C11
### Problem 1 Solution
  * **(a) Compute Predicted Selling Price:**
  Extract scaled features:
  * $\text{age} = 5$
  * $\text{mileage\_1k} = \frac{60{,}000}{1{,}000} = 60$
  * $\text{is\_leather} = 1$
  $$\widehat{\text{price}} = 28.0 - 1.2(5) - 0.15(60) + 4.5(1)$$
  $$\widehat{\text{price}} = 28.0 - 6.0 - 9.0 + 4.5 = \mathbf{17.500 \quad (\$17{,}500)}$$
  * **(b) Observational Interpretation of $-0.15$:**
  *"Holding vehicle age and seat material fixed, each additional $1{,}000$ miles on the odometer is associated with a predicted **$\$150$ decrease** in market price on average."*
  * **(c) Evaluation of Causal Aftermarket Claim:**
  * **The conclusion is false.**
  * The $+4.5$ ($\$4{,}500$) coefficient reflects an observational correlation in historical market data.
  * In production datasets, cars equipped with factory leather seats are overwhelmingly higher-tier luxury trim models that also include upgraded V6/V8 engines, premium sound systems, advanced safety sensors, and sunroofs ($Z$).
  * The feature `is_leather` absorbs proxy variance from these unmeasured luxury upgrades. Spending $\$800$ to install aftermarket leather in an old base-model car will not transform it into a luxury trim, and will not cause its value to appreciate by $\$4{,}500$. `[L12, Review C11]`
  
  ---
### Problem 2 Solution
  * **(a) Point Predictions for Suburban vs. Downtown Homes:**
  1. *Suburban Home (`is_downtown = 0`):*
     $$\widehat{\text{price}} = 80.0 + 0.12(2{,}000) + 150.0(0) + 0.18(2{,}000 \times 0)$$
     $$\widehat{\text{price}} = 80.0 + 240.0 = \mathbf{320.000 \quad (\$320{,}000)}$$
  2. *Downtown Home (`is_downtown = 1`):*
     $$\widehat{\text{price}} = 80.0 + 0.12(2{,}000) + 150.0(1) + 0.18(2{,}000 \times 1)$$
     $$\widehat{\text{price}} = 80.0 + 240.0 + 150.0 + 360.0 = \mathbf{830.000 \quad (\$830{,}000)}$$
  * **(b) Marginal Rate in the Suburbs (`is_downtown = 0`):**
  $$\frac{\partial \widehat{\text{price}}}{\partial \text{sqft}} = 0.12 + 0.18(\text{is\_downtown}) = 0.12 + 0.18(0) = \mathbf{0.120 \quad (\$120/\text{sq ft})}$$
  * **(c) Marginal Rate Downtown (`is_downtown = 1`):**
  $$\frac{\partial \widehat{\text{price}}}{\partial \text{sqft}} = 0.12 + 0.18(1) = \mathbf{0.300 \quad (\$300/\text{sq ft})}$$
  *An additive model without an interaction term would force the slope per square foot to be identical across both locations. The multiplicative interaction term allows the marginal rate of price expansion to depend dynamically on geographic context.* `[L09, L12]`
  
  ---
### Problem 3 Solution
  * **(a) Point Prediction for Corporate Salary:**
  Substitute $\text{tenure} = 10, \; \text{has\_masters} = 1$:
  $$\widehat{\text{salary}} = 45.0 + 3.2(10) - 0.08(10^2) + 12.0(1)$$
  $$\widehat{\text{salary}} = 45.0 + 32.0 - 0.08(100) + 12.0 = 45.0 + 32.0 - 8.0 + 12.0 = \mathbf{81.000 \quad (\$81{,}000)}$$
  * **(b) Marginal Returns to Tenure ($\frac{\partial \widehat{\text{salary}}}{\partial \text{tenure}}$):**
  $$\frac{\partial \widehat{\text{salary}}}{\partial \text{tenure}} = 3.2 - 2(0.08)\cdot\text{tenure} = \mathbf{3.2 - 0.16 \cdot \text{tenure}}$$
  * At $\text{tenure} = 5\text{ years}$:
    $$\frac{\partial \widehat{\text{salary}}}{\partial \text{tenure}} = 3.2 - 0.16(5) = 3.2 - 0.80 = \mathbf{+\$2.400\text{k} \quad (+\$2{,}400/\text{year})}$$
  * At $\text{tenure} = 25\text{ years}$:
    $$\frac{\partial \widehat{\text{salary}}}{\partial \text{tenure}} = 3.2 - 0.16(25) = 3.2 - 4.00 = \mathbf{-\$0.800\text{k} \quad (-\$800/\text{year})}$$
  * **(c) Peak Salary Tenure:**
  Set the partial derivative to zero:
  $$3.2 - 0.16 \cdot \text{tenure}^* = 0 \implies \text{tenure}^* = \frac{3.2}{0.16} = \mathbf{20.000 \text{ years}}$$ `[L10, L12]`
  
  ---
### Problem 4 Solution
  * **(a) Observational Interpretation of $+850$:**
  *"Holding BMI and physical activity status constant, each additional annual doctor visit is associated with a predicted **$\$850$ increase** in annual medical claims on average."*
  * **(b) Deconstructing the Causal Fallacy:**
  * **Reverse Causation ($Y \to X$):** Patients visit the doctor *because* they are experiencing acute or chronic illness ($Y$). The underlying illness causes both the doctor visit and subsequent prescription and surgical expenditures.
  * **Omitted Confounders ($Z$):** Underlying health conditions (e.g., heart disease, cancer, diabetes) are unmeasured common causes driving both doctor visits and claims ($X \leftarrow Z \to Y$).
  * Mandating fewer doctor visits will not prevent disease; it will delay diagnoses, leading to higher emergency room costs. `[L12]`
  
  ---
### Problem 5 Solution
  * **(a) Wage Prediction in Dollars:**
  $$\ln(\widehat{\text{wage}}) = 1.60 + 0.08(16) + 0.03(10) = 1.60 + 1.28 + 0.30 = \mathbf{3.180}$$
  $$\widehat{\text{wage}} = e^{3.180} \approx \mathbf{\$24.047 \text{ per hour}}$$
  * **(b) Percentage Interpretation of $\beta_1$ in Semi-Log Models:**
  In a semi-log model, differentiating yields $\frac{d(\ln y)}{dx} = \frac{1}{y}\frac{dy}{dx} \approx \frac{\%\Delta y}{\Delta x}$.  
  Multiplying the coefficient by $100$ gives the percentage change:
  $$\% \Delta y \approx 100 \times \beta_1$$
  *Meaning:* Each additional year of education is associated with an approximate **$8.0\%$ increase** in hourly wages, holding experience constant.
  * **(c) Why Flat Dollar Additions Are Incorrect:**
  The dependent variable is the logarithm of wages, not raw dollars. A $+0.08$ shift in log space represents a **multiplicative geometric increase** ($e^{0.08} \approx 1.0833 \implies +8.33\%$), meaning the dollar value scales with base wage (an $8\%$ raise is worth much more to a high earner than a low earner). `[L08, L12]`
  
  ---
### Problem 6 Solution
  * **(a) Why the Raw Model Assertion Is a Statistical Illusion:**
  * Raw regression coefficients depend on the physical measurement scale:
    $$w_j \propto \frac{1}{\text{Scale}(X_j)}$$
  * `superplasticizer_m3` is measured in **cubic meters** (an enormous unit for a chemical additive, typical values $\approx 0.001\text{ m}^3$).
  * `cement_kg` is measured in **kilograms** (typical values $\approx 500\text{ kg}$).
  * A single unit change in superplasticizer ($+1\text{ m}^3$) represents an impossible real-world addition that would overwhelm the concrete mix. The raw coefficient $120.0$ is large solely because its physical unit is huge.
  * **(b) Variance Analysis via Standardized Coefficients:**
  * Standardized coefficients $\beta^*$ evaluate changes per standard deviation ($1\sigma$):
    $$\beta_{\text{cement}}^* = 0.580 \quad \text{vs.} \quad \beta_{\text{superplasticizer}}^* = 0.180$$
  * A $1\sigma$ increase in cement content yields a **$0.58\sigma$ increase** in strength, which is more than **$3.2\times$ larger** than the variance contribution of superplasticizer ($0.18\sigma$). Cement is the dominant driver of compressive strength. `[L08, L12]`
  
  ---
### Problem 7 Solution
  * **(a) Clinical & Mathematical Cause of the Sign Flip:**
  * In Model 1, total cholesterol serves as a proxy for unmeasured harmful blood lipids. In the population, total cholesterol correlates positively with triglycerides ($\text{Corr} > 0$), so the simple regression slope absorbs this positive association ($+0.45$).
  * In Model 2, triglycerides are held constant. Total cholesterol equals $\text{LDL} + \text{Triglycerides} + \text{HDL}$. Holding triglycerides fixed means that an increase in total cholesterol reflects an increase in protective **High-Density Lipoproteins (HDL / "good cholesterol")**. Thus, the true conditional partial slope flips negative ($-0.29$).
  * **(b) Governing Theorem:**
  The **Omitted Variable Bias (OVB) Theorem**:
  $$\tilde{\beta}_1 = \beta_1 + \beta_2 \cdot \frac{\text{Cov}(x_1, x_2)}{\text{Var}(x_1)}$$
  * **(c) Causal Assessment:**
  * **No.** The coefficient represents an observational association, not an interventional clinical law ($P(Y \mid X) \neq P(Y \mid do(X))$).
  * Consuming dietary cholesterol increases both LDL and triglycerides simultaneously in the human body; it is physiologically impossible to intervene on total cholesterol while artificially freezing triglycerides. `[L12, Review C11]`
  
  ---
### Problem 8 Solution
  * **(a) Predict Revenue:**
  $$\widehat{\text{revenue}} = 100.0 + 4.2(10) + 0.8(50) = 100.0 + 42.0 + 40.0 = \mathbf{182.000 \quad (\$182{,}000)}$$
  * **(b) Unobserved Seasonal Confounder ($Z$):**
  * **Holiday Seasonality / Macroeconomic Shopping Cycles ($Z$):**
  * E-commerce retailers spend heavily on digital advertising during peak shopping holidays (e.g., Black Friday, Cyber Monday, Christmas in Q4) when consumer buying intent is naturally surging.
  * Holiday demand causes both high ad spend ($Z \to X$) and high revenue ($Z \to Y$).
  * Taking out a loan to spend $\$1{,}000{,}000$ on ads during a dead shopping period (such as February) will not replicate holiday consumer demand, and will fail to generate the projected $\$4.2\text{M}$ in returns. `[L02, L12]`
  
  ---
### Problem 9 Solution
  * **(a) Predicted GPA for Humanities (Reference Group):**
  For a Humanities student, all three dummy variables equal zero:
  $$\widehat{\text{GPA}} = 2.80 + 0.40(0) + 0.25(0) - 0.15(0) = \mathbf{2.800}$$
  * **(b) Predicted GPA for STEM Student:**
  $$\widehat{\text{GPA}} = 2.80 + 0.40(1) + 0.25(0) - 0.15(0) = \mathbf{3.200}$$
  * **(c) Comparative Interpretation of $+0.40$:**
  *"STEM students are predicted to have a GPA that is **$0.40$ points higher on average than Humanities students** (the reference baseline group)."*
  * **(d) Mathematical Consequence of Adding `major_Humanities`:**
  * **The Dummy Variable Trap:** The four dummy columns sum to the vector of ones:
    $$\mathbf{x}_{\text{STEM}} + \mathbf{x}_{\text{Business}} + \mathbf{x}_{\text{Arts}} + \mathbf{x}_{\text{Humanities}} = \mathbf{1}$$
  * This creates perfect multicollinearity with the model's intercept column ($\mathbf{1}$).
  * The Gram matrix $X^T X$ becomes singular ($\det(X^T X) = 0$) and cannot be inverted, crashing Ordinary Least Squares. `[L08, L10]`
  
  ---
### Problem 10 Solution
  * **Statement A (Revised):**  
  *"In our observational sample, employees working in the office exhibited a $14\%$ higher recorded productivity score on average compared to remote employees."*
  * **Statement B (Revised):**  
  *"Holding CPU cores, storage type, and network workload constant, cloud servers configured with 64GB RAM are associated with a $50\%$ lower observed crash rate than servers with 16GB RAM."*
  * **Statement C (Revised):**  
  *"In historical account data, customers paying $\$10$ less per month in subscription fees are associated with an average account lifetime that is $8$ months longer."* `[L12]`
  
  ---