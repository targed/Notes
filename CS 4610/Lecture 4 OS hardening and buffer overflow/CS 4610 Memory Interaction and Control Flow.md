## I. Interacting with Memory (Slides 37–41)
As established, the CPU can only do math inside its **Registers**. It cannot do math directly in RAM. 

*   **The Problem (Slide 37):** How do you add two numbers that live in memory?
*   **The Solution (Slide 38):** You must fetch them into registers, do the math, and write the result back.
  1.  `mov address1, %eax` (Fetch first number)
  2.  `mov address2, %ebx` (Fetch second number)
  3.  `add %ebx, %eax` (Add them inside the CPU)
  4.  `mov %eax, address1` (Store the result back in memory)
- ### Dereferencing Pointers in Assembly (Slide 41)
  In AT&T syntax, parentheses `()` act exactly like the `*` (dereference) operator in C.
  *   **`mov %eax, %edx`**: Copies the *number* inside EAX into EDX.
  *   **`mov (%eax), %edx`**: Treats the number in EAX as a memory address. It goes to RAM, looks up that address, and copies the *contents* of that address into EDX. (In C: `edx = *eax;`)
- ## II. Endianness (The "Backwards" Memory) (Slides 42–44)
  Memory is essentially one giant array of bytes (1 byte = 8 bits). Every byte has its own address (0, 1, 2, 3...). However, registers (like EAX) hold **4 bytes** (32 bits).
  
  When you move 4 bytes from memory into a 32-bit register, what order do they go in? This is determined by **Endianness**.
  *   **x86 uses Little Endian:** The "Least Significant Byte" (the little end) is stored at the lowest memory address.
  *   **Example (Slide 44):** 
    *   Let's say memory contains the following bytes at addresses 3, 4, 5, and 6: ``.
    *   If you tell the CPU to read 4 bytes starting at address `0x3`, it reads them "backwards."
    *   The register will contain: `0xffeeddcc`.
  *   *Security Context (Crucial for your project!):* When you write a Buffer Overflow exploit and want to inject a malicious memory address like `0x0804841d` into the stack, you must format it backwards in your payload string: `\x1d\x84\x04\x08`. If you don't use Little Endian, the CPU will misread your injected address and the exploit will crash.
- ## III. `MOV` vs. `LEA` (Load Effective Address) (Slides 45–49)
  This is the most common point of confusion when reading assembly code.
  
  **1. `MOV` (Move)**
  *   `mov $5, %eax`: Puts the literal number 5 into EAX.
  *   `mov 5, %eax`: Goes to memory address 5, reads what is there, and puts it in EAX.
  *   `mov (%ebx), %eax`: Goes to the memory address stored in EBX, reads what is there, and puts it in EAX.
  
  **2. `LEA` (Load Effective Address)**
  *   `LEA` **does not touch RAM.** It is strictly a math calculator used for computing memory addresses (pointer arithmetic).
  *   `lea (%ebx), %eax`: Takes the value of EBX and puts it in EAX. (Effectively just a copy).
  *   `lea -1(%ebx), %eax`: Computes `EBX - 1` and puts that number into EAX.
  *   *Why use LEA?* It is a highly optimized instruction. Compilers love using `LEA` to calculate array indexes quickly (e.g., finding the address of `buf` without having to use slow `add` or `mul` instructions).
- ## IV. Control Flow: Turning C into "Spaghetti" (Slides 50–56)
  CPUs do not understand `if/else`, `while`, or `for` loops. The CPU only understands **Jumps**. High-level code is translated into a series of jumps, making it look like "spaghetti code."
- ### 1. The Instruction Pointer (EIP)
  *   **EIP** points to the next instruction.
  *   You **cannot** modify EIP directly (e.g., `mov $0x1234, %eip` is an illegal instruction).
  *   You can only modify EIP by using a `jmp` (Jump) or `call` instruction.
    *   **Direct Jump:** `jmp 0x45` (Go exactly to address 0x45).
    *   **Indirect Jump:** `jmp *%eax` (Go to whatever address is currently saved inside EAX).
- ### 2. Conditional Jumps and EFLAGS (Slides 54–56)
  How does an `if` statement work in hardware? It is a two-step process: **Compare, then Jump.**
  
  *   **Step 1: The Comparison (`cmp`)**
    *   The `cmp` instruction basically subtracts two numbers but throws away the result. It only uses the result to set the **EFLAGS**.
    *   *Zero Flag (ZF):* Set to 1 if the two numbers were equal (because $x - x = 0$).
    *   *Sign Flag (SF):* Set to 1 if the result was negative (meaning $x < y$).
  *   **Step 2: The Conditional Jump**
    *   Instructions like `JE` (Jump if Equal) or `JL` (Jump if Less) look at the EFLAGS.
    *   If the flag is set, they overwrite **EIP** and jump to a new code block (the `if` block).
    *   If the flag is not set, they do nothing, and the CPU just falls through to the next instruction (the `else` block).
  
  ---
- ### Study Questions for Part 2
  1.  **Endianness Practice:** You are reading raw memory and see the bytes `` at addresses 0x10, 0x11, 0x12, and 0x13. If you execute `mov (0x10), %eax`, exactly what 32-bit hex value will be sitting inside the EAX register?
  2.  **LEA vs MOV:** Assuming register `%ecx` holds the value `0x1000`:
    *   What does `lea 0x4(%ecx), %eax` do to `%eax`?
    *   What does `mov 0x4(%ecx), %eax` do to `%eax`?
  3.  **Control Flow Logic:** In C, you write `if (password == "admin")`. In assembly, the CPU runs a `cmp` instruction to compare the two strings. Which specific flag in the **EFLAGS** register will the CPU check to decide if it should jump to the "Access Granted" code?
  
  ---