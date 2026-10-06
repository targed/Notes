## I. The Mechanisms of Protection (Slides 7–8)
To stop a malicious program (like a worm) or a buggy program from corrupting the system, we use three primary mechanisms:
1.  **Hardware:** The CPU itself checks memory access (Memory Protection).
2.  **Software:** Compilers and interpreters restrict what a program can do (Sandboxing / Software Fault Isolation).
3.  **Hardware + Software:** Hypervisors creating entire Virtual Machines.

This section focuses entirely on **Hardware Memory Protection**. The goal is *Logical Separation*: multiple programs share the same physical RAM chips, but the hardware forces them to stay in their own designated lanes.
- ## II. The Evolution of Memory Isolation (Slides 9–13)
  How does the CPU actually stop Program A from reading Program B's memory? It evolved in stages.
- ### 1. The Fixed Fence (Slide 9)
  *   **Concept:** The simplest protection. The OS sits at address `0` to `n`. User programs sit from `n+1` to `high`. The CPU has a hardcoded rule: "User programs cannot access addresses less than `n`."
  *   **The Flaw:** It is rigid. If the OS needs an update and gets larger, the fence must be moved. Furthermore, if you have *two* user programs, there is no fence between them. Program A can easily overwrite Program B.
- ### 2. The Variable Fence / Limit Register (Slide 10)
  *   **Concept:** Instead of a hardcoded fence, the CPU uses an "Address Limit Register." The OS can change this register's value dynamically. 
  *   **The Flaw:** This is still just a "One-way protection." It protects the OS from the users, but provides **no protection within user program space**.
- ### 3. Base and Bounds Registers (Slide 11)
  This was a massive leap forward. Every process gets *two* hardware registers:
  *   **Base Register:** The starting address of the program (e.g., `1000`).
  *   **Bounds Register:** The ending address or length of the program (e.g., `2000`).
  *   **How it works:** Before the CPU executes *any* memory read/write, it checks: `Is Base <= Address <= Bounds?` If not, the CPU throws a fatal error (Segmentation Fault).
  *   **The Context Switch:** When the OS pauses Program A to run Program B, the OS swaps out the values in the Base/Bounds registers. Now, Program B is locked into its own space.
  *   **The Flaw:** While programs are now protected *from each other*, a program can still hurt *itself*. A buffer overflow can still overwrite the program's own code because the entire program lives in one big Read/Write/Execute bucket.
- ### 4. Multiple Base/Bounds Registers (Slides 12–13)
  To stop a program from hurting itself, we split the program into parts and give each part its own Base/Bounds pair:
  *   **Code Base/Bounds:** Marked as **Read/Execute Only**.
  *   **Data Base/Bounds:** Marked as **Read/Write Only**.
  *   *Security Payoff:* This directly mitigates classic buffer overflows! If an attacker injects shellcode into the Data segment (a buffer) and tries to execute it, the CPU looks at the Data Bounds register, sees it does not have "Execute" permission, and kills the program.
- ## III. Segmentation vs. Paging (Slides 14–17)
  As programs got more complex, dealing with simple Base/Bounds registers wasn't enough. Operating Systems needed better ways to manage memory.
- ### 1. Segmentation (Slide 14)
  *   **Concept:** Divide the program into logical, named pieces (e.g., The "Main" segment, the "LibC" segment, the "Stack" segment).
  *   **Security Pro:** Excellent for protection because permissions map perfectly to logical functions. The Stack segment gets Read/Write. The Code segment gets Read/Execute.
  *   **Performance Con (Fragmentation):** As different-sized segments are loaded and unloaded from RAM, you get "External Fragmentation" (Swiss-cheese memory). You might have 100MB of free RAM, but it's broken into tiny 1MB chunks, so a 5MB segment cannot load. Boundary checking is also mathematically slower for the CPU.
- ### 2. Paging (Slide 15)
  *   **Concept:** Ignore the "logical" chunks. Just chop all of RAM into completely uniform, fixed-size blocks called **Pages** (usually 4KB each). 
  *   **Performance Pro:** Completely eliminates external fragmentation. Any 4KB page of a program can fit into any free 4KB slot in physical RAM.
  *   **Security Con:** Logical segmentation isn't preserved. A single 4KB page might accidentally contain the end of the executable code *and* the beginning of a writable data buffer. You have to mark the whole 4KB page as Read/Write/Execute, which weakens security.
- ### 3. The Modern Compromise (Slides 16–17)
  *   **Intel x86 Paged Segmentation:** Historically, Intel tried to combine both—dividing the program into logical segments, and then chopping those segments into 4KB pages.
  *   **The Reality Today:** It was too complex. **Modern Windows and Linux rely almost entirely on Paging** and have effectively abandoned Segmentation. 
    *   *How do they handle security?* Modern CPUs added specific protection bits to the Page Tables (like the **NX Bit** - No eXecute), allowing the OS to mark specific 4KB pages as non-executable, bridging the security gap left by abandoning segmentation.
  
  ---
- ### Study Questions for Part 2
  1.  **Hardware Checks:** During a Context Switch from Program A to Program B, what must the Operating System do to the CPU's Base and Bounds registers to ensure memory protection is maintained?
  2.  **Mitigating Exploits:** Explain how separating a program into a "Code Base" and a "Data Base" (Slide 12) directly interferes with a hacker trying to run shellcode injected via a buffer overflow.
  3.  **Memory Management:** Why did modern operating systems (Linux/Windows) choose to adopt **Paging** rather than strict **Segmentation**, despite Segmentation offering arguably cleaner logical security boundaries? (Hint: Think about memory fragmentation).
  
  ---