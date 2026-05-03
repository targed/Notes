## I. The Core Problem: Who is asking? (Slide 14 & 36)
When a dangerous request is made (like `File.createNewFile()`), the Security Manager must intercept it. 
*   **The Difficulty:** The code actually executing the file creation is a trusted `java.io.File` class from the Java API. If the Security Manager just asks, "Are you allowed to do this?", the `File` class will say "Yes, I'm system code!"
*   **The Solution:** The Security Manager cannot just look at the *current* class; it must look at the **entire history of who called who** to get to this point. It does this via **Stack Inspection**.
- ## II. The Stack Inspection Operations (Slide 15)
  To manage permissions dynamically, trusted Java code can annotate its own stack frame using specific operations before making a call:
  
  1.  **`enablePrivilege(P)`**: The trusted code asserts its authority. It says: *"I am a trusted system class. I am about to do something dangerous, but I have verified it is safe. Trust me, and ignore who called me."*
  2.  **`disablePrivilege(P)`**: The trusted code drops its privileges. It says: *"I am about to execute an untrusted applet. Prevent this applet from using my high-level permissions to do bad things."*
  3.  **`revertPrivilege(P)`**: Undoes the previous `enable` or `disable` annotation on that specific stack frame.
  4.  **`checkPrivilege(P)`**: The actual test performed by the Security Manager. It searches the stack from the **newest frame to the oldest frame** (tracing backwards in time).
- ## III. The Stack Inspection Algorithm (Slides 16–32)
  When `checkPrivilege(P)` is called, the JVM looks at the current active thread's call stack and searches downwards (from the most recently called function back up to `main()`). 
  
  **The Search Rules:**
  *   If it hits an **untrusted frame**, access is instantly **DENIED** (unless a trusted frame previously "enabled" the privilege).
  *   If it hits a frame with an **"enable"** tag, access is instantly **GRANTED**.
  *   If it hits a frame with a **"disable"** tag, access is instantly **DENIED**.
- ### Case Studies in Stack Inspection
  *   **Case I (Slides 18-21): The Normal Allowed Access**
    *   Trusted Code calls `enablePrivilege()`.
    *   Trusted Code then calls `File.createNewFile()`.
    *   `checkPrivilege()` looks at the stack. The very first flag it sees (looking backwards) is "enable." **Access Granted.**
  *   **Case II (Slides 22-25): The Preemptive Block**
    *   Trusted Code calls `enablePrivilege()`, but then realizes it needs to run a sketchy function, so it calls `disablePrivilege()`.
    *   The code calls `File.createNewFile()`.
    *   `checkPrivilege()` looks backwards. The first flag it hits is "disable." **Access Denied!** (Even though an "enable" is further back on the stack, the algorithm stops at the *first* flag it finds).
  *   **Case III (Slides 26-30): The Revert**
    *   Trusted Code calls `enable`, then `disable`, then changes its mind and calls `revertPrivilege()`.
    *   `revert` wipes the `disable` tag off the stack frame.
    *   When `checkPrivilege()` looks backwards, the `disable` tag is gone, so it hits the `enable` tag. **Access Granted.**
  
  *Expanded Context on Slide 31:* What happens if the algorithm searches the entire stack and falls off the end without hitting *any* tags? Early implementations (Netscape) defaulted to "Deny" (fail-safe). Modern implementations (Sun JDK) default to "Allow", assuming that if the absolute root of the thread is a trusted system process, it implicitly has permission.
- ## IV. Java 2 Security Policy: The "Shades of Gray" (Slides 33–35)
  As Java evolved (JDK 1.2+), the binary "Trusted vs. Untrusted" model was replaced by a highly granular, configurable **Security Policy**.
- ### 1. Identity (Who are you?)
  In Java 2, a program's identity is defined by its `CodeSource`, which consists of two things:
  *   **Origin (`CodeBase`):** Where did the code come from? (e.g., `https://www.rstcorp.com/users/gem`). Code from an internal corporate server is trusted more than code from a random public IP.
  *   **Signature (`SignedBy`):** Who digitally signed the code? (e.g., verified by VeriSign or a corporate IT certificate).
- ### 2. Permissions (What can you do?)
  Permissions are specific objects that map to exact actions. Examples include:
  *   `FilePermission` (Read, Write, Delete specific directories)
  *   `SocketPermission` (Connect, Listen, Accept on specific ports)
  *   `RuntimePermission` (Exit the VM, load dynamic libraries)
- ### 3. The Policy File
  System administrators write a configuration file that maps **Identities** to **Permissions**. 
  ```java
  grant CodeBase "https://www.rstcorp.com/*" SignedBy "IT_Dept" {
    permission java.io.FilePermission "/tmp/scratch/*", "read,write";
  };
  ```
  *Note on Additive Permissions (Slide 35):* If Policy A says you can "read", and Policy B says you can "write", the JVM combines them. You do not lose permissions if multiple policies apply to you; you gain the sum of all of them.
  
  ---
- ### Study Questions for Part 2
  1. **The Confused Deputy:** Why is it not enough for the Java Security Manager to solely check the permissions of the class that actually executes the `createNewFile()` system call? 
  2. **Stack Search Order:** When `checkPrivilege()` is invoked, why does the algorithm search the stack from the *newest* frame to the *oldest* frame, rather than the other way around? 
  3. **Privilege Operations:** If a highly trusted Java application is about to download and execute an unknown third-party plugin, which Stack Inspection Operation should it call immediately before executing the plugin to protect the host system?
  
  ---