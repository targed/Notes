# Part C2 — Batch, Mini-Batch, or Stochastic Practice Problems
#### Problem 1 (Code Forensics — Chunked Array Slicing)
  A training dataset has $n = 32{,}000$ samples. An engineer implements the following optimization loop:
  ```python
  B = 64
  for epoch in range(5):
    for i in range(0, n, B):
        X_batch = X[i : i + B]
        y_batch = y[i : i + B]
        grad = (2 / len(X_batch)) * X_batch.T @ (X_batch @ w - y_batch)
        w -= lr * grad
  ```
  * **(a)** Which gradient descent regime is used: **batch**, **mini-batch**, or **stochastic**?  
  * **(b)** How many parameter updates are executed per single epoch?  
  * **(c)** What is the total number of parameter updates executed across all 5 epochs?
  
  ---
#### Problem 2 (PyTorch DataLoader Mechanics)
  A computer vision model is trained on $n = 40{,}000$ images using the following PyTorch configuration:
  ```python
  loader = DataLoader(train_dataset, batch_size=len(train_dataset), shuffle=False)
  for images, labels in loader:
    optimizer.zero_grad()
    loss = criterion(model(images), labels)
    loss.backward()
    optimizer.step()
  ```
  * **(a)** Which gradient descent regime does this loop execute?  
  * **(b)** How many parameter updates occur per epoch?  
  * **(c)** What is the primary systems risk of setting `batch_size=len(train_dataset)` on large image datasets?
  
  ---
#### Problem 3 (Online Stream Processing)
  A real-time fraud detection pipeline processes an incoming stream of transaction records:
  ```python
  for idx in range(len(X_train)):
    x_i = X_train[idx : idx + 1]  # Slice 1 row
    y_i = y_train[idx : idx + 1]
    pred = x_i @ w
    grad = 2 * (pred - y_i) * x_i.T
    w -= lr * grad
  ```
  * **(a)** Which gradient descent regime is used?  
  * **(b)** If the dataset contains $n = 15{,}000$ samples, how many parameter updates occur across 3 complete passes (epochs) through the data?  
  * **(c)** Why is there no division by $n$ in the gradient calculation `grad = 2 * (pred - y_i) * x_i.T`?
  
  ---
#### Problem 4 (Reverse Batch-Size Calculation)
  A deep learning practitioner trains a text classifier on a corpus of $n = 80{,}000$ clinical notes using Mini-Batch Gradient Descent. The training logs report that across **$4$ complete epochs**, the optimizer executed exactly **$1{,}000$ parameter updates**.
  * **(a)** How many parameter updates were executed per single epoch?  
  * **(b)** What was the exact mini-batch size ($B$) configured in the training pipeline? Show your work.
  
  ---
#### Problem 5 (Hardware & GPU Memory Limits)
  An enterprise tabular dataset contains $n = 1{,}000{,}000$ samples and $d = 2{,}000$ continuous features (`float32`, 4 bytes per float). A data scientist writes a script attempting to compute **Batch Gradient Descent** ($B = n$) on an NVIDIA GPU with $16\text{ GB}$ of VRAM.
  * **(a)** Calculate the raw memory footprint of the design matrix $X$ in gigabytes (GB).  
  * **(b)** During neural network backpropagation, intermediate activation caches and gradient buffers require approximately $4\times$ to $6\times$ the memory of the raw input matrix. Will this Batch GD script run or crash with an out-of-memory error (`torch.cuda.OutOfMemoryError`)?  
  * **(c)** What gradient descent regime must be used to train this model on this GPU hardware?
  
  ---
#### Problem 6 (Statistical Variance of Gradient Estimators)
  Let $\Sigma$ denote the covariance matrix of individual sample loss gradients ($\text{Var}(\nabla \mathcal{L}_i) = \Sigma$).
  * **(a)** State the mathematical formula for the variance of the gradient estimator in:
  1. *Stochastic Gradient Descent ($B = 1$)*
  2. *Mini-Batch Gradient Descent (batch size $B$)*
  3. *Batch Gradient Descent ($B = n$)*  
  * **(b)** By what exact numerical factor is gradient variance reduced when an engineer transitions from single-sample SGD ($B = 1$) to Mini-Batch GD with $B = 64$?  
  * **(c)** By what factor does variance reduce when scaling from $B = 512$ to $B = 1{,}024$? Explain the law of diminishing returns in mini-batch scaling.
  
  ---
#### Problem 7 (Loss Curve Forensics & Optimization Pathologies)
  Three loss curves are recorded when optimizing a linear regression model on identical training data:
  * **Curve A:** The loss drops during early iterations, but then enters an erratic, high-frequency oscillatory band around a loss value of $15.0$, fluctuating continuously without ever settling into the minimum.
  * **Curve B:** Decreases monotonically at every single update step, forming an ultra-smooth, convex descent curve.
  * **Curve C:** Decreases for 6 updates, then spikes to $10^4$, $10^8$, and prints `NaN` on update 19.
  
  *Match each curve to its underlying optimization cause:*
  1. *Batch Gradient Descent with a well-calibrated learning rate ($\eta < \frac{2}{\lambda_{\max}}$).*
  2. *Gradient Descent with an unstable learning rate exceeding the Hessian curvature bound ($\eta \ge \frac{2}{\lambda_{\max}}$).*
  3. *Stochastic Gradient Descent ($B = 1$) operating under a constant, non-decaying learning rate in a limit cycle.*
  
  ---
#### Problem 8 (Non-Convex Optimization & Generalization Dynamics)
  In deep neural networks with non-convex loss surfaces, Batch Gradient Descent evaluates the mathematically exact gradient of the training data, yet Mini-Batch Gradient Descent almost universally finds parameter solutions with superior test-set generalization. 
  
  Explain the statistical mechanism that allows Mini-Batch Gradient Descent to escape sharp, non-generalizing local minima and saddle points.
  
  ---
#### Problem 9 (Gradient Accumulation & Virtual Batch Sizes)
  An engineer wants to train with an effective batch size of $B_{\text{eff}} = 256$, but their GPU runs out of memory whenever the batch size exceeds $B = 64$. They implement **gradient accumulation**:
  ```python
  loader = DataLoader(train_ds, batch_size=64, shuffle=True)
  optimizer.zero_grad()
  
  for step, (x_b, y_b) in enumerate(loader):
    loss = criterion(model(x_b), y_b) / 4
    loss.backward()
    
    if (step + 1) % 4 == 0:
        optimizer.step()
        optimizer.zero_grad()
  ```
  * **(a)** How many forward/backward passes are accumulated before calling `optimizer.step()`?  
  * **(b)** What is the mathematical and operational effective batch size ($B_{\text{eff}}$) of this loop?  
  * **(c)** Which gradient descent regime does this code emulate?
  
  ---
#### Problem 10 (Uneven Batch Slicing Across Epochs)
  A training dataset contains $n = 25{,}000$ instances. A mini-batch data loader is initialized with `batch_size=512` and `drop_last=False`.
  * **(a)** How many full batches of size $512$ are generated per epoch?  
  * **(b)** What is the sample size of the final remaining batch in each epoch?  
  * **(c)** How many total parameter updates are executed across $12$ complete epochs?
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C2
### Problem 1 Solution
  id:: 6abe78a9-308c-485d-831a-d41ab9c7b600
  * **(a) Regime:** **Mini-Batch Gradient Descent**.  
  *Justification:* The loop slices data in subsets of size $B = 64$, which satisfies $1 < B < n$.
  * **(b) Updates per Epoch:**
  $$\text{Updates per Epoch} = \left\lceil \frac{n}{B} \right\rceil = \frac{32{,}000}{64} = \mathbf{500 \text{ updates per epoch}}$$
  * **(c) Total Updates (5 Epochs):**
  $$\text{Total Updates} = 5 \times 500 = \mathbf{2{,}500 \text{ parameter updates}}$$
  
  ---
### Problem 2 Solution
  * **(a) Regime:** **Batch Gradient Descent (BGD)**.  
  *Justification:* Even though implemented using PyTorch's `DataLoader`, setting `batch_size=len(train_dataset)` loads all $40{,}000$ samples into a single tensor per iteration.
  * **(b) Updates per Epoch:** Exactly **1 parameter update per epoch**.
  * **(c) Systems Risk:**  
  Loading all $40{,}000$ high-resolution images into GPU VRAM simultaneously to compute forward passes and backpropagation graphs requires tens of gigabytes of memory, triggering an unrecoverable `torch.cuda.OutOfMemoryError (OOM)`. `[L04, L11]`
  
  ---
### Problem 3 Solution
  * **(a) Regime:** **Stochastic Gradient Descent (Online SGD)**.  
  *Justification:* Slicing `X_train[idx : idx + 1]` extracts a single row ($B = 1$). Parameters update after processing every individual sample.
  * **(b) Total Updates (3 Epochs):**
  $$\text{Updates per Epoch} = n = 15{,}000$$
  $$\text{Total Updates} = 3 \times 15{,}000 = \mathbf{45{,}000 \text{ parameter updates}}$$
  * **(c) Why No Division by $n$:**  
  For a single observation ($n = 1$), the cost function is $\mathcal{L}_i = (\hat{y}_i - y_i)^2$. The derivative with respect to $w$ is:
  $$\nabla_w \mathcal{L}_i = 2(\hat{y}_i - y_i) x_i$$
  The factor $\frac{1}{n}$ is an average across $n$ samples. For a single sample, $\frac{1}{1} = 1$, so there is no division by $n$. `[L11]`
  
  ---
### Problem 4 Solution
  * **(a) Updates per Epoch:**
  $$\text{Updates per Epoch} = \frac{\text{Total Updates}}{\text{Total Epochs}} = \frac{1{,}000}{4} = \mathbf{250 \text{ updates per epoch}}$$
  * **(b) Mini-Batch Size ($B$):**
  $$\text{Updates per Epoch} = \left\lceil \frac{n}{B} \right\rceil = 250 \implies B = \frac{n}{250}$$
  $$B = \frac{80{,}000}{250} = \mathbf{320 \text{ samples per batch}}$$
  
  ---
### Problem 5 Solution
  * **(a) Raw Matrix Footprint:**
  $$\text{Total Floats} = 1{,}000{,}000 \times 2{,}000 = 2 \times 10^9 \text{ elements}$$
  $$\text{Memory} = 2 \times 10^9 \times 4\text{ bytes} = 8{,}000{,}000{,}000\text{ bytes} = \mathbf{8.0\text{ GB}}$$
  * **(b) OOM Assessment:**  
  **It will CRASH.** While the raw matrix ($8\text{ GB}$) fits within the $16\text{ GB}$ physical VRAM pool, backpropagation requires allocating intermediate activation tensors, weight gradient matrices, optimizer momentum states (Adam stores two additional float buffers per parameter), and CUDA workspace:
  $$\text{Peak Memory} \approx 4\times \text{ to } 6\times \text{ input size} \approx 32\text{ to } 48\text{ GB}$$
  This vastly exceeds the $16\text{ GB}$ hardware limit, triggering an immediate OOM crash.
  * **(c) Required Regime:**  
  **Mini-Batch Gradient Descent** (e.g., $B = 256$ or $B = 512$). Slicing data into small chunks bounds activation memory to a few megabytes per step. `[L04, L11]`
  
  ---
### Problem 6 Solution
  * **(a) Variance Formulas:**
  1. *Stochastic GD ($B = 1$):* $\text{Var}(\nabla \mathcal{L}_i) = \mathbf{\Sigma}$
  2. *Mini-Batch GD ($B$):* $\text{Var}\left( \frac{1}{B}\sum_{i \in \mathcal{B}} \nabla \mathcal{L}_i \right) = \mathbf{\frac{\Sigma}{B}}$
  3. *Batch GD ($B = n$):* Evaluates the full population sum $\implies \mathbf{\text{Variance} = 0}$ (exact gradient).
  * **(b) Variance Reduction ($B = 1 \to B = 64$):**
  $$\text{Reduction Factor} = \frac{\Sigma / 1}{\Sigma / 64} = \mathbf{64\times \text{ reduction in variance (or } 98.4\% \text{ noise drop)}}$$
  * **(c) Diminishing Returns ($B = 512 \to B = 1{,}024$):**
  * Scaling from $512$ to $1{,}024$ reduces variance by only **$2\times$**, while **doubling** the floating-point operations required per step. 
  * Beyond $B = 512$, the gradient estimate is already highly stable; doubling the batch size provides negligible optimization improvement while increasing compute time and memory footprint. `[L11]`
  
  ---
### Problem 7 Solution
  * **1. Curve B corresponds to:** **Batch Gradient Descent with optimal learning rate**.  
  *Reasoning:* Evaluates all $n$ samples simultaneously; the absence of sampling noise produces a perfectly smooth, monotonic descent.
  * **2. Curve C corresponds to:** **Gradient Descent exceeding the stability bound ($\eta \ge \frac{2}{\lambda_{\max}}$)**.  
  *Reasoning:* When $\eta \ge \frac{2}{\lambda_{\max}(H)}$, updates overshoot the quadratic loss bowl with expanding amplitude, diverging exponentially to `inf` and `NaN`.
  * **3. Curve A corresponds to:** **Stochastic Gradient Descent ($B = 1$) in a limit cycle**.  
  *Reasoning:* Because single-sample gradients have constant non-zero variance ($\Sigma$), a non-decaying learning rate causes the parameters to bounce around the minimum in a bounded, noisy limit cycle rather than settling into the center. `[L11]`
  
  ---
### Problem 8 Solution
  * **Mechanism:**
  * Batch Gradient Descent computes the exact empirical gradient $\nabla J(w)$. On non-convex loss surfaces, it moves deterministically down the steepest path, frequently converging into the nearest **sharp, narrow local minimum**. Sharp minima have high curvature ($\lambda_{\max}$ is large) and generalize poorly to test data because small test shifts produce large prediction errors.
  * Mini-Batch Gradient Descent introduces stochastic gradient noise with covariance $\frac{\Sigma}{B}$.
  * This noise acts as an internal thermal perturbation (simulated annealing). It provides the kinetic energy necessary to **bounce parameters out of sharp, brittle local minima and saddle points**, allowing the optimizer to settle into **broad, flat minima**, which generalize significantly better under distribution shift. `[L11]`
  
  ---
### Problem 9 Solution
  * **(a) Accumulation Passes:** Exactly **4 forward/backward passes** are computed before updating weights (`(step + 1) % 4 == 0`).
  * **(b) Effective Batch Size ($B_{\text{eff}}$):**
  $$B_{\text{eff}} = 4 \times 64 = \mathbf{256 \text{ samples per update}}$$
  * **(c) Emulated Regime:** **Mini-Batch Gradient Descent** with batch size $256$.  
  *Systems Value:* Gradient accumulation allows an engineer to simulate a large batch size ($B = 256$) that does not fit in physical VRAM by computing smaller $64$-sample slices sequentially and summing their gradients before taking an optimizer step. `[L04, L11]`
  
  ---
### Problem 10 Solution
  * **(a) Full Batches:**
  $$\text{Full Batches} = \left\lfloor \frac{25{,}000}{512} \right\rfloor = \left\lfloor 48.828 \right\rfloor = \mathbf{48 \text{ full batches}}$$
  *(Total samples covered in full batches: $48 \times 512 = 24{,}576$).*
  * **(b) Remainder Batch Size:**
  $$\text{Last Batch Size} = 25{,}000 - 24{,}576 = \mathbf{424 \text{ samples}}$$
  * **(c) Total Updates across 12 Epochs:**
  * Each epoch executes $48 \text{ (full)} + 1 \text{ (partial)} = 49 \text{ updates}$.
  $$\text{Total Updates} = 12 \times 49 = \mathbf{588 \text{ parameter updates}}$$
  
  ---