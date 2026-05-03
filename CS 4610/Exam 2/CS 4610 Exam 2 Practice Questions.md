## Lecture 5: Protection via Separation Practice Questions

**Question 1 (True/False)**
Using a single pair of Base and Bounds registers is sufficient to prevent a program from overwriting its own executable code during a buffer overflow attack.

**Question 2 (Multiple Choice)**
Two distinct applications running on the same server are separated by storing their respective data using different AES-256 encryption keys. Which method of separation does this primarily represent?
A) Physical Separation
B) Temporal Separation
C) Logical Separation
D) Cryptographic Separation

**Question 3 (Short Answer)**
Modern operating systems provide robust hardware-based memory protection (like Paging and Virtual Memory). Explain why developers still use Software Fault Isolation (SFI) to secure web browser plugins or media codecs instead of just relying on OS-level hardware protection.

**Question 4 (True/False)**
In the SFI "Sandboxing" scheme, if an untrusted plugin attempts to jump to a malicious memory address outside its fault domain, the system will immediately trap and halt the execution before the jump occurs, providing precise debugging information.

**Question 5 (Multiple Choice)**
Which of the following is the primary reason why modern operating systems (like Linux and Windows) favor Paging over strict Segmentation for memory management and protection?
A) Paging allows logical boundaries of a program to be perfectly preserved.
B) Paging eliminates external memory fragmentation by using fixed-size blocks.
C) Paging requires fewer CPU registers to execute than segmentation.
D) Segmentation cannot be used to separate Code and Data.

**Question 6 (Short Answer)**
Early operating systems used a single "Limit Register" (a variable fence) that separated the OS memory from the user program memory. Give the major security flaw with this "one-way" protection model in a multiprogramming environment.

**Question 7 (True/False)**
In Software Fault Isolation (SFI), the fault domain of an untrusted extension is identified by a unique identifier stored in the *lowest* (least significant) bits of its memory addresses.

**Question 8 (Multiple Choice)**
A Digital Rights Management (DRM) system allows a user to open and read a PDF document but specifically blocks the user from printing or copying text from it. This represents which degree of separation?
A) Binary sharing
B) Limited sharing
C) Complete isolation
D) Usage control

**Question 9 (Short Answer)**
Contrast an "External Reference Monitor" with an "Inlined Reference Monitor" (IRM). Provide one example of each from the lecture.

**Question 10 (Code / Logic Explanation)**
In the SFI Sandboxing scheme, the system uses two bitwise operations to rewrite a memory address before allowing an untrusted plugin to access it: 
1. `dr = (addr & mask)` 
2. `dr = (dr | sr)` 
Explain exactly what these two mathematical operations do to the malicious memory address (`addr`) to render it harmless.

---
---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* A single pair of Base/Bounds registers only separates Program A from Program B. The entire program (Code + Data) sits in one read/write/execute bucket. To prevent a program from overwriting its own code via a buffer overflow, you need *Multiple* Base/Bounds registers (e.g., separating the Code segment as Read-Only, and the Data segment as Read/Write).
  
  **2. D) Cryptographic Separation.**
  *Explanation:* Because they are running on the same server at the same time, they are not physically or temporally separated. Because they rely on encryption keys to conceal the data rather than OS-level boundaries, it is Cryptographic separation.
  
  **3. Short Answer:**
  *Explanation:* Hardware-based memory protection requires a "Context Switch" to move between different processes. Context switches are computationally heavy and slow. Because a media player needs to communicate with a video codec thousands of times a second to render video frames, using hardware isolation would ruin performance. SFI allows for **highly efficient cross-domain calls** by keeping the untrusted code in the same process but restricting it via software.
  
  **4. False.**
  *Explanation:* That describes the "Segment Matching" scheme. The "Sandboxing" scheme does *not* do a comparison or trap/halt. It simply forces the top bits of the address to match the safe domain and executes it. This usually causes the plugin to crash (which is acceptable), but it does not provide precise point-of-fault debugging like Segment Matching does.
  
  **5. B) Paging eliminates external memory fragmentation by using fixed-size blocks.**
  *Explanation:* Segmentation divides programs into logical chunks of varying sizes, which causes "Swiss cheese" fragmentation in RAM. Paging chops RAM into equal-sized blocks (usually 4KB), making it incredibly efficient to fit any page into any open block of memory, even though it ruins the "logical" boundaries of the program.
  
  **6. Short Answer:**
  *Explanation:* A single limit register only protects the Operating System from the user programs. It provides absolutely **zero protection between different user programs**. Program A could easily overwrite the memory of Program B, violating isolation.
  
  **7. False.**
  *Explanation:* Fault domains are identified by the **top** (most significant) bits of the address (e.g., all addresses starting with `010...` belong to Domain 2).
  
  **8. D) Usage control.**
  *Explanation:* Usage control is the most granular degree of separation. It dictates not just *who* can access the resource (which would be Limited Sharing/ACLs), but exactly *how* the resource can be used once accessed.
  
  **9. Short Answer:**
  *Explanation:* An **External Reference Monitor** sits outside the untrusted program; an example is the OS/Hardware Memory Protection (like Paging or Base/Bounds) which enforces rules externally. An **Inlined Reference Monitor (IRM)** is woven directly into the untrusted program's code itself; an example is Software Fault Isolation (SFI), where a compiler injects security checks immediately before every memory access instruction in the binary.
  
  **10. Logic Explanation:**
  *Explanation:* 
  1. The bitwise AND operation (`& mask`) strips away (zeros out) the top identifier bits of whatever malicious address the plugin requested.
  2. The bitwise OR operation (`| sr`) slaps the correct, safe top bits (stored in the dedicated segment register `sr`) back onto the address. 
  *Result:* This forces the memory address to stay within the plugin's legally assigned fault domain. Even if the plugin asks for `OS_Kernel_Address`, the math transforms it into `Plugin_Domain_Address`.
- ---
- ---
- ---
- ## Lecture 6: The JAVA Sandbox and Virtual Machines Practice Questions
  
  **Question 1 (Multiple Choice)**
  In the Java Virtual Machine (JVM) architecture, which specific component is responsible for ensuring "namespace separation" so that a malicious applet downloaded from the internet cannot overwrite or replace core Java API classes (like `java.lang.String`)?
  A) The Bytecode Verifier
  B) The Java Compiler (`javac`)
  C) The Classloader
  D) The Security Manager
  
  **Question 2 (True/False)**
  Type safety in Java inherently defends against buffer overflow attacks because it prohibits arbitrary pointer arithmetic and prevents a program from directly accessing memory outside the bounds of an instantiated object.
  
  **Question 3 (Short Answer)**
  In the Java 2 Security Policy (JDK 1.2 and beyond), the trust model evolved from a simple binary system (Trusted vs. Untrusted) to a highly granular policy. What two specific characteristics make up a program's "CodeSource" (Identity) in this modern trust model?
  
  **Question 4 (Multiple Choice)**
  The primary reason the Java Security Manager uses "Stack Inspection" to evaluate a sensitive request (like deleting a file) rather than just checking the permissions of the currently executing function is to prevent which of the following?
  A) The Base-Rate Fallacy
  B) An untrusted applet tricking a highly-privileged core Java class into executing a dangerous action on its behalf.
  C) A covert channel attack between two separate Java Virtual Machines.
  D) The Bytecode Verifier from running an infinite loop.
  
  **Question 5 (Code / Logic Explanation)**
  Imagine a Java call stack where an untrusted applet calls a Trusted System Class. 
  The Trusted System Class executes the following operations in order:
  1. It calls `enablePrivilege()`.
  2. It calls `disablePrivilege()`.
  3. It calls `File.createNewFile()`.
  When the `checkPrivilege()` algorithm is triggered by the file creation attempt, will the Security Manager **grant** or **deny** the access? Explain exactly *why* based on how the algorithm searches the stack.
  
  **Question 6 (Short Answer)**
  Explain how a "Lie-Detector" check utilizing Virtual Machine Introspection (VMI) works. Why is this check performed by the Virtual Machine Monitor (VMM) vastly more effective at catching stealth malware (like rootkits) than a standard Antivirus program running inside the Guest OS?
  
  **Question 7 (True/False)**
  A perfectly configured Virtual Machine Monitor (VMM) completely eliminates the possibility of two isolated Guest Operating Systems communicating with each other. 
  
  **Question 8 (Multiple Choice)**
  Which of the following is a fundamental security assumption of a Virtual Machine Monitor (VMM) architecture?
  A) Malware cannot infect a Guest OS or its applications.
  B) The VMM must ask for the Guest OS's consent before inspecting its internal memory.
  C) If malware infects one Guest VM, it cannot escape that VM to infect the Host OS or other VMs on the same physical hardware.
  D) The Host OS should regularly run standard user applications (like web browsers) to monitor the Guest VMs.
  
  **Question 9 (Short Answer)**
  Advanced malware and rootkits often look for hardware anomalies (like an unexpected i440bx chipset) or measure precise CPU time latency to detect if they are running inside a Virtual Machine. Give two distinct reasons why a malicious program would want to know if it is running inside a VM.
  
  **Question 10 (True/False)**
  In the Java Stack Inspection algorithm, if a trusted class calls `enablePrivilege()`, then calls `disablePrivilege()`, and finally calls `revertPrivilege()`, the `disable` tag is wiped from the stack frame, meaning a subsequent file access request will be granted.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. C) The Classloader**
  *Explanation:* The Classloader brings the compiled classes into the JVM's memory. Its primary security role is enforcing namespace separation, ensuring that untrusted remote code cannot masquerade as or overwrite trusted core local classes.
  
  **2. True.** 
  *Explanation:* Type safety is the cornerstone of Java security. Unlike C, where a developer can manipulate a pointer (e.g., `*(&b - 1)`) to write data anywhere in RAM, Java hides memory addresses. An operation is only permitted if it is valid for that specific object's type and bounds, completely neutralizing classic buffer overflows.
  
  **3. Short Answer:**
  *Explanation:* A program's identity (CodeSource) is defined by its **Origin** (where the code was downloaded from, aka the `CodeBase`) and its **Signature** (who digitally signed/vouched for the code, aka `SignedBy`).
  
  **4. B) An untrusted applet tricking a highly-privileged core Java class into executing a dangerous action on its behalf.**
  *Explanation:* This is the classic "Confused Deputy" problem. If an untrusted applet asks the `java.io.File` class to delete a file, checking only the currently executing class (`java.io.File`) would show it has full system privileges. By inspecting the *entire* stack, the Security Manager sees the untrusted applet at the root of the request and blocks it.
  
  **5. Logic Explanation:**
  *Answer:* The request will be **DENIED**.
  *Explanation:* The `checkPrivilege()` algorithm searches the stack frames starting from the *newest* (most recent) frame to the *oldest*. Because the trusted class called `disablePrivilege()` *after* `enablePrivilege()`, the "disable" tag is the newest annotation on the stack. As soon as the algorithm encounters the first flag (which is "disable"), it immediately stops searching and denies access.
  
  **6. Short Answer:**
  *Explanation:* Stealth malware and rootkits hide by modifying the Guest OS (e.g., altering the `ps` command to hide malicious processes). An Antivirus running inside the Guest OS relies on these hacked OS commands, making it blind. A VMM, however, sits below the Guest OS and controls the physical RAM. During a "lie-detector" check, the VMM directly reads the raw memory to count the actual running processes, and compares it to the list the Guest OS reports. If the OS reports fewer processes than the VMM physically sees in RAM, the VMM instantly knows the OS is compromised/lying.
  
  **7. False.**
  *Explanation:* Even in a perfectly isolated VMM setup where no network or shared folders exist, malware can still communicate using **Covert Channels**. For example, Malware A can spike the physical CPU usage at a specific time, and Malware B can measure the resulting latency in its own calculations. By interpreting the lag as a binary "1" or "0", they can slowly transmit data across the VMM boundary.
  
  **8. C) If malware infects one Guest VM, it cannot escape that VM to infect the Host OS or other VMs on the same physical hardware.**
  *Explanation:* The core assumption of VMM security is containment. We assume malware *will* infect a Guest OS (A is wrong), but the VMM boundary prevents lateral movement. The VMM does not need consent to inspect memory (B is wrong), and the Host OS should *never* run user applications to keep its attack surface as small as possible (D is wrong).
  
  **9. Short Answer:**
  *Explanation:* 
  1) **Evasion of Analysis:** If malware detects it is in a VM, it assumes a security researcher or automated sandbox is analyzing it. It will immediately stop its malicious behavior to hide its true purpose (avoiding a signature being written for it).
  2) **VM-Based Rootkits (VMBR):** The malware might be a VMBR attempting to install its own malicious hypervisor. If it detects a VMM is already present, it knows it cannot easily install its own layer underneath the OS. *(A third acceptable reason is DRM: software refusing to run in a VM to prevent piracy/cloning).*
  
  **10. True.**
  *Explanation:* This mirrors "Case III" from the slides. The `revertPrivilege()` command removes the most recent annotation from the stack frame (which was the `disable` tag). When `checkPrivilege()` looks down the stack, the `disable` tag is gone, so the first flag it encounters is the original `enablePrivilege()` tag, and access is granted.
- ---
- ---
- ---
- ## Lecture 7: Client-State Manipulation Practice Questions
  
  **Question 1 (True/False)**
  Because the HTTP protocol is inherently stateless, storing a product's price in a `<input type="hidden">` HTML form field is a secure way to maintain state, provided the web server uses HTTPS to encrypt the traffic in transit.
  
  **Question 2 (Multiple Choice)**
  An attacker wants to exploit a "1-cent pizza" vulnerability by submitting a forged HTTP request, but they want to completely bypass the web browser's UI and any client-side JavaScript validations. Which of the following tools would the attacker most likely use to accomplish this?
  A) A JavaScript debugger
  B) `curl` or `wget`
  C) A packet sniffer
  D) `strace`
  
  **Question 3 (Short Answer)**
  A web developer decides to fix a client-state manipulation vulnerability by keeping the server "stateless." The server will still send the price of the pizza to the client's HTML form, but it will attach a Message Authentication Code (MAC) to the transaction data. 
  Explain how this prevents an attacker from successfully modifying the price to $0.01.
  
  **Question 4 (Multiple Choice)**
  Which of the following is the primary security vulnerability associated with using the HTTP `GET` method to transmit sensitive session IDs?
  A) `GET` requests cannot be encrypted using TLS/SSL.
  B) `GET` requests do not support Message Authentication Codes (MACs).
  C) Session IDs are placed in the URL, which can be leaked via copy-pasting or through the HTTP `Referer` header.
  D) `GET` requests cannot trigger database lookups on the server.
  
  **Question 5 (Short Answer)**
  Alice is checking out on an e-commerce site, and her URL currently looks like this: 
  `https://shop.com/checkout?session-id=12345ABC`. 
  The checkout page includes an advertisement image hosted on a third-party website: `<img src="http://ad-server.com/banner.gif">`. 
  Without Alice ever clicking a link or copy-pasting her URL, explain exactly how the administrator of `ad-server.com` can steal Alice's session ID.
  
  **Question 6 (True/False)**
  In a Session Fixation attack, the attacker intercepts the network traffic *after* the victim successfully logs in, steals the victim's newly generated session token, and uses it to hijack their account.
  
  **Question 7 (Logic / Code Explanation)**
  You are auditing a login script for a banking website. The current logic is:
  1. User visits `index.html` $\rightarrow$ Server assigns anonymous token `A1B2`.
  2. User submits username and password via `POST`.
  3. Server validates credentials.
  4. Server marks token `A1B2` as "Logged-In" and grants access.
  Identify the specific vulnerability in this logic and state exactly how the developer must change Step 4 to fix it.
  
  **Question 8 (Multiple Choice)**
  Which of the following represents a significant *performance* drawback of using Authoritative Server State (e.g., storing a table of randomly generated 128-bit Session IDs in a backend database)?
  A) It requires the server to send large cryptographic signatures back and forth over the network.
  B) It leaves the server vulnerable to a Denial of Service (DoS) attack if an attacker floods the server with random, fake session IDs.
  C) It relies on client-side JavaScript to compute the session state.
  D) The 128-bit tokens are easily guessable by automated scripts.
  
  **Question 9 (Short Answer)**
  A junior web developer writes a JavaScript function that checks if a user's password meets complexity requirements and calculates their shopping cart total before enabling the "Submit" button. Why does the Golden Rule of Web Security dictate that the server must entirely recalculate and re-verify this data upon receiving the `POST` request?
  
  **Question 10 (True/False)**
  Using a highly secure, 128-bit unpredictable Session ID protects a web application from Cross-Site Scripting (XSS) attacks.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* HTTPS only protects the data *in transit* against eavesdroppers. It does not protect the data from the client themselves. An attacker can easily use "View Page Source" or Developer Tools to alter the hidden HTML field (e.g., changing the price to $0.01) before the browser encrypts it and sends it back to the server.
  
  **2. B) `curl` or `wget`**
  *Explanation:* These command-line tools allow an attacker to craft raw HTTP requests (like a `POST` request) and send them directly to the server. Because these tools are not web browsers, they completely ignore JavaScript, hidden UI constraints, and HTML rendering, automating and speeding up the attack.
  
  **3. Short Answer:**
  *Explanation:* To generate a valid MAC, the server hashes the transaction data (price) combined with a **Secret Key** known *only* to the server. While the attacker can easily change the HTML price to $0.01, they do not know the Secret Key, so they cannot calculate the new, valid cryptographic signature for the tampered price. When the server receives the forged request, it recalculates the MAC, sees that the signatures do not match, and rejects the order.
  
  **4. C) Session IDs are placed in the URL, which can be leaked via copy-pasting or through the HTTP Referer header.**
  *Explanation:* Unlike `POST`, which hides parameters in the HTTP body, `GET` appends them to the address bar. This makes them highly visible, prone to accidental sharing, and exposes them to third-party logs via the `Referer` header.
  
  **5. Short Answer:**
  *Explanation:* When Alice's browser loads the checkout page, it automatically sends an HTTP `GET` request to `ad-server.com` to fetch the `banner.gif` image. Because browsers automatically include the URL of the *current* page in the **Referer header** of outgoing requests, the browser will silently send `Referer: https://shop.com/checkout?session-id=12345ABC` to the ad server. The attacker simply checks their own server logs to read Alice's active session ID.
  
  **6. False.**
  *Explanation:* That describes a standard Session Hijacking (or Cookie Theft) attack. In a Session *Fixation* attack, the attacker obtains an anonymous token *first*, tricks the victim into using that exact token to log in, and then uses the elevated token to access the account.
  
  **7. Logic / Code Explanation:**
  *Explanation:* This logic is vulnerable to a **Session Fixation attack**. Because the server simply elevates the *existing* anonymous token (`A1B2`) to a logged-in state, an attacker who previously obtained `A1B2` and tricked the victim into using it will now have full access to the victim's account. 
  *The Fix:* To fix this, Step 4 must be changed: The server must **throw away the old token and issue a brand-new, unpredictable Session ID** at the exact moment of login.
  
  **8. B) It leaves the server vulnerable to a Denial of Service (DoS) attack if an attacker floods the server with random, fake session IDs.**
  *Explanation:* Because the server is the "Authority," it must query its backend database for *every single* HTTP request it receives to check if the session ID is valid. An attacker can easily overload the database by sending thousands of garbage session IDs, causing a performance bottleneck. (Option A describes the drawback of the MAC/Signed State approach).
  
  **9. Short Answer:**
  *Explanation:* The Golden Rule of Web Security is "Never trust the client." Because JavaScript executes locally on the user's machine, the attacker has absolute control over it. The attacker can easily disable JavaScript in their browser settings, alter the script in memory using Developer Tools, or bypass the browser entirely using tools like `curl`. Therefore, all calculations and validations must be redone on the server where the attacker has no control.
  
  **10. False.**
  *Explanation:* A 128-bit unpredictable Session ID defends against *Session Guessing* and *Brute-Force* attacks. However, it provides absolutely no defense against XSS. In an XSS attack, the malicious script executes *inside* the victim's browser and can simply ask the browser to read the victim's perfectly secure cookie and send it to the attacker.
- ---
- ---
- ---
- ## Lecture 8: Reverse Engineering, Database Security, and Web Security Practice Questions
  
  **Question 1 (Multiple Choice)**
  An analyst is trying to understand a suspicious Linux binary without reading thousands of lines of assembly code. They use a tool to monitor the program as it runs, observing that it attempts to make an `openat()` system call to access `/etc/shadow`. Which specific tool is the analyst most likely using?
  A) A Decompiler
  B) `strace`
  C) `objdump`
  D) IDA Pro
  
  **Question 2 (True/False)**
  In a Stored (Type 2) Cross-Site Scripting (XSS) attack, the malicious payload is bounced off the web server and requires the attacker to trick the victim into clicking a specially crafted URL containing the script.
  
  **Question 3 (Logic / Code Explanation)**
  You are auditing a login portal that executes the following query:
  `SELECT * FROM Users WHERE user=' $user_input ' AND pwd=' $pwd_input '`
  Explain exactly how an attacker injecting the string `' OR 1=1 --` into the username field successfully bypasses the authentication check. What specific role does the `--` play?
  
  **Question 4 (Multiple Choice)**
  A malware author wants to prevent security researchers from analyzing their virus using static analysis tools. They intentionally insert extra, random bytes into the executable file that look like the start of new, variable-length x86 instructions. What is the goal of this anti-analysis technique?
  A) To strip the debug symbols.
  B) To trigger a timing check in a debugger.
  C) To misalign the disassembler so it outputs fake, garbage assembly code.
  D) To encrypt the binary payload.
  
  **Question 5 (Short Answer)**
  Alice is logged into her bank at `https://www.bank.com`. An attacker tricks her into opening a new tab and visiting `http://www.bank.com:81/promo.html`, which contains a malicious script attempting to read her banking cookies. 
  Will the browser's Same Origin Policy (SOP) block this script? List the specific components the SOP evaluates to justify your answer.
  
  **Question 6 (Multiple Choice)**
  An attacker uses an SQL Injection vulnerability in a search box to successfully extract thousands of credit card numbers from a completely different, hidden table in the database. Which SQL operator is primarily used to combine the legitimate search query results with the stolen table data?
  A) `JOIN`
  B) `UNION`
  C) `DROP`
  D) `SELECT ALL`
  
  **Question 7 (True/False)**
  The most effective and robust way to defend against Cross-Site Scripting (XSS) is to implement a negative security policy (a blocklist) that deletes known dangerous tags like `<script>` from all user input.
  
  **Question 8 (Short Answer)**
  A programmer runs `strace` on a mystery binary and sees it successfully finds and opens a file named `.hidden_authorization_file`. However, the program still prints "Authorization Failed." Why must the programmer now switch to using a dynamic Debugger (like GDB) instead of relying solely on `strace` or a static Disassembler to solve the challenge?
  
  **Question 9 (Logic Explanation)**
  To prevent SQL injection, a developer updates their backend code to use **Prepared Statements (Parameterized Queries)**. If an attacker inputs the payload `'; DROP TABLE Users --` into the username field, why doesn't the database delete the table? Explain how the database engine handles the input differently.
  
  **Question 10 (Short Answer)**
  A web application implements Output Encoding. When an attacker inputs `<script>alert('XSS')</script>`, the server transforms the `<` and `>` characters into HTML entities (`&lt;` and `&gt;`) before sending the page to the victim. What exactly happens when the victim's browser receives this encoded payload? Does the script execute?
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. B) `strace`**
  *Explanation:* `strace` is a dynamic monitoring tool that sits at the OS layer and watches the system calls (like file opening, networking) a program makes while it runs. Decompilers, `objdump`, and IDA Pro are static analysis tools that look at the code at rest.
  
  **2. False.**
  *Explanation:* That describes a *Reflected (Type 1)* XSS attack. A *Stored (Type 2)* XSS attack occurs when the payload is permanently saved on the target server's database (like in a forum post or a MySpace profile). Anyone who naturally visits the infected page later will automatically execute the script without needing to click a special link.
  
  **3. Logic Explanation:**
  *Explanation:* The injected string breaks out of the intended query logic. 
  1) The single quote (`'`) closes the string intended for the username.
  2) The `OR 1=1` statement creates a condition that is mathematically always true, overriding the username requirement for every row in the database.
  3) The double dash (`--`) tells the SQL engine to treat the rest of the query as a comment. This completely neutralizes and ignores the `AND pwd=' $pwd_input '` section, allowing the attacker to log in (usually as the first user in the database, like the Admin) without needing a password.
  
  **4. C) To misalign the disassembler so it outputs fake, garbage assembly code.**
  *Explanation:* Because x86 instructions are variable-length (1 to 15 bytes), injecting "junk bytes" confuses disassemblers into reading the wrong bytes as the start of an instruction, effectively hiding the true assembly logic from the researcher.
  
  **5. Short Answer:**
  *Explanation:* Yes, the SOP will block the script. The Same Origin Policy strictly requires three things to match: the Protocol, the Host, and the Port. While the Host (`www.bank.com`) matches, the Protocol differs (`https` vs `http`) AND the Port differs (default `443` vs `81`). Because they are not the exact same origin, the browser will isolate the tabs.
  
  **6. B) `UNION`**
  *Explanation:* The `UNION` operator in SQL allows an attacker to append the results of a secondary, malicious `SELECT` query (e.g., pulling from the credit cards table) to the results of the primary, legitimate query, displaying the stolen data on the webpage.
  
  **7. False.**
  *Explanation:* Negative security policies (blocklists) are highly ineffective because of "Filter Evasion." Attackers can bypass a filter looking for `<script>` by using mixed cases (`<ScRiPt>`) or alternative tags that execute JavaScript without the word script (`<img src=x onerror=alert(1)>`). The best defense is a Positive Policy (allowlist) combined with strict Output Encoding.
  
  **8. Short Answer:**
  *Explanation:* `strace` is only a monitor; it can only see the *system calls* (I/O) the program asks the OS to do, like opening a file. It cannot see internal CPU computations. A disassembler is static and highly complex to trace manually. The programmer needs a dynamic Debugger (like GDB) to pause the program's execution *after* it opens the file, inspect the CPU registers and memory, and see exactly what specific text/data the program is mathematically expecting to find inside that file.
  
  **9. Logic Explanation:**
  *Explanation:* With Prepared Statements, the database engine compiles the SQL query structure *before* the user input is inserted. The input is then bound to the query strictly as literal string data. Instead of interpreting the `;` and `DROP TABLE` as executable SQL commands, the database literally searches the Users table for a person whose actual, legal first name is `'; DROP TABLE Users --`. Since nobody is named that, the query safely fails.
  
  **10. Short Answer:**
  *Explanation:* The script does **not** execute. When the browser receives the encoded entities (`&lt;script&gt;`), it interprets them as literal text characters meant to be drawn on the screen, rather than as structural HTML tags meant to execute code. The user will harmlessly see the literal word `<script>` printed on the webpage.
- ---
- ---
- ---
- ## Lecture 9: Authentication and Access Controls Practice Questions
  
  **Question 1 (True/False)**
  The primary purpose of appending a unique, random salt to a user's password before hashing is to allow the server to easily decrypt the hash back into the original plaintext password if the user requests a password reset.
  
  **Question 2 (Multiple Choice)**
  A high-security government facility requires employees to insert a physical Smart Card into a reader and scan their iris before the door unlocks. Which two factors of authentication are being utilized?
  A) Something you know & Something you are
  B) Something you have & Something you know
  C) Something you have & Something you are
  D) Somewhere you are & Something you know
  
  **Question 3 (Short Answer)**
  In the S/Key (disposable password) protocol, Alice's computer generates a chain of 100 hashes and securely registers the final hash, $x_{100}$, with the server. For her very first login attempt, what exact value does Alice transmit over the network, and what mathematical operation does the server perform to verify her identity?
  
  **Question 4 (Multiple Choice)**
  Which of the following password attacks is **completely neutralized** by appending a unique 64-bit random salt to each user's password before hashing it in the database?
  A) Online Dictionary Attack
  B) Standard Offline Dictionary Attack
  C) Pre-computed Dictionary Attack (Rainbow Tables)
  D) Keystroke Logging (Trojan Attack)
  
  **Question 5 (True/False)**
  Address-based authentication (verifying a user based on their incoming IP address) is a highly secure mechanism for public internet applications because it relies on the TCP 3-way handshake, making it mathematically impossible for an attacker to spoof the source address.
  
  **Question 6 (Logic / Explanation)**
  The S/Key protocol brilliantly defeats passive eavesdroppers (sniffers) who capture a user's login hash, because hashing is a one-way function. However, the slides note that S/Key is fundamentally vulnerable to an active Man-in-the-Middle (MITM) attack. 
  Explain exactly why a MITM attacker can successfully hijack Alice's session using S/Key, even though the attacker cannot reverse the hash function.
  
  **Question 7 (Multiple Choice)**
  Which of the following is considered the primary security drawback of using biometric characteristics (like a fingerprint or facial recognition) for authentication, compared to a traditional password?
  A) Biometrics are highly susceptible to pre-computed offline dictionary attacks.
  B) Biometric data cannot be easily revoked or replaced if the underlying database is compromised.
  C) Biometrics are generally classified as "Something you have," making them easy to physically steal.
  D) Biometrics require the server to store the comparison data in plaintext.
  
  **Question 8 (Short Answer)**
  In a Cryptographic Challenge-Response authentication protocol, the server sends Alice a random, one-time value called a "nonce" (challenge). How does Alice use this nonce to securely prove her identity to the server without *ever* transmitting her actual secret password across the network?
  
  **Question 9 (True/False)**
  If an attacker breaches a company's database and steals the file containing the Usernames, the unique Plaintext Salts, and the Hashed Passwords, the presence of the salt mathematically prevents the attacker from performing a standard Offline Dictionary Attack to guess Alice's password.
  
  **Question 10 (Logic Explanation)**
  A corporate employee types in their correct username and password on the company's login page. The system recognizes them and loads the employee dashboard. However, when the employee clicks on the "Executive Payroll Database" link, they receive a "403 Forbidden" error. 
  Using strict cybersecurity terminology, explain which security process succeeded in step one, and which distinct security process blocked the employee in step two.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* Cryptographic hashes are mathematically one-way functions; they can *never* be decrypted or reversed, regardless of a salt. The true purpose of a salt is to ensure that two identical passwords result in completely different hashes, which defeats pre-computed dictionary attacks (Rainbow Tables).
  
  **2. C) Something you have & Something you are.**
  *Explanation:* The Smart Card is a physical object the user possesses ("Something you have"), and the iris scan evaluates a physical human characteristic ("Something you are").
  
  **3. Short Answer:**
  *Explanation:* For her first login, Alice transmits **$x_{99}$** (the second-to-last hash in the chain). To verify her, the server hashes the value it received one single time: **$H(x_{99})$**. It then compares the result to the verifier it currently holds on file ($x_{100}$). If they match perfectly, Alice is authenticated, and the server replaces $x_{100}$ with $x_{99}$ for the next login.
  
  **4. C) Pre-computed Dictionary Attack (Rainbow Tables).**
  *Explanation:* A Rainbow Table requires the attacker to pre-calculate the hash of millions of words *before* stealing the database. If a 64-bit random salt is used, there are $2^{64}$ possible variations for every single word. It is computationally and physically impossible to build and store a pre-computed database that large. (Note: Salt does *not* stop online attacks, and it only slightly slows down standard offline attacks).
  
  **5. False.**
  *Explanation:* As highlighted on Slide 4, Address-based authentication is highly vulnerable to "Spoofing of network address." IP addresses are simply text fields in a packet header. While TCP sequences make spoofing a full connection *harder*, it is absolutely not mathematically impossible, and UDP is trivially easy to spoof. IP addresses prove routing location, not user identity.
  
  **6. Logic Explanation:**
  *Explanation:* S/Key provides user authentication, but it **does not authenticate the server**. In a MITM attack, the attacker intercepts Alice's connection and pretends to be the server. When the fake server asks Alice for her password, Alice correctly provides $x_{i-1}$. Because the attacker is actively intercepting the traffic, the attacker simply takes $x_{i-1}$ and immediately forwards it to the *real* server. The real server accepts the valid hash and grants the attacker an authenticated session.
  
  **7. B) Biometric data cannot be easily revoked or replaced if the underlying database is compromised.**
  *Explanation:* You can change a compromised password in seconds. You cannot change your DNA, fingerprints, or retinas. If a hacker steals the biometric hash database, your physical identity is permanently compromised for any system that uses that biometric factor.
  
  **8. Short Answer:**
  *Explanation:* Alice takes the plaintext "nonce" provided by the server and combines it with her Secret Key. She runs this combination through a cryptographic algorithm (like a Hash function to create a MAC, or an asymmetric encryption algorithm to create a Digital Signature). She sends the resulting mathematical output back to the server. The server, which also knows Alice's secret key (or her public key), performs the same math. If the results match, Alice has successfully proven she knows the secret key without actually revealing the key itself.
  
  **9. False.**
  *Explanation:* A salt does not *prevent* a standard Offline Dictionary Attack (Attack 2 in the slides); it merely slows it down. Because the attacker has the stolen database, they can see Alice's plain text salt. The attacker simply takes their dictionary, appends Alice's specific salt to the word "Apple," hashes it, and checks for a match. They must repeat this calculation for every single user, which is slower, but mathematically very possible. 
  
  **10. Logic Explanation:**
  *Explanation:* **Authentication (AuthN)** succeeded, but **Authorization (AuthZ)** failed. 
  When the employee provided their credentials, the system successfully *Authenticated* their identity (verified *who* they were). However, when the employee tried to access the payroll database, the system checked its Access Control Lists (ACLs) and determined the user did not have the proper privileges. The system successfully denied *Authorization* (determining *what* they are allowed to do).
- ---
- ---
- ---
- ### Lecture 10: Firewalls and Intrusion Detection Practice Questions
  
  **Question 1 (True/False)**
  In Intrusion Detection Systems (IDS) terminology, a "False Negative" occurs when normal, innocent network traffic is incorrectly flagged as a malicious intrusion, causing alarm fatigue for the system administrator.
  
  **Question 2 (Multiple Choice)**
  Which type of firewall is highly susceptible to a "tiny-fragment" attack because it evaluates every packet in isolation and does not maintain any state information about previous packets?
  A) Application-Level Proxy
  B) Session Filtering Firewall
  C) Packet Filtering Firewall
  D) Circuit-Level Proxy
  
  **Question 3 (Short Answer)**
  An organization routes all of its internal network traffic through end-to-end TLS encryption. Explain why the security team must deploy a Host-Based IDS (HIDS) on their servers instead of relying solely on a Network-Based IDS (NIDS) to detect malicious payloads.
  
  **Question 4 (Multiple Choice)**
  How does a "Rootkit" actively defeat a system administrator who is trying to monitor a compromised host?
  A) It encrypts all incoming network traffic so the NIDS cannot read it.
  B) It replaces fundamental OS monitoring binaries (like `ps` or `netstat`) with trojan versions that hide the attacker's processes.
  C) It generates millions of fake alerts to trigger the Base-Rate Fallacy.
  D) It fragments its packets to bypass the stateless firewall.
  
  **Question 5 (Logic / Explanation)**
  A security system alerts an administrator that an employee who normally works from 9 AM to 5 PM locally has just established an SSH connection at 3:30 AM from an IP address in a foreign country. 
  Does this alert represent a **Signature-Based (Misuse)** detection or an **Anomaly-Based** detection? Briefly explain why.
  
  **Question 6 (True/False)**
  The "Base-Rate Fallacy" in intrusion detection illustrates that because actual intrusions are incredibly rare compared to normal network traffic, even an IDS with an impressive 99% accuracy rate (1% False Positive Rate) will generate an overwhelming number of false alarms.
  
  **Question 7 (Multiple Choice)**
  Which type of firewall operates by physically breaking the TCP connection, serving as a relay, and is capable of performing deep inspection on the actual payload (e.g., checking an email for viruses before forwarding it)?
  A) Packet Filtering Firewall
  B) Application-Level Proxy
  C) Session Filtering Firewall
  D) Circuit-Level Proxy
  
  **Question 8 (Short Answer)**
  In an Anomaly-Based IDS, the system must undergo a "Training Phase" to build a profile of normal system behavior. What is the severe security risk if an attacker has already stealthily compromised the network *before* the training phase begins?
  
  **Question 9 (True/False)**
  An attacker can successfully evade a Network-Based IDS (NIDS) by flooding the network with massive amounts of garbage data, overwhelming the NIDS CPU, and forcing it to drop packets before sending the actual exploit payload.
  
  **Question 10 (Logic Explanation)**
  A Signature-Based IDS is configured with a rule to drop any packet containing a large sequence of `0x90` (NOP) bytes to prevent classic remote buffer overflow attacks. However, the system is later compromised by a brand-new, zero-day exploit that relies on an integer overflow. 
  Explain exactly why the Signature-Based IDS failed to stop this new attack.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* That describes a **False Positive** (a false alarm). A **False Negative** is the worst-case scenario: an actual malicious intrusion occurs, but the IDS incorrectly diagnoses it as normal traffic and fails to sound the alarm (it misses the attack entirely).
  
  **2. C) Packet Filtering Firewall**
  *Explanation:* Packet filters are stateless. Because they do not track the context of a connection, an attacker can split the TCP header across two tiny IP fragments. The firewall inspects the first fragment, doesn't see the malicious port number (which is pushed into the second fragment), and lets both pass. A Session (Stateful) firewall defeats this by tracking the full connection state.
  
  **3. Short Answer:**
  *Explanation:* A Network-Based IDS (NIDS) sits on the wire and is passive; if the traffic is encrypted with TLS, the NIDS only sees scrambled ciphertext and is completely blind to the attack payload. A Host-Based IDS (HIDS) runs directly on the endpoint. It can see the decrypted data and monitor local application behavior (like a program suddenly making an unexpected system call or touching sensitive files).
  
  **4. B) It replaces fundamental OS monitoring binaries (like `ps` or `netstat`) with trojan versions that hide the attacker's processes.**
  *Explanation:* A rootkit hides the attacker's presence by lying to the operating system. When an admin runs a command to list network connections or active processes, the trojanized command filters out the attacker's malware, making the system look perfectly clean.
  
  **5. Logic Explanation:**
  *Answer:* **Anomaly-Based detection.**
  *Explanation:* There is no specific exploit payload, bad code, or known "signature" here. The alert was generated purely because the event was a statistical deviation from the user's established baseline of "normal" behavior (time of day and location).
  
  **6. True.**
  *Explanation:* This is the core mathematical dilemma of intrusion detection. If your network sees 10 million normal packets a day, a 1% False Positive Rate means the IDS will generate 100,000 false alarms every single day. The tiny handful of real attacks will be completely buried in the noise, causing "alarm fatigue" for analysts.
  
  **7. B) Application-Level Proxy**
  *Explanation:* Application-Level Proxies operate at OSI Layer 7. Because they understand specific protocols (like HTTP or SMTP), they can inspect the actual payload for viruses or content violations. Circuit-Level proxies (like SOCKS) only set up the connection at the TCP level and don't inspect the payload.
  
  **8. Short Answer:**
  *Explanation:* The risk is **Poisoning**. If the attacker is already active during the training phase, the IDS will incorporate the attacker's malicious activity (such as daily data exfiltration or unauthorized logins) into its baseline profile. The IDS will essentially "learn" that the hack is normal behavior, and will never sound an alarm when the attacker continues to exploit the system.
  
  **9. True.**
  *Explanation:* NIDS appliances must process network traffic in real-time. If an attacker floods the network with gigabits of garbage data, the NIDS will max out its processing power and be forced to drop packets. The attacker then slips the real exploit through the network while the NIDS is "blind" and overwhelmed.
  
  **10. Logic Explanation:**
  *Explanation:* Signature-Based (Misuse) detection systems can **only detect already-known attacks**. They operate by comparing incoming traffic to a specific database of rules (like searching for `0x90` NOP sleds). Because a zero-day exploit is brand new, it has no existing signature in the database. The IDS simply saw traffic that didn't match the `0x90` rule, assumed it was benign, and let it pass.
- ---
- ---
- ---
- ### Lecture 11: Cryptography Practice Questions
  
  **Question 1 (True/False)**
  In Public Key (Asymmetric) Cryptography, if Bob wants to send a highly confidential message to Alice over the internet, he should encrypt the message using his own Private Key so that Alice can decrypt it using his Public Key.
  
  **Question 2 (Multiple Choice)**
  An attacker gains physical access to a military encryption device. The attacker can type any text they want into the device and observe the resulting scrambled output. They use these observations to mathematically deduce the secret key. Which type of cryptanalysis attack does this represent?
  A) Ciphertext-Only Attack
  B) Known-Plaintext Attack
  C) Chosen-Plaintext Attack
  D) Man-In-The-Middle (MITM) Attack
  
  **Question 3 (Short Answer)**
  The Diffie-Hellman protocol is an ingenious mathematical method for two parties to establish a shared secret key over a public channel. However, it is fundamentally vulnerable to a Man-in-the-Middle (MITM) attack. 
  Explain exactly *why* this vulnerability exists in raw Diffie-Hellman, and how a modern system (like TLS) fixes it.
  
  **Question 4 (Multiple Choice)**
  Which of the following cryptographic properties ensures that it is computationally infeasible for an attacker to find *any two random, completely distinct messages* that happen to produce the exact same hash output?
  A) The One-Way Property
  B) Weak collision free
  C) Strong collision free
  D) Asymmetric reversibility
  
  **Question 5 (True/False)**
  A Digital Certificate is a mathematically signed document that binds a user's identity (like a domain name) to their Private Key, and the Certificate Authority (CA) must remain online 24/7 for clients to verify it.
  
  **Question 6 (Logic / Math Explanation)**
  In the RSA algorithm, security relies on the difficulty of factoring a large modulus, $n$. 
  If the two chosen prime numbers are $p = 5$ and $q = 11$, what is the value of the modulus **$n$**, and what is the value of Euler's totient **$\phi(n)$** used to calculate the keys?
  
  **Question 7 (Multiple Choice)**
  Alice wants to ensure that a message she sends to Bob over the internet is not altered by an attacker (Integrity). Why is sending the plaintext message alongside a plain cryptographic hash (e.g., sending $m$ and $H(m)$) insufficient to protect against an active attacker?
  A) Hash algorithms require a Public Key Infrastructure (PKI) to operate securely.
  B) Hashes are easily reversed to reveal the plaintext.
  C) An attacker can simply alter the plaintext message, compute a brand new hash for their altered message, and forward both to Bob.
  D) The hash will naturally change due to network latency, causing false alarms.
  
  **Question 8 (Short Answer)**
  Explain the core architectural difference between a Key Distribution Center (KDC) and a Public Key Infrastructure (PKI) in terms of the underlying cryptography used, and state what happens in each system if the central server is hacked and its keys are stolen.
  
  **Question 9 (True/False)**
  Hash algorithms like SHA-256 are commonly used to encrypt large video files before transmitting them over the internet because they compress the data into a fixed-length output, making transmission much faster.
  
  **Question 10 (Logic Explanation)**
  Alice sends a plaintext message $M$ to Bob, along with a digital signature $S$. Alice generated the signature using the RSA formula $S = M^d \pmod n$ (where $d$ is her private key). 
  Explain the exact mathematical operation Bob performs to verify this signature, and why a successful verification proves that *only* Alice could have sent it.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* If Bob encrypts the message with his *Private Key*, anyone in the world with his Public Key can decrypt it, meaning there is zero confidentiality (this is actually how Digital Signatures work, not encryption!). To send a confidential message to Alice, Bob must encrypt it using **Alice's Public Key**. That way, only Alice's Private Key can decrypt it.
  
  **2. C) Chosen-Plaintext Attack**
  *Explanation:* Because the attacker has the capability to *choose* arbitrary plaintexts, feed them into the system, and obtain the corresponding ciphertexts to analyze the algorithm's behavior, this is a Chosen-Plaintext Attack. (In a Known-Plaintext attack, the attacker only possesses pre-existing pairs of plain/cipher text but cannot generate new ones on demand).
  
  **3. Short Answer:**
  *Explanation:* Raw Diffie-Hellman is vulnerable to MITM because it provides **zero user authentication**. The protocol relies entirely on math and does not verify *who* is sending the numbers. Trudy can intercept Alice's transmission, negotiate a key with Alice, and negotiate a separate key with Bob, secretly decrypting and re-encrypting the traffic. Modern systems (like TLS) fix this by having the server use **RSA and Digital Certificates** to mathematically sign the Diffie-Hellman parameters, proving to the client that the parameters genuinely came from the authenticated server.
  
  **4. C) Strong collision free**
  *Explanation:* "Weak collision free" means given a *specific* message, you can't find a second one that matches it. "Strong collision free" is a broader, stricter requirement: it means you cannot find *any* two random messages in the universe that produce the same hash.
  
  **5. False.**
  *Explanation:* This statement has two fatal flaws. First, a certificate binds an identity to a **Public Key**, not a private key (private keys are never shared). Second, the CA **does not** need to be online. Once the CA digitally signs the certificate, the math stands on its own. A client can verify the CA's signature completely offline.
  
  **6. Logic / Math Explanation:**
  *Answer:* 
  *   The modulus **$n$** $= p \times q \rightarrow 5 \times 11 = \mathbf{55}$.
  *   Euler's totient **$\phi(n)$** $= (p-1) \times (q-1) \rightarrow 4 \times 10 = \mathbf{40}$.
  *Explanation:* $n = 55$ is the "trapdoor" published in the public key. $\phi(n) = 40$ is the secret value used to mathematically link the public exponent $e$ and the private exponent $d$.
  
  **7. C) An attacker can simply alter the plaintext message, compute a brand new hash for their altered message, and forward both to Bob.**
  *Explanation:* A standard hash algorithm is public. If Alice only sends $m$ and $H(m)$, Trudy can intercept it, change $m$ to $m'$ ("Transfer $10,000 to Trudy"), run it through the public hash algorithm to get $H(m')$, and send it to Bob. Bob will verify the hash and assume it is authentic. The fix is to use a **MAC (Message Authentication Code)**, which mixes a shared Secret Key into the hash: $H(m|k)$.
  
  **8. Short Answer:**
  *Explanation:* A KDC relies on **Symmetric Cryptography**, whereas PKI relies on **Asymmetric (Public Key) Cryptography**. If a KDC is hacked, it is a catastrophic failure for confidentiality: the KDC knows the symmetric keys for every user, so the hacker can decrypt the entire organization's past and present traffic. If a PKI Certificate Authority (CA) is hacked, the attacker can issue fake certificates to impersonate people, but they **cannot** decrypt past traffic because the CA only stores Public Keys and never possesses the users' Private Keys.
  
  **9. False.**
  *Explanation:* Hash functions are **one-way functions**, meaning they are perfectly irreversible. If you "encrypt" a video file using a hash algorithm, it turns into a 256-bit string and the video data is permanently destroyed and can never be recovered. Hashes are used for integrity checks and signatures, not for encryption/decryption.
  
  **10. Logic Explanation:**
  *Explanation:* Bob verifies the signature by taking the signature $S$ and raising it to the power of Alice's public key exponent $e$, modulo $n$. The formula is: **$M' = S^e \pmod n$**. Bob then compares the resulting value $M'$ to the plaintext message $M$ Alice sent. If they match, the signature is verified. This proves only Alice could have sent it because only Alice possesses the Private Key ($d$) that perfectly mathematically aligns with her Public Key ($e$) to reverse the operation.
- ---
- ---
- ---
- ### Lecture 12: Transport Layer Security: SSL and TLS Practice Questions
  
  **Question 1 (True/False)**
  During a standard web browsing session (e.g., navigating to your bank's website), the TLS handshake natively verifies the user's identity by requiring the client's web browser to transmit a digital certificate to the server.
  
  **Question 2 (Multiple Choice)**
  In the 2012 Chase.com Android app vulnerability discussed in the lecture, an attacker successfully executed a Man-in-the-Middle (MITM) attack against users connecting to their bank. Which critical step of certificate validation did the app developers fail to implement?
  A) Verifying that the certificate was signed by a legitimate Certificate Authority (CA).
  B) Verifying that the certificate had not expired.
  C) Verifying that the hostname on the certificate matched the domain the app was trying to reach.
  D) Verifying the hash function used for the digital signature.
  
  **Question 3 (Short Answer)**
  The SSL/TLS protocol is divided into two distinct sub-protocols: the **Handshake Protocol** and the **Record Protocol**. Briefly explain the distinct cryptographic purpose of each protocol, and state whether they rely primarily on Symmetric or Asymmetric cryptography.
  
  **Question 4 (Multiple Choice)**
  Which of the following scenarios does a perfectly implemented, error-free TLS connection successfully protect against based on its "End-to-End" security guarantee?
  A) A malicious ISP attempting to sniff and read packets in transit.
  B) A Trojan keylogger secretly installed on the client's laptop.
  C) An SQL injection attack launched against the destination web server.
  D) An attacker exploiting a buffer overflow in the web server's memory.
  
  **Question 5 (True/False)**
  In the classic SSL 3.0 / TLS 1.2 RSA handshake, the client encrypts the Pre-Master Secret using its own Private Key to prove its identity to the server before sending it across the network.
  
  **Question 6 (Logic / Explanation)**
  An intelligence agency records all of Alice's encrypted TLS web traffic for three years. Today, they successfully hack the web server and steal its long-term RSA Private Key. They use this key to decrypt all of Alice's past traffic.
  Identify the specific architectural flaw in the classic RSA handshake that makes this "Store Now, Decrypt Later" attack possible, and state the name of the security property that modern systems use to prevent it.
  
  **Question 7 (Multiple Choice)**
  Which specific cryptographic algorithm must be utilized during the TLS Handshake to achieve **Perfect Forward Secrecy (PFS)**?
  A) AES-256 (Advanced Encryption Standard)
  B) SHA-256 Hashing
  C) RSA Key Exchange
  D) Ephemeral Diffie-Hellman (DHE / ECDHE)
  
  **Question 8 (Short Answer)**
  During the initial "Client Hello" and "Server Hello" phases of the TLS handshake, both the client and the server generate and exchange random numbers (nonces) in *plaintext*. Why are these plaintext nonces critical for the creation of the final symmetric session keys, and what specific type of attack do they prevent?
  
  **Question 9 (True/False)**
  Because the TCP/IP model does not natively include the "Presentation" and "Session" layers found in the strict 7-layer OSI model, SSL/TLS was designed to fill this gap by acting as a secure layer sitting directly between the Application layer (HTTP) and the Transport layer (TCP).
  
  **Question 10 (Logic Explanation)**
  Raw Diffie-Hellman is mathematically excellent for negotiating a shared secret key, but it is highly vulnerable to a Man-in-the-Middle (MITM) attack because it lacks authentication. 
  Explain exactly how the modern TLS protocol solves this by combining Diffie-Hellman with RSA/Digital Certificates. What does the server do with its Private Key to secure the Diffie-Hellman exchange?
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. False.** 
  *Explanation:* In standard web browsing, the TLS handshake is completely anonymous from the client's perspective (the client does *not* send a certificate to prove who they are). The TLS tunnel is established to prove the *Server's* identity. Once the secure tunnel is built, the client proves their identity at the Application Layer (e.g., by typing a username and password into the bank's HTML login page).
  
  **2. C) Verifying that the hostname on the certificate matched the domain the app was trying to reach.**
  *Explanation:* The attackers got a perfectly valid, mathematically sound certificate from GoDaddy, but it was for their own fake domain (`AllYourSSLAreBelongTo.us`). Because the Chase app developers forgot to write code checking if the certificate's hostname actually said `chase.com`, the app blindly accepted the attacker's certificate, enabling the MITM attack.
  
  **3. Short Answer:**
  *Explanation:* The **Handshake Protocol** uses heavy, slow *Asymmetric (Public-Key) Cryptography* (like RSA or Diffie-Hellman) to securely authenticate the server and negotiate a shared secret key over the public internet. The **Record Protocol** uses fast *Symmetric (Secret-Key) Cryptography* (like AES) and Hash functions to actually encrypt and verify the integrity of the bulk HTTP data being sent back and forth using the keys established during the handshake.
  
  **4. A) A malicious ISP attempting to sniff and read packets in transit.**
  *Explanation:* TLS only guarantees "End-to-End" security *on the wire* (the network). It protects against eavesdropping, MITM attacks, and packet tampering by ISPs, rogue routers, or coffee-shop Wi-Fi snoops. It does **not** protect the endpoints themselves from malware, rootkits, or application-level attacks like SQLi.
  
  **5. False.**
  *Explanation:* The client encrypts the Pre-Master Secret using the **Server's Public Key** (which it pulled from the server's certificate). This ensures that only the server (who holds the matching Private Key) can decrypt it, protecting the secret from eavesdroppers. 
  
  **6. Logic / Explanation:**
  *Explanation:* The flaw in the classic RSA handshake is that the Pre-Master Secret is encrypted and sent across the wire using the server's static, long-term public key. If the server's private key is ever compromised in the future, the attacker can use it to mathematically decrypt that specific historical packet, recover the Pre-Master Secret, and derive the session keys to read the entire conversation. The security property that prevents this is called **Perfect Forward Secrecy (PFS)**.
  
  **7. D) Ephemeral Diffie-Hellman (DHE / ECDHE)**
  *Explanation:* "Ephemeral" means temporary. By using Diffie-Hellman, the client and server negotiate temporary mathematical parameters to generate the session key, and then *permanently delete those parameters from RAM* once the session ends. Because the parameters are never saved to a hard drive, a future breach of the server yields no mathematical data to decrypt past traffic.
  
  **8. Short Answer:**
  *Explanation:* The random nonces guarantee that every single TLS session is mathematically unique. Even if a user sends the exact same password to a website 100 times, the resulting encrypted session keys will be completely different every time because the nonces change. This explicitly prevents **Replay Attacks**, where an attacker records a secure session today and tries to blindly re-transmit those exact same encrypted packets to the server tomorrow.
  
  **9. True.**
  *Explanation:* As shown on Slide 4, the Internet Protocols (TCP/IP) squash the top three OSI layers into one "Application" layer, leaving out explicit Presentation and Session protocols. SSL/TLS was explicitly invented to sit right below the Application layer and above the Transport layer to provide that missing security, privacy, and session integrity.
  
  **10. Logic Explanation:**
  *Explanation:* Modern TLS combines both algorithms to get Perfect Forward Secrecy *and* Authentication. The server generates its temporary Diffie-Hellman parameters ($T_S$), but before sending them to the client, the server **digitally signs the D-H parameters using its long-term RSA Private Key**. It sends the signed parameters along with its Digital Certificate to the client. The client uses the Public Key inside the certificate to verify the signature. Because the signature matches, the client knows mathematically that the D-H parameters genuinely came from the authenticated server, completely blocking a Man-in-the-Middle attacker from substituting their own parameters.
- ---
- ---
- ---
- ### Practice Questions: The Programming Aspects
  
  **Question 1: Buffer Overflow Pointer Arithmetic (C Code)**
  You are writing a C program to hijack control flow. You declare a pointer `int *p = (int *)&p;`. 
  Due to compiler padding, you calculate that `p` is physically located at `[ebp - 16]`. 
  Write the **three lines of C code** using pointer arithmetic to:
  1. Overwrite the Return Address with the address of `secret_function`.
  2. Provide a dummy return address (e.g., `0x0`).
  3. Pass the integer argument `999` to `secret_function`. 
  *(Hint: Remember that `p` is an integer pointer, so each index `p[i]` moves 4 bytes up the stack).*
  
  **Question 2: SQL Injection Payload (Authentication Bypass)**
  A login page uses the following backend PHP/SQL query:
  `$sql = "SELECT * FROM Employees WHERE username='$user' AND password='$password'";`
  Write the exact string you would type into the `$user` input box to log in as the user `Admin` without knowing their password, while successfully preventing a syntax error.
  
  **Question 3: Fixing SQL Injection (Prepared Statements in PHP)**
  You are hired to fix the vulnerable code from Question 2. 
  Write the **three lines of PHP code** required to prepare the statement, bind the parameters (treating both inputs as strings), and execute the query using the `mysqli` framework. Assume the connection variable is `$conn`.
  
  **Question 4: SWS Denial of Service Fix (Java Try/Catch)**
  The Simple Web Server (SWS) has the following vulnerable code:
  ```java
  String request = br.readLine();
  StringTokenizer st = new StringTokenizer(request, " ");
  String command = st.nextToken();
  String pathname = st.nextToken();
  ```
  Rewrite this code snippet, wrapping it in the proper Java structure to catch the exception caused by an empty string, write a `"HTTP /1.0 400 Bad Request \n\n"` response using the `osw` (OutputStreamWriter) object, and safely `return;`.
  
  **Question 5: SQL Injection Payload (UNION Data Exfiltration)**
  A vulnerable search page executes this query:
  `SELECT article_name, publish_date FROM articles WHERE id = $id;`
  You want to steal usernames and passwords from a hidden table named `admin_credentials`. 
  Write the exact payload you would provide for `$id` to force the query to return empty for the article, but successfully append and display the `username` and `password` columns from the `admin_credentials` table. 
  
  **Question 6: SWS Denial of Service Fix (File Streaming)**
  The SWS uses the following `while` loop to send files to the client, making it vulnerable to infinite stream devices like `/dev/random`:
  ```java
  while (c != -1) { 
    osw.write(c);
    c = fr.read(); 
  }
  ```
  Rewrite this `while` loop to include a secondary condition that enforces a maximum file size limit using a variable named `sentBytes` and a constant named `MAX_LIMIT`. Assume `sentBytes` is initialized to 0.
  
  **Question 7: XSS Payload (Filter Evasion)**
  You are targeting a forum vulnerable to Stored XSS. The developer attempted to secure the input by writing a backend script that perfectly removes the exact string `<script>`. 
  Write a short HTML/JavaScript payload that bypasses this filter and successfully executes `alert('XSS')` without using any `<script>` tags.
  
  **Question 8: Buffer Overflow Padding Math**
  You are attacking a vulnerable C function. The vulnerable buffer is `char buf[64];`. 
  Right above the buffer is the Saved EBP (4 bytes), and above that is the Return Address (4 bytes).
  Your malicious shellcode is exactly **24 bytes** long. 
  If you want to place your shellcode at the very beginning of the buffer, exactly how many bytes of padding/garbage data do you need to write *after* your shellcode to perfectly reach (but not overwrite) the Return Address?
  
  **Question 9: SQL Injection Payload (Destructive)**
  Assume a web application uses a database driver that allows "stacked queries" (multiple SQL statements separated by a semicolon). 
  The vulnerable query is: `SELECT * FROM accounts WHERE email = '$email';`
  Write the exact payload for the `$email` field that will close the first query, completely delete a table named `logs`, and cleanly comment out the rest of the original query.
  
  **Question 10: XSS Defense (Output Encoding)**
  A developer wants to safely echo untrusted user input into an HTML page. To do this, they must encode special characters into HTML entities. 
  What are the exact HTML entities you must translate the characters `<` and `>` into so the browser renders them as harmless text instead of executable tags?
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. Buffer Overflow Pointer Arithmetic (C Code)**
  ```c
  p[5] = (int)secret_function;  // 1. Overwrite Return Address
  p[6] = 0x0;                   // 2. Dummy return address for secret_function
  p[7] = 999;                   // 3. First argument for secret_function
  ```
  *Explanation:* If `p` is at `ebp - 16`, we calculate the offsets by adding 4 bytes per index. `p[0]` is ebp-16, `p[1]` is ebp-12, `p[2]` is ebp-8, `p[3]` is ebp-4, `p[4]` is the Saved EBP, and **`p[5]` is the Return Address (`ebp + 4`)**. According to the `cdecl` calling convention, the dummy return address goes immediately after the Return Address (`p[6]`), and the arguments follow (`p[7]`).
  
  **2. SQL Injection Payload (Authentication Bypass)**
  `Admin' -- `  *(or `Admin' #`)*
  *Explanation:* The single quote closes the `$user` string variable. The `-- ` (with a trailing space) or `#` turns the remainder of the SQL query (the password check) into a comment, forcing the database to ignore it. 
  
  **3. Fixing SQL Injection (Prepared Statements in PHP)**
  ```php
  $stmt = $conn->prepare("SELECT * FROM Employees WHERE username=? AND password=?");
  $stmt->bind_param("ss", $user, $password);
  $stmt->execute();
  ```
  *Explanation:* The `prepare` function compiles the structure using `?` as placeholders. The `bind_param` function explicitly types both inputs as strings (`"ss"`), ensuring the database engine treats any injection attempts strictly as literal text.
  
  **4. SWS Denial of Service Fix (Java Try/Catch)**
  ```java
  try {
    String request = br.readLine();
    StringTokenizer st = new StringTokenizer(request, " ");
    String command = st.nextToken();
    String pathname = st.nextToken();
  } catch (Exception e) {
    osw.write("HTTP /1.0 400 Bad Request \n\n");
    osw.close();
    return;
  }
  ```
  *Explanation:* If an attacker sends an empty string, `st.nextToken()` throws an exception. Wrapping it in a `try/catch` block prevents the program from crashing, alerts the user, and closes the connection cleanly.
  
  **5. SQL Injection Payload (UNION Data Exfiltration)**
  `-1 UNION SELECT username, password FROM admin_credentials -- `
  *Explanation:* By supplying `-1` (or any non-existent ID), the first `SELECT` query returns an empty set. The `UNION` operator then executes the second query and appends the `username` and `password` columns from the hidden table to the output.
  
  **6. SWS Denial of Service Fix (File Streaming)**
  ```java
  while (c != -1 && sentBytes < MAX_LIMIT) { 
    osw.write(c);
    sentBytes++;
    c = fr.read(); 
  }
  ```
  *Explanation:* This adds a second boundary check. Even if `/dev/random` provides an infinite stream of characters (meaning `c` will never equal `-1`), the loop will safely terminate once `sentBytes` hits the `MAX_LIMIT`.
  
  **7. XSS Payload (Filter Evasion)**
  `<img src="x" onerror="alert('XSS')">` 
  *(Other acceptable answers: `<svg onload="alert(1)">` or `<body onload="alert(1)">`)*
  *Explanation:* Because the naive filter only looks for the exact string `<script>`, you can bypass it using an HTML image tag with a broken source (`src="x"`). When the image fails to load, the `onerror` event handler automatically executes the malicious JavaScript.
  
  **8. Buffer Overflow Padding Math**
  **44 bytes.**
  *Explanation:* 
  *   The distance from the start of the buffer to the Return Address is: Buffer size (64 bytes) + Saved EBP (4 bytes) = **68 total bytes to fill**.
  *   Your shellcode is 24 bytes long.
  *   68 total bytes - 24 shellcode bytes = **44 bytes of padding needed.** 
  *   *(Payload structure: [24 bytes shellcode] + [44 bytes NOP padding] + [4 byte New Return Address])*
  
  **9. SQL Injection Payload (Destructive)**
  `'; DROP TABLE logs -- `
  *Explanation:* The single quote (`'`) closes the email string. The semicolon (`;`) terminates the `SELECT` statement. The attacker then issues the `DROP TABLE` command, followed by `-- ` to comment out the rest of the original query.
  
  **10. XSS Defense (Output Encoding)**
  `<` becomes **`&lt;`**
  `>` becomes **`&gt;`**
  *Explanation:* `&lt;` stands for "Less Than" and `&gt;` stands for "Greater Than". When a browser sees these entities, it renders them visually as `<` and `>` on the screen, but it structurally refuses to treat them as actual HTML tags, perfectly neutralizing XSS.