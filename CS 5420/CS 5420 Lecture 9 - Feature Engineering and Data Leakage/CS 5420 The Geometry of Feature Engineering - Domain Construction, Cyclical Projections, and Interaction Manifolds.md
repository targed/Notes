## 1. The Inductive Limit: Where Features Come From (Slide 2)
  
  Slide 2 establishes the core premise of applied representation engineering:
  
  $$\mathbf{\text{"The Golden Rule: ML models only see the explicit columns you feed them, in the exact form you provide."}}$$
  
  ```
                     The Feature Engineering Flow
  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
  │  Raw Databases,  │     │   Select, Join   │     │ Clean, Transform │     │  Final Invariant │
  │ Telemetry Logs,  │ ──▶ │  & Entity Merge  │ ──▶ │   & Feature Math │ ──▶ │  Design Matrix X │ ──▶ [Model]
  │ Unparsed Strings │     │                  │     │ (Ratios/Cycles)  │     │  (ℝ^(N × d))     │
  └──────────────────┘     └──────────────────┘     └──────────────────┘     └──────────────────┘
  ```
### The Limitations of Tabular Estimators:
  * Unlike deep convolutional or transformer architectures that process raw pixels or continuous token streams, **classical tabular estimators (Linear Models, SVMs, Tree Ensembles) cannot infer latent temporal cycles or relational physical laws automatically**.
  * If an engineer provides raw column values, a linear model is restricted to evaluating independent linear combinations:
  $$f(x) = \sum_{j=1}^d w_j x_j + b$$
  * If the true physical phenomenon involves ratios, rate of change, periodic time boundaries, or multiplicative dependencies, the engineer must explicitly construct those basis expansions in the design matrix.
  
  ---
## 2. Cyclical Feature Encoding: Resolving the Boundary Discontinuity (Slides 3–4)
  
  Slide 3 poses a classic temporal representation problem:
  
  $$\mathbf{\text{"Hour of Day (0–23): Hour 23 and Hour 0 are 1 hour apart in time, but 23 units apart numerically!"}}$$
  
  ```
                     The Linear Encoding Boundary Fracture
       21:00      22:00      23:00                  00:00      01:00      02:00
  ───────●──────────●──────────●──────────────────────●──────────●──────────●───────▶
        21         22         23                      0          1          2
                               │                      │
                               └────── Δ = |23 - 0| ──┘
                                        = 23 UNITS!
  • True Temporal Distance: 1 hour apart
  • Numerical Distance:     23 units apart (A catastrophic boundary jump)
  ```
### The Failure of Linear Encodings:
  * In a linear model predicting traffic congestion, hospital admissions, or energy consumption, assigning an integer $t \in \{0, 1, \dots, 23\}$ forces the optimization algorithm to treat $23:00$ and $00:00$ as the two most distant events in the domain.
  * The model must either:
  1. Fit a steep step discontinuity across midnight, distorting predictions between $23:00$ and $01:00$, or
  2. Over-smooth the transition, destroying predictive accuracy at both ends of the day.
  
  ---
### The Trigonometric Solution: Projection onto the Unit Circle (Slide 4)
  Slide 4 details the canonical mathematical fix: mapping a 1-dimensional periodic scalar into a **2-dimensional coordinate space on the unit circle** using sine and cosine transformations:
  
  $$x_{\sin} = \sin\left(\frac{2\pi \cdot t}{T}\right), \quad x_{\cos} = \cos\left(\frac{2\pi \cdot t}{T}\right)$$
  
  where $t$ is the current time index, and $T$ is the fundamental period of oscillation:
  * Hour of Day: $T = 24$
  * Day of Week: $T = 7$
  * Day of Year: $T = 365.25$
  * Month of Year: $T = 12$
  
  ```
                    Trigonometric Unit Circle Mapping (Slide 4)
                                     x_cos
                                       ▲  Hour 0 (Midnight)
                                       │  [sin = 0.00, cos = 1.00]
                 Hour 23               │               Hour 1
        [-0.26, 0.97]  ●───────────────┼───────────────●  [0.26, 0.97]
                        \              │              /
                         \             │             /
                          \            │            /
  x_sin ◀──────────────────\───────────┼───────────/──────────────────▶ x_sin
                            \          │          /
                             \         │         /
                              \        │        /
                               ●───────┼───────●
                                       │  Hour 12 (Noon)
                                       ▼  [sin = 0.00, cos = -1.00]
  ```
  
  ---
### Mathematical Proof of Preserved Metric Continuity:
  Let $v(t) = \left[\sin\left(\frac{2\pi t}{24}\right), \; \cos\left(\frac{2\pi t}{24}\right)\right]^T$ denote the feature vector.
  
  1. **Euclidean Distance Between Hour 23 and Hour 0:**
   $$\Delta_{\sin} = \sin\left(\frac{46\pi}{24}\right) - \sin(0) = -0.2588 - 0 = -0.2588$$
   $$\Delta_{\cos} = \cos\left(\frac{46\pi}{24}\right) - \cos(0) = 0.9659 - 1.0 = -0.0341$$
   $$\|v(23) - v(0)\|_2 = \sqrt{(-0.2588)^2 + (-0.0341)^2} = \sqrt{0.0670 + 0.00116} \approx \mathbf{0.2611}$$
  2. **Euclidean Distance Between Hour 0 and Hour 1:**
   $$\Delta_{\sin} = \sin\left(\frac{2\pi}{24}\right) - \sin(0) = 0.2588 - 0 = 0.2588$$
   $$\Delta_{\cos} = \cos\left(\frac{2\pi}{24}\right) - \cos(0) = 0.9659 - 1.0 = -0.0341$$
   $$\|v(0) - v(1)\|_2 = \sqrt{(0.2588)^2 + (-0.0341)^2} \approx \mathbf{0.2611}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Result:                                                            │
  │   ||v(23) - v(0)||_2 ≡ ||v(0) - v(1)||_2 ≈ 0.2611                      │
  │                                                                        │
  │ • The metric distance across midnight is identical to the distance     │
  │   between any two consecutive daytime hours.                           │
  │ • Both sine AND cosine are mandatory: using sine alone introduces a    │
  │   collision where Hour 6 (06:00) and Hour 18 (18:00) share sin = 1.0.  │
  │   Cosine resolves the phase ambiguity, ensuring every hour maps to     │
  │   a unique coordinate on S¹.                                           │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Multiplicative Interactions: House Size & Location (Slide 5)
  
  Slide 5 introduces non-linear feature interactions using a real estate pricing dilemma:
  
  $$\mathbf{\text{"+500 sq ft is worth vastly more downtown than in distant rural areas."}}$$
  
  ```
                       The Interaction Dilemma (Slide 5)
         Downtown Urban Core                            Distant Rural Area
  ┌─────────────────────────────────────┐       ┌─────────────────────────────────────┐
  │ 1,000 sq ft house ──▶ $500,000      │       │ 1,000 sq ft house ──▶ $200,000      │
  │ 1,500 sq ft house ──▶ $750,000      │       │ 1,500 sq ft house ──▶ $220,000      │
  │                                     │       │                                     │
  │ Marginal Value: +$250,000 / 500 sqft│       │ Marginal Value: +$20,000 / 500 sqft │
  │ Slope: $500 / sq ft                 │       │ Slope: $40 / sq ft                  │
  └─────────────────────────────────────┘       └─────────────────────────────────────┘
  ```
  
  Slide 5 evaluates three successive formulations of linear hypothesis spaces:
  
  ---
### Model 1: Univariate Size Model
  $$\text{Price} = \beta_0 + \beta_1 \text{Size}$$
  * **Pathology:** Assumes real estate value is purely a function of interior volume, completely ignoring geographic location. 
  * **Outcome:** Underfits severely; averages urban and rural prices into an uninformative regression line.
  
  ---
### Model 2: Purely Additive Multi-Feature Model
  $$\text{Price} = \beta_0 + \beta_1 \text{Size} + \beta_2 \text{Location}$$
  where $\text{Location} = 1$ for Downtown and $\text{Location} = 0$ for Rural.
  
  * **Analysis of Slopes:** Compute the partial derivative of $\text{Price}$ with respect to $\text{Size}$:
  $$\frac{\partial \text{Price}}{\partial \text{Size}} = \mathbf{\beta_1 \quad (\text{Constant across all geographies})}$$
  * **Geometric Reality:** This model fits **two parallel hyperplanes**:
  * Rural ($\text{Loc} = 0$): $\text{Price} = \beta_0 + \beta_1 \text{Size}$
  * Downtown ($\text{Loc} = 1$): $\text{Price} = (\beta_0 + \beta_2) + \beta_1 \text{Size}$
  
  ```
                      The Additive Model's Parallel Trap
       Price
         ▲
         │                                       / Downtown Plane (Intercept: β₀ + β₂)
         │                                      /
         │                                     /
         │                                    /  Slope is strictly β₁ for BOTH!
         │                                   /
         │                                  /  / Rural Plane (Intercept: β₀)
         │                                 /  /
         │                                /  /
         │                               /  /
         └──────────────────────────────/──/────────────────────────▶ Size
  ```
  
  * **The Failure:** The additive model shifts the intercept up by $\beta_2$, but forces the **marginal price per square foot ($\beta_1$) to be identical in both markets**. It asserts that expanding a house by $500\text{ sq ft}$ adds the exact same dollar amount in downtown Manhattan as it does in rural farmland.
  
  ---
### Model 3: Multiplicative Cross-Product Interaction Model
  $$\text{Price} = \beta_0 + \beta_1 \text{Size} + \beta_2 \text{Location} + \mathbf{\beta_3 (\text{Size} \times \text{Location})}$$
  
  Now, compute the partial derivative with respect to $\text{Size}$:
  
  $$\frac{\partial \text{Price}}{\partial \text{Size}} = \mathbf{\beta_1 + \beta_3 \text{Location}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Mathematical Triumph: The slope itself is now a linear function of │
  │ the Location variable!                                                 │
  │                                                                        │
  │ • When Location = 0 (Rural):                                           │
  │   Price = β₀ + β₁ Size ──▶ Marginal Rate = β₁ ($40 / sq ft)            │
  │                                                                        │
  │ • When Location = 1 (Downtown):                                        │
  │   Price = (β₀ + β₂) + (β₁ + β₃) Size ──▶ Marginal Rate = β₁ + β₃       │
  │                                           ($500 / sq ft)               │
  │                                                                        │
  │ By engineering the interaction term x_3 = x_1 · x_2, a simple linear   │
  │ model can fit non-parallel, context-dependent slopes across subsets.   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why does projecting a cyclical temporal feature (such as `month` $\in \{1, \dots, 12\}$) into a 2D space via $[\sin(2\pi m / 12), \cos(2\pi m / 12)]$ allow linear and distance-based models to capture seasonal continuity without creating boundary discontinuities?*
  2. *Why is using $\sin(2\pi t / T)$ alone insufficient for cyclical feature encoding? What geometric ambiguity arises if cosine is omitted?*
  3. *In a linear model $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 (x_1 x_2)$, prove mathematically that the marginal effect of feature $x_1$ on $y$ depends directly on the current value of feature $x_2$.*
  
  ---