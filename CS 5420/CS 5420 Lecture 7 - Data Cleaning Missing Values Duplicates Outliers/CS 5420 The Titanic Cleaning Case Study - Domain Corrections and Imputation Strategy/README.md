## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why You Must Never Recompute Scaler Parameters on Out-of-Bounds Test Data:**
  * Preprocessing parameters ($\mu_{\text{train}}, \sigma_{\text{train}}, \min_{\text{train}}, \max_{\text{train}}$) represent the **frozen empirical estimates** of the data-generating distribution learned from the training split.
  * In production, inference occurs on single streaming observations ($N=1$) or unbatched payloads where computing a sample mean or maximum is mathematically undefined. 
  * Recomputing scaling parameters on the test set incorporates out-of-sample test knowledge into the transformation, violating the fundamental out-of-sample evaluation contract. If a test value exceeds observed training bounds, it must simply yield a $Z$-score greater than $3.0$ or a normalized value $> 1.0$. The downstream estimator must handle tail inputs through regularization and clipping.
  2. **Why Target Encoding Outside a `Pipeline` Causes Catastrophic Leakage:**
  * Target encoding replaces a categorical level with the expected target value: $\hat{x}_c = \mathbb{E}[Y \mid X = c]$.
  * If computed globally prior to cross-validation, the encoded value for a category in the training folds directly incorporates the **ground-truth labels ($y_{\text{val}}$)** of the validation fold. 
  * The model learns a direct proxy of the validation targets, generating artificially high cross-validation scores that collapse when deployed on genuinely unobserved data. Encapsulating the encoder inside a `Pipeline` guarantees that target means are computed strictly from the $K-1$ training folds.
  3. **`fit_transform(X_train)` vs. `transform(X_test)`:**
  * **`pipeline.fit_transform(X_train)`:** Calculates the state parameters (means, variances, medians, imputation dictionaries, category mappings) from the training data, stores them as internal attributes, and transforms `X_train`.
  * **`pipeline.transform(X_test)`:** Computes **zero new parameters**. It strictly evaluates `X_test` against the frozen state parameters learned during the training step.
  
  ---
## 2. OpenML Titanic Missingness Treatment Matrix (Slide 10)
  
  Slide 10 presents the initial tabular audit of the complete OpenML Titanic dataset ($N = 1{,}309$ passengers across $14$ variables):
  
  ```python
  from sklearn.datasets import fetch_openml
  import pandas as pd
  
  df = fetch_openml('titanic', version=1, as_frame=True).frame
  ```
  
  Slide 10 tasks the class with formulating explicit engineering treatments for the three primary missing features:
  
  ```
                  OpenML Titanic Missingness Treatment Matrix (Slide 10)
  ┌──────────┬───────────────┬─────────────┬────────────────────────────────────────────────────────┐
  │ Column   │ Missing Count │ Missing Pct │ Engineering Treatment & Statistical Rationale          │
  ├──────────┼───────────────┼─────────────┼────────────────────────────────────────────────────────┤
  │ age      │ 263           │ 20.1%       │ Grouped Median Imputation (by pclass + honorific title)│
  │          │               │             │ + Append Missingness Indicator: 'age_is_missing'.      │
  ├──────────┼───────────────┼─────────────┼────────────────────────────────────────────────────────┤
  │ cabin    │ 1,014         │ 77.5%       │ Do NOT impute names! Extract structural deck letter    │
  │          │               │             │ (first char: 'C', 'E') with 'U' for Unknown/Missing.   │
  ├──────────┼───────────────┼─────────────┼────────────────────────────────────────────────────────┤
  │ fare     │ 1             │ < 0.1%      │ Class-conditional median imputation based on pclass=3  │
  │          │               │             │ and embarked='S'. Single sample (Thomas Storey).       │
  └──────────┴───────────────┴─────────────┴────────────────────────────────────────────────────────┘
  ```
  
  ---
### In-Depth Analysis of Missingness Mechanisms (Rubin's Framework)
  To justify these treatments at a graduate level, we categorize each variable under **Donald Rubin’s Missing Data Taxonomy (1976)**:
#### 1. `age` (Missing at Random — MAR)
  * **The Mechanism:** The probability that age is missing depends on observed variables (socioeconomic class and gender), not on the unobserved age itself:
  $$P(\text{Age is Missing} \mid \text{Pclass}, \text{Sex}, \text{Age}) = P(\text{Age is Missing} \mid \text{Pclass}, \text{Sex})$$
  * **The Treatment:** Steerage passengers (3rd class) had missing age records far more frequently than 1st class aristocrats. A naive global median ($\approx 28.0$) distorts young and old sub-populations. 
  * **The Solution:** Extract the social title from the `name` column (`"Master"`, `"Miss"`, `"Mr"`, `"Mrs"`), group by `pclass` and `title`, and impute localized medians. An aristocrat with the title `"Master"` receives a child's median age ($\approx 4$), whereas a global median would assign them an adult age ($28$).
#### 2. `cabin` (Missing Not at Random — MNAR)
  * **The Mechanism:** The missingness of the cabin is causally driven by the unobserved value itself:
  $$P(\text{Cabin is Missing} \mid \text{Cabin}, \text{Pclass}) \neq P(\text{Cabin is Missing} \mid \text{Pclass})$$
  * **The Physical Reality:** 1st class passengers on upper decks were assigned private staterooms with recorded numbers. 3rd class passengers in steerage were largely unassigned specific cabin suites. The presence or absence of a cabin number is itself a direct proxy for physical ship elevation and socioeconomic status.
  * **The Treatment:** Imputing missing cabins with a mode or median manufactures non-existent staterooms. Instead, extract the **Deck Letter** (`df['cabin'].str[0]`) and assign missing values to an explicit category: `'U'` (Unknown / Steerage).
#### 3. `fare` (Missing Completely at Random — MCAR)
  * **The Mechanism:** Exactly one passenger (Thomas Storey, a 60-year-old 3rd-class passenger traveling alone from Southampton) has a missing fare due to a clerical logging oversight.
  * **The Treatment:** Missingness is uncorrelated with any unobserved variables. Impute using the median fare of single 3rd-class passengers departing from Southampton.
  
  ---
## 3. Dissecting the Two Domain Pathologies in `fare` (Slide 11)
  
  Slide 11 demonstrates that numerical inspection without domain awareness leads to incorrect preprocessing decisions:
  
  ```python
  df['fare'].describe()
  # min: 0.00 | median: 14.45 | max: 512.33
  ```
  
  Slide 11 highlights two structural anomalies embedded in the summary statistics:
  
  ```
                          The Dual Anomalies in 'fare'
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Anomaly 1: The Zero-Fare Sentinel (min = 0.00)                         │
  │  • 17 observations have fare == 0.00.                                  │
  │  • Domain Truth: These were White Star Line employees, ship guarantee  │
  │    groups (contractors from Harland & Wolff), and company executives.  │
  │  • They were NOT free civilian commercial tickets. Treating them as    │
  │    standard 3rd-class passengers distorts the relationship between     │
  │    ticket price and survival.                                          │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Anomaly 2: The Shared-Ticket Distortion (max = 512.33)                 │
  │  • The top 4 highest fares all equal exactly 512.33.                   │
  │  • Domain Truth: In 1912, White Star Line charged fares PER TICKET,    │
  │    not PER PERSON. The Cardeza family party of 4 traveled on a single  │
  │    shared ticket (PC 17755).                                           │
  │  • The total aggregate party cost (£512.33) was logged against every   │
  │    individual passenger's record, creating artificial extreme outliers!│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Domain Feature Engineering: Transforming Group Fares (Slide 11)
  
  Slide 11 emphasizes a critical principle:
  
  $$\mathbf{\text{"Fix the definition — fare / party\_size — instead of deleting the row."}}$$
  
  ```python
  # 1. Audit the highest fares and verify ticket sharing
  df.nlargest(4, 'fare')[['name', 'pclass', 'ticket', 'fare']]
  # All four share one identical ticket identifier: PC 17755
  
  # 2. Compute group ticket party size using transform('size')
  df['party'] = df.groupby('ticket')['fare'].transform('size')
  
  # 3. Calculate true individual fare per person
  df['fare_pp'] = df['fare'] / df['party']
  
  # 4. Inspect resulting distribution
  df['fare_pp'].describe()
  # max: 512.33 ──▶ 128.08 | median: 14.45 ──▶ 8.05
  ```
  
  ```
                   Impact of Domain Correction on Fare Distribution
           Original Metric (df['fare'])              Corrected Metric (df['fare_pp'])
    ┌──────────────────────────────────────┐    ┌──────────────────────────────────────┐
    │ Maximum: £512.33 (Severe outlier)    │ ──▶│ Maximum: £128.08 (True individual max│
    │ Median:  £14.45                      │ ──▶│ Median:  £8.05                       │
    │ Meaning: Blends individual tickets   │    │ Meaning: Represents true economic    │
    │          with pooled group bookings. │    │          resources per individual.   │
    └──────────────────────────────────────┘    └──────────────────────────────────────┘
  ```
### Systems Breakdown of `groupby().transform('size')`:
  1. `df.groupby('ticket')['fare']`: Partitions the DataFrame into sub-tables grouped by the shared ticket string.
  2. `.transform('size')`: Calculates the row count within each group and **broadcasts the scalar back to the original index dimension** ($1{,}309$).
  3. **Why this beats row deletion:** Deleting the four passengers who paid $512.33$ would discard prominent historical figures (e.g., Charlotte Ward, Thomas Cardeza) and reduce statistical power. Correcting the metric normalizes the distribution while retaining all training samples.
  
  ---
## Summary Review Questions for Section 4
  
  1. *Under Donald Rubin's missing data framework, why does missingness in the `cabin` column qualify as Missing Not at Random (MNAR), and why does imputing it with the modal cabin letter introduce severe statistical bias?*
  2. *In the Titanic dataset, why does calculating `fare_pp = fare / party_size` reduce the maximum fare from $512.33$ to $128.08$, and how does this affect linear and distance-based estimators?*
  3. *Why is deleting observations with `fare == 0.00` problematic if your production model must handle crew members or company staff in real-world deployments?*
  
  ---
## Complete Lecture 7 Synthesis Reference
  
  | Data Cleaning Challenge | Statistical / Systems Mechanism | Production Mitigation Protocol |
  | :--- | :--- | :--- |
  | **Enterprise Data Corruption** | Unstructured headers, unit strings in numericals, sentinel codes (-999, 9999). | Execute an automated 3-tier audit (`audit_dataset`) before writing pipeline code. |
  | **Outlier Detection: $Z$-Score** | Measures deviations in standard units: $Z = \frac{x - \bar{x}}{s}$. Masking effect hides true anomalies. | Use **Modified $Z$-score via MAD** ($M_i = \frac{0.6745(x - \tilde{x})}{\text{MAD}}$) for robust Gaussian auditing. |
  | **Outlier Detection: Tukey IQR** | Non-parametric order fences: $[Q_1 - 1.5\cdot\text{IQR}, \; Q_3 + 1.5\cdot\text{IQR}]$. | Robust to skewed distributions; offers a $25\%$ breakdown point against corruption. |
  | **Outlier Detection: Isolation Forest** | Partitions space randomly; anomalies isolate near root ($h(x) \to 0$, $s \to 1.0$). | Best for high-dimensional tabular manifolds; detects multi-feature interaction anomalies. |
  | **The Deletion Fallacy** | Reflexively deleting outliers truncates the support of $P(X, Y)$. | Defend deletion with causal/physical proofs. If genuine tail events, use robust estimators (Huber loss, tree ensembles). |
  | **Preprocessing Leakage** | Fitting scalers or imputers globally before splitting contaminates training data with test statistics. | **Split FIRST!** Fit transformers strictly on $\mathcal{D}_{\text{train}}$; apply frozen `.transform()` to $\mathcal{D}_{\text{test}}$. |
  | **Cross-Validation Leakage** | Scaling outside the cross-validation loop leaks validation folds into training iterations. | Encapsulate sub-pipelines inside `Pipeline` and `ColumnTransformer` to enforce strict fold isolation. |
  | **Missingness Imputation (Titanic)** | `age` is MAR (grouped median by class/title); `cabin` is MNAR (extract deck category `'U'`). | Audit missingness mechanisms under Rubin's framework before selecting imputation strategies. |
  | **Domain Feature Engineering** | Fares charged per ticket rather than per person created artificial $512.33$ outliers. | Compute group sizes via `groupby('ticket').transform('size')` to engineer true individual features (`fare_pp`). |
  
  ---