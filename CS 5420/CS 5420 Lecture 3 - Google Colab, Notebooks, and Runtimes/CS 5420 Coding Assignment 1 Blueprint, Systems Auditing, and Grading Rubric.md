## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why `load_iris(return_X_y=True)` Returns $X$ as 2D `(150, 4)` and $y$ as 1D `(150,)`:**
  * In `scikit-learn`, design matrices must be 2-dimensional tensors of shape `(n_samples, n_features)`. 
  * Targets for single-output supervised tasks are expected to be 1-dimensional vectors of shape `(n_samples,)`. If $y$ were formatted as a 2D column vector `(150, 1)`, passing it to downstream models would trigger an explicit `DataConversionWarning: A column-vector y was passed when a 1d array was expected` and could cause unintended broadcasting errors during loss or residual computations.
  2. **The Risk of Omitting `random_state` in `train_test_split`:**
  * Without a fixed seed, the pseudorandom number generator pulls entropy non-deterministically from the host OS (e.g., system clock, `/dev/urandom`). 
  * Every execution shuffles the data into different train/test splits. Consequently, evaluation scores, decision tree splits, and loss trajectories fluctuate on every run. In automated grading pipelines, this non-determinism breaks test assertions that check for exact metric thresholds.
  3. **Hyperparameters vs. Fitted Parameters:**
  * **Hyperparameters** (`n_estimators=100`, `max_depth=None`, `random_state=42`) are architectural configuration choices set by the engineer **prior to training**. They define the structure of the hypothesis class $\mathcal{H}$ and the optimization behavior.
  * **Fitted Parameters** (the split features, numerical thresholds, tree depths, and leaf-node class counts stored in `model.estimators_`) are learned **from data** during `.fit()` via empirical risk minimization.
  
  ---
## 2. Deconstructing Coding Assignment 1 (CA1) (Slide 19)
  
  Slide 19 marks the release of the course's first hands-on engineering milestone:
  
  ```
                  Coding Assignment 1 (CA1) Specifications
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Title:        Colab, Environment Setup, and Reproducibility            │
  │ Value:        100 Points (Contributes ~5.71% to overall course grade)  │
  │ Due Date:     Friday, September 11 at 11:59 PM                         │
  │ Format:       Single, fully reproducible Google Colab Notebook (.ipynb)│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 19 divides CA1 into **five distinct components**:
  
  ```
                              The 5 Parts of CA1
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Part 1: Environment Report     Audit OS, CPU, RAM, Disk & GPU specs    │
  │        │                                                               │
  │        ▼                                                               │
  │ Part 2: GPU Timing Benchmark   Quantify CPU vs. GPU matrix throughput  │
  │        │                                                               │
  │        ▼                                                               │
  │ Part 3: Reproducibility Audit  Eliminate hidden state; linear order    │
  │        │                                                               │
  │        ▼                                                               │
  │ Part 4: Smoke-Test Pipeline    End-to-end tabular model verification   │
  │        │                                                               │
  │        ▼                                                               │
  │ Part 5: Technical Narrative    Markdown report & systems reflection    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Deep Dive: Implementing Parts 1 & 2 ("Finish Tonight")
  
  Slide 19 explicitly notes: *"Parts 1 and 2 you can finish tonight from what we did in class."* Here is how those components are designed and executed from an ML systems perspective.
### A. Part 1: The Systems Environment Report
  This section queries the Linux virtual container using the shell commands introduced in Slide 11:
  
  ```python
  # Part 1: System Hardware and Software Audit Script
  import platform
  import torch
  
  print("=== OPERATING SYSTEM & RUNTIME ===")
  print(f"Python Version : {platform.python_version()}")
  print(f"PyTorch Version: {torch.__version__}")
  print(f"CUDA Available : {torch.cuda.is_available()}")
  
  if torch.cuda.is_available():
    print(f"Active GPU     : {torch.cuda.get_device_name(0)}")
    print(f"Device Count   : {torch.cuda.device_count()}")
  else:
    print("Active GPU     : None (Running on CPU)")
  
  # Query OS Pseudo-Filesystems and Utilities via Ephemeral Subshells
  print("\n=== CPU SPECIFICATIONS ===")
  !cat /proc/cpuinfo | grep 'model name' | head -n 1
  !cat /proc/cpuinfo | grep 'cpu cores' | head -n 1
  
  print("\n=== SYSTEM MEMORY (RAM) ===")
  !free -h
  
  print("\n=== STORAGE FILESYSTEM ===")
  !df -h /content
  
  print("\n=== GPU DRIVER & HARDWARE STATUS ===")
  !nvidia-smi
  ```
  
  ---
### B. Part 2: CPU vs. GPU Matrix Multiplication Benchmark
  Part 2 requires empirical timing of large-scale tensor computations on CPU vs. GPU.
#### The CUDA Asynchronous Execution Trap:
  A frequent mistake in GPU benchmarking is failing to account for **asynchronous kernel dispatch**. In PyTorch, GPU operations (like `torch.matmul`) are dispatched to the CUDA stream asynchronously; control returns to the Python CPU thread immediately while the GPU is still running the computation.
  
  $$\text{If you benchmark without synchronization, you measure CPU dispatch time, not GPU execution time!}$$
  
  To capture true hardware execution latency, you must enforce a hardware synchronization barrier using `torch.cuda.synchronize()`:
  
  ```python
  # Part 2: Rigorous Hardware Latency Benchmarking
  import time
  import torch
  
  # Define Large Dense Matrix Dimensions (N x N)
  MATRIX_DIM = 5000
  
  print(f"Allocating {MATRIX_DIM}x{MATRIX_DIM} FP32 Matrices...")
  
  # 1. CPU Benchmark
  A_cpu = torch.randn(MATRIX_DIM, MATRIX_DIM, dtype=torch.float32)
  B_cpu = torch.randn(MATRIX_DIM, MATRIX_DIM, dtype=torch.float32)
  
  start_cpu = time.perf_counter()
  C_cpu = torch.matmul(A_cpu, B_cpu)
  end_cpu = time.perf_counter()
  cpu_duration = end_cpu - start_cpu
  print(f"CPU Matrix Multiplication Time : {cpu_duration:.4f} seconds")
  
  # 2. GPU Benchmark (Requires Active CUDA Runtime)
  if torch.cuda.is_available():
    # Allocate directly on GPU device memory (VRAM)
    A_gpu = torch.randn(MATRIX_DIM, MATRIX_DIM, dtype=torch.float32, device="cuda")
    B_gpu = torch.randn(MATRIX_DIM, MATRIX_DIM, dtype=torch.float32, device="cuda")
  
    # Warm-up pass to initialize CUDA context and memory pools
    _ = torch.matmul(A_gpu, B_gpu)
    torch.cuda.synchronize()
  
    # Timed run
    start_gpu = time.perf_counter()
    C_gpu = torch.matmul(A_gpu, B_gpu)
    torch.cuda.synchronize()  # BLOCK CPU until GPU kernel finishes
    end_gpu = time.perf_counter()
  
    gpu_duration = end_gpu - start_gpu
    speedup = cpu_duration / gpu_duration
    print(f"GPU Matrix Multiplication Time : {gpu_duration:.4f} seconds")
    print(f"Hardware Compute Acceleration   : {speedup:.2f}x Speedup")
  else:
    print("GPU acceleration unavailable. Toggle Runtime -> T4 GPU.")
  ```
  
  ---
## 4. Parts 3, 4, and 5: Reproducibility, Smoke-Testing & Reporting
  
  ```
                        CA1 Completion Architecture
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Part 3: Reproducibility Verification                                   │
  │  • Enforce global deterministic seed initialization:                   │
  │    import random; random.seed(42)                                      │
  │    import numpy as np; np.random.seed(42)                              │
  │    torch.manual_seed(42)                                               │
  │  • Verify all notebook execution counters follow monotonic order:      │
  │    [1] ──▶ [2] ──▶ [3] ──▶ [4] ──▶ ... (Zero out-of-order executions) │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Part 4: Smoke-Test ML Model Pipeline                                   │
  │  • Implement the complete 5-stage pipeline from Slides 13–18:          │
  │    1. Ingestion: load_iris(return_X_y=True)                            │
  │    2. EDA: Shape assertion checks (assert X.shape == (150, 4))         │
  │    3. Partition: train_test_split(..., test_size=0.2, random_state=42) │
  │    4. Estimator: RandomForestClassifier(random_state=42).fit(...)      │
  │    5. Metric: Assert model.score(X_test, y_test) >= 0.90               │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Part 5: Technical Documentation & Systems Reflection                   │
  │  • Formatted Markdown cells explaining:                                │
  │    - Why the GPU achieves order-of-magnitude acceleration on GEMM      │
  │      operations, but would not speed up tabular tree building.         │
  │    - Confirmation that the notebook was validated via                  │
  │      "Runtime -> Restart and run all".                                 │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Strategic Rubric Analysis & The Four Autograder Traps
  
  Slide 19 emphasizes: *"Rubric is posted with the assignment — read it before you start, not after."* In automated grading workflows, simple oversights can cause disproportionate grade loss:
  
  ```
                            The Four Autograder Traps
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Trap 1: The Hidden State / Out-of-Order Trap                                           │
  │  • Description: Writing code that relies on a variable defined in a deleted cell.       │
  │  • Result: Autograder runs cleanly from top-to-bottom and throws a NameError.          │
  │  • Penalty: Immediate 0 credit on that functional block.                               │
  │  • Prevention: Always run "Runtime -> Restart and run all" as your final check.        │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Trap 2: Hardcoded Local Paths                                                          │
  │  • Description: Using absolute local paths (e.g., C:\Users\name\Desktop\data.csv).    │
  │  • Result: Fails in the cloud container with FileNotFoundError.                        │
  │  • Prevention: Use relative paths or load directly from scikit-learn/Hugging Face APIs.│
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Trap 3: Unseeded Stochastic Calls                                                      │
  │  • Description: Omitting random_state=42 in train_test_split or RandomForest.          │
  │  • Result: Numerical outputs drift; autograder test assertions fail.                   │
  │  • Prevention: Hardcode random_state=42 across all probabilistic initializations.      │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Trap 4: Asynchronous Timing Measurement                                                │
  │  • Description: Omitting torch.cuda.synchronize() during GPU benchmarking.             │
  │  • Result: Reports false GPU timings (~0.0001s) reflecting CPU dispatch latency.       │
  │  • Prevention: Enforce synchronization barriers immediately before and after timers.   │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 6. Action Items & Timeline (Slide 20)
  
  Slide 20 outlines the concrete deliverables before the next lecture:
  1. **Immediate Execution:** Open a new Google Colab session and complete **Parts 1 and 2** while the shell and PyTorch commands are fresh.
  2. **Review the Grading Rubric:** Read the CA1 rubric on Canvas to understand point allocations across environment reporting, benchmark correctness, pipeline execution, and documentation.
  3. **Submission Readiness:** Plan to complete Parts 3–5 ahead of the **Friday, September 11, 11:59 PM** deadline to reserve time for troubleshooting and office hours with the graduate TA (Jiaqian Zhu).
  
  ---
## Complete Lecture 3 Synthesis Reference
  
  | Architectural Concept | Key Systems Mechanism | Production / Grading Impact |
  | :--- | :--- | :--- |
  | **Notebook Model** | Serialized JSON (`.ipynb`) + persistent IPython kernel process | Deleting a cell does not wipe variables from RAM; creates the hidden-state trap. |
  | **Execution Counter** | `In [n]` monotonic bracket counter | Exposes out-of-order execution history; autograders enforce strict linear order. |
  | **Reproducibility Test**| `Runtime -> Restart and run all` | Mandatory submission protocol across all 7 coding assignments. |
  | **Compute Partition** | CPU for sequential/branching logic; GPU for dense GEMM math | PCIe bandwidth overhead makes GPUs inefficient for small tabular models. |
  | **Subshell Escaping** | `!` spawns child `/bin/bash` subshell | Directory changes (`!cd`) do not persist; use `%cd` magic commands instead. |
  | **CUDA Benchmarking** | `torch.cuda.synchronize()` | Asynchronous kernel execution requires explicit barriers for valid latency profiling. |
  | **Scikit-Learn Pipeline**| 5 stages: Load $\to$ Explore $\to$ Split $\to$ Fit $\to$ Score | Standardized API pattern that ensures leak-free training and reproducible evaluation. |
  
  ---