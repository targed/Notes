### 1. Threads vs. Tasks (The "Deep Dive" Distinction)
Slide 9 makes a crucial point: **"Don’t mention threads."**
*   **The Problem with Threads:** In standard programming (like Java Threads or Pthreads), the programmer has to manually manage which core runs which code. This is hard to scale.
*   **The Task Solution:** We write **Logical Parallelism**. we tell the computer which parts *can* run in parallel, and a **Scheduler** (part of the OS or language runtime) automatically maps those tasks to physical hardware threads.
- ### 2. The Three Key Keywords
  To convert a sequential algorithm to a parallel one, we use three primitive instructions:
  
  1.  **`spawn`**: "The Fork."
    *   It tells the parent function: "You can start this sub-task on another processor, but **do not wait** for it. Continue to the next line of code immediately."
  2.  **`sync`**: "The Join."
    *   It tells the current function: "Stop here. You cannot move forward until **all** previously spawned children have finished their work."
  3.  **`parallel for`** (Implicit in logic):
    *   A loop where every iteration can run simultaneously.
  
  ---
- ### 3. Converting Sequential to Parallel (The "Stick" Example)
  Slide 2-4 shows the `breakBundleOfSticks` algorithm. 
  
  **Sequential Logic:**
  1. Pick up a bundle.
  2. Break the left half.
  3. **Wait** until left is done.
  4. Break the right half.
  
  **Parallel Logic (Task-Based):**
  ```c
  void p-breakBundleOfSticks(bundle, first, last) {
    if (first == last) break stick;
    else {
        mid = (first + last) / 2;
        // The parent "spawns" the left half as a separate task
        spawn p-breakBundleOfSticks(bundle, first, mid);
        
        // The parent continues to the right half immediately
        p-breakBundleOfSticks(bundle, mid + 1, last);
        
        // Before finishing, the parent must wait for the left side child
        sync; 
    }
  }
  ```
  
  ---
- ### 4. The Fibonacci "Warning" (Slides 13–16)
  Your slides use **Recursive Fibonacci** to illustrate parallelism. 
  *   **Note:** We know this is a bad algorithm sequentially ($O(2^n)$). 
  *   **The Lesson:** Parallelism cannot fix a bad algorithm. If the underlying work is exponential, adding 100 processors just makes the exponential disaster happen 100 times faster. It doesn't change the "Work" ($T_1$).
  
  **Parallel Fibonacci Code (Slide 15):**
  1.  `x = spawn P-FIB(n-1)`
  2.  `y = P-FIB(n-2)`
  3.  `sync`
  4.  `return x + y`
  *   *Why don't we spawn both?* We could, but it's inefficient. If the parent spawned both and then hit `sync`, the parent would just sit idle doing nothing. By spawning one and doing the other itself, we keep the parent CPU busy.
  
  ---
- ### 5. Professor Deep Dive: "Logical Parallelism"
  On a quiz, the professor might ask: **"Does `spawn` guarantee that the code runs on a different core?"**
  *   **Answer:** **No.** 
  *   **Explanation:** `spawn` indicates **potential** parallelism. If the computer only has one core, the scheduler will run them one after another. If the computer has 100 cores, the scheduler will distribute them. This makes the code **portable**.
  
  ---
- ### Practice Questions
  
  **Q1: The `sync` placement**
  In the Fibonacci example, what happens if you move the `sync` to be *before* the line `y = P-FIB(n-2)`?
  *   *Hint:* Think about the "Do not wait" rule of `spawn`.
  
  **Q2: The `findMin` Logic (Slide 18)**
  In the parallel `findMin` algorithm, we `spawn` the left search and then run the right search.
  *   If we have an array of 1,000,000 elements, how many `sync` operations will occur total in the recursion tree? (Is it 1, or is it many?)
  
  **Q3: Task Overhead**
  If "breaking a stick" takes 1 nanosecond, but "spawning a task" takes 100 nanoseconds, why is the parallel version actually **slower** than the sequential version? How do we fix this? (Think back to the "Hybrid Merge Sort" $k$ threshold).