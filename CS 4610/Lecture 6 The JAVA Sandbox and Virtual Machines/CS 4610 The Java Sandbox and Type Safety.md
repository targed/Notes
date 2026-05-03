## I. The Transition: Why not just fix C? (Slides 2–4)
Before diving into Java, the slides recap the "Separation Design Space" and ask a crucial discussion question: *Why do buffer overflows happen in C in the first place?*

**The "Bonus Question" Answered (Slide 4):**
Buffer overflows in C are a "perfect storm" of three design flaws:
1. **Design flaw in C:** C is **not type-safe** and has no built-in boundary checking. It trusts the programmer implicitly. If you want to write 100 bytes into a 10-byte array, C will let you do it using direct memory pointers.
2. **Design flaw in the calling convention:** The `cdecl` convention stores the critical **Return Address** (control flow data) on the exact same stack as the local variables (user data). Mixing control data and user data is always a security risk.
3. **Design flaw in x86 architecture:** Historically, x86 execution stacks grew downwards, while buffers filled upwards, creating a direct physical path for buffers to overwrite return addresses. Furthermore, early x86 architectures didn't differentiate between "data" and "executable code" in memory (pre-NX bit/DEP).

**The Solution:** Instead of trying to patch C, environments like Java enforce **Isolation** and **Type Safety** from the ground up.
- ## II. The Java Ecosystem (Slide 5)
  To understand the Java Sandbox, you must understand how Java is packaged:
  *   **Java API / Core Classes:** The fundamental libraries (`java.lang`, `java.io`). Programs use these to interact with the system instead of making direct OS system calls.
  *   **JVM (Java Virtual Machine):** The software that actually executes Java Bytecode. It acts as an abstraction layer over the physical hardware.
  *   **JRE (Java Runtime Environment):** `JVM + Core Classes`. This is what a standard user installs to *run* Java programs.
  *   **JDK (Java Development Kit):** `JRE + Compiler (javac) + Debugger`. This is what a developer installs to *write* Java programs.
- ## III. The JVM Sandbox Architecture (Slides 6–8)
  Java programs cannot touch the Operating System or hardware directly. They run completely inside the JVM. The JVM protects the host OS using three core components:
  
  1. **The Classloader:**
    *   Loads classes into memory.
    *   *Security Role:* It ensures **Namespace Separation**. It guarantees that an untrusted applet downloaded from the internet cannot replace a core Java class (like `java.lang.String`) with its own malicious version.
  2. **The Bytecode Verifier:**
    *   *Security Role:* Before a single line of code is executed, the verifier checks the compiled `.class` files. It ensures the code doesn't contain illegal instructions, doesn't forge pointers, and respects access rules (like `private` vs `public` variables). 
    *   *Why it's needed:* A hacker could use a maliciously modified compiler to create evil Bytecode. The verifier catches this *before* runtime.
  3. **The Security Manager:**
    *   *Security Role:* This is the **Runtime Reference Monitor**. Whenever the Java program wants to do something dangerous (like open a file or connect to the internet), the Java API asks the Security Manager for permission. If denied, it throws a `SecurityException`.
- ## IV. Type Safety: The Cornerstone of Java Security (Slides 9–10)
  **Type Safety** means a program can only perform operations on an object that are strictly defined for that object's type.
  
  *   **No Pointers / No Forging:** In C, you can create a pointer to an arbitrary memory address and read it. In Java, memory addresses are hidden. You only have "References" to objects, and you cannot mathematically alter a reference (e.g., `reference + 1` is illegal in Java).
  *   **The C Counter-Example (Slide 10):**
    ```c
    unsigned char a = 1;
    unsigned char b = 2; 
    *(&b-1) = 10; // THIS IS TERRIBLE
    ```
    In C, `*(&b-1)` looks at the memory address of `b`, moves backward one byte, and writes the number `10`. Since `a` is stored right next to `b`, this actually overwrites the value of `a`. Java entirely forbids this. You cannot access memory outside the bounds of the specific variable or array you are referencing.
- ## V. Evolution of the Trust Model (Slides 12–13)
  Java was originally famous for "Applets" (small Java programs embedded in web pages). The security model had to evolve to handle these safely:
  
  *   **JDK 1.0.2 (The Black & White Model):**
    *   Local Applications = Fully Trusted.
    *   Web Applets = Fully Untrusted (trapped in a strict sandbox).
  *   **JDK 1.1 (The Signed Model):**
    *   Introduced digital signatures. If an applet was signed by a trusted entity (like Microsoft or IBM), it was given full access. Unsigned applets remained in the sandbox.
  *   **JDK 1.2 and beyond (Granular Control):**
    *   Introduced the concept of "Shades of Gray." Instead of "all or nothing," you can assign specific permissions based on exactly where the code came from and who signed it. (We will cover this deeply in Part 2).
  
  **What Untrusted Code CANNOT Do (Slide 13):**
  To protect the host, untrusted code in the sandbox is strictly forbidden from:
  *   Reading, writing, or deleting files on the user's hard drive.
  *   Connecting to any network IP address *except* the exact server it was downloaded from (Same-Origin Policy).
  *   Making critical system calls like `System.exit()` (which would kill the JVM).
  *   Creating its own Security Manager to bypass the rules.
  
  ---
- ### Study Questions for Part 1
  1. **Architecture Flaws:** How does the `cdecl` calling convention inherently violate the principle of "Separation" between control data and user data?
  2. **JVM Components:** If a malicious developer writes a Java program, compiles it, and then uses a hex editor to manually alter the `.class` bytecode to bypass an array bounds check, which JVM component is responsible for catching this before the program runs?
  3. **Type Safety:** Look at the C code on Slide 10. Why is the ability to do pointer arithmetic (`&b - 1`) considered a violation of Type Safety? 
  
  ---