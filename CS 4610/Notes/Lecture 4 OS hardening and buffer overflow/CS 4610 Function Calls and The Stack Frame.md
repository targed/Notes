# Part 4: Function Calls & The Stack Frame (Slides 83–107)

Assembly does not have "functions." It only has Jumps. To simulate functions (where `A` calls `B` and then `B` returns to `A`), we use the **Stack Frame** and a set of rules called the **Calling Convention**.
- ## I. The Challenge of Functions (Slide 84)
  When `main()` calls `func()`, several things must be coordinated:
  1.  **Parameters:** How does `main` pass arguments (like `a`, `b`, `c`) to `func`?
  2.  **Return Address:** How does `func` know where to go back to when it is done?
  3.  **Local Variables:** Where does `func` store its private variables?
  4.  **Register Sharing:** There is only *one* set of registers (EAX, EBX, etc.). If `func` uses EAX, it might overwrite valuable data `main` was keeping there.
- ## II. The Solution: The Stack Frame (Slide 86)
  Every function gets its own slice of the stack, called a **Frame**.
  *   **Boundaries:**
    *   **Top (Low Address):** Marked by the Stack Pointer (`%esp`).
    *   **Bottom (High Address):** Marked by the Base Pointer (`%ebp`).
  *   **The Chain:** The frames are linked. `func`'s frame sits directly below `main`'s frame.
- ## III. The `cdecl` Calling Convention (Slide 88)
  Linux/GCC uses the **cdecl** convention. This dictates the order of operations.
- ### Step 1: The Caller (`main`) Prepares (Slides 89-96)
  Before calling `func(1, 2)`, the caller must:
  1.  **Push Arguments:** Pushed onto the stack in **Reverse Order** (Right-to-Left).
    *   Push `2`.
    *   Push `1`.
  2.  **Call Instruction:** `call func`
    *   This instruction does two things atomically:
        *   **Push Return Address:** Pushes the address of the *next* instruction (where we want to resume) onto the stack.
        *   **Jump:** Changes `%eip` to the address of `func`.
- ### Step 2: The Callee (`func`) Starts - The Prologue (Slide 90)
  Now `func` is running. It needs to set up its own frame.
  1.  **Save Old Base Pointer:** `push %ebp`.
    *   We save `main`'s base pointer so we can restore it later.
  2.  **Set New Base Pointer:** `mov %esp, %ebp`.
    *   The current stack top becomes the new base for `func`.
  3.  **Allocate Locals:** `sub $16, %esp`.
    *   Moves the stack pointer down to reserve space for local variables (like `char buf[8]`).
- ### Step 3: Register Hygiene (Caller-Save vs. Callee-Save) (Slide 91)
  To prevent register overwrites, the convention divides registers into two teams:
  *   **Caller-Save (EAX, ECX, EDX):** "Volatile." If `main` cares about these, `main` must save them before calling `func`. `func` is free to trash them.
  *   **Callee-Save (EBX, ESI, EDI):** "Stable." `func` must promise not to change these. If `func` needs to use EBX, it must `push %ebx` first (save it) and `pop %ebx` (restore it) before returning.
- ### Step 4: The Return - The Epilogue (Slides 98-100)
  When `func` is done:
  1.  **Return Value:** Stored in `%eax`.
  2.  **Restore Stack:** `mov %ebp, %esp` (Drops all local variables instantly).
  3.  **Restore Base:** `pop %ebp` (Puts `main`'s base address back into `%ebp`).
  4.  **Return:** `ret`.
    *   Pops the **Return Address** off the stack and jumps to it.
- ## IV. The Key Takeaway: The "Saved EIP" (Slide 96)
  Look closely at the stack diagram on Slide 96.
  *   **Buffer (Local Variable)** is at a lower address.
  *   **Saved EBP** is directly above it.
  *   **Return Address (Saved EIP)** is directly above that.
  
  *Because the stack grows down (from High to Low addresses), but buffers fill up (from Low to High addresses), a local buffer is positioned perfectly to overflow "up" into the Return Address.*
  
  ---
- ### Study Questions for Part 4
  1.  **Stack Geography:** Inside `func`, if you access `0x4(%ebp)` (4 bytes above the base pointer), what specific value are you reading? (Hint: Refer to Slide 96).
  2.  **The "Ret" Instruction:** The `ret` instruction essentially performs a `pop`. Which register does it pop the stack value into?
  3.  **Convention Logic:** Why are arguments pushed in **Reverse Order**? (This is a C feature that allows for "Variadic Functions" like `printf` where the number of arguments can change).
  
  ---