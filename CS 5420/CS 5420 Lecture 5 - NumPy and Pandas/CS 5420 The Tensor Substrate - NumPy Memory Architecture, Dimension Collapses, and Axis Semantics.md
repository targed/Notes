## 1. The Anatomy of an `ndarray`: What Lives Under the Hood? (Slide 2)
  
  Slide 2 introduces the fundamental data structure of numerical Python:
  ```python
  # Array Creation Routines
  np.array([1, 2, 3])
  np.zeros((5, 3))
  np.arange(0, 10, 2)
  np.random.randn(5, 3)
  
  # Structural Inspection Attributes
  a.shape   # Tuple of array dimensions
  a.ndim    # Integer rank / number of axes
  a.dtype   # Internal element data type
  a.size    # Total scalar element count
  ```
### The In-Memory Layout of an `ndarray`
  In standard Python, a list is an array of pointers to boxed heap objects (`PyObject`). A NumPy `ndarray`, by contrast, is a thin C-structure wrapper pointing to a **flat, contiguous block of unboxed homogeneous memory**:
  
  ```
                       The NumPy ndarray Architecture
  ┌────────────────────────────────────────────────────────────────────────┐
  │                        Python ndarray Object Wrapper                   │
  │  ┌─────────────────────────┬────────────────────────────────────────┐  │
  │  │ data                    │ Pointer to the raw memory buffer (0x7f)│  │
  │  │ dtype                   │ float64 (8 bytes, IEEE 754 double)     │  │
  │  │ shape                   │ (5, 3)                                 │  │
  │  │ strides                 │ (24, 8)  <-- Bytes to step per axis    │  │
  │  │ flags                   │ C_CONTIGUOUS = True, OWNDATA = True    │  │
  │  └─────────────────────────┴────────────────────────────────────────┘  │
  └───────────────────────────────────┬────────────────────────────────────┘
                                    │ points to
                                    ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │                         Raw Memory Buffer (Heap)                       │
  │ [ a00 | a01 | a02 | a10 | a11 | a12 | a20 | ... | a40 | a41 | a42 ]    │
  │  0x00   0x08  0x10  0x18  0x20  0x28  0x30        0x70  0x78  0x80   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### Strides: The Secret to Zero-Copy Operations
  The internal `strides` tuple dictates how many **bytes** the CPU must step across physical memory to advance one index along a given axis:
  * For a `float64` array (8 bytes per scalar) with shape `(5, 3)` in standard row-major (C-style) layout:
  $$\text{strides} = (3 \times 8\text{ bytes}, 1 \times 8\text{ bytes}) = \mathbf{(24, 8)}$$
  * To move to the next row (`axis=0`), jump forward $24$ bytes ($3$ floats).
  * To move to the next column (`axis=1`), jump forward $8$ bytes ($1$ float).
  * **Zero-Copy Transposition:** When you compute `a.T`, NumPy does **not** move any raw bytes in memory. It simply creates a new wrapper that swaps the shape to `(3, 5)` and the strides to `(8, 24)`. It runs in $\mathcal{O}(1)$ time with zero memory allocation.
  
  ---
## 2. Memory Precision: `float64` vs. `float32` (Slide 2)
  
  Slide 2 presents a critical engineering principle:
  
  $$\mathbf{\text{float64} \longrightarrow \text{float32} \quad \text{halves memory consumption and often execution time.}}$$
  
  ```
                       Precision and Cache Saturation
                     float64 (Standard NumPy Default)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1 Sign Bit │ 11 Exponent Bits │ 52 Fraction Bits (Double Precision)    │
  │ Total: 64 bits = 8 bytes per scalar                                    │
  └────────────────────────────────────────────────────────────────────────┘
  
                       float32 (Deep Learning Standard)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1 Sign Bit │ 8 Exponent Bits │ 23 Fraction Bits (Single Precision)     │
  │ Total: 32 bits = 4 bytes per scalar                                    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### The Three Hardware Consequences:
  1. **Host & GPU Memory Footprint:**
   * An $N \times d$ data matrix with $10^7$ floats requires **$80\text{ MB}$** in `float64`, but only **$40\text{ MB}$** in `float32`.
   * On VRAM-constrained GPUs (such as the 16 GB Tesla T4 from Lecture 4), default `float64` allocations trigger premature Out-Of-Memory (OOM) failures.
  2. **CPU/GPU Memory Bandwidth & Cache Occupancy:**
   * Modern CPU cache lines are $64$ bytes wide.
   * A single cache line fetch pulls **8 scalars** of `float64`, but **16 scalars** of `float32`.
   * Halving the byte footprint effectively doubles L1/L2/L3 cache capacity and cuts DRAM bus traffic by $50\%$.
  3. **SIMD Vectorization Throughput:**
   * A 512-bit vector register (AVX-512) can execute 8 double-precision (`float64`) arithmetic operations per cycle, or **16 single-precision (`float32`) operations per cycle**. Switching to `float32` doubles theoretical peak throughput.
  
  ---
## 3. The Axis Principle: "The Axis That Disappears" (Slide 2)
  
  Slide 2 states the universal rule for understanding tensor reductions:
  
  $$\mathbf{\text{"An axis argument names the axis that disappears."}}$$
  
  ```
                      The Tensor Axis Reduction Model
                   Matrix A of Shape (5, 3): 5 Rows, 3 Columns
                                  axis = 1  (Columns)
                             Col 0     Col 1     Col 2
                         ┌─────────┬─────────┬─────────┐
                   Row 0 │  a_00   │  a_01   │  a_02   │ ──▶ sum across axis=1
                   Row 1 │  a_10   │  a_11   │  a_12   │     collapses 3 columns
       axis = 0    Row 2 │  a_20   │  a_21   │  a_22   │     into a single row value.
        (Rows)     Row 3 │  a_30   │  a_31   │  a_32   │     Result Shape: (5,)
                   Row 4 │  a_40   │  a_41   │  a_42   │
                         └─────────┴─────────┴─────────┘
                              │         │         │
                              ▼         ▼         ▼
                    sum across axis=0 collapses 5 rows
                    into a single column value.
                    Result Shape: (3,)
  ```
### Mathematical Formulation:
  Let $A \in \mathbb{R}^{d_0 \times d_1}$ be a rank-2 matrix.
  
  1. **Global Reduction (`a.sum()`):**
   Summation over all axes simultaneously:
   $$S = \sum_{i=0}^{d_0-1} \sum_{j=0}^{d_1-1} A_{i, j} \quad \implies \quad \text{Shape: Scalar ()}$$
  2. **Reduction Along `axis=0` (`a.sum(axis=0)`):**
   The index corresponding to axis 0 ($i$) is iterated over and collapsed:
   $$S_j = \sum_{i=0}^{d_0-1} A_{i, j} \quad \implies \quad \text{Shape: } (d_1,) = (3,)$$
   *Physical Meaning:* We compress rows downward to obtain **one summary value per column** (e.g., feature means).
  3. **Reduction Along `axis=1` (`a.sum(axis=1)`):**
   The index corresponding to axis 1 ($j$) is iterated over and collapsed:
   $$S_i = \sum_{j=0}^{d_1-1} A_{i, j} \quad \implies \quad \text{Shape: } (d_0,) = (5,)$$
   *Physical Meaning:* We compress columns sideways to obtain **one summary value per row** (e.g., sample totals).
  
  ---
## 4. Array Slicing Mechanics & Rank Preservation (Slide 2)
  
  Slide 2 details basic slicing syntax:
  
  ```python
  a[2, 3]       # One element (Rank-0 scalar extraction)
  a[1, :]       # A row (Rank-1 vector)
  a[:, 1]       # A column (Rank-1 vector)
  a[1:4, 2:5]   # A sub-block (Rank-2 matrix slice)
  ```
### The Rank Reduction Trap: `(N,)` vs. `(1, N)` vs. `(N, 1)`
  Slide 2 warns: *"Shape errors are your #1 bug all semester — print shapes constantly."*
  
  A frequent bug in applied ML arises from confusing **scalar indexing** with **slice indexing**:
  
  ```
                         Rank Reduction vs. Preservation
      Indexing Operation            Resulting Shape          Tensor Rank
  ┌─────────────────────────────┬──────────────────────────┬─────────────────┐
  │ a[1, :]   (Scalar row index)│ (3,)                     │ Rank-1 Vector   │
  ├─────────────────────────────┼──────────────────────────┼─────────────────┤
  │ a[1:2, :] (Slice row index) │ (1, 3)                   │ Rank-2 Matrix   │
  ├─────────────────────────────┼──────────────────────────┼─────────────────┤
  │ a[:, 1]   (Scalar col index)│ (5,)                     │ Rank-1 Vector   │
  ├─────────────────────────────┼──────────────────────────┼─────────────────┤
  │ a[:, 1:2] (Slice col index) │ (5, 1)                   │ Rank-2 Column   │
  └─────────────────────────────┴──────────────────────────┴─────────────────┘
  ```
#### Why This Breaks Neural Networks & Loss Functions:
  If you subtract a true target vector $y$ of shape `(N,)` from a model prediction $\hat{y}$ of shape `(N, 1)`:
  ```python
  # Unintended Broadcasting Disaster:
  diff = y_pred - y_true   # Shape: (N, 1) - (N,)  ==> Shape: (N, N)!
  ```
  NumPy's broadcasting rules will not throw an error; instead, it automatically stretches both arrays into an **$N \times N$ matrix**, calculating all pairwise differences rather than element-wise errors. An array of $10{,}000$ predictions silently becomes a **$100{,}000{,}000$-element matrix**, consuming gigabytes of memory and distorting downstream loss calculations.
  
  ---
## 5. Dissecting the "Your Turn: Shape and Axis" Exercise (Slide 3)
  
  Slide 3 presents an interactive diagnostic exercise:
  
  ```python
  import numpy as np
  
  rng = np.random.default_rng(0)
  A = rng.normal(size=(5, 3))
  
  # 1) A.shape, A.dtype, A.size
  # 2) column 1 -> what shape?
  # 3) mean of every column
  # 4) mean of every row
  ```
  
  ```
                     Step-by-Step Exercise Solutions
  ┌─────────────────────────────────┬───────────────────┬────────────────────────────────┐
  │ Operation                       │ Evaluated Output  │ Theoretical Justification      │
  ├─────────────────────────────────┼───────────────────┼────────────────────────────────┤
  │ 1a) A.shape                     │ (5, 3)            │ 5 rows (instances), 3 cols     │
  │ 1b) A.dtype                     │ dtype('float64')  │ Default float precision of PRNG│
  │ 1c) A.size                      │ 15                │ 5 × 3 = 15 total scalar values │
  ├─────────────────────────────────┼───────────────────┼────────────────────────────────┤
  │ 2) A[:, 1].shape                │ (3,) ──▶ WRONG!   │ Extracting via a scalar integer│
  │    (Column 1 extraction)        │ (5,)  ✔ CORRECT   │ collapses the second axis;     │
  │                                 │                   │ returns 5 elements along axis 0│
  ├─────────────────────────────────┼───────────────────┼────────────────────────────────┤
  │ 3) A.mean(axis=0).shape         │ (3,)              │ Axis 0 (rows) collapses;       │
  │    (Mean of every column)       │                   │ yields one scalar per column   │
  ├─────────────────────────────────┼───────────────────┼────────────────────────────────┤
  │ 4) A.mean(axis=1).shape         │ (5,)              │ Axis 1 (cols) collapses;       │
  │    (Mean of every row)          │                   │ yields one scalar per row      │
  └─────────────────────────────────┴───────────────────┴────────────────────────────────┘
  ```
### Explaining the Common "Wrong Guess" on Question 2:
  Dr. Yu notes: *"Wrong guess? That is the useful one."*
  * When asked for "column 1", students frequently guess `(3,)` because they associate columns with width $3$.
  * However, a single column contains an observation for **every row** in the dataset. Because matrix $A$ has $5$ rows, taking column 1 extracts an array spanning across those 5 rows, producing shape **`(5,)`**.
  
  ---
## Summary Review Questions for Section 1
  
  1. *How does NumPy's internal `strides` mechanism allow operations like matrix transposition (`A.T`) to execute in $\mathcal{O}(1)$ time without copying data in memory?*
  2. *Why does executing `y_pred - y_true` where `y_pred.shape == (N, 1)` and `y_true.shape == (N,)` result in an array of shape `(N, N)` instead of throwing a shape mismatch error?*
  3. *If a 3D video tensor has shape `(batch, frames, channels, height, width) = (32, 10, 3, 224, 224)`, what is the output shape if you compute `.mean(axis=1)`? Which physical attribute of the video was collapsed?*
  
  ---