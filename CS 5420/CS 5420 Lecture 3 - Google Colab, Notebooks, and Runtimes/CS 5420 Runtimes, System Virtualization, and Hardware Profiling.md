## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why Variables Persist After Cell Deletion:**
  * Python’s execution engine maintains a persistent global symbol table for the active session (`globals()`). When you assign a variable (e.g., `x = 5`), a reference to an integer object is placed on the process heap. 
  * Deleting the cell in the browser UI modifies the client-side document (the `.ipynb` JSON file), but sends no signal to the background kernel to remove the variable. The variable remains bound in memory until the kernel process is explicitly terminated.
  2. **Implications of Execution Counters `In [1]`, `In [4]`, `In [2]`, `In [3]`:**
  * This non-monotonic sequence proves that the code was executed **out of order**. The cell labeled `[4]` was executed before the cells labeled `[2]` and `[3]`. 
  * Any downstream logic in cell `[2]` or `[3]` that relies on global state was evaluated against mutations introduced by cell `[4]`, creating a divergence between the visual flow of the document and its actual computational history.
  3. **Why Automated Grading (`nbconvert --execute`) Exposes Hidden-State Bugs:**
  * Headless evaluation engines start a completely clean Python subprocess and execute cells strictly from index $0$ to index $K-1$.
  * If a solution depends on variables initialized in deleted cells, manual re-runs of cells out of order, or accumulated loops, the linear execution will immediately encounter an unhandled `NameError`, `AttributeError`, or dimension mismatch.
  
  ---
## 2. The Colab Execution Fabric: Cloud Virtualization & Ephemeral Runtimes (Slide 8)
  
  Slide 8 details how user interactions in a browser map to remote, containerized hardware:
  
  ```
                      Colab Architecture & Resource Allocation
  ┌────────────────────────────────────────────────────────────────────────┐
  │                              Your Browser                              │
  │         (Interactive GUI: Code Cells, Markdown, Plotly, WebGL)        │
  └───────────────────────────────────┬────────────────────────────────────┘
                                    │ WebSocket / HTTPS JSON-RPC
                                    ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │                         Google Colab Backend                           │
  │     (Dynamic Provisioning, Identity Management, Notebook Gateway)      │
  └───────────────────────────────────┬────────────────────────────────────┘
                                    │ Virtual Container Bridge
                                    ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │                   Ephemeral Guest Container (Ubuntu OS)                │
  │                                                                        │
  │   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌─────────┐ │
  │   │ Virtual CPU   │  │ System RAM    │  │ GPU (Optional)│  │ Storage │ │
  │   │ (Intel Xeon / │  │ (~12.7 GB     │  │ (NVIDIA T4    │  │ (/content││
  │   │  AMD EPYC)    │  │  Available)   │  │  16 GB VRAM)  │  │  ~100 GB)││
  │   └───────────────┘  └───────────────┘  └───────────────┘  └─────────┘ │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The Ephemeral Nature of Cloud Instances
  * **Container Isolation:** Every Colab runtime runs inside an isolated, unprivileged Linux container (managed via Google’s Borg container infrastructure, analogous to Docker).
  * **Idle Timeouts vs. Absolute Lifespans:**
  * **Idle Timeout (~90 minutes):** If the browser tab is closed or no cells are actively computing, the kernel is deallocated, and the container is destroyed.
  * **Maximum Lifespan (12 hours on Free Tier):** Regardless of active computation, a free-tier container is terminated after 12 continuous hours to enforce cluster availability.
  * **Storage Volatility:** Everything saved to the local container disk (`/content`) is **ephemeral**. When the runtime terminates, all untracked artifacts (trained weights, downloaded CSVs) are permanently erased. Long-term assets must be synced to Google Drive via:
  ```python
  from google.colab import drive
  drive.mount('/content/drive')
  ```
  
  ---
## 3. Computational Paradigms: CPU vs. GPU (Slide 9)
  
  Slide 9 introduces a fundamental systems question: *When does an ML workload actually require a GPU?*
  
  ```
                       CPU vs. GPU Compute Architectures
             CPU (Latency-Optimized)               GPU (Throughput-Optimized)
      ┌───────────────────────────────────┐ ┌───────────────────────────────────┐
      │  Few Heavy Cores (2 to 8 vCPUs)   │ │  Thousands of Light Cores (SIMT)  │
      │  High Clock Speeds (~2.5–3.5 GHz) │ │  Moderate Clock Speeds (~1.5 GHz) │
      │  Large L1/L2/L3 Caches            │ │  Massive Memory Bandwidth         │
      │  Branch Predictors & Speculation  │ │  (GDDR6 / HBM: ~300–900 GB/sec)   │
      └───────────────────────────────────┘ └───────────────────────────────────┘
  ```
### The Architectural Divide
  * **The Central Processing Unit (CPU):**
  * Designed for **low-latency sequential execution** and complex control flows (MIMD — Multiple Instruction, Multiple Data).
  * Outfitted with deep instruction pipelines, branch predictors, and out-of-order execution logic.
  * **The Graphics Processing Unit (GPU):**
  * Designed for **massively parallel, uniform throughput** (SIMT — Single Instruction, Multiple Threads).
  * Strips away branch prediction and caching in exchange for thousands of Arithmetic Logic Units (ALUs) executing identical mathematical operations across large arrays simultaneously.
  
  ---
### Task Mapping & Computational Bottlenecks (Slide 9 Analysis)
  
  ```
  ┌───────────────────────────────────┬─────────┬─────────┬──────────────────────────────────────────┐
  │ Machine Learning Task             │ CPU     │ GPU     │ Computational Bottleneck                 │
  ├───────────────────────────────────┼─────────┼─────────┼──────────────────────────────────────────┤
  │ Data Preprocessing (Pandas)       │    ✔    │         │ Memory bandwidth, string hashing,        │
  │                                   │         │         │ irregular pointer traversal (CPU-bound)  │
  │                                   │         │         │                                          │
  │ Linear / Ridge / Lasso Regression │    ✔    │         │ OLS closed-form solves (X^T X)^(-1) X^T y;│
  │                                   │         │         │ matrix is small (d < 1,000); CPU is fast │
  │                                   │         │         │                                          │
  │ Decision Trees & Random Forests   │    ✔    │         │ Recursive conditional branching:         │
  │                                   │         │         │ IF x_j < threshold; CPU caches branch    │
  │                                   │         │         │ predictors efficiently                   │
  │                                   │         │         │                                          │
  │ Principal Component Analysis (PCA)│    ✔    │         │ SVD decomposition of d × d matrix;       │
  │                                   │         │         │ LAPACK / BLAS libraries handle on CPU    │
  │                                   │         │         │                                          │
  │ Small Neural Networks (MLPs)      │    ✔    │ Optional│ Matrix dimensions are too small to       │
  │                                   │         │         │ saturate thousands of GPU cores          │
  │                                   │         │         │                                          │
  │ Convolutional Networks (CNNs)     │         │    ✔    │ High-dimensional 2D/3D sliding tensor    │
  │                                   │         │ (Useful)│ convolutions; massive GEMM parallelism   │
  │                                   │         │         │                                          │
  │ Transformers & Modern LLMs        │         │    ✔    │ Scaled dot-product self-attention:       │
  │                                   │         │ (Needed)│ QK^T and softmax across huge dimensions  │
  └───────────────────────────────────┴─────────┴─────────┴──────────────────────────────────────────┘
  ```
  
  **Key Systems Insight:** GPUs introduce non-trivial overhead. Transferring data from system host RAM over the PCIe bus to GPU VRAM takes time:
  $$\text{Latency}_{\text{PCIe}} \gg \text{Latency}_{\text{Host RAM}}$$
  For small tabular models (e.g., training a Random Forest on $10{,}000$ rows), the PCIe data transfer overhead often exceeds the computation time itself. Standard classical ML algorithms are best executed on CPUs.
  
  ---
## 4. Subshell Escapes & Environment Mechanics (Slide 10)
  
  Slide 10 details the exclamation mark (`!`) syntax used in Jupyter and Colab notebooks:
  
  ```bash
  !nvidia-smi
  !ls
  !pip install package_name
  ```
  
  ```
                     Subshell Process Spawning (The ! Escape)
  ┌─────────────────────────────────┐
  │     IPython Kernel Process      │
  │     (Python Interpreter)        │
  └────────────────┬────────────────┘
                 │ Encountered line starting with '!'
                 ▼ Spawns child process via /bin/bash -c
  ┌─────────────────────────────────┐
  │     Ephemeral Bash Subshell     │  ──▶ Runs shell binary (e.g., /usr/bin/nvidia-smi)
  │     (Isolate Process Space)     │  ──▶ Pipes stdout / stderr back to notebook DOM
  └────────────────┬────────────────┘
                 │ Subshell terminates; state is discarded!
                 ▼
  ┌─────────────────────────────────┐
  │     IPython Kernel Resumes      │  (Environment changes like !cd DO NOT persist!)
  └─────────────────────────────────┘
  ```
### The Directory Trap: `!cd` vs. `%cd`
  A common failure in environment setup involves switching directories:
  * Running `!cd /content/drive` launches a child bash process, changes directory *inside that ephemeral subshell*, and then immediately terminates the subshell. The parent Python process remains in its original working directory.
  * To modify the working directory of the **actual IPython kernel process**, you must use the **IPython Magic Command**:
  ```python
  %cd /content/drive
  ```
  
  ---
## 5. Hardware Introspection & System Profiling (Slide 11)
  
  Slide 11 provides the commands required for **Part 1 of Coding Assignment 1 (CA1)**. Each command queries a distinct layer of the underlying Linux OS and hardware stack:
  
  ```
                            Linux Hardware Audit Map
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Command                      Target Subsystem     System Diagnostic Extracted          │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ !nvidia-smi                  GPU Driver & VRAM    GPU Architecture, Total VRAM, Driver │
  │                                                   Version, CUDA API Version, Wattage   │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ torch.cuda.is_available()    PyTorch CUDA Runtime Dynamic library check; confirms      │
  │                                                   PyTorch can execute kernels on GPU   │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ !cat /proc/cpuinfo | head    Virtual Processor    Microarchitecture (e.g., Intel Xeon) │
  │                                                   Clock Frequency, L2/L3 Cache bounds  │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ !free -h                     Host Memory (RAM)    Total, Used, Free, and Buffer/Cache  │
  │                                                   system memory (Human-readable format)│
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ !df -h                       Filesystem Disk      Storage capacity, mount partitions,  │
  │                                                   and remaining space on /content      │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Reading an `nvidia-smi` Output Like an ML Systems Engineer
  
  ```
  +-----------------------------------------------------------------------------+
  | NVIDIA-SMI 535.104.05             Driver Version: 535.104.05 CUDA Version: 12.2 |
  |-------------------------------+----------------------+----------------------+
  | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
  | Fan  Temp  Perf          Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
  |===============================+======================+======================|
  |   0  Tesla T4            Off  | 00000000:00:04.0 Off |                    0 |
  | N/A   31C    P8              9W /  70W |      0MiB / 15360MiB |      0%      Default |
  +-------------------------------+----------------------+----------------------+
  ```
  
  * **Tesla T4:** NVIDIA Turing architecture (Compute Capability 7.5). Features specialized **Tensor Cores** for mixed-precision (`FP16`/`INT8`) matrix math.
  * **Driver Version (535.104.05):** The host kernel-level driver that manages communication between the Linux kernel and the physical PCIe board.
  * **CUDA Version (12.2):** The maximum CUDA runtime API version supported by the current driver.
  * **Memory-Usage (0MiB / 15360MiB):** Shows current VRAM allocation. The T4 provides $16\text{ GB}$ (15,360 MiB) of GDDR6 memory. If tensor allocations exceed this limit, PyTorch throws an unrecoverable `CUDA out of memory (OOM)` error.
  
  ---
## 6. Live Coding Walkthrough: Activating the Hardware Accelerator (Slide 12)
  
  Slide 12 instructs the class to run a real-time verification script:
  
  ```python
  !nvidia-smi
  
  import torch, platform
  
  print("Python Version: ", platform.python_version())
  print("PyTorch Version:", torch.__version__)
  print("CUDA Available: ", torch.cuda.is_available())
  
  device_name = torch.cuda.get_device_name(0) if torch.cuda.is_available() else "CPU only"
  print("Active Device:  ", device_name)
  ```
  
  ```
                 State Transformation: CPU Mode vs. GPU Mode
           Default Runtime State                  T4 GPU Accelerated State
    ┌───────────────────────────────────┐    ┌───────────────────────────────────┐
    │ !nvidia-smi ──▶ Command Not Found │    │ !nvidia-smi ──▶ Displays Tesla T4 │
    │ torch.cuda.is_available() ──▶ False│   │ torch.cuda.is_available() ──▶ True│
    │ Active Device ──▶ "CPU only"      │    │ Active Device ──▶ "Tesla T4"      │
    └───────────────────────────────────┘    └───────────────────────────────────┘
  ```
### The Execution Lifecycle:
  1. When you select `Runtime` $\to$ `Change runtime type` $\to$ `T4 GPU`, Google Colab **terminates the current virtual container**.
  2. A new container image is provisioned with the NVIDIA Container Toolkit and bound to a physical GPU slice on the host machine.
  3. Any variables previously stored in RAM are destroyed. The notebook environment restarts with a clean global symbol table.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why does executing a command like `!cd sample_data` fail to change the working directory for subsequent Python cells, while `%cd sample_data` succeeds?*
  2. *From a hardware architecture perspective, why is an algorithm like Random Forest poorly suited for GPU acceleration compared to a deep Convolutional Neural Network?*
  3. *If `!nvidia-smi` shows that 14.8 GB of VRAM is currently allocated, what happens inside PyTorch if you attempt to instantiate an additional tensor requiring 2.0 GB of memory?*
  
  ---