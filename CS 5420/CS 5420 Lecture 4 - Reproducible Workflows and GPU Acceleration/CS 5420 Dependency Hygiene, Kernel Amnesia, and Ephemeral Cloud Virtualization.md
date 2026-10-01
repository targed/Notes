## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Why Unshuffled Data Causes Optimization Failure:**
  * Mini-batch Stochastic Gradient Descent relies on the statistical assumption that each batch provides an **unbiased estimate of the population gradient**:
   $$\mathbb{E}_{\mathcal{B}} \left[ \nabla_\theta \mathcal{L}_{\mathcal{B}}(\theta) \right] = \nabla_\theta J(\theta)$$
  * When data is ordered by class label (e.g., all Class 0 instances, followed by Class 1, then Class 2), an unshuffled data loader feeds batches consisting of a single target. The parameter updates oscillate wildly between the class extremes, causing gradient thrashing and **catastrophic forgetting** of earlier classes. Shuffling breaks temporal autocorrelation between consecutive updates.
  2. **Why `np.random.seed(42)` Does Not Seed PyTorch Dropout:**
  * Python, NumPy, and PyTorch maintain completely separate, isolated Pseudo-Random Number Generator (PRNG) state vectors in memory. 
  * PyTorch executes Dropout masks using its own internal C++/CUDA PRNG engine (Philox algorithms on GPU or the ATen generator on CPU). NumPy's global seed mutates only `numpy.random`. PyTorch remains unseeded and pulls entropy non-deterministically from the host operating system.
  3. **Why Floating-Point Non-Associativity Breaks GPU Determinism:**
  * Under the IEEE 754 floating-point standard, round-off truncation makes addition mathematically non-associative:
   $$(a + b) + c \neq a + (b + c)$$
  * On highly parallel GPU architectures, hundreds of threads write gradient accumulations back to memory using atomic addition operations (`atomicAdd`). The exact chronological arrival order of thread writes is non-deterministic, governed by dynamic hardware latencies and streaming multiprocessor (SM) scheduling. 
  * Summing identical numbers in different arbitrary orders yields slightly different floating-point results, accumulating numerical divergence across hundreds of training epochs.
  
  ---
## 2. Source 2: Dependency Drift & Semantic Versioning Traps (Slide 7)
  
  Slide 7 addresses the second primary source of pipeline breakdown:
  
  $$\text{"pip install scikit-learn installs whatever is newest today, not what you used."}$$
  
  ```
                      The Dependency Drift Timeline
    September 2026 (Author Builds Code)           November 2026 (TA Evaluates Code)
  ┌────────────────────────────────────────┐     ┌────────────────────────────────────────┐
  │ Command: pip install scikit-learn      │     │ Command: pip install scikit-learn      │
  │ Installed: scikit-learn v1.5.0         │     │ Installed: scikit-learn v1.6.2 (New!)  │
  │                                        │     │                                        │
  │ Default Solver: 'lbfgs'                │     │ Default Solver Changed / Tolerances Cut│
  │ Behavior: Converges in 42 iterations   │     │ Behavior: Optimization Diverges / Warns│
  │ Metric: Accuracy = 91.4%               │     │ Metric: Accuracy = 84.1% (Regressed!)  │
  └────────────────────────────────────────┘     └────────────────────────────────────────┘
  ```
### A. The Fragility of Semantic Versioning (SemVer) in ML
  Software packages nominally adhere to Semantic Versioning (`MAJOR.MINOR.PATCH`):
  * `MAJOR`: Incompatible API breaking changes.
  * `MINOR`: Backward-compatible new functionality.
  * `PATCH`: Backward-compatible bug fixes.
  
  **Why ML Libraries Break Even on Minor or Patch Updates:**
  1. **Default Hyperparameter Drift:** Libraries frequently update default arguments between releases (e.g., changing regularizer penalties, default convergence tolerances like `tol=1e-4` to `1e-5`, or swapping default linear solvers).
  2. **Algorithmic Refactoring:** A patch update may optimize a numerical solver using a different underlying BLAS/LAPACK routine, altering the trajectory of iterative solvers.
  3. **API Deprecations:** Function signatures or keyword arguments are removed, transforming working code into fatal runtime exceptions.
  
  ---
### B. Pinning Strategies: Requirements Files vs. Strict Lockfiles
  Slide 7 notes: *"For real projects: `requirements.txt` or an environment lockfile."*
  
  ```
                           Dependency Pinning Tiers
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Tier 1: Unpinned (Anti-Pattern / High Risk)                            │
  │   scikit-learn                                                         │
  │   ──▶ Pulls whatever version is newest at execution time.              │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Tier 2: Pinned Requirements (Standard Academic Baseline)               │
  │   scikit-learn == 1.5.0                                                │
  │   torch == 2.2.1                                                       │
  │   ──▶ Guarantees primary library versions, but transitive dependencies │
  │       (e.g., SciPy, Joblib) can still drift.                           │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Tier 3: Deterministic Lockfiles (Production MLOps Baseline)             │
  │   pip freeze > requirements.lock  OR  poetry.lock / conda-lock         │
  │   ──▶ Records an exact cryptographic hash and version for every        │
  │       transitive package down to the C-extension bindings.             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Operational Dependency Hygiene in Colab (Slide 8)
  
  Slide 8 details practical command-line techniques for managing dependencies inside Google Colab notebooks:
  
  ```bash
  !pip list | grep -i scikit
  pip install -r requirements.txt
  ```
  
  ```
                   The Colab Silent Image Update Hazard
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Google Colab runtimes are NOT static frozen environments!              │
  │ Google updates the preinstalled software stack across all Colab        │
  │ container images on a rolling schedule without prior announcement.     │
  │                                                                        │
  │ • A notebook that executes successfully on Colab in September may fail │
  │   in November due to an underlying image update by Google engineers.   │
  │ • DEFENSE: Always print explicit package versions in the notebook      │
  │   stdout so exported submission PDFs maintain immutable evidence.      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The In-Memory Module Cache Trap (`sys.modules`)
  Slide 8 emphasizes a common pitfall:
  $$\text{"After !pip install of a different version, restart the runtime before importing, or you get the old one."}$$
  
  ```
                        The sys.modules Execution Trap
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Step 1: User runs 'import sklearn'                                     │
  │         Python loads scikit-learn v1.4 from disk into process RAM and  │
  │         registers it in the global module cache: sys.modules['sklearn']│
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 2: User runs '!pip install scikit-learn==1.5.0'                   │
  │         Pip downloads and overwrites files on the container disk.      │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 3: User runs 'import sklearn' AGAIN                               │
  │         Python checks sys.modules, sees 'sklearn' is already mapped to │
  │         RAM, and skips loading from disk.                              │
  │         RESULT: The active session STILL executes the old v1.4 code!   │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 4: RESOLUTION: Runtime ──▶ Restart Runtime                        │
  │         Kills the Python interpreter process, flushing sys.modules.    │
  │         Re-importing now loads the new v1.5.0 build from disk.         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Source 3: Hidden State & Kernel Persistence (Slide 9)
  
  Slide 9 revisits the fundamental challenge of interactive computing:
  
  $$\text{"The kernel remembers what the file does not. The only reliable check is Restart and run all."}$$
  
  ```
                   The Serialization Gap (Slide 9)
  ┌─────────────────────────────────┐     ┌─────────────────────────────────┐
  │     Notebook File (.ipynb)      │     │      Active Kernel Memory       │
  │  (Static Serialized JSON Text)  │     │   (Dynamic Heap / Symbol Table) │
  ├─────────────────────────────────┤     ├─────────────────────────────────┤
  │ • Contains visible code cells   │ ≠   │ • Contains variables from       │
  │ • Sliced and ordered visually   │     │   deleted cells                 │
  │ • What the grader/evaluator sees│     │ • Contains out-of-order state   │
  │                                 │     │ • What your local browser sees  │
  └─────────────────────────────────┘     └─────────────────────────────────┘
  ```
### The Golden Rule of Submission
  Before submitting any notebook for grading in CS 5420:
  1. Navigate to: `Runtime` $\to$ `Restart and run all`.
  2. Confirm that every cell evaluates sequentially without raising an unhandled exception.
  3. Verify that the execution counters run monotonically: `[1], [2], [3], \dots, [K]`.
  
  ---
## 5. Ephemeral Virtualization: Colab Forgets Everything (Slide 10)
  
  Slide 10 details the cloud resource constraints governing Google Colab:
  
  $$\text{"Plan for the disconnect, not for the documentation."}$$
  
  ```
                     Colab Lifecycle & Resource Boundaries
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Constraint                    Operational Mechanism & Impact           │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Ephemeral Local Storage       The container filesystem (/content) is   │
  │                               completely wiped on session termination. │
  │                               Checkpoints, models, and data vanish.    │
  ├────────────────────────────────────────────────────────────────────────┤
  │ The 90-Minute Idle Timeout    The connection drops after ~90 minutes of│
  │ (User Interaction Trap)       zero BROWSER interaction. Code execution │
  │                               does NOT prevent this timeout if your    │
  │                               browser tab is closed or asleep.         │
  ├────────────────────────────────────────────────────────────────────────┤
  │ The 12-Hour Hard Limit        Regardless of user activity, a free-tier │
  │                               session is forcibly terminated at 12 hrs.│
  ├────────────────────────────────────────────────────────────────────────┤
  │ Dynamic GPU Provisioning      GPUs are provisioned on demand. If GPU   │
  │                               clusters face heavy load, free accounts  │
  │                               are silently throttled or limited to CPU.│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Production Defense: Persistent Checkpointing via Google Drive
  Because local storage on the virtual machine (`/content`) is ephemeral, training long-running models (e.g., deep networks in Modules 10–13) requires piping weights directly to persistent cloud storage:
  
  ```python
  import os
  from google.colab import drive
  import torch
  
  # 1. Mount Persistent Google Drive Storage
  drive.mount('/content/drive')
  
  # 2. Establish Dedicated Course Directory on Drive
  CHECKPOINT_DIR = '/content/drive/MyDrive/CS5420/checkpoints'
  os.makedirs(CHECKPOINT_DIR, exist_ok=True)
  
  # 3. Save Model State Dictionaries During Training Loops
  def save_checkpoint(model, optimizer, epoch, loss, filename="model_checkpoint.pt"):
    checkpoint_path = os.path.join(CHECKPOINT_DIR, filename)
    torch.save({
        'epoch': epoch,
        'model_state_dict': model.state_dict(),
        'optimizer_state_dict': optimizer.state_dict(),
        'loss': loss,
    }, checkpoint_path)
    print(f"Durable checkpoint successfully committed to Drive: {checkpoint_path}")
  ```
  
  If your Colab session severs unexpectedly due to an idle timeout or hardware preemption, your training progress is safely committed to Google Drive rather than lost in an ephemeral container wipe.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does executing `!pip install scikit-learn==1.5.0` fail to update the active library version if an `import sklearn` statement was already executed in a previous cell?*
  2. *Explain the distinction between Colab's 90-minute "idle timeout" and its 12-hour "maximum session limit." Why does running an active training loop fail to prevent the 90-minute disconnect if you close your laptop lid?*
  3. *Why is printing package versions (e.g., `sklearn.__version__`) directly into the notebook output considered essential scientific evidence when exporting assignments to PDF?*
  
  ---