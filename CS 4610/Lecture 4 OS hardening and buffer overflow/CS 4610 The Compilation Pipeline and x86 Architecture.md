# Part 1: The Compilation Pipeline and x86 Architecture (Slides 13–36)

To understand how a hacker breaks a program, you must first understand how a computer reads and executes a program. High-level languages like C hide the "metal" of the computer from the programmer. Hackers attack the metal.
- ## I. From Concept to Execution: The Compilation Pipeline (Slides 14–25)
  When you write a C program (e.g., `42.c`), the computer cannot read it directly. It must be translated into raw binary (1s and 0s). This happens in four distinct steps:
  
  1.  **Preprocessor (`cpp`)**: 
    *   **Action:** Handles all the lines starting with `#` (like `#include <stdio.h>` or `#define MAX 100`).
    *   **Result:** It essentially copies and pastes the contents of included files directly into your source code and swaps out macros.
  2.  **Compiler (`cc` / `gcc -S`)**:
    *   **Action:** Translates the expanded C code into **Assembly Language** (e.g., `pushl %ebp`). 
    *   **Result:** A `.s` file containing human-readable, CPU-specific instructions.
  3.  **Assembler (`as`)**:
    *   **Action:** Translates the Assembly instructions into raw **Machine Code** (binary).
    *   **Result:** An Object file (`.o`). At this point, the code is machine-readable, but it doesn't know where standard functions like `printf` live yet.
  4.  **Linker (`ld`)**:
    *   **Action:** Stitches your `.o` file together with external libraries (like the GNU C Library, `glibc`).
    *   **Result:** The final **Executable Binary** (e.g., `a.out` or your program name).
- ## II. The Binary Execution Model (Slides 26–27)
  When you run the executable, the Operating System loads it into RAM. It organizes the program into distinct "Segments":
  *   **Code Segment (`.text`):** Where the actual executable CPU instructions live. This section is usually marked as *Read-Only* so the program doesn't accidentally overwrite its own logic.
  *   **Data Segment (`.data`):** Where global variables and constants (like the string `"hello world"`) are stored.
  *   **The Stack & Heap:** Two dynamically growing areas of memory used for variables created while the program is running (we will cover these deeply in later sections).
  *   **The Processor's Job:** The CPU sits in an infinite loop: **Fetch** (an instruction from the Code Segment), **Decode** (figure out what it means), **Execute** (do the math or move the memory), and repeat.
- ## III. The x86 Processor Architecture (Slides 28–33)
  The CPU uses **Registers**—tiny, ultra-fast storage buckets physically located inside the CPU—to do its math. Memory (RAM) is huge but slow; Registers are tiny but lightning-fast.
- ### 1. General Purpose Registers (32-bit)
  *   **EAX (Accumulator):** Often used for math operations and storing the *return values* of functions.
  *   **EBX (Base):** Used as a base pointer for memory access.
  *   **ECX (Counter):** Used as a loop counter.
  *   **EDX (Data):** Used for I/O operations and extending EAX for large math.
- ### 2. Pointer / Index Registers (Crucial for Exploits)
  *   **ESP (Stack Pointer):** Points to the *top* of the current Stack.
  *   **EBP (Base/Frame Pointer):** Points to the *bottom* (base) of the current function's Stack Frame.
  *   **ESI / EDI:** Source and Destination Indexes, used for copying chunks of data (like strings).
- ### 3. The "Keys to the Kingdom" Registers
  *   **EIP (Instruction Pointer):** *This is the most important register in computer security.* It holds the memory address of the **next instruction** the CPU is supposed to execute. If a hacker can change the value of EIP, they can force the CPU to run malicious code instead of the real program.
  *   **EFLAGS:** A register where each individual bit is a "flag" (True/False) that tells you the result of the last math operation (e.g., Did the last subtraction result in a Zero?).
- ### 4. Register Slicing (Slide 31)
  Registers are backwards-compatible. In a 32-bit system, the 32-bit register is **EAX**.
  *   If you only want the lower 16 bits, you call it **AX**.
  *   If you want the upper 8 bits of AX, you call it **AH** (High).
  *   If you want the lower 8 bits of AX, you call it **AL** (Low).
  *   *64-bit Evolution (Slide 33):* In modern 64-bit CPUs, the registers are expanded to 64 bits and prefixed with an 'R' (**RAX, RBX, RSP, RIP**, etc.), plus they added 8 new registers (R8 through R15).
- ## IV. Basic Assembly Syntax (Slides 34–36)
  There are two ways to write Assembly: **Intel** syntax and **AT&T** syntax. Linux tools (like `objdump` or `gcc`) default to **AT&T syntax**, which this course uses.
  
  **AT&T Syntax Rules:**
  1.  **Order:** `<instruction> <source>, <destination>` 
    *(Intel is backwards: dest, src).*
  2.  **Registers:** Always prefixed with a `%` (e.g., `%eax`).
  3.  **Immediates (Constants):** Always prefixed with a `$` (e.g., `$5`).
  4.  **Examples:**
    *   `mov %ebx, %eax` ➔ Move the value inside EBX into EAX. (EAX = EBX)
    *   `add %ebx, %eax` ➔ Add EBX to EAX, store result in EAX. (EAX = EAX + EBX)
  
  ---
- ### Study Questions for Part 1
  1.  **Pipeline:** If a programmer makes a syntax error in a `#define` macro, which stage of the compilation pipeline (cpp, cc, as, ld) will process it first?
  2.  **Architecture:** Why is the **EIP** (or RIP in 64-bit) register considered the ultimate target for a control hijacking attack?
  3.  **Syntax Check:** Translate the following AT&T assembly instruction into plain English: `sub $8, %esp` (Hint: Think about what `%esp` stands for and what the `$` means).
  
  ---