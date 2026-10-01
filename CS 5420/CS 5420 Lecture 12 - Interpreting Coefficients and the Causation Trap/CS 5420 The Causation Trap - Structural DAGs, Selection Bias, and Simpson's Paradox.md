## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Proof of Overestimation in Omitted Variable Bias:**
  * The short OLS estimator satisfies:
   $$\tilde{\beta}_1 = \beta_1 + \beta_2 \cdot \gamma_{21}, \quad \text{where } \gamma_{21} = \frac{\widehat{\text{Cov}}(x_1, x_2)}{\widehat{\text{Var}}(x_1)}$$
  * If the omitted variable has a positive direct effect on the target ($\beta_2 > 0$) and is positively correlated with the included feature ($\text{Cov}(x_1, x_2) > 0$), then the auxiliary slope is strictly positive:
   $$\gamma_{21} > 0 \implies \text{Bias} = \beta_2 \cdot \gamma_{21} > 0$$
  * Therefore:
   $$\tilde{\beta}_1 = \beta_1 + (\text{Positive Quantity}) > \beta_1$$
  * The univariate model attributes both the direct effect of $x_1$ and the indirect proxy effect of $x_2$ to $\tilde{\beta}_1$, overestimating the true partial slope.
  2. **The Clinical Mechanism of the Diabetes Sign Flip:**
  * Total serum cholesterol ($s_1$) correlates positively with serum triglycerides ($s_5$) ($\text{Corr} = +0.516$). In the general population, patients with high total cholesterol also have high triglycerides, creating a positive unconditioned association with diabetes progression ($w_{s_1} = +0.449$).
  * However, total cholesterol is the sum of fractions: $\text{Total} = \text{LDL} + \text{VLDL} + \text{HDL}$. 
  * When $s_5$ (triglycerides / VLDL) is included in the model, the partial derivative $\frac{\partial Y}{\partial s_1}$ evaluates the effect of increasing total cholesterol **while holding triglycerides fixed**. At a fixed level of triglycerides, higher total cholesterol indicates a higher concentration of protective **High-Density Lipoprotein (HDL / "good cholesterol")**. Thus, its true conditional partial slope flips negative ($w_{s_1} = -0.288$).
  3. **Frisch-Waugh-Lovell Residual Projections:**
  * By the FWL Theorem, computing the multiple regression parameter $w_j$ is mathematically equivalent to regressing the **residualized target** $\tilde{y}$ on the **residualized feature** $\tilde{x}_j$:
   $$w_j = \frac{\langle \tilde{x}_j, \, \tilde{y} \rangle}{\|\tilde{x}_j\|_2^2}$$
   where $\tilde{x}_j = (I - H_{-j}) x_j$ is the component of $x_j$ orthogonal to all other features, and $\tilde{y} = (I - H_{-j}) y$ is the component of $y$ unexplained by all other features.
  
  ---
## 2. The Observational Barrier: Why Correlation Is Not Causation (Deck 1 Slide 5)
  
  Deck 1 Slide 5 states an essential methodological boundary:
  
  $$\mathbf{\text{"A regression cannot distinguish among these. Only design can."}}$$
  
  A regression estimator optimizes an empirical loss function over an observed joint probability distribution:
  
  $$\mathbb{E}[Y \mid X = x] = \int y \, P(y \mid x) \, dy$$
  
  Regression computes **observational conditional expectations**. It calculates the optimal mathematical surface that summarizes statistical associations. It possesses zero intrinsic awareness of time, physical agency, or causal directionality.
  
  ---
## 3. The Three Structural Mechanisms of Non-Causal Association (Deck 2 Slide 7)
  
  Deck 2 Slide 7 utilizes **Directed Acyclic Graphs (DAGs)** (Pearl, 2000) to formalize the three topological structures that manufacture non-causal correlations:
  
  ```
                  The Three Structural Mechanisms (Deck 2 Slide 7)
    1. Confounding (The Fork)        2. Reverse Causation            3. Selection Bias (The Collider)
              Z                               X                               X               Y
            /   \                             ▲                                \             /
           ▼     ▼                            │                                 ▼           ▼
          X - - - Y                           Y                                      [ S ]
       (Z drives both)                  (Y causes X)                    (Conditioning on common
   Ice cream & drownings share       Hospital visits and                collider S creates link)
        summer heat                    illness severity
  ```
  
  ---
### Mechanism 1: Confounding (The Common Cause / The Fork)
  * **The Topology:** A third variable $Z$ acts as an upstream common cause of both feature $X$ and outcome $Y$:
  $$X \longleftarrow Z \longrightarrow Y$$
  * **The Classical Example:** Ice cream sales ($X$) and coastal drownings ($Y$) exhibit a statistically significant positive correlation ($r_{xy} > 0$).
  * **The Mechanism:** Summer seasonal temperature ($Z$) causes people to buy ice cream ($Z \to X$) and independently causes people to swim in the ocean ($Z \to Y$).
  * **Resolution via Backdoor Conditioning:** Because $Z$ opens a "backdoor path" between $X$ and $Y$, including $Z$ as a covariate in the regression design matrix blocks the path, rendering $X$ and $Y$ conditionally independent:
  $$P(Y \mid X, Z) = P(Y \mid Z) \implies w_{\text{ice\_cream}} \longrightarrow 0$$
  
  ---
### Mechanism 2: Reverse Causation (Simultaneity / Temporal Inversion)
  * **The Topology:** The hypothesized causal arrow flows in reverse, from the outcome to the feature:
  $$X \longleftarrow Y$$
  * **The Clinical Example:** A regression of `Illness Severity` ($Y$) on `Number of Hospital Visits` ($X$) produces a large positive coefficient ($w_1 > 0$).
  * **The Fallacy:** Concluding that visiting the hospital causes patients to become critically ill.
  * **The Mechanism:** Severe physiological illness causes patients to seek emergency hospitalization ($Y \to X$). Observational regression cannot differentiate whether $X$ causes $Y$ or $Y$ causes $X$; both yield identical positive dot products ($X^T y > 0$).
  
  ---
### Mechanism 3: Selection Bias / Berkson’s Fallacy (The Collider Trap)
  Deck 2 Slide 7 illustrates a subtle and dangerous structural trap:
  
  $$\mathbf{\text{"Selection — the sample was chosen in a way that manufactures the association."}}$$
  $$\mathbf{\text{"Conditioning on collider } S \text{ creates the link."}}$$
  
  * **The Topology:** Two completely independent features $X$ and $Y$ ($X \perp Y$) both causally influence an enrollment, admission, or survival variable $S$ (a **collider**):
  $$X \longrightarrow [S] \longleftarrow Y$$
  * **The Mathematical Defect:** While $X$ and $Y$ are marginally independent across the general population, **conditioning on the collider $S$ (by filtering the dataset to rows where $S = 1$) induces a spurious negative correlation between them**:
  $$X \perp Y \quad \text{but} \quad \mathbf{X \not\perp Y \mid S}$$
  
  ```
                          The Collider Conditioning Trap
         General Population: Independent                 Admitted Cohort (Conditioned on S = 1)
   Athletic Ability (X)                            Athletic Ability (X)
      ▲                                               ▲
      │    ●   ●   ●   ● (True Independence:          │            ●   ●   ●  [S = 1 Pool]
      │  ●   ●   ●   ●    Corr = 0.0)                 │          ●   ●   ●
      │    ●   ●   ●   ●                              │        ●   ●   ●
      │  ●   ●   ●   ●                                │      ●   ●   ●   (Spurious Negative Slope!)
      └────────────────────────▶ Academic Skill (Y)   └────────────────────────▶ Academic Skill (Y)
  ```
#### Real-World Example (Berkson’s Paradox):
  * Let $X$ be musical talent and $Y$ be mathematical ability (assume they are uncorrelated in the global population).
  * An elite university admits students ($S = 1$) if they possess exceptional musical talent OR exceptional mathematical ability.
  * If a researcher evaluates only admitted students ($S = 1$), observing that an admitted student has low musical talent guarantees they must possess high mathematical ability to have been admitted. Conditioning on the sample selection filter manufactures a false negative association.
  
  ---
## 4. Simpson’s Paradox: Sign Reversal in the Aggregate (Deck 1 Slide 6)
  
  Slide 6 formalizes the canonical paradox of observational data:
  
  $$\mathbf{\text{"An association present in every subgroup can reverse in the aggregate."}}$$
  
  ```
                       Simpson's Paradox Geometry
    Aggregate Distribution: NEGATIVE Slope       Subgroup Breakdown: POSITIVE Slopes
    Outcome y                                    Outcome y
       ▲                                            ▲
    80 │  ●                                      80 │         Subgroup 2
       │   \                                        │        /   ●   ●
    60 │    \  Combined Trend                       60 │       /   ●   ●
       │     \ (Apparent Negative Effect)           │      /
    40 │      \                                  40 │     Subgroup 1
       │       ●                                    │    /   ●   ●
    20 │        \                                20 │   /   ●   ●
       └────────────────────────▶ Feature x         └────────────────────────▶ Feature x
       0        20       40                         0        20       40
  ```
  
  ---
### The 1973 UC Berkeley Admissions Benchmark (Bickel et al., 1975)
  Deck 1 Slide 6 cites the famous empirical study:
  * **The Aggregate Observation:**
  * Men applicants admitted: **$44\%$**
  * Women applicants admitted: **$35\%$**
  * The University of California, Berkeley was sued for systemic gender discrimination based on this $9\%$ aggregate disparity.
  * **The Subgroup Investigation:**
  * When researchers disaggregated the applications across individual academic departments, women were admitted at an **equal or higher rate than men in nearly every single department**.
  
  ```
                   The Berkeley Admissions Data (Extract)
  ┌────────────┬─────────────────────────────┬─────────────────────────────┐
  │ Department │ Men Applicants (Admit Rate) │ Women Applicants (Admit Rate│
  ├────────────┼─────────────────────────────┼─────────────────────────────┤
  │ Dept A     │ 825 applied  (62% Admitted) │ 108 applied  (82% ADMITTED!)│
  │ Dept B     │ 560 applied  (63% Admitted) │  25 applied  (68% ADMITTED!)│
  │ Dept C     │ 325 applied  (37% Admitted) │ 593 applied  (34% Admitted) │
  │ Dept D     │ 417 applied  (33% Admitted) │ 375 applied  (35% ADMITTED!)│
  │ Dept E     │ 191 applied  (28% Admitted) │ 393 applied  (24% Admitted) │
  │ Dept F     │ 373 applied  ( 6% Admitted) │ 341 applied  ( 7% ADMITTED!)│
  ├────────────┼─────────────────────────────┼─────────────────────────────┤
  │ AGGREGATE  │ 8,442 total  (44% Admitted) │ 4,321 total  (35% Admitted) │
  └────────────┴─────────────────────────────┴─────────────────────────────┘
  ```
  
  Slide 6 emphasizes:
  $$\mathbf{\text{"The mechanism: women applied disproportionately to more competitive departments."}}$$
  $$\mathbf{\text{"Lesson: the omitted variable did not just weaken the estimate. It reversed its sign."}}$$
  
  Women disproportionately applied to humanities departments (Departments C, E, F), which had very low baseline admission quotas ($<25\%$). Men disproportionately applied to engineering and physical sciences (Departments A, B), which had large admission quotas ($>60\%$).
  
  ---
## 5. Simpson’s Paradox: Argue Both Sides (Deck 1 Slide 7)
  
  Slide 7 presents a classroom Think–Pair–Share activity:
  
  $$\mathbf{\text{"Both numbers are correct. Pair A: argue the aggregate number is the honest one."}}$$
  $$\mathbf{\text{"Pair B: argue the per-department numbers are. Then: what would settle it?"}}$$
  $$\mathbf{\text{"Hint: it is a question about the data-generating process, not the data."}}$$
  
  ```
                      The Competing Perspectives (Slide 7)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Pair A: The Aggregate Perspective (44% Men vs. 35% Women)              │
  │ • Argument: "The university as a whole admits a smaller fraction of    │
  │   female applicants. If the university allocates more institutional    │
  │   funding and faculty lines to historically male-dominated departments │
  │   while starving female-dominated departments of capacity, the         │
  │   institutional structure discriminates against women."                │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Pair B: The Per-Department Perspective (Women Admitted at Higher Rates)│
  │ • Argument: "Admissions decisions are made independently by faculty    │
  │   committees inside each department. Department chairs never saw       │
  │   university-wide totals. At the decision level, women were admitted   │
  │   at equal or higher rates than men with identical qualifications.     │
  │   No admissions committee acted with bias."                            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### What Settles It? The Causal Data-Generating Process
  Slide 7’s hint resolves the dispute: you cannot settle Simpson’s Paradox by staring at the numbers; you must formulate the **underlying Causal Graph (DAG)**:
  
  ```
                         Causal Graph A: Confounder
                             Department Choice (Z)
                                  /         \
                                 ▼           ▼
                            Gender (X) ──▶ Admission (Y)
  (If Department Choice causally dictates Gender, Z is a confounder. Condition on Z!)
  ```
  
  ```
                          Causal Graph B: Mediator
                             Department Choice (M)
                                  ▲         \
                                 /           ▼
                            Gender (X) ──▶ Admission (Y)
  (If Gender dictates Department Choice, M is a MEDIATOR on the causal path!)
  ```
  
  * If **Department Choice is a Mediator ($X \to M \to Y$)**:
  * Gender ($X$) causes students to apply to specific departments ($M$), which affects admissions ($Y$).
  * If you want to know the **Total Causal Effect** of gender on admission, you **must not control for department**, because controlling for a mediator blocks the indirect causal pathway!
  * If **Department Choice is a Confounder ($X \leftarrow Z \to Y$)**:
  * Controlling for department removes spurious confounding.
  
  Slide 6 concludes:
  $$\mathbf{\text{"This is why 'we controlled for the obvious things' is not a defense."}}$$
  
  Controlling for intermediate variables or colliders can introduce bias where none existed.
  
  ---
## 6. When Causal Language Is Legitimate (Deck 1 Slide 8)
  
  Slide 8 establishes the strict standard for scientific reporting:
  
  $$\mathbf{\text{"Absent one of these, write 'associated with', not 'causes' or 'leads to'."}}$$
  $$\mathbf{\text{"This is a writing standard on your final project report."}}$$
  
  ```
                The Three Pillars of Legitimate Causal Claims (Slide 8)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Randomized Controlled Experiments (RCTs)                            │
  │    • Random assignment of treatment X severs all incoming backdoor     │
  │      arrows (eliminating confounding by physical design: do(X)).       │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Credible Natural Experiments or Instrumental Variables (IV)         │
  │    • Exploits exogenous arbitrary variations (lotteries, policy        │
  │      borders, weather shocks) that isolate exogenous variance in X.    │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Explicit Structural Causal Models with Defensible Assumptions       │
  │    • Requires a fully articulated DAG satisfying the Backdoor Criterion│
  │      with verifiable conditional independence implications.            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Course Scientific Writing Rubric:                                  │
  │                                                                        │
  │ ❌ PROHIBITED (Unjustified Causal Claims):                             │
  │   • "Increasing advertising spend causes an increase in revenue."      │
  │   • "Higher churn leads to lower customer lifetime value."             │
  │                                                                        │
  │ ✔ MANDATORY (Observational Rigor):                                     │
  │   • "Higher advertising spend is positively associated with revenue,   │
  │     holding market region and seasonal promotions constant."           │
  │   • "Our linear model indicates that each unit increase in x is        │
  │     associated with a w_j unit predicted change in y."                 │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 3
  
  1. *In a Directed Acyclic Graph, what is a "collider" ($X \to S \leftarrow Y$), and why does conditioning on a collider induce a spurious correlation between two variables that are marginally independent?*
  2. *State Simpson’s Paradox in your own words, and explain how women could be admitted at a higher rate than men in every department at UC Berkeley while having a lower aggregate admission rate across the university.*
  3. *Why does physical randomization in an experiment allow a researcher to use causal verbs like "causes" and "increases," whereas observational multiple linear regression only justifies "is associated with"?*
  
  ---