## 1. Quick Review: Answers to Section 4 Checkpoints
  
  1. **Why Manual Scaling Outside the Cross-Validation Loop Causes Leakage:**
  * If `scaler.fit(X)` runs before the cross-validation loop, the scaling parameters ($\mu, \sigma$) are computed across all samples. When the dataset is subsequently partitioned into $K$ folds, the training split in each iteration contains information about the mean and variance of the validation fold. 
  * Wrapping the scaler and estimator inside an `sklearn.pipeline.Pipeline` guarantees that `.fit()` is invoked strictly on the training fold during every cross-validation split, with `.transform()` applied to the out-of-fold validation data.
  2. **The Deception of 96.2% Accuracy on Imbalanced Data:**
  * If a fraud dataset contains $98\%$ non-fraudulent transactions and $2\%$ fraudulent transactions, a naive `DummyClassifier(strategy='most_frequent')` that predicts "non-fraud" for every record achieves **$98.0\%$ accuracy** while detecting $0\%$ of actual fraud cases.
  * An accuracy of $96.2\%$ is actually **underperforming the trivial majority baseline** by $1.8\%$. For imbalanced distributions, accuracy must be replaced by metrics sensitive to the minority class: Precision-Recall AUC (PR-AUC), F1-score, and cost-weighted utility matrices.
  3. **Cross-Validation Tuning vs. Test-Set Snooping:**
  * Tuning hyperparameters on the training set using $K$-Fold cross-validation optimizes model parameters against out-of-fold validation splits while leaving the test set untouched.
  * If you instead adjust hyperparameters (e.g., tree depth, learning rate) based on test-set performance, the test set ceases to be an independent out-of-sample proxy. You commit **meta-overfitting** (information leakage through the human loop), producing an overly optimistic estimate of true generalization error.
  
  ---
## 2. Dr. Yu's AI Tool Policy: The Professional-Integrity Balance (Slide 20)
  
  Slide 20 introduces an institutional policy regarding Generative AI (LLMs, Copilot, Cursor, Claude Code, etc.):
  
  ```
                     The AI Tool Dual-Track Policy
  ┌────────────────────────────────────────────────────────────────────────┐
  │  Coding Assignments & Group Project  ──▶  PERMITTED (Under 3 Rules)    │
  │  In-Semester Exams (Exams 1, 2, 3)   ──▶  STRICTLY PROHIBITED (Closed) │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The Three Mandatory Conditions for Assignments & Projects:
  1. **Explicit Disclosure:**
   * You must state which AI tools were used, which sections of the code or pipeline they assisted with, and the nature of the prompts used.
  2. **Line-by-Line Verification:**
   * You are personally responsible for the syntactic, algorithmic, and numerical correctness of every generated line. AI hallucinations (e.g., calling deprecated `sklearn` parameters or silently swapping matrix axes) are graded as student errors.
  3. **The Oral Defense Rule (The Integrity Trap):**
   * *“Code you cannot explain is an academic integrity violation.”*
   * If the TA or Dr. Yu asks you to explain the tensor operations, vector-Jacobian products, or data flow of a submitted script during office hours and you cannot walk through it, the submission is treated as an academic dishonesty infraction.
### The Strategic Rationale & Exam Risk:
  * The policy mirrors modern engineering workflows: industry developers use AI assistance, but senior engineers are accountable for runtime failures and system correctness.
  * **The Exam Trap:** Because all three exams (worth **40%** of your grade) are strictly **closed-book and closed-device**, relying on AI as a crutch during weekly coding assignments will leave you unprepared for handwritten or terminal-based exam questions covering derivations, algorithmic tracing, and parameter debugging.
  
  ---
## 3. Classroom Seminar Presentations & Academic Value (Slide 19)
  
  Slide 19 outlines student-led technical presentations on applied ML tools:
  * **Presentation Topics:** PyTorch and AI Agents.
  * **Course Credit & Research Visibility:** Dr. Yu encourages students to present either these core technologies or their own ML research work.
  * **Teaching Assistant Contact:** Jiaqian Zhu (`jzbmn@mst.edu`).
  
  ```
                    Bridging Class Topics to Research
  ┌────────────────────────────────────────────────────────────────────────┐
  │ PyTorch Presentations    ──▶ Foundational autograd, tensor ops, GPU    │
  │                              acceleration (Modules 9–11).              │
  │                                                                        │
  │ AI Agents Presentations  ──▶ Closed-loop execution harnesses, POMDPs,  │
  │                              MCP, constrained decoding (Module 14).    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### Connecting Agent Topics to Dr. Yu's Evaluation Criteria:
  When delivering a technical presentation on **AI Agents** in this course, align it directly with Dr. Yu’s recurring themes from Slide 3:
  1. **What models assume:** Autoregressive models assume the next-token distribution can be modeled statically; agents assume a closed execution loop over a Partially Observable Markov Decision Process (POMDP).
  2. **Where they fail:** Multi-step compounding errors ($P(\text{Success}) = \prod p_k$), context pollution, and syntax crashes.
  3. **How to evaluate honestly:** Moving away from static memorization benchmarks toward dynamic execution environments like SWE-bench Verified and SWE-bench Pro.
  
  ---
## 4. Immediate Onboarding & Action Items (Slides 21–22)
  
  The introductory lecture concludes with two administrative tasks:
### 1. Google Account & Colab Environment Initialization (Slide 21)
  * **Action:** Ensure you have an active Google account and verify access to [Google Colaboratory](https://colab.research.google.com).
  * **Technical Best Practices for Graduate Work:**
  * **Runtime Allocation:** Learn to toggle hardware accelerators: `Runtime` $\to$ `Change runtime type` $\to$ `T4 GPU` (free tier) or higher for Module 11 deep learning pipelines.
  * **Drive Mounting:** Use persistent storage for larger datasets and model checkpoints rather than ephemeral Colab disk storage:
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```
  * **Environment Pinning:** Track library versions (`torch.__version__`, `sklearn.__version__`) to ensure reproducible evaluation across execution sessions.
### 2. Student Background Survey (Slide 22)
  * **URL:** `https://forms.gle/FPuDXkw3Rno9rLNk8`
  * **Purpose:** Profiles incoming graduate student backgrounds across mathematical preparation (calculus, linear algebra, probability), programming experience, research domains, and project interests to help calibrate in-class instruction.
  
  ---
## Complete Lecture 1 Synthesis Reference
  
  | Domain | Key Policy / Architectural Rule | Strategic Impact |
  | :--- | :--- | :--- |
  | **Course Thesis** | "What models assume, where they fail, how to evaluate honestly" | Focus on inductive bias, failure pathologies, and leak-free validation over raw accuracy. |
  | **Tool Stack** | NumPy $\to$ Pandas $\to$ scikit-learn $\to$ PyTorch $\to$ Hugging Face | Progression from contiguous arrays to tabular frames, classical pipelines, GPU autograd, and transformers. |
  | **Grade Leverage** | Assignments: $40\%$ (7 tasks) \| Exams: $40\%$ (3 exams) \| Project: $20\%$ | Exams carry $2.33\times$ the weight of assignments. Missing an exam eliminates the possibility of an "A". |
  | **Rounding Policy** | Strict fixed scale ($90.0\%$ A, $80.0\%$ B). **No rounding.** | Attendance sign-ins provide additive bonus points that protect against borderline non-rounded grades. |
  | **Late Penalties** | $-10\%$ per 24 hours up to 72 hours; zero credit thereafter | Worth using for significant quality improvements within 24 hours; rarely advantageous past 24 hours. |
  | **Group Project** | Teams of 2–4; real public data; scikit-learn `Pipeline`; 3 model families | Test set evaluated strictly once. Preprocessing must be contained within `Pipeline` to prevent leakage. |
  | **AI Usage** | Allowed on assignments/project with disclosure; prohibited on exams | You must be able to explain all submitted code line-by-line upon request. |
  
  ---