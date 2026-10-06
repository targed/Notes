## I. The Need for Software Protection (Slides 18–19)
Hardware memory protection is great, but it is "heavy." Switching between two completely different hardware-isolated processes (a Context Switch) takes a lot of CPU time. 

*   **The Problem:** Modern applications rely heavily on third-party extensions.
  *   A web browser uses plugins (like a PDF viewer).
  *   A media player uses external codecs (like an MP4 decoder).
  *   An OS kernel loads third-party device drivers (like a graphics card driver).
*   **The Goal of SFI:** If a buggy codec crashes, it shouldn't take down the entire media player. We must **confine faults** inside the distrusted extension. However, because the media player needs to send video frames to the codec thousands of times a second, we need **efficient cross-domain calls**. Hardware isolation is too slow for this; we must enforce the isolation in *software*.
- ## II. Software Fault Isolation (SFI) Core Idea (Slides 20–22)
  **SFI** (often called Sandboxing) is essentially memory protection implemented by rewriting the software code itself.
  
  1.  **Fault Domains:** The main application allocates a specific "chunk" of memory (a contiguous space) for the untrusted plugin. This is its Fault Domain.
  2.  **The Identifier:** The domain is given a unique ID, which corresponds to the top bits of the memory addresses in that chunk (e.g., all addresses starting with `010...` belong to Domain 2).
  3.  **Rewriting the Code:** Before the untrusted plugin is allowed to run, an SFI compiler analyzes its binary code. It forcibly inserts extra security instructions before *every single* memory read, write, or jump. 
  
  *Real-World Example:* **Google's NativeClient (NaCl)** used SFI to allow developers to run highly efficient C/C++ code directly inside the Chrome web browser without letting that code access the rest of your computer.
- ## III. Two SFI Enforcement Schemes (Slides 23–25)
  How do we actually rewrite the code to enforce this? The slides present two different mathematical approaches: **Segment Matching** and **Sandboxing**.
- ### Scheme 1: Segment Matching (Slide 23)
  *   **Mechanism:** It explicitly checks if you are allowed to go there.
    1.  Take the target memory address.
    2.  Bitwise shift (`>>`) it to extract the top identifier bits.
    3.  Compare those bits to the dedicated Segment Register (`sr`).
    4.  **If they do not match, trap (throw a fatal error) immediately.**
  *   **Pros:** Pinpoints the *exact* moment and instruction where the plugin tried to do something illegal. Great for debugging.
  *   **Cons:** Slower. It requires more CPU instructions (shifting, comparing, and conditional branching).
- ### Scheme 2: Sandboxing (Slide 24)
  *   **Mechanism:** It doesn't check; it just forces compliance.
    1.  Take the target memory address.
    2.  Use a bitwise AND (`& mask`) to strip away the top bits.
    3.  Use a bitwise OR (`| sr`) to slap the correct, safe top bits back onto the address.
    4.  Execute the memory access. 
  *   **Pros:** Much faster. Fewer instructions, and no `if/else` branching to slow down the CPU pipeline.
  *   **Cons:** If the plugin calculates a malicious address, the Sandboxing scheme will rewrite it to a random "safe" address inside the plugin's own domain. The plugin will overwrite its own data and probably crash. But as the slide says: **"Crash is ok."** The goal is to protect the *main application*, not to keep the buggy plugin alive.
- ## IV. The Concept of Reference Monitors (Slide 26)
  A **Reference Monitor** is an abstract security concept. It is the "bouncer at the door" that enforces access control policies. It must be tamper-proof, always invoked, and small enough to be thoroughly verified.
  
  *   **External Reference Monitor:** Enforced by a separate entity. 
    *   *Example:* The Hardware/OS Memory Protection we studied in Part 2. The OS sits outside the user program and uses hardware (Base/Bounds, Paging) to enforce the rules.
  *   **Inlined Reference Monitor (IRM):** Enforced from within.
    *   *Example:* SFI. The security checks (Segment Matching or Sandboxing) are woven ("inlined") directly into the untrusted program's instruction flow.
  
  ---
- ### Study Questions for Part 3
  1.  **SFI vs. Hardware:** If hardware-based memory protection (like Paging) is so secure, why do developers use Software Fault Isolation (SFI) to secure web browser plugins? (Hint: Think about the performance cost of communication).
  2.  **SFI Schemes:** Between "Segment Matching" and "Sandboxing", which scheme is faster for the CPU to execute, and what is the trade-off of using that faster method?
  3.  **Reference Monitors:** If an antivirus program scans an executable and actively injects security checks into the application's binary code before allowing it to run, is this acting as an *External* Reference Monitor or an *Inlined* Reference Monitor?
  
  ---