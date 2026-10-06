## I. The Ethics and Motivation (Slide 2)
Reverse engineering is a dual-use skill. 
*   **The Good (White Hat):** 
  *   *Malware Analysis:* Security researchers reverse-engineer viruses and ransomware to understand how they spread and how to build decryption tools or antivirus signatures.
  *   *Legacy Code:* Understanding old, undocumented systems where the original source code was lost.
  *   *Interoperability:* Figuring out how a proprietary protocol works so you can build compatible software (e.g., Samba reverse-engineering the Windows file-sharing protocol).
*   **The Evil (Black Hat):**
  *   *Cracking:* Removing DRM (Digital Rights Management), license checks, or 30-day trial restrictions.
  *   *Finding Exploits:* Searching for zero-day vulnerabilities (like hidden buffer overflows) to exploit.
  *   *Game Cheating:* Creating aimbots or wallhacks by analyzing how a game stores player locations in memory.
- ## II. The Tools of the Trade (Slides 3–5)
  Reverse engineers use a combination of static and dynamic tools to understand a binary.
- ### 1. Static Analysis (Looking at the code at rest)
  *   **Disassemblers:** Translate raw machine code (1s and 0s) back into Assembly language (`mov`, `push`, `jmp`). 
    *   *Examples:* **IDA Pro** (the industry standard), **Ghidra** (a powerful, free tool released by the NSA), **objdump** (Linux command line).
    *   *Pros/Cons:* Gives a perfect view of the logic, but can be overwhelming and easily confused if the attacker intentionally inserts "junk data" to break the disassembly tool.
  *   **Decompilers:** Attempt to take Assembly and turn it back into high-level C, Java, or Python source code.
    *   *Expansion:* Decompiling C/C++ is notoriously difficult because compiler optimizations destroy the original structure and variable names. However, decompiling "managed" languages like Java or Python is usually highly successful because they compile to intermediate "bytecode" that retains a lot of structure.
  *   **Hex Editors:** Allow direct viewing and manipulation of the raw hexadecimal bytes of a file.
- ### 2. Dynamic Analysis (Looking at the code in motion)
  *   **Debuggers:** Allow the analyst to run the program step-by-step, pause execution (using Breakpoints), and inspect the exact values in CPU registers and RAM at any given millisecond.
    *   *Examples:* **GDB** (Linux), **WinDBG** or **OllyDbg** (Windows).
  *   **Monitoring Tools:** Instead of looking at CPU registers, monitors sit at the OS layer and watch what the program *asks the OS to do*.
    *   *Examples:* **strace** (Linux: traces System Calls), **ltrace** (Linux: traces Library Calls), **Process Monitor** (Windows).
- ## III. Practical Examples of Reverse Engineering (Slides 6–10)
  The slides walk through three distinct examples of how RE is applied.
  
  1.  **Python Decompilation (Slide 6):** Python compiles to `.pyc` files. Using tools like "Easy Python Decompiler," an analyst can almost perfectly recover the original `cryptotest.py` source code.
  2.  **Minecraft Modding (Slide 7):** Minecraft is written in Java but does not officially support mods. The entire modding community exists because tools like the Mod Coder Pack (MCP) reverse-engineer and decompile the `.jar` files, allowing modders to inject their own code and "reobfuscate" it back into the game.
  3.  **The `strace` Shortcut (Slides 8–10):** 
    *   *Scenario:* A program (`do_thing`) prints "You are not authorized." You need to figure out how to authorize yourself.
    *   *The Hard Way:* Open a disassembler, read thousands of lines of assembly, and trace the logic.
    *   *The Smart/Cheap Way:* Run `strace ./do_thing`. The monitor reveals the program made an `openat()` system call looking for a file named `.hidden_authorization_file` and got an `ENOENT` (File Not Found) error. 
    *   *The Fix:* You simply `touch .hidden_authorization_file` to create it, run the program again, and you bypass the security check! *(Note: If the program actually needed specific data INSIDE that file, you would then have to use a debugger to figure out what data it was looking for).*
- ## IV. Anti-Reverse Engineering Defenses (Slide 11)
  When developers (or malware authors) know people will try to reverse-engineer their code, they implement defenses to make the analyst's life miserable.
  
  *   **Basics (Stripping):** Removing "Debug Symbols." Symbols are the variable and function names used by developers (e.g., `check_password()`). Stripping them replaces them with meaningless memory addresses (e.g., `sub_804841d`), forcing the analyst to figure out what the function does from scratch.
  *   **Anti-Disassembly:** 
    *   *Encryption/Packing:* The actual code is encrypted on disk. It only decrypts itself in RAM right before it runs, rendering static disassemblers useless.
    *   *Junk Code:* x86 instructions are variable-length (between 1 and 15 bytes). If an attacker inserts a single junk byte that looks like the start of a new instruction, the disassembler will misalign and output completely fake assembly code.
  *   **Anti-Debugging:** The program constantly checks if a debugger is attached. For example, it might check how long an instruction took to execute. If it took 5 seconds instead of 1 microsecond, it knows a human is "stepping through" it in a debugger, and it will intentionally crash or alter its behavior.
  *   **Obfuscation:** Intentionally writing "spaghetti code." Adding thousands of lines of useless math equations or unreachable code just to waste the reverse engineer's time.
  
  ---
- ### Study Questions for Part 1
  1. **Tool Selection:** You are analyzing a piece of malware and you want to know exactly what IP addresses it reaches out to and what files it tries to open on the victim's hard drive, but you don't want to read assembly code. What kind of dynamic tool should you use?
  2. **Decompilation:** Why is it much easier for a reverse engineer to decompile a Java program (like Minecraft) compared to a C++ program (like a Windows OS kernel driver)?
  3. **Anti-Debugging:** How does measuring the time it takes to execute a block of code help a piece of malware realize it is being analyzed by a human using GDB?
  
  ---
-