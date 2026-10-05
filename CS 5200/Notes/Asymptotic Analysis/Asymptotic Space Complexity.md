### What is Space Complexity?
Just like Time Complexity counts "steps," Space Complexity counts **units of memory** required by the algorithm relative to the input size ($N$).

We generally look at two types of memory:
1.  **Input Space:** The memory needed to store the input itself (e.g., the array you are sorting). We usually ignore this unless stated otherwise.
2.  **Auxiliary Space:** The *extra* memory the algorithm needs to do its job (temporary variables, new arrays, or the **Call Stack**). **This is usually what we measure.**
- ### The Reversal Example
  **Goal:** Reverse the string "SATISH" to "HSITAS".
  
  **Approach A: Out-of-Place (The "Expensive" Way)**
  1.  Create a completely new array of size $N$ called `temp`.
  2.  Copy letters from the end of `SATISH` to the beginning of `temp`.
  3.  **Space Complexity:** $O(N)$ because we created a new array of the same size.
  
  **Approach B: In-Place (The "Efficient" Way)**
  1.  Use two pointers: one at the start (`S`) and one at the end (`H`).
  2.  Swap them. Move pointers inward.
  3.  **Space Complexity:** $O(1)$. You only need one temporary variable `temp_char` to perform the swap, regardless of whether the string is 6 letters or 6 million letters.
  
  ---
- ### The Quiz
  **Scenario:** An algorithm reverses a character array of $N$ letters using a temporary array of $N$ letters.
  **Example:** RAINMAN $\to$ NAMNIAR.
  
  **Question:** What are the Time and Space complexities?
  
  **Analysis:**
  1.  **Time:** To copy the letters from the old array to the new array, you must touch every letter once.
    *   Time = $O(N)$.
  2.  **Space:** The prompt explicitly states you are using a "temporary array of $N$ letters."
    *   Space = $O(N)$.
  
  **Correct Answer:**
  $\square$ Time complexity of $O(N)$ and space complexity of $O(N)$
  
  *(Note: If the prompt had said "using an in-place swap," the space would be $O(1)$. Always read the constraints carefully!)*
  
  ---
  
  This is the most common trap for students.
  "I don't see any variables or arrays declared, so the space must be $O(1)$, right?"
  **WRONG.**
- ### The Hidden Cost: The Call Stack
  When a function calls itself (recursion), the computer must "remember" where it left off. It pushes the current state (variables, return address) onto the **Call Stack**.
  
  *   If you recurse 10 times, you occupy 10 frames on the stack.
  *   If you recurse $N$ times, you occupy $N$ frames.
  *   **Formula:** Space Complexity of Recursion $\approx$ Maximum Depth of the Recursion Tree.
- ### Slide 4 Analysis: `sum(int n)`
  ```cpp
  int sum(int n) {
    if (n <= 0) return 0;
    return n + sum(n-1);
  }
  ```
  *   **Trace:** `sum(3)` calls `sum(2)` calls `sum(1)` calls `sum(0)`.
  *   **Depth:** The chain is $N$ calls deep before it hits the base case (`n <= 0`) and starts returning.
  *   **Space Complexity:** **$O(N)$**.
  
  ---
- ### Quiz: `traverse(int a)`
  
  **The Code:**
  ```cpp
  void traverse(int a) {
    if(a == 0) return;
    else {
        a = a - 1;
        traverse(a);
    }
  }
  
  main() {
    traverse(100);
  }
  ```
  
  **The Analysis:**
  1.  The function takes an integer `a`.
  2.  It decrements `a` by 1 and calls itself again.
  3.  It stops when `a == 0`.
  4.  **Depth:** If the input is $100$, it creates a stack 100 frames deep. If the input is $a$, it creates a stack $a$ frames deep.
  
  **The Options:**
  *   $O(100)$ - *This is technically constant $O(1)$, but usually not how we describe input-dependent algorithms.*
  *   $O(a)$ - *Linear relative to the input value.*
  *   $O(1)$ - *Constant.*
  *   $O(a * a)$ - *Quadratic.*
  
  **Correct Answer:**
  $O(a)$
  
  **Reasoning:**
  In Asymptotic Analysis, we treat the input parameters as variables. Even though `main` passes the hardcoded number 100, the *algorithm's* complexity is defined by its parameter `a`. The memory required grows linearly with the size of `a`.
- ### Summary Checklist
  1.  **Iterative Loops:** Usually $O(1)$ space unless you allocate a new structure (like an array or list).
  2.  **Recursive Functions:** Space is determined by the **Max Depth** of the recursion.
    *   $N$ calls deep = $O(N)$ space.
    *   Binary Search (dividing by 2) = $O(\log N)$ space.
- #### **1. Total Space vs. Auxiliary Space**
  *   **The Nuance:** Quiz questions often use these terms interchangeably, but they are different.
  *   **The Addition:**
    *   **Total Space:** Input Space + Auxiliary Space.
    *   **Auxiliary (Extra) Space:** Only the extra memory used by the algorithm (temporary variables, stack space).
    *   **The Trap:** If a question asks for the space complexity of "Selection Sort," the answer is $O(1)$ (Auxiliary). If it asks for the *Total Space* of Selection Sort on an array of size $N$, the answer is $O(N)$ (because the array itself exists).
    *   **Rule of Thumb:** Unless "Total" is specified, professors usually want the **Auxiliary Space**.
- #### **2. Pass-by-Value vs. Pass-by-Reference**
  *   **The Nuance:** This is a classic "C++ vs. Java/Python" trap that professors love.
  *   **The Addition:**
    *   **Pass-by-Reference (Java/Python objects, C++ pointers):** Passing an array of size $N$ to a recursive function adds **$O(1)$** space per stack frame because you are just passing a memory address.
    *   **Pass-by-Value (C++ structs/vectors by value):** If you pass an array by value, the computer **copies** the entire array for every call. 
    *   **The "Deep Dive" Math:** If you have a recursive function that goes $N$ deep and copies an array of size $N$ each time, the space complexity explodes to **$O(N^2)$**.
- #### **3. Recursive Space: Tree Depth, not Total Nodes**
  *   **The Nuance:** Students often see $O(2^N)$ recursive calls and think the space is $O(2^N)$.
  *   **The Addition:** Space complexity for recursion is determined by the **maximum depth** of the recursion tree at any single point in time.
  *   **Example: Recursive Fibonacci**
    ```cpp
    int fib(n) {
       if (n <= 1) return n;
       return fib(n-1) + fib(n-2);
    }
    ```
    *   There are $O(2^N)$ total calls (Time).
    *   However, the computer finishes the left branch (`fib(n-1)`) and **pops it off the stack** before starting the right branch (`fib(n-2)`).
    *   The stack never gets deeper than $N$.
    *   **Result:** Space is **$O(N)$**.
- #### **4. The "Hidden" String Builder Space**
  *   **The Nuance:** String manipulation is almost always a space-heavy operation.
  *   **The Addition:** 
    *   In many languages, strings are **immutable**.
    *   If you do this in a loop: `string s = ""; for(i=0 to n) { s += "a"; }`
    *   You aren't just adding a letter. You are creating a new string of size 1, then a new string of size 2, then size 3...
    *   The total space used (before garbage collection) is $1 + 2 + 3 \dots + N = \mathbf{O(N^2)}$.
    *   **Deep Dive Solution:** Using a `StringBuilder` (Java) or `list.append` (Python) uses **$O(N)$** space.
- #### **5. In-Place vs. Out-of-Place**
  *   **Definition:** An algorithm is "In-place" if it uses $O(1)$ (or $O(\log N)$) auxiliary space.
  *   **Examples for the Quiz:**
    *   **In-Place:** Selection Sort, Insertion Sort, Heapsort, Quicksort (mostly, $O(\log N)$).
    *   **Out-of-Place:** Merge Sort (needs $O(N)$ to hold the merged halves).
- ### **Deep Dive Practice Questions (Space & Theory)**
  
  Try these to test the "Nuances" we just added:
  
  **Q1: The "Recursive Copy" Trap**
  You have a recursive function `findMax(int[] arr, int n)` that finds the maximum value in an array. It splits the array into two halves, **copies** each half into a new array, and calls itself. 
  1. What is the **Time** complexity?
  2. What is the **Space** complexity?
  
  **Q2: Stack Depth**
  If an algorithm uses a "Work Queue" (a Loop + a List) to traverse a tree instead of recursion, and at any time the queue holds one "level" of the tree:
  1. What is the worst-case space complexity if the tree is a "spindle" (just a straight line of $N$ nodes)?
  2. What is the worst-case space complexity if the tree is "perfectly balanced" ($N$ total nodes)?
  
  **Q3: Formal Definition Check**
  If $f(n) = O(g(n))$, is it guaranteed that $g(n) = \Omega(f(n))$? (Hint: Review the "Transpose Symmetry" property in the Formal Theory append).
  
  **Q4: Amortized vs. Worst Case**
  In a dynamic array (ArrayList), what is the **Worst Case** space complexity of a single `add()` operation, and what is the **Amortized** space complexity of a single `add()` operation? (Careful—this is a trick question about space vs. time).