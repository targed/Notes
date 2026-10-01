## 1. Context & The Reproducibility Crisis in Machine Learning (Slide 2)
  
  Slide 2 introduces a harsh reality of contemporary applied machine learning:
  
  $$\text{"It worked on my machine" is not an acceptable engineering outcome.}$$
  
  ```
              The Reproducibility Imperative Across Domains
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Academic Research Dimension:                                           │
  │ An empirical metric (e.g., Accuracy = 94.2%) that cannot be           │
  │ independently reconstructed by an external peer is an unverified       │
  │ hypothesis, not scientific evidence.                                   │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Production Engineering Dimension:                                      │
  │ An irreproducible model pipeline cannot be audited, debugged under     │
  │ production regressions, or certified under regulatory frameworks       │
  │ (e.g., FDA Software as a Medical Device, EU AI Act, Basel IV credit).  │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Root Causes:                                                           │
  │ 1. Randomness (Stochasticity in optimization and data shuffling)       │
  │ 2. Dependencies (Semantic versioning drift and CUDA toolchain shifts)  │
  │ 3. Hidden State (Mutable memory in long-lived interactive kernels)     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The Anatomy of the Scientific Crisis in ML
  In physical sciences (e.g., chemistry or physics), experimental variance stems from environmental noise, instrument calibration, and sample impurities. In computer science, computers are nominally deterministic discrete-state machines. Yet, modern machine learning suffers from a massive **replication crisis**:
  1. **The "Lucky Seed" Phenomenon (Seed Hacking):** Researchers often evaluate tens or hundreds of random seeds and report the performance of the single best outlier run without reporting the variance across trials (Bouthillier et al., 2021).
  2. **Silent Pipeline Leakage:** Undocumented preprocessing choices (e.g., imputing missing values on the full dataset before cross-validation) inflate published benchmarks.
  3. **Hardware & Numerical Inconsistency:** Non-deterministic atomic operations inside GPU kernels (e.g., non-associative floating-point addition) yield different weights across different GPU microarchitectures even with identical source code.
  
  ---
## 2. Deconstructing the ACM Terminology: Repeatable vs. Reproducible vs. Replicable (Slide 3)
  
  Slide 3 cites the formal definitions standardized by the **Association for Computing Machinery (ACM Artifact Review and Badging Version 1.1)**. These three terms are often colloquially treated as synonyms, but in computer science, they occupy distinct operational boundaries:
  
  ```
                      The ACM Scientific Triad (Slide 3)
  ┌────────────────────────────────────────────────────────────────────────┐
  │  REPEATABILITY (Same Team, Same System)                                │
  │  Can the original author run the exact same experimental setup on the  │
  │  exact same machine and obtain the identical numerical output?         │
  │  [Measurement of Internal Operational Determinism]                     │
  │         │                                                              │
  │         ▼                                                              │
  │  REPRODUCIBILITY (Different Team, Same System)                         │
  │  Can an independent evaluator (e.g., TA, peer reviewer, auditor) run  │
  │  the author's codebase and data artifacts and obtain the same numbers? │
  │  [Measurement of Code Artifact Portability & Documentation Rigor]      │
  │         │                                                              │
  │         ▼                                                              │
  │  REPLICABILITY (Different Team, Different System / Implementation)     │
  │  Can an independent group write their own implementation from scratch, │
  │  collect new data, and arrive at the same scientific conclusions?      │
  │  [Measurement of Scientific Generalizability & Algorithmic Truth]      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Formal Comparison of the Triad
  
  | Dimension | **Repeatability** | **Reproducibility** | **Replicability** |
  | :--- | :--- | :--- | :--- |
  | **Experimental Team** | **Same** (You) | **Different** (e.g., Dr. Yu / TA) | **Different** (External Lab / Competitor) |
  | **Code Artifact** | **Same** (Original script) | **Same** (Your submitted notebook) | **New** (Re-implemented from paper description) |
  | **Dataset** | **Same** (Original data) | **Same** (Original data files) | **New / Resampled** (Drawn from population) |
  | **Execution Environment** | **Same** (Your local PC/VM) | **Different** (Standardized Cloud Container) | **Different** (Arbitrary hardware/OS) |
  | **Target Criterion** | Exact numerical match | **Exact numerical match** | Consistent statistical conclusion |
  | **Course Grading Role** | Necessary baseline | **The Primary Grading Standard** | Final project research objective |
  
  ---
### The Reality Gap in Modern Machine Learning
  Slide 3 highlights a major disconnect in published literature:
  > *"Most published ML claims replicability, is evaluated on reproducibility, and delivers repeatability at best."*
  
  ```
                       The ML Literature Reality Gap
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Claimed in Paper:     "Our novel architecture achieves SOTA across     │
  │                        all vision tasks" (Claiming Replicability)      │
  │                                │                                       │
  │ Evaluated in Review:   Reviewers verify open-source code runs on       │
  │                        provided benchmark (Testing Reproducibility)    │
  │                                │                                       │
  │ Delivered in Reality:  Code only runs inside author's bespoke Conda    │
  │                        environment on an A100 GPU without crashing     │
  │                        (Delivering Fragile Repeatability at best)      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. The Grading Contract in CS 5420 (Slide 3)
  
  Dr. Yu explicitly scopes the assessment standard for every assignment in this course:
  $$\mathbf{\text{Reproducibility Standard: } \mathcal{A}_{\text{evaluator}}(\text{Notebook}_{\text{student}}, \text{Data}) \equiv \text{Metric}_{\text{student}}}$$
  
  * **The Grading Mechanism:** The teaching assistant (Jiaqian Zhu) will clone your submitted Colab notebook, spin up a clean virtual container on Google Colab, and execute the runtime end-to-end.
  * **The Failure Condition:** If your report states:
  $$\text{Test Accuracy: } 0.9667$$
  but the evaluator's automated re-run yields:
  $$\text{Test Accuracy: } 0.9000 \quad \text{or throws } \texttt{NameError: name 'x' is not defined}$$
  the submission fails the reproducibility contract, incurring heavy rubric penalties regardless of the theoretical sophistication of the code.
  
  ---
## 4. The Three Sources of Irreproducibility: An Overview (Slide 2)
  
  To design reproducible systems, an ML engineer must systematically isolate and control three independent sources of entropy:
  
  ```
                            The Entropy Triad in ML
                                       │
        ┌──────────────────────────────┼──────────────────────────────┐
        ▼                              ▼                              ▼
  1. Stochasticity               2. Dependencies                3. Hidden State
  • PRNG generators              • Package versions             • Out-of-order execution
  • Weight initialization        • Python ABI compatibility     • Mutated global heap
  • Mini-batch shuffling         • C++/CUDA driver APIs         • Ephemeral disk wipeouts
  • Data augmentation            • Underlying OS libraries      • Memory persistence
  • Dropout sampling               (e.g., glibc, cuDNN)           in interactive kernels
  ```
  
  1. **Stochasticity (Software & Hardware Randomness):** Pseudo-Random Number Generators (PRNGs) exist across multiple decoupled runtime layers (Python standard library, NumPy, PyTorch CPU, PyTorch CUDA). Failing to seed even one engine introduces run-to-run drift.
  2. **Dependencies (Environmental Drift):** Relying on unpinned package installations (`pip install scikit-learn`) pulls the bleeding-edge release. API signatures change, default hyperparameter values mutate (e.g., default regularizers), and algorithmic implementations are updated.
  3. **Hidden State (Architectural Memory Traps):** The interactive nature of notebooks allows execution counters to decouple from visual top-to-bottom layout, leaving behind residual objects in the Python heap that do not exist in the serialized source file.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Under the official ACM terminology, what is the precise distinction between a pipeline that is "repeatable" versus one that is "reproducible"?*
  2. *Why is claiming algorithmic "replicability" in a research paper fundamentally bolder than demonstrating "reproducibility"?*
  3. *If you run a notebook five times consecutively in the same session without restarting the kernel and obtain the exact same accuracy every time, have you demonstrated repeatability, reproducibility, or neither? Why?*
  
  ---