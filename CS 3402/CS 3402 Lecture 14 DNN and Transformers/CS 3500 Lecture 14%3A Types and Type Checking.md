### 1. Semantics vs. Syntax
**Referencing PDF Page 1:**
*   **Syntax:** Checks if the sentence is formed correctly (Subject-Verb-Object).
*   **Semantics:** Checks if the sentence has meaning.
  *   *Example of Valid Syntax but Invalid Semantics (C++):*
      ```cpp
      float x, y, z;
      *x = y[z] << x( z[3.14], x->z );
      ```
  *   The parser accepts this because the grammar is correct (identifiers, operators, parentheses match).
  *   The **Semantic Analyzer** rejects this because:
      *   You can't dereference (`*`) a float.
      *   You can't use bitshift (`<<`) on floats.
      *   You can't call a variable `x` as a function.

**Type Checking:**
This is the main job of semantic analysis. It asks: *"Are identifiers used in a manner consistent with their definition?"*

---
- ### 2. The Symbol Table
  **Referencing PDF Page 1 & 2:**
  To check types, the compiler needs a memory. It needs to remember that `x` was declared as an `int`. It does this using a **Symbol Table**.
  
  *   **Structure:** A table of Names and Attributes.
  *   **Examples:**
    *   **Variable:** `float y` $\to$ Name: `y`, Attr: `float`.
    *   **Constant:** `const int z=3` $\to$ Name: `z`, Attr: `int, const, 3`.
    *   **Struct:** `struct color { int r,g,b }` $\to$ Name: `color`, Attr: `struct [int r, int g, int b]`.
  
  **Language Differences (PDF Page 2):**
  The complexity of the symbol table depends on the language.
  1.  **C++:** Very detailed. Stores function parameter types (`int`, `bool`, `char&`) because it checks them at compile time.
  2.  **Python:** Less detailed. It knows `foo` is a function, but often doesn't know the parameter types until runtime (unless using type hints).
  3.  **Pascal:** Very strict.
    *   It stores exact Array ranges (e.g., `ARRAY [10..12]`).
    *   *Enumerated Arrays:* You can even index arrays with words, like `myWeek[Wednesday]`.
  
  ---
- ### 3. Type Conversion
  **Referencing PDF Page 2 (Bottom) & Page 3:**
  Sometimes types don't match exactly, but the compiler can fix it (Implicit Conversion).
  
  *   **Implicit Conversion (Coercion):**
    *   `z = a + x` (where `a` is float, `x` is integer).
    *   The compiler automatically promotes `int` $\to$ `float`.
  
  **Strictness Spectrum (Page 3):**
  1.  **C++ (Flexible):**
    *   `int` $\leftrightarrow$ `float` $\leftrightarrow$ `char` $\leftrightarrow$ `bool`.
    *   **User-Defined Conversion:** You can create your own!
        *   *The Wombat Example:* If you have a constructor `Wombat(string s)`, you can pass a string `"banana"` to a function expecting a `Wombat`. The compiler implicitly creates a temporary Wombat.
  2.  **Pascal (Strict):**
    *   It does **not** like mixing types.
    *   *Example:* In C, you can do math on chars (`'W' - x`). In Pascal, you must explicitly convert: `ORD('W') - x`. To turn it back, you use `CHR()`.
  
  ---
- ### 4. Type Equivalence
  **Referencing PDF Page 3 (Bottom) & Page 4:**
  When are two variables considered the "same" type?
  
  **1. Name-Type Equivalence:**
  *   Two entities are the same type **only if** they share the same Type Name.
  *   *Example:* `Penguin Bob;` and `Bear Fred;`. Different names (`Penguin` vs `Bear`), so they are different types.
  *   *Sub-classing:* Even declared classes like `Emperor Penguin : public Penguin` are treated as distinct types (though compatible via inheritance).
  
  **2. Structural-Type Equivalence:**
  *   Two entities are the same type if they share the **same structure** (internal layout), regardless of the name.
  *   *Example (Page 4):*
    *   `struct point { int x,y,z; }`
    *   `struct color { int r,g,b; }`
    *   `struct cell { int row, col; }`
  *   *Verdict:*
    *   Under Name equivalence, none are the same.
    *   Under Structural equivalence, `point` and `color` **ARE** the same (both are 3 integers). `cell` is different (only 2 integers).
  *   *Note:* This is hard to implement because you have to recursively check nested structures.
  
  ---
- ### 5. Strong vs. Weak Typing
  **Referencing PDF Page 5:**
  This characterizes how a language handles types.
  
  **Definitions (For this class):**
  *   **Strongly Typed:**
    *   Type violations are detected at **Compile Time**.
    *   Type conversions are explicit.
    *   The type of a variable remains fixed.
  *   **Weakly Typed:**
    *   Types are fluid or checked only at runtime.
  
  **The Timeline/Spectrum:**
  *   **Weak/Unstructured:** Assembly, BASIC, Perl.
  *   **Middle:** C, C++. (Stronger, but allows lots of implicit casting).
  *   **Strong:** Pascal, Java, Rust, Go.
  *   *Note on Python/JS:* Originally on the "Weak" side (runtime checking), but moving toward Strong with the introduction of TypeScript and Python Type Hints.