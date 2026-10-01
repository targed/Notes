## 1. Context & Motivation: Why Colab for Applied Machine Learning? (Slide 2)
  
  Slide 2 details why this course standardizes on Google Colaboratory rather than local Python environments:
  
  ```
                          The Colab Environment Matrix
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Dimension               Local Native Environment           Managed Cloud (Google Colab)│
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Dependency Consistency  Prone to local library mismatches, Fixed Ubuntu container with │
  │                         OS-specific C++ compiler breaks    pre-built NumPy, SciPy,     │
  │                         and conflicting CUDA drivers.      PyTorch, and scikit-learn.  │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Accelerator Access      Constrained by local hardware      Free-tier access to NVIDIA  │
  │                         (e.g., integrated Intel/Apple GPUs)T4 GPUs with high VRAM.     │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Submission Medium       Scripts require separate PDF plots Integrated document: code, │
  │                         and written reports.               live outputs, and Markdown. │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
  
  **Graduate Takeaway:** In enterprise and academic ML research, environment drift is a primary source of wasted engineering hours. Colab acts as a standardized virtualization sandbox, ensuring that runtime behavior on your screen matches the grading environment used by the teaching assistant.
  
  ---
## 2. Notebooks vs. Python Source Files: The File Model (Slide 3)
  
  Slide 3 states: *"A notebook is more than a Python file. Cells, their results, and the order you ran them — all saved in one file."*
### A. The Structural Anatomy of an `.ipynb` File
  A standard script (`model.py`) is flat UTF-8 text containing code alone. An IPython Notebook (`model.ipynb`) is a structured **JSON document** conforming to the official `nbformat` schema:
  
  ```json
  {
  "cells": [
    {
      "cell_type": "code",
      "execution_count": 3,
      "metadata": {},
      "source": [
        "model = LinearRegression()\n",
        "model.fit(X_train, y_train)"
      ],
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {},
      "source": [
        "### Model Evaluation\n",
        "Evaluating $R^2$ on held-out test data."
      ]
    }
  ],
  "metadata": {
    "kernelspec": {
      "display_name": "Python 3",
      "language": "python",
      "name": "python3"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 5
  }
  ```
### B. What This Means Under the Hood
  1. **Bundled Artifact Storage:** An `.ipynb` file serializes the source code, terminal outputs (`stdout`/`stderr`), binary image blobs (e.g., base64-encoded Matplotlib plots), and execution counts into a single disk file.
  2. **State Divergence:** The rendered output displayed in a cell reflects the state of the kernel **at the instant that cell was evaluated**. If underlying variables change later in memory, the printed cell output does not update automatically until re-executed.
  
  ---
## 3. The Notebook Execution Engine: Client-Server-Kernel Architecture (Slide 4)
  
  Slide 4 introduces the operational runtime: *"Cells share one kernel — one long-lived Python process holding all your variables."*
  
  ```
                 The Jupyter Client-Server Architecture
  ┌───────────────────────┐                    ┌─────────────────────────┐
  │     User Browser      │                    │     IPython Kernel      │
  │ (Google Colab / Web)  │                    │  (Persistent OS Process)│
  │                       │                    │                         │
  │  [Cell 1: Code]       │  ZeroMQ / WebSock  │  Global Symbol Table:   │
  │  [Cell 2: Markdown]   │ ─────────────────▶ │    globals() = {        │
  │  [Cell 3: Code]       │ ◀───────────────── │      'x': 15,           │
  │                       │   Execution Result │      'model': <Obj>,    │
  │  Display: [n] Counter │                    │      'X_train': <Array> │
  └───────────────────────┘                    │    }                    │
                                             └─────────────────────────┘
  ```
### Key Architectural Components:
  1. **The Web Client:** Handles text editing, syntax highlighting, and rendering outputs (DOM manipulation).
  2. **The IPython Kernel:** A background child process running Python (e.g., `python -m ipykernel_launcher`). 
   * It exposes a single, global memory heap and namespace (`globals()`).
   * **Variables persist indefinitely** across cell executions until the kernel process is killed or explicitly restarted.
  3. **The Execution Counter (`In [n]`):**
   * The integer inside the bracket indicates the **sequential order of execution** relative to the kernel lifetime.
   * `[1]` was executed first, `[2]` second, and so on.
   * An empty bracket `[ ]` indicates an unexecuted cell.
   * An asterisk `[*]` indicates a cell currently executing or waiting in the execution queue.
  
  ---
## 4. The Hidden-State Trap (Slide 5)
  
  Slide 5 describes a critical issue in interactive development:
  > *"You define $x = 5$, run a cell, then delete the line that defined it. Your notebook still works — because $x$ lives in the kernel, not in the file. You share it. It fails immediately for everyone else."*
  
  ```
                     Anatomy of the Hidden-State Trap
  Step 1: Write and Run Code             Step 2: Delete Code from Document
  ┌──────────────────────────────┐       ┌──────────────────────────────┐
  │ Cell 1:                      │       │ Cell 1:                      │
  │   x = 5                      │ ──▶   │   # (Cell deleted or empty)  │
  │                              │       │                              │
  │ Kernel Memory:               │       │ Kernel Memory:               │
  │   globals()['x'] = 5         │       │   globals()['x'] = 5 STILL!  │
  └──────────────────────────────┘       └──────────────────────────────┘
                                                       │
  Step 3: Downstream Cell Still Works                    ▼
  ┌──────────────────────────────┐       Step 4: Fresh Run by TA / Peer
  │ Cell 2:                      │       ┌──────────────────────────────┐
  │   print(x + 10)  # Prints 15 │       │ Fresh Kernel Started:        │
  │                              │       │ Cell 2 executed first:       │
  │ Status: RUNS LOCALLY         │       │ NameError: name 'x' is       │
  │         (False Confidence)   │       │            not defined       │
  └──────────────────────────────┘       └──────────────────────────────┘
  ```
### The Systemic Pathology:
  * In Python, running `x = 5` binds the string identifier `'x'` to an integer object on the process heap.
  * Deleting the source code in the browser user interface **does not call `del globals()['x']`**. The reference count of the object remains non-zero, and the variable stays in memory.
  * When code relies on variables created in deleted cells or out-of-order executions, the notebook becomes a **non-reproducible artifact**.
  
  ---
## 5. Why Out-of-Order Execution Breaks Logic (Slide 6)
  
  Slide 6 presents an interactive prediction exercise demonstrating how mutable state behaves under repeated executions:
  
  ```python
  # Cell 1
  x = 10
  
  # Cell 2
  x += 5
  print(x)
  ```
  
  ```
                          Trace of In-Memory State
  ┌──────────────┬──────────────────┬─────────────────┬────────────────────────┐
  │ Action       │ Execution Count  │ Memory State    │ Printed Output         │
  ├──────────────┼──────────────────┼─────────────────┼────────────────────────┤
  │ Run Cell 1   │ In [1]           │ x ──▶ 10        │ (None)                 │
  │ Run Cell 2   │ In [2]           │ x ──▶ 10 + 5    │ 15                     │
  │ Run Cell 2   │ In [3]           │ x ──▶ 15 + 5    │ 20  <-- Hidden Mutation│
  └──────────────┴──────────────────┴─────────────────┴────────────────────────┘
  ```
### The Diagnostic Takeaway:
  * Unlike compiled binaries or static scripts that execute sequentially from line 1 to line $M$, a notebook allows **arbitrary execution paths** across its cells.
  * If a cell performs an in-place mutation (`x += 5`, `df.drop()`, or `list.append()`), running that cell multiple times produces different results on each invocation.
  * **The Bracket Audit:** If you inspect a notebook and see execution numbers like `In [14]` followed immediately below by `In [3]`, that notebook was executed out of order, and the internal state may not match the visual layout of the code.
  
  ---
## 6. The Reproducibility Invariant (Slide 7)
  
  Slide 7 outlines the core requirement for submitting work in this course:
  
  ```
                      The Reproducibility Protocol
  ┌────────────────────────────────────────────────────────────────────────┐
  │  Step 1: Runtime ──▶ Restart Runtime (Terminates and respawns kernel)  │
  │  Step 2: Run All Cells (Executes strictly top-to-bottom: 1, 2, ..., K) │
  │  Step 3: Verification (Confirm every cell executes without exceptions) │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### Why This Is a Graded Requirement Across All 7 Assignments:
  * Grading in CS 5420 uses automated test harnesses (such as `nbconvert` or headless execution runners) that run:
  ```bash
  jupyter nbconvert --to notebook --execute submission.ipynb
  ```
  * Automated runners initialize a completely clean kernel and execute all cells in strict linear sequence from top to bottom.
  * If your notebook depends on:
  1. Variables defined in deleted cells,
  2. Running a lower cell before an upper cell, or
  3. Running a cell multiple times to accumulate mutations,
  * The headless grader will encounter a fatal `NameError`, `KeyError`, or shape mismatch, resulting in an immediate loss of credit on that component.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why does deleting a cell containing a variable assignment in a Jupyter/Colab notebook leave that variable accessible to all other cells in the current session?*
  2. *If a notebook exhibits execution counters in the sequence `In [1]`, `In [4]`, `In [2]`, `In [3]`, what does this imply about how the in-memory state was formed?*
  3. *Why does automated grading via `nbconvert --execute` immediately expose hidden state errors that do not appear during interactive development?*
  
  ---