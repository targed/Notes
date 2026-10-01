## 1. Quick Review: Answers to Section 4 Checkpoints
  
  1. **Threshold Tuning in Clinical Screening:**
  * When missing a fatal condition carries an extreme cost while treatment is safe and cheap, optimize for **Recall / Sensitivity** (minimize False Negatives, $FN \to 0$).
  * By lowering the classification threshold $\tau$ (e.g., from $0.50$ to $0.15$):
   $$\hat{y} = \mathbf{1}_{\{P(Y=1 \mid x) \ge 0.15\}}$$
  * **Confusion Matrix Shifts:** True Positives ($TP$) and False Positives ($FP$) both increase, while False Negatives ($FN$) and True Negatives ($TN$) decrease. You trade lower Precision (accepting more false alarms) to guarantee high Recall.
  2. **The ROC-AUC vs. PR-AUC Divergence Under Severe Imbalance:**
  * Suppose a test set has $1{,}000$ samples: $1$ positive ($P = 1$) and $999$ negatives ($N = 999$).
  * A model generates $20$ positive predictions: $1$ is correct ($TP = 1, FN = 0$), and $19$ are incorrect ($FP = 19, TN = 980$).
  * **ROC Metric:** The False Positive Rate is negligible:
   $$\text{FPR} = \frac{FP}{N} = \frac{19}{999} \approx 0.019 \implies \text{Specificity} = 98.1\%$$
   The ROC curve plots $\text{TPR} = 1.0$ against $\text{FPR} = 0.019$, yielding an exceptional ROC-AUC ($\approx 0.99$).
  * **PR Metric:** Precision exposes the reality:
   $$\text{Precision} = \frac{TP}{TP + FP} = \frac{1}{1 + 19} = \mathbf{0.05} \quad (5\%)$$
   The Precision-Recall curve plummets to $0.05$, reflecting an unviable clinical pipeline.
  3. **Covariate Shift Mechanics:**
  * In Covariate Shift, the input marginal distribution changes ($P_{\text{train}}(X) \neq P_{\text{test}}(X)$), while the true concept conditional distribution remains invariant ($P(Y \mid X)_{\text{train}} = P(Y \mid X)_{\text{test}}$).
  * Standard random cross-validation draws training and validation folds uniformly from the pooled dataset, ensuring $P_{\text{val}}(X) \approx P_{\text{train}}(X)$. This hides covariate shift until the model is deployed on a genuinely distinct operational population.
  
  ---
## 2. Where Machine Learning Runs in Production (Slides 15–16)
  
  Slide 15 catalogs key operational sectors, while Slide 16 highlights two engineering milestones:
  
  ```
                            Applied ML Domains (Slide 15)
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Medical & Bio:       In silico drug candidate screening, molecular property prediction │
  │ Finance:             Sub-millisecond high-frequency trading, real-time fraud scoring   │
  │ Social & Web:        Large-scale recommendation graphs, multi-stage retrieval & ranking│
  │ NLP & Speech:        Multimodal speech-to-text, neural machine translation, LLMs       │
  │ Industrial:          Predictive fleet maintenance, semiconductor yield optimization    │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
### Deep Dive: Two Transformative Deployments (Slide 16)
  
  ```
        Autonomous Vehicle Perception                     DeepMind AlphaFold 2
  ┌───────────────────────────────────────────┐ ┌───────────────────────────────────────────┐
  │ • Input: 8 raw surround-view camera feeds │ │ • Input: 1D linear amino acid sequence    │
  │ • Task: 3D Occupancy Grid Prediction      │ │ • Task: 3D atomic coordinates (x, y, z)   │
  │ • Mechanics: Cross-attention maps 2D      │ │ • Mechanics: Evoformer attention blocks   │
  │   pixels into unified 3D vector space;    │ │   process Multiple Sequence Alignments    │
  │   runs under strict latency (<50ms).      │ │   (MSAs) and invariant point matrices.    │
  └───────────────────────────────────────────┘ └───────────────────────────────────────────┘
  ```
  
  * **Tesla Autonomous Perception:** Inverts classical robotic pipelining. Instead of tracking objects using hand-coded heuristics over radar points, multi-camera feeds are passed into spatial transformers that construct a continuous, real-time vector representation of the surrounding 3D world.
  * **AlphaFold 2 (Jumper et al., 2021):** Solved a 50-year-old challenge in structural biology (Levinthal’s paradox). Given an arbitrary protein sequence of amino acids, it predicts the equilibrium 3D tertiary structure to sub-angstrom accuracy ($\approx 1.6\text{Å}$ r.m.s.d.), compressing years of crystallography experiments into minutes of GPU compute.
  
  ---
## 3. Case File Autopsies: Deconstructing Real-World Failures (Slides 24–26)
  
  Dr. Yu presents four real-world case studies demonstrating how each of the four failure modes causes catastrophic failures in production.
  
  ---
### Case 1: Amazon’s Experimental Résumé Screener (2014–2017)
  * **The Failure Mode:** **Failure Mode 1 — Wrong Problem Framing (The Proxy Trap)**
  * **The System:** An NLP classification pipeline designed to score incoming applicant resumes on a 1-to-5 star scale to automate corporate talent acquisition.
  * **The Pathology:**
  * The model was trained on historical resumes submitted to Amazon over a 10-year period.
  * In the tech sector during that window, hiring decisions were heavily male-skewed.
  * The model optimized the proxy label:
    $$Y = \text{"Did Amazon historically hire this person?"}$$
    instead of the true intent:
    $$Y^* = \text{"Is this candidate technically competent?"}$$
  * **The Consequence:** The model penalized resumes containing the word *"women's"* (e.g., *"captain of women's soccer team"*) and downgraded graduates of all-women's colleges.
  * **The Naive Patch That Failed:** Engineers manually stripped out gender-specific keywords. The model immediately found correlated proxies in the high-dimensional feature space (e.g., word choice, sentence structure, high school names, club activities), preserving the bias.
  * **Systems Takeaway:** You cannot fix a fundamentally misaligned target label by suppressing individual features.
  
  ---
### Case 2: Multi-Hospital Chest Radiograph Screening (Zech et al., 2018)
  * **The Failure Mode:** **Failure Mode 2 — Distribution Shift with Shortcut Confounding**
  * **The System:** Deep Convolutional Neural Networks trained on $158{,}323$ chest X-rays to detect pneumonia across three major medical systems:
  $$\text{Institutions: } \{\text{NIH Clinical Center, Mount Sinai Health System, Indiana University}\}$$
  * **The Pathology:**
  * Models achieved internal validation ROC-AUCs exceeding $0.85$.
  * When evaluated on scans from a **different hospital**, performance dropped substantially (internal beat external in 3 of 5 hospital evaluations).
  * A secondary CNN was able to predict which hospital—and even which specific radiology suite—a scan originated from with **$99.9\%$ accuracy**.
  
  ```
                The Hospital Confounder Mechanism
    Mount Sinai ICU (Severe Cases)               Outpatient Clinic (Mild Cases)
  ┌─────────────────────────────────────┐     ┌─────────────────────────────────────┐
  │ • Patients bedridden, cannot stand  │     │ • Ambulatory patients walk into lab │
  │ • Scans taken via Bedside Portable  │     │ • Scans taken via Standing Rig      │
  │   scanner (low resolution)          │     │ • High spatial resolution           │
  │ • Text marker: "PORTABLE L"         │     │ • Standard anatomical orientation   │
  │ • High Pneumonia Prevalence (P ≈ 35%)│    │ • Low Pneumonia Prevalence (P ≈ 2%) │
  └──────────────────┬──────────────────┘     └──────────────────┬──────────────────┘
                   │                                           │
                   ▼                                           ▼
      Network learns the shortcut: "PORTABLE" marker in image ──▶ Classify as Pneumonia!
  ```
  
  * **The Mechanism:** Portable X-ray machines used for bedridden patients in Intensive Care Units burned the word `"PORTABLE"` into the corner of the radiograph. Because ICU patients had a much higher prevalence of pneumonia, the CNN learned to detect the machine marker rather than pulmonary infiltrates in lung tissue.
  * **Systems Takeaway:** Standard train/test splits drawn from the same site hide confounding shortcuts. Validating real generalization requires **Leave-One-Center-Out (LOCO)** validation.
  
  ---
### Case 3: The Epic Sepsis Model (Wong et al., JAMA Internal Medicine, 2021)
  * **The Failure Mode:** **Failure Mode 3 — Data Leakage (Target & Temporal Leakage)**
  * **The System:** Epic Systems’ proprietary early-warning algorithm deployed across hundreds of US hospitals to detect sepsis hours before clinical collapse.
  * **The Pathology:**
  * Epic reported an internal validation **ROC-AUC of 0.76–0.83**.
  * An independent external validation cohort at Michigan Medicine ($38{,}455$ hospitalizations) revealed an actual operational **ROC-AUC of only 0.63**.
  * **Clinical Reality:** Sensitivity was only $33\%$ with a Positive Predictive Value (PPV) of **$12\%$**.
  * **Alert Fatigue:** Nurses and doctors had to investigate **109 false alarms** to identify a single true sepsis case.
  
  ```
                      The Sepsis Temporal Leakage
  Timeline: ──▶ t_0 (Onset) ──────────▶ t_1 (Suspicion) ──────────▶ t_2 (Shock)
                                              │
                                              ▼
                                Clinician Orders Antibiotics
                                              │
            ┌─────────────────────────────────┴─────────────────────────────────┐
            │ Leakage: The model was given "antibiotic administration" as an    │
            │ input feature. It predicted sepsis ONLY AFTER the clinician had   │
            │ already identified it and initiated critical treatment!           │
            └───────────────────────────────────────────────────────────────────┘
  ```
  
  * **The Leakage Mechanism:** The model included clinical action features—such as antibiotic orders and blood culture draws—that occur **after** a physician suspects sepsis. The model learned to predict clinician behavior rather than biological progression.
  * **Systems Takeaway:** For every feature $x_j$, verify its availability timestamp:
  $$\tau(x_j) < \tau(\text{Prediction Event})$$
  
  ---
### Case 4: The COMPAS Recidivism Risk Controversy (2016)
  * **The Failure Mode:** **Failure Mode 4 — Metric Blindness & Fairness Impossibility**
  * **The System:** The Correctional Offender Management Profiling for Alternative Sanctions (COMPAS) algorithm, used in US criminal courts to predict two-year recidivism risk.
  * **The Conflict:** ProPublica investigated ~7,000 arrests in Broward County, Florida, alleging algorithmic racial bias. Northpointe (the vendor) disputed the findings.
  
  ```
                     The Fairness Metric Divide (Slide 25)
        ProPublica's Metric                          Northpointe's Metric
     [Error Rate Parity / FPR]                   [Predictive Parity / Calibration]
  ┌───────────────────────────────────┐       ┌───────────────────────────────────┐
  │ Question: Among defendants who    │       │ Question: If the model predicts a │
  │ NEVER reoffended, what percentage │       │ risk score of 7, do defendants    │
  │ were falsely flagged high-risk?   │       │ reoffend at the same rate?        │
  │                                   │       │                                   │
  │ • Black Defendants: 44.9%         │       │ • Black Risk Score 7: ~60% Recid  │
  │ • White Defendants: 23.5%         │       │ • White Risk Score 7: ~60% Recid  │
  │                                   │       │                                   │
  │ Verdict: UNFAIR (2× False Alarms) │       │ Verdict: FAIR (Well-Calibrated)   │
  └───────────────────────────────────┘       └───────────────────────────────────┘
  ```
  
  * **The Mathematical Resolution (Kleinberg et al., 2016; Chouldechova, 2017):**
  Both sides' arithmetic was correct. This case led to the **Impossibility Theorem of Algorithmic Fairness**:
  > *Unless the base rates of two demographic groups are identical ($P(Y=1 \mid A=0) = P(Y=1 \mid A=1)$), it is mathematically impossible for a predictive model to satisfy both Calibration within Groups and Error Rate Parity (Equal FPR and FNR) simultaneously.*
  * **Systems Takeaway:** Metric selection is an ethical and structural decision. Relying on a single metric hides trade-offs that appear when broken down by subgroup.
  
  ---
## 4. Debrief Synthesis Reference (Slide 26)
  
  Slide 26 provides the answer key to these four case studies:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Case             Primary Failure Mode        Systemic Diagnostic Audit                 │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ 1. Amazon Resumes Wrong Problem Framing      Audit where the label came from; ensure   │
  │                   (Proxy Trap)               the proxy aligns with true human intent.  │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ 2. Hospital X-Rays Distribution Shift        Enforce Leave-One-Site-Out splits; verify │
  │                   (Shortcut Confounders)     models don't learn acquisition artifacts. │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ 3. Epic Sepsis    Data Leakage               Audit feature timestamps; verify features │
  │                   (Downstream Actions)       precede the diagnostic decision event.    │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ 4. COMPAS Scores  Metric Blindness           Publish and evaluate multidimensional     │
  │                   (Fairness Impossibility)   fairness and error-rate parity metrics.   │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Immediate Action Items & Looking Ahead (Slide 27)
  
  Slide 27 outlines practical next steps for the upcoming lab:
  1. **Bring a Laptop to Next Class:**
   * Transitioning from conceptual foundations to hands-on Python engineering.
   * Execution platform: **Google Colaboratory** (pre-configured Linux runtime with NumPy, Pandas, scikit-learn, and CUDA support).
  2. **Account Readiness:** Verify that your Google account can access [Colab](https://colab.research.google.com).
  3. **Coding Assignment 1:**
   * **Released:** Friday (Module 1 practical release).
   * **Due Date:** Friday, September 11 at 11:59 PM.
   * **Core Focus:** Vectorized array operations, exploratory data analysis, pipeline construction, and leak-free cross-validation.
  
  ---
## Complete Lecture 2 Synthesis Checkpoint
  
  To ensure complete command of Lecture 2, make sure you can answer these three synthesis questions:
  1. *Why did removing explicit protected terms fail to fix bias in the Amazon resume screening model?*
  2. *In the Mount Sinai pneumonia study, why did pooling data across all three hospitals improve internal test performance while failing to fix performance on a newly introduced fourth hospital?*
  3. *State Kleinberg’s Impossibility Theorem in your own words, and explain why Northpointe and ProPublica reached opposite conclusions on COMPAS.*
  
  ---