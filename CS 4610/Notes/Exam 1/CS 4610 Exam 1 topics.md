### Scope of the Exam: Which Chapters?
**The exam covers the first 4 modules/chapters.** 
It goes from basic Security Concepts up to Buffer Overflows and Defenses. 

Here is how we know:
*   **Included:** Intro/Concepts (CIA Triad), Secure System Design (Simple Web Server), Malware basics (Virus, Worm, Trojan), and Low-Level execution (x86, Stack, Buffer Overflows, Integer Overflows, Format Strings, Defenses).
*   **Explicitly Excluded:** The **Aho-Corasick Algorithm** (from the Malware/Anti-virus chapter).
*   **Not Covered:** **Chapter 5 (Protection via Separation, Java Sandbox, VMMs, Covert Channels)**. There is absolutely no mention of Java, virtual machines, or software fault isolation in the midterm review deck. You do not need to study the 5th slide deck for this exam.

---
- ### Topics List/Study Guide
- #### 1. Code Explanation & Defense (The Heavy Hitters)
  *Your professor explicitly said these will be on the exam. Be ready to write paragraphs or annotate code for these.*
  
  *   **Simple Web Server (SWS) DoS Vulnerability 1 (Malformed Request):**
    *   *The Vulnerability:* Input validation / Unhandled Exception.
    *   *The Line of Code:* `Command = st.nextToken();` 
    *   *The Problem:* If an attacker sends an empty string (like a carriage return `\r\n`), `br.readLine()` reads it, but `StringTokenizer` finds no tokens. Calling `nextToken()` throws an exception. Because there is no error handling, the entire server program crashes (Denial of Service).
    *   *The Fix:* Wrap the parsing code in a `try/catch` block. Catch the exception, write a `400 Bad Request` response, and close the socket so the server stays alive.
  *   **Simple Web Server (SWS) DoS Vulnerability 2 (`/dev/random`):**
    *   *The Vulnerability:* Resource Exhaustion (DoS).
    *   *The Line of Code:* `while (c != -1) { osw.write(c); ... }` (or `sb.append((char)c);`).
    *   *The Problem:* If an attacker requests a special Linux device file like `/dev/random`, the file has a reported length of 0, but it outputs an infinite stream of random data. The `while` loop never reaches EOF (`-1`), so the server gets stuck forever serving one attacker, denying service to everyone else.
    *   *The Fix:* Impose a strict **Max Download Limit** variable (`sentBytes < MAX_DOWNLOAD_LIMIT`). 
  *   **Return-to-Libc Exploit:**
    *   *The Problem it Solves:* Modern systems use DEP/NX bits (Non-Executable Stacks) to prevent hackers from running their own injected shellcode.
    *   *How it works:* Instead of injecting code, the attacker overflows the buffer and overwrites the Return Address with the memory address of a standard C library function that is *already loaded in memory* (like `exec()` or `system()`). They then place fake arguments (like `"/bin/sh"`) further down the stack so the library function executes a terminal shell.
  *   **Homework 2 - Pointer Manipulation (Stack Hijacking):**
    *   Understand how you used pointer arithmetic (e.g., `p[6] = (int)fun2;`) to overwrite the return address without a direct function call. Know that `[ebp+4]` holds the return address, and `[ebp+8]` holds the first argument for the next function.
- #### 2. The Stack & Function Calls (Crucial for Buffer Overflows)
  *You must know exactly how the stack changes during a function call.*
  
  *   **Stack Geography:** The stack grows **downward** (from high addresses to low addresses). Buffers (arrays) fill **upward** (from low to high). This is why a buffer overflow walks right up into the saved pointers.
  *   **The Call Chain:** 
    *   When `main` calls `func`: The CPU pushes the **Return Address** (EIP) onto the stack, then jumps to `func`.
  *   **The Function Prologue:** 
    1.  `push %ebp` (Saves the caller's Base Pointer).
    2.  `mov %esp, %ebp` (Sets the new frame).
    3.  `sub $X, %esp` (Allocates space for local variables).
  *   **What happens when a function returns? (The Epilogue):**
    *   `leave` instruction: Executes `mov %ebp, %esp` (clears local vars) and `pop %ebp` (restores caller's base pointer).
    *   `ret` instruction: **Pops the top of the stack into the Instruction Pointer (EIP).** *This is exactly where the control hijacking happens in a buffer overflow.*
- #### 3. x86 Execution & Memory Addressing (For T/F & Multiple Choice)
  *   **Compilation Steps:** Preprocessor (`cpp`) $\rightarrow$ Compiler (`cc`) $\rightarrow$ Assembler (`as`) $\rightarrow$ Linker (`ld`).
  *   **Registers:** 
    *   `EAX` (Math/Return values), `EBX`, `ECX`, `EDX`.
    *   `ESP` (Stack Pointer), `EBP` (Base Pointer - holds the memory address of the caller's saved EBP).
    *   `EIP` (Instruction Pointer - Cannot be changed directly, only via Jumps/Calls).
    *   `EFLAGS` (Condition codes updated by `cmp` instructions to determine if a jump happens).
  *   **Byte Addressing (Little Endian):** *He explicitly said this will be on the exam!* (Review Quiz 3).
    *   x86 stores the "Least Significant Byte" at the lowest memory address.
    *   If memory addresses 1, 2, 3, 4 hold `0xAA`, `0xBB`, `0xCC`, `0xDD`, and you read 4 bytes starting at address 1 into `%eax`, the register reads it backward: **`0xDDCCBBAA`**.
  *   **Instruction Syntax (AT&T):** `mov <source>, <destination>`. 
    *   `mov $3, %eax` = Put literal number 3 into EAX.
    *   `mov 3, %eax` = Go to memory address 3, read it, put it in EAX.
    *   `mov (%ebx), %eax` = Dereference the address stored in EBX.
    *   `lea (%ebx), %eax` = Load Effective Address (just does math/copies the pointer, doesn't read RAM).
- #### 4. Definitions & Conceptual Understanding
  *   **Authentication vs. Authorization:**
    *   *Authentication:* Identity Verification (Who are you?). 3 Factors: Know (Password), Have (Token/Phone), Are (Biometrics).
    *   *Authorization:* Checking permissions (What can you do?). Example: Access Control Lists (ACLs).
  *   **CIA Triad + 2:**
    *   *Confidentiality:* Keeping data secret (Encryption).
    *   *Integrity:* Ensuring data isn't modified (Hashing, MACs).
    *   *Availability:* System uptime/access (Defending against DoS).
    *   *Accountability:* Tracing actions to a specific user (Secure Logging).
    *   *Non-Repudiation:* Undeniable proof of an action (Digital Signatures).
  *   **Malware Classifications:**
    *   *Virus:* Needs a host file and user interaction to spread.
    *   *Worm:* Standalone, self-replicating over a network. **Needs NO user interaction.** (Quiz 2 question).
    *   *Trojan:* Disguised as legitimate software. Does not replicate itself.
  *   **Other Vulnerabilities:**
    *   *Integer Overflow:* `unsigned char` (8 bits) wraps around from 255 back to 0. Can bypass size checks (e.g., `12 + 254 = 10` in 8-bit math).
    *   *Format String Bug:* Using `printf(user_input)` allows an attacker to input `%x` to read raw stack memory or `%n` to write to memory.
- #### 5. Defense Mechanisms
  *   **StackGuard / Canaries:** A random string (or a string of terminators like `\0`) placed between the local variables and the Return Address. If a buffer overflows, it destroys the canary. The function checks the canary before `ret` and crashes safely if it was modified. *Bypassed only if the attacker can leak the random canary value first.*
  *   **ASLR (Address Space Layout Randomization):** Randomizes where the Stack, Heap, and Libraries load into memory so the attacker cannot hard-code jump addresses.
  *   **DEP (Data Execution Prevention) / NX Bit:** Marks the stack as non-executable.
- ### Tips for the Exam Format
  *   **10 True/False & 4-5 Multiple Choice:** Expect trick questions pulled directly from the quizzes. (e.g., "The caller's EBP and `%ebp` are the same thing" $\rightarrow$ FALSE. "Worms spread through physical media" $\rightarrow$ FALSE).
  *   **3 Definitions:** Pick from the CIA triad, Authentication/Authorization, or the Malware definitions. Keep them concise (2-3 sentences).
  *   **4 Code Explanations:** 
    1. SWS exception crash.
    2. SWS `/dev/random` crash.
    3. Tracing a stack frame (like your HW2).
    4. Explaining a Return-to-Libc attack or how a Canary stops an attack.