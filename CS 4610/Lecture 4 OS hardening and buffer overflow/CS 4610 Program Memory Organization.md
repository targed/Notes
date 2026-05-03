## I. The Process View of Memory (Slides 58–59)
When you run a program in a 32-bit Linux environment, the Operating System creates a **Process**. It gives this process a virtual "sandbox" of memory.

*   **The 4GB Limit:** 32-bit addresses can only reference $2^{32}$ bytes, which equals 4GB.
  *   **User Space (3GB):** The lower 3GB (Addresses `0x00000000` to `0xBFFFFFFF`) is where your program code and variables live.
  *   **Kernel Space (1GB):** The top 1GB (Addresses `0xC0000000` to `0xFFFFFFFF`) is reserved for the Operating System itself. Your program cannot touch this directly (or it will crash with a SegFault).
*   **Virtual Memory:** Each process thinks it owns the entire 3GB. Address `0x08048000` in "Firefox" is physically different from address `0x08048000` in "Spotify." The hardware handles the translation.
- ## II. The Anatomy of User Space (Slide 59)
  The user's 3GB is divided into specific zones. From **Low Address (Bottom)** to **High Address (Top)**:
  
  1.  **Code Segment (`.text`):** The compiled machine instructions. Fixed size.
  2.  **Data Segment (`.data`):** Global variables and static constants. Fixed size.
  3.  **The Heap:**
    *   **Usage:** Used for dynamic memory (variables created with `malloc()` or `new`).
    *   **Growth Direction:** Grows **UP** (towards higher addresses). As you `malloc` more, the "program break" moves up.
  4.  **Shared Libraries:** Where `glibc` and other `.so` (Shared Object) files are loaded.
  5.  **The Stack:**
    *   **Usage:** Used for local variables, function parameters, and return addresses.
    *   **Growth Direction:** Grows **DOWN** (towards lower addresses). This is a crucial legacy feature (Slide 80).
- ## III. Stack vs. Heap Variables (Slide 60)
  *   **Stack Variables:** Declared inside a function (e.g., `int b;`). They are "automatic"—they exist only while the function is running. When the function returns, they are popped off the stack and disappear.
  *   **Heap Variables:** Declared with a pointer (e.g., `int* p = malloc(...)`). They persist until you explicitly `free()` them.
- ## IV. System Calls (Slides 61–64)
  How does a user program ask the Kernel for more Heap memory?
  *   **The Boundary:** Your program cannot just write to any random RAM. It is fenced in.
  *   **The Request:** When you call `malloc()`, the `glibc` library prepares the CPU registers with the specific request ID.
  *   **The Interrupt (`int $0x80`):** This assembly instruction pauses your program and hands control to the Linux Kernel.
    *   The Kernel checks if you are allowed to have more memory.
    *   If yes, it moves the "Heap Boundary" (program break) up, giving you more valid addresses to use.
    *   The Kernel then uses `iret` (Interrupt Return) to hand control back to your program.
- ## V. The User Stack: LIFO and "Downwards Growth" (Slides 66–80)
  The Stack is a **LIFO (Last-In, First-Out)** data structure.
  *   **Push:** Adds an element to the "Top" of the stack.
    *   *Because the stack grows down,* pushing **subtracts** 4 bytes from the Stack Pointer (`%esp`).
  *   **Pop:** Removes an element from the "Top".
    *   *Because the stack grows down,* popping **adds** 4 bytes to the Stack Pointer (`%esp`).
  *   **Visualizing "Down":**
    *   Imagine the stack starts at address 100.
    *   Push A: A is at 96. (ESP = 96)
    *   Push B: B is at 92. (ESP = 92)
    *   Pop: You get B back. (ESP returns to 96).
- ## VI. Memory Alignment (Slides 81–82)
  *   Memory is byte-addressable (you can read address 101, 102, etc.).
  *   However, the CPU prefers reading in "chunks" (Memory Units or Words) of 4 bytes at a time (32 bits).
  *   **The Rule:** A "Memory Unit" usually starts at an address divisible by 4.
    *   If you access address `0x1004`, it is aligned (fast).
    *   If you access address `0x1005`, it is unaligned (slow, or sometimes illegal depending on the CPU architecture, though x86 handles it).
  
  ---
- ### Study Questions for Part 3
  1.  **Memory Map:** If the **Heap** grows UP and the **Stack** grows DOWN, what theoretically happens if a program runs recursively forever without stopping? (Hint: They will eventually collide).
  2.  **Growth Logic:** If the Stack Pointer (`%esp`) is currently at address `0xBFFFF100`, and you execute `push %eax`, what will the new value of `%esp` be? (Remember: Pushing on x86 *subtracts*).
  3.  **Kernel Protection:** Why does the hardware prevent a user program from simply writing data to address `0xC0000000` directly without using a System Call?
  
  ---