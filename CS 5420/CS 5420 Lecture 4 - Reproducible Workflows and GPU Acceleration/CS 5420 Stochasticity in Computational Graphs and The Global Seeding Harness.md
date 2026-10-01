## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Repeatability vs. Reproducibility under ACM Definitions:**
  * **Repeatability:** The original research team executes their original codebase and datasets on their original local hardware setup and obtains the identical numerical result. It measures internal operational consistency.
  * **Reproducibility:** An independent third party (e.g., a TA or peer reviewer) takes the original codebase and data artifacts, executes them on an independent or standardized platform (e.g., a fresh cloud container), and obtains the exact same numerical result. It measures software portability, dependency stability, and documentation integrity.
  2. **Why Replicability Is a Bolder Claim:**
  * Replicability demands that an independent team write a *new, separate implementation* or collect *new, out-of-sample data* and still arrive at the same qualitative scientific conclusion. 
  * It tests whether the underlying algorithmic inductive bias is true across different software frameworks and data distributions, rather than simply proving that a specific script executes without crashing.
  3. **Running Five Times Consecutively Without Restarting:**
  * This demonstrates **neither** valid reproducibility nor rigorous repeatability. 
  * Consecutive executions inside a persistent, un-restarted kernel operate over an already mutated memory heap (`globals()`). You are merely verifying that the cached state in RAM does not change between cell runs. True repeatability requires terminating the process, clearing memory, and re-executing from an uninitialized state.
  
  ---
## 2. Source 1: The Manifestation of Randomness in Machine Learning (Slide 4)
  
  Slide 4 highlights how stochasticity is embedded across the end-to-end machine learning pipeline:
  
  $$\text{"Random appears in: weight init, data shuffling, train/test split, augmentation, etc."}$$
  
  ```
                Stochastic Junctions in the ML Pipeline (Slide 4)
  ┌────────────────────────────────────────────────────────────────────────┐
  │  1. Weight Initialization:  W^[l] ~ 𝒩(0, 2/n_in) [He/Kaiming Normal]   │
  │         │                                                              │
  │         ▼                                                              │
  │  2. Data Partitioning:      train_test_split(..., shuffle=True)        │
  │         │                                                              │
  │         ▼                                                              │
  │  3. Data Augmentation:      RandomCrop, ColorJitter, RandomAffine      │
  │         │                                                              │
  │         ▼                                                              │
  │  4. Mini-Batch Shuffling:   DataLoader(dataset, shuffle=True)          │
  │         │                                                              │
  │         ▼                                                              │
  │  5. Stochastic Dropping:    Dropout Mask m ~ Bernoulli(1 - p)          │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Deep Dive into the Slide 4 Case Studies
#### A. Weight Initialization & Symmetry Breaking
  * **The Mathematical Necessity:** If weights in an artificial neural network are initialized to a constant (e.g., all zeros or all ones), every hidden unit in a layer computes the identical activation:
  $$a_1^{[1]} = a_2^{[1]} = \dots = a_k^{[1]} = \sigma(0 \cdot x + b)$$
  * Consequently, during backpropagation, all neurons receive identical gradients ($\nabla_{W_{ij}} \mathcal{L}$), causing them to update identically across all epochs.
  * To break this symmetry, weights must be sampled from continuous random distributions (e.g., Xavier/Glorot or He/Kaiming initialization).
  * **The Reproducibility Problem:** If the seed governing this random initialization is omitted, every training run starts at a different point in the non-convex loss landscape, converging to different local minima with distinct generalization performance.
#### B. Mini-Batch Shuffling: `unshuffled_data.py` vs. `shuffled_data.py`
  Slide 4 contrasts two identical networks trained with and without dataset shuffling:
  
  ```
        Unshuffled Data Training                       Shuffled Data Training
  ┌─────────────────────────────────────┐       ┌─────────────────────────────────────┐
  │ Epoch [100/100]:                    │       │ Epoch [100/100]:                    │
  │   Loss:     116.14                  │       │   Loss:     0.00                    │
  │   Accuracy: 61.91%                  │       │   Accuracy: 100.00%                 │
  │                                     │       │                                     │
  │ Verdict: FAILS TO CONVERGE          │       │ Verdict: CONVERGES SEAMLESSLY       │
  └─────────────────────────────────────┘       └─────────────────────────────────────┘
  ```
  
  * **Why Unshuffled Data Fails:** 
  * Mini-batch Stochastic Gradient Descent assumes that the sample gradient $\nabla_\theta \mathcal{L}_{\mathcal{B}}(\theta)$ is an **unbiased estimator** of the full population gradient:
    $$\mathbb{E}_{\mathcal{B}} \Big[ \nabla_\theta \mathcal{L}_{\mathcal{B}}(\theta) \Big] = \nabla_\theta J(\theta)$$
  * If a dataset is ordered by class (e.g., all Class 0 samples, followed by Class 1, then Class 2), an unshuffled data loader feeds batches consisting entirely of a single label.
  * The parameter vector $\theta$ oscillates wildly, over-correcting toward the class in the current batch while experiencing **catastrophic forgetting** on preceding classes.
  * Shuffling decorrelates consecutive updates, ensuring that each mini-batch approximates the true data-generating distribution.
#### C. Stochastic Regularization (Dropout)
  * During forward passes, Dropout samples an element-wise binary mask $m \in \{0, 1\}^d$ where each component is drawn independently from a Bernoulli distribution:
  $$m_j \sim \text{Bernoulli}(1 - p), \quad \tilde{h} = \frac{1}{1 - p} (h \odot m)$$
  * An unseeded random engine deactivates completely different sub-networks on every epoch across different runs, introducing high run-to-run metric variance.
  
  ---
## 3. The Decoupled PRNG Architecture (Slide 5)
  
  Slide 5 addresses a widespread misconception in computational data science:
  $$\text{"Seeding only NumPy is the most common half-measure — it does not cover PyTorch."}$$
### The Reality of Multiple Isolated PRNG Engines
  A Pseudorandom Number Generator (PRNG) is a deterministic mathematical recurrence relation. Given an initial internal state vector (the **seed** $s_0$), it produces a deterministic sequence of numbers that satisfy statistical tests for randomness:
  
  $$s_{t+1} = g(s_t), \quad u_t = h(s_t)$$
  
  In a modern Python machine learning environment, there is **no single unified random generator**. Instead, several decoupled, independent engines operate in isolated memory spaces:
  
  ```
                  The Isolated PRNG Ecosystem in Python
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Python Built-in Generator: random.seed(s)                           │
  │    • Implements the Mersenne Twister (MT19937) algorithm in C.         │
  │    • Used by native Python scripts, random.shuffle(), and standard lib.│
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. NumPy PRNG Engine: np.random.seed(s)                                │
  │    • Global BitGenerator (legacy MT19937 or modern PCG64).             │
  │    • Used by Pandas transformations and classical array algorithms.    │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. PyTorch CPU Generator: torch.manual_seed(s)                         │
  │    • Implemented in C++ via ATen; manages CPU tensor allocations       │
  │      (torch.randn, torch.randperm for CPU data loaders).               │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 4. PyTorch CUDA Generators: torch.cuda.manual_seed_all(s)              │
  │    • Manages distinct, parallel Philox PRNG states for EVERY physical  │
  │      GPU core and device attached to the PCIe bus.                     │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 5. Scikit-Learn Local Estimators: random_state=s                       │
  │    • Encapsulates localized np.random.RandomState instances per class. │
  │    • Completely ignores global seeds if parameterized directly.        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **The Half-Measure Failure Mode:** If you execute `np.random.seed(42)` and then instantiate a PyTorch neural network via `torch.nn.Linear(10, 5)`, PyTorch queries its **unseeded CPU generator**, drawing random initial weights from an arbitrary seed pulled from the operating system's entropy pool (`/dev/urandom`). Your pipeline remains non-deterministic.
  
  ---
## 4. The Canonical Global Seeding Protocol: `seed_all()` (Slides 5–6)
  
  Slide 6 introduces the standard live-coding seeding function that should appear at the top of every notebook in this course:
  
  ```python
  import random
  import numpy as np
  import torch
  
  def seed_all(s=42):
    """
    Globally binds the pseudorandom number generators across all 
    runtime engines to a fixed, deterministic state.
    """
    random.seed(s)                        # 1. Python standard library
    np.random.seed(s)                     # 2. NumPy numerical arrays
    torch.manual_seed(s)                  # 3. PyTorch CPU tensor engine
    torch.cuda.manual_seed(s)             # 4. PyTorch active GPU device
    torch.cuda.manual_seed_all(s)         # 5. All available physical GPUs
  
  # Enforce deterministic initialization
  seed_all(42)
  print("Draw 1:", np.random.rand(3))
  ```
  
  ```
                   Tracing PRNG State Transformations (Slide 6)
  Action                              Internal PRNG State       Output Emitted
  ──────────────────────────────────  ────────────────────────  ──────────────────────
  1. Invoke seed_all(42)              State initialized to s_0  (None)
  2. Run np.random.rand(3)            Advances s_0 ──▶ s_1      [0.3745, 0.9507, 0.7320]
  3. Run np.random.rand(3) AGAIN      Advances s_1 ──▶ s_2      [0.5986, 0.1560, 0.1559]
   (Different numbers! The PRNG state shifted forward along its deterministic sequence.)
  
  4. Re-invoke seed_all(42)           State reset back to s_0   (None)
  5. Run np.random.rand(3)            Advances s_0 ──▶ s_1      [0.3745, 0.9507, 0.7320]
   (Identical numbers! The internal register was reset to the identical starting point.)
  ```
  
  ---
## 5. Beyond PRNGs: Hardware Non-Determinism on GPUs (The Graduate Fill-In)
  
  Slide 5 notes: *"Seed everything! Test in Colab."* However, in graduate-level deep learning, setting PRNG seeds is **necessary, but mathematically insufficient** to guarantee bitwise reproducibility on GPUs.
### The IEEE 754 Floating-Point Non-Associativity Trap
  In pure real analysis, addition is associative:
  $$(a + b) + c \equiv a + (b + c)$$
  
  In computer systems using IEEE 754 floating-point arithmetic, **addition is non-associative** due to finite-precision round-off error:
  $$(a + b) + c \neq a + (b + c)$$
  
  ```
          Parallel Atomic Summation Non-Determinism
  GPU Thread 1 (Value a) ──┐
  GPU Thread 2 (Value b) ──┼──▶ [Hardware Atomic Add: atomicAdd()] ──▶ Final Accumulator
  GPU Thread 3 (Value c) ──┘
  ```
  
  * **The Mechanism:** When hundreds of CUDA threads compute gradients concurrently across mini-batches, they write back to global VRAM using atomic addition operations (`atomicAdd`).
  * Because thread scheduling across physical streaming multiprocessors (SMs) depends on dynamic hardware latencies, the **chronological arrival order** of values changes between runs.
  * Because $(a + b) + c \neq c + (b + a)$ in floating-point math, the final accumulated loss gradient drifts slightly:
  $$\nabla_\theta \mathcal{L}^{(t)}_{\text{Run 1}} \neq \nabla_\theta \mathcal{L}^{(t)}_{\text{Run 2}}$$
  * Over hundreds of training epochs, this microscopic floating-point divergence compounds, causing model weights to drift into different regions of parameter space.
  
  ---
### The Complete Deterministic PyTorch Configuration
  To enforce complete, bitwise mathematical reproducibility on GPU hardware (required in safety-critical medical and financial systems), you must constrain the underlying CUDA and cuDNN libraries:
  
  ```python
  import torch
  
  def enforce_strict_determinism(s=42):
    # 1. Global PRNG Seeding
    random.seed(s)
    np.random.seed(s)
    torch.manual_seed(s)
    torch.cuda.manual_seed_all(s)
  
    # 2. Disable cuDNN Auto-Tuner Benchmarking
    # Prevents cuDNN from testing non-deterministic heuristic convolution algorithms
    torch.backends.cudnn.benchmark = False
  
    # 3. Force cuDNN to use Deterministic Convolution Algorithms
    torch.backends.cudnn.deterministic = True
  
    # 4. Restrict PyTorch to Fully Deterministic CUDA Primitive Kernels
    # Throws a runtime error if an operation only has a non-deterministic implementation
    torch.use_deterministic_algorithms(True)
  ```
  
  **The Performance Trade-Off:** Forcing strict hardware determinism disables optimized non-deterministic atomic CUDA kernels, which can introduce a **$10\%\text{ to }30\%$ computational throughput penalty** during deep network training.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why does training a neural network on an unshuffled dataset often lead to gradient thrashing and a complete failure to converge?*
  2. *If an engineer calls `np.random.seed(42)` at the beginning of a script, why does a PyTorch model's `torch.nn.Dropout()` layer still produce non-deterministic outputs across repeated runs?*
  3. *Why does the non-associativity of IEEE 754 floating-point addition prevent bitwise reproducibility on GPUs even when all random seeds are fixed?*
  
  ---