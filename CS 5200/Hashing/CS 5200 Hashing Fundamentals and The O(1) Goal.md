### 1. The Core Motivation: Breaking the $\log N$ Barrier
Before hashing, if you wanted to store data and search for it later, your best option was a **Balanced Binary Search Tree** (like an AVL tree or Red-Black tree).
*   **Tree Search/Add Time:** $O(\log N)$.
*   If you have 1 billion records, a tree takes about 30 steps to find a record.
*   **The Hashing Goal:** Can we do it in **$O(1)$** time? Can we find a record in 1 step, regardless of whether the database has 10 items or 10 billion items?
- ### 2. The Hashing Pipeline (Slide 6 & 14-15)
  To achieve $O(1)$ time, Hashing uses an under-the-hood **Array**. Arrays have $O(1)$ lookup *if you know the index*. The magic of hashing is translating a complicated piece of data (like a String or a giant ID number) into a simple array index.
  
  This happens in two distinct steps (often confused by students):
  
  **Step A: The Hash Function $\rightarrow$ Hash Code**
  *   Takes the raw data (e.g., the string `"Charlie"`) and converts it into a standard integer (e.g., `8472910`). 
  *   This integer is called the **Hash Code**. 
  *   *Deep Dive Note:* In Java, every object has a `.hashCode()` method that does exactly this.
  
  **Step B: The Compression Function $\rightarrow$ Array Index**
  *   Your hash table (array) is not infinitely large. If your array has a size of $m = 100$, you cannot put an item at index `8472910`.
  *   We use a **Compression Function** to force the hash code to fit inside the array.
  *   The most common compression function is the **Modulo Operator (`%`)**: 
    $$ \text{Index} = \text{Hash Code} \pmod m $$
    *Example:* `8472910 % 100 = 10`. The item goes into index 10.
- ### 3. Implementations & Abstractions (Slides 7 & 9-12)
  In the real world, you rarely write the raw array logic yourself unless you are doing a systems-level interview. You use language abstractions:
  *   **Python:** `Dictionary`
  *   **C++:** `unordered_map` (Note: `std::map` is a Tree, `unordered_map` is a Hash Table!).
  *   **Java:** `HashMap` (Not thread-safe, faster) or `Hashtable` (Thread-safe, older).
  
  **Standard HashMap Operations (Slide 10):**
  *   `put(key, value)`: Adds a key-value pair. ($O(1)$ average time)
  *   `get(key)`: Retrieves the value for the key. ($O(1)$ average time)
  *   `containsKey(key)`: Returns `true`/`false`. ($O(1)$ average time)
- ### 4. Classic Use Cases: Frequencies and De-duplication (Slides 8, 20-21)
  Hashing is the absolute best tool for **Counting Frequencies** or **Removing Duplicates**.
  
  **The Frequency Count Algorithm (Slide 21):**
  Given a stream of numbers: `{1, 2, 3, 1, 2, 2, 4}`
  How do we count occurrences in $O(N)$ total time?
  ```java
  for (each element e in array) {
    if (map.containsKey(e)) {
        // We've seen it before! Get current count, add 1.
        int current_count = map.get(e);
        map.put(e, current_count + 1);
    } else {
        // First time seeing it. Add to map with count 1.
        map.put(e, 1);
    }
  }
  ```
  *   **Time Complexity:** The loop runs $N$ times. Inside the loop, `containsKey`, `get`, and `put` all take $O(1)$ time. $N \times O(1) = \mathbf{O(N)}$.
  *   *Compare to naive approach:* If you didn't use a hash map, you'd have to use nested loops to count duplicates, taking $O(N^2)$ time.
  
  ---
- ### Part 1 Practice Questions
  
  **Q1: Hash Code vs. Compression**
  In a hashing system, a student's ID number is `123456789`. The hash table array has a size of `50`. 
  1. Which mathematical operation is typically used to ensure the ID fits into the array?
  2. What specific index will this ID evaluate to?
  
  **Q2: Abstraction Identification**
  If you are writing C++ code and you need to keep your keys in **alphabetical order**, should you use a Hash Table (`unordered_map`) or a Binary Search Tree (`std::map`)? Why?
  
  **Q3: De-Duplication**
  Imagine you have an array of 1 million user emails and want to remove all duplicates. 
  Describe how you would use a `HashSet` (which only stores keys, no values) to return a list of unique emails in $O(N)$ time.