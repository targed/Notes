### Part 1: Pre-Midterm Review (≈30% of the Exam)
- *   **Basic Security Concepts:** 
    *   The CIA Triad (Confidentiality, Integrity, Availability).
    *   Authentication vs. Authorization.
  *   **Buffer Overflows & x86 Architecture:**
    *   **Why C is vulnerable:** Lack of boundary checking (`strcpy`), lack of type safety.
    *   **The Stack Frame:** Understand that the stack grows *downward* and buffers fill *upward*. Know the layout: `[Buffer] -> [Saved EBP] -> [Return Address (EIP)]`.
    *   **The Exploit:** Overwriting the return address to redirect control flow (hijacking EIP).
  *   **The Java Sandbox & Type Safety:**
    *   Why Java is safer than C (Type safety, no direct memory/pointer manipulation).
    *   **JVM Architecture:** Classloader, Bytecode Verifier, Security Manager.
    *   **Stack Inspection:** How Java checks permissions (searching the stack newest to oldest for `enablePrivilege` or `disablePrivilege` flags).
- ### Part 2: Post-Midterm Material (≈70% of the Exam)
- #### 1. System Isolation & Virtual Machines
  *   **Virtual Machine Monitors (VMMs/Hypervisors):** Host OS vs. Guest OS isolation.
  *   **Virtual Machine Introspection (VMI):** Using the VMM to catch malware that hides *inside* the Guest OS (e.g., Rootkits).
    *   *The "Lie-Detector" Check:* The VMM counts raw processes in memory and compares it to what the Guest OS `ps` command reports. If there's a mismatch, the Guest OS is compromised.
  *   **Covert Channels:** How two perfectly isolated VMs can communicate (e.g., manipulating CPU load/timing at specific intervals).
- #### 2. Web Application Security
  *   **Client-State Manipulation:** 
    *   Why HTTP is stateless, and why relying on "Hidden HTML form fields" for prices/data is fundamentally insecure.
    *   **Fix 1:** Authoritative Server State (using random Session IDs stored in a backend DB).
    *   **Fix 2:** Signed Client State (Using MACs to cryptographically sign the hidden fields so tampering is detected).
  *   **GET vs. POST:** Why `GET` is insecure for state (parameters are visible in the URL and leak via the HTTP `Referer` header to third-party sites).
  *   **Session Management Flaws:**
    *   *Session Hijacking:* Stealing a token via unencrypted Wi-Fi or XSS.
    *   *Session Fixation:* An attacker tricks you into logging in using a token *they* generated. (Fix: The server must issue a brand-new token the moment a user logs in).
  *   **Same Origin Policy (SOP):** The browser security rule where Protocol, Host, and Port must match exactly.
  *   **Cross-Site Scripting (XSS):**
    *   *Reflected (Type 1):* The payload bounces off the server (e.g., a malicious search query link).
    *   *Stored (Type 2):* The payload is saved in the DB (e.g., a malicious forum post like the Samy Worm).
    *   *Defense:* **Output Encoding/Filtering** (changing `<script>` to `&lt;script&gt;`). Input validation is good, but not enough alone.
- #### 3. Database Security (SQL Injection)
  *   **The Vulnerability:** Gluing untrusted user input directly into an SQL string.
  *   **The Exploits:**
    *   *Authentication Bypass:* `' OR 1=1 --`
    *   *Destructive:* `'; DROP TABLE Users --`
    *   *Data Exfiltration:* Using `UNION SELECT` to dump data from other tables.
  *   **The Defense:** Using **Prepared Statements / Parameterized Queries** (which compile the SQL structure first, treating user input strictly as literal data).
- #### 4. Password Security
  *   **Dictionary Attacks:** Online vs. Offline. 
  *   **Rainbow Tables:** Pre-computed hashes of millions of dictionary words.
  *   **The Defense (Salting):** Appending a random $n$-bit number to the password *before* hashing. It defeats Rainbow Tables because the attacker would have to generate a new table for every possible salt value.
- #### 5. Firewalls & Intrusion Detection Systems (IDS)
  *   **Firewalls:** 
    *   *Packet Filtering:* Stateless. Looks at IP/Ports. Vulnerable to "Tiny-Fragment" attacks.
    *   *Session Filtering:* Stateful. Tracks TCP handshakes.
    *   *Proxies:* Application-level (deep inspection, expensive) vs. Circuit-level (SOCKS).
  *   **Intrusion Detection:**
    *   *Signature-based (Misuse):* Matches known bad patterns (e.g., NOP sleds, Slammer worm). High false-negative rate for new attacks.
    *   *Anomaly-based:* Builds a "normal" profile. High false-positive rate. Vulnerable to attackers slowly poisoning the training data.
    *   *The Base-Rate Fallacy:* Because real attacks are incredibly rare compared to normal traffic, even a 1% false positive rate will drown admins in thousands of fake alarms.
    *   *Deployment:* Host-based (HIDS - sees local system calls) vs. Network-based (NIDS - sees broad traffic but is blind to encrypted TLS payloads).
- #### 6. Cryptography
  *   **Symmetric vs. Asymmetric:** Secret Key vs. Public Key cryptography.
  *   **Hash Functions:** Properties required (One-way, Weak Collision-free, Strong Collision-free). Used for Digital Signatures and MACs ($H(message | key)$).
  *   **RSA:** Factoring large primes. $c = m^e \pmod n$. Signing is done with the *Private Key*, verifying is done with the *Public Key*.
  *   **Diffie-Hellman (D-H):** Negotiating a shared secret key over a public channel ($T_A = g^{S_A} \pmod p$). Vulnerable to Man-in-the-Middle (MITM) attacks because it lacks authentication.
  *   **Key Distribution:** KDC (Symmetric, single point of failure) vs. PKI/Certificates (Asymmetric, uses a Certificate Authority).
- #### 7. Transport Layer Security (SSL/TLS)
  *   **The Handshake:** 
    1. Client Hello (Plaintext).
    2. Server Hello + **Certificate** (Plaintext).
    3. Client Key Exchange (Encrypts a pre-master secret using the Server's RSA Public Key).
  *   **Certificate Validation Flaw:** Developers often check if a certificate is unexpired and signed by a CA, but forget to check if the **Hostname** actually matches the site they are connecting to.
  *   **Perfect Forward Secrecy (PFS):** The classic RSA handshake is vulnerable to "Store Now, Decrypt Later" if the server's long-term private key is stolen years later. **Fix:** Using Ephemeral Diffie-Hellman (DHE) for the key exchange, and only using RSA to *sign* the D-H parameters for authentication.
  
  ---
- ###  The Programming Aspects to Prepare For
  
  1.  **Buffer Overflow Pointer Arithmetic (Like HW2):**
    *   Be able to write a few lines of C code showing how to manipulate a pointer to overwrite a return address.
    *   *Example:* `int *p = (int *)&p; p[6] = (int)malicious_function;` (Know that you have to step past the local variables and the saved EBP to hit the Return Address).
  2.  **Writing an SQL Injection Payload:**
    *   You will likely be given a vulnerable PHP/SQL query and asked to write the exact string you would type into the input box to bypass it (e.g., writing `' OR '1'='1' -- `).
  3.  **Writing a Prepared Statement:**
    *   Be ready to rewrite a vulnerable SQL query into a secure one. Know the syntax for swapping variables with `?` and binding them. 
    *   *Example to memorize:* 
        `$stmt = $conn->prepare("SELECT id FROM users WHERE name= ?");`
        `$stmt->bind_param("s", $input_name);`
  4.  **Fixing the Simple Web Server (SWS) DoS:**
    *   Be prepared to write a `try { ... } catch (Exception e) { ... }` block to wrap vulnerable parsing code, or write a `while (bytes < MAX_LIMIT)` condition to stop the `/dev/random` infinite loop.
  5.  **Basic Cryptography Math (Modulo Arithmetic):**
    *   You might be asked to manually calculate a tiny RSA or Diffie-Hellman problem. Be very comfortable doing modular arithmetic on paper (e.g., $3^3 \pmod 7$).
- ---
- ---
- ---
- ### The Programming Aspects Practice Questions
  
  **Question 1 (Logic / Pointer Arithmetic)**
  You are writing a buffer overflow exploit in C without explicitly calling a function, similar to your Homework 2. You create a local pointer `int *p = (int *)&p;`. 
  Because of compiler padding, you determine via `objdump` that your pointer `p` is located at `[ebp - 12]`. 
  Using pointer arithmetic (where each index `p[i]` jumps by 4 bytes), which exact array index of `p` must you overwrite to hijack the **Return Address** (which is located at `[ebp + 4]`)?
  
  **Question 2 (Code / SQL Injection)**
  A vulnerable PHP application uses the following backend code to authenticate users:
  `$sql = "SELECT id FROM credential WHERE name='$uname' AND Password='$pwd'";`
  You want to log into the application as the user `Admin` without knowing their password. Write the exact payload string you would type into the `$uname` (Username) input box on the webpage to successfully bypass the password check.
  
  **Question 3 (Multiple Choice)**
  In the Simple Web Server (SWS) Java code, the server crashes and suffers a Denial of Service (DoS) if an attacker sends an empty string (e.g., `\r\n`) instead of a properly formatted `GET` request. 
  Which of the following is the correct programming solution to fix this specific vulnerability?
  A) Use an `if` statement to check if the file size is greater than `Runtime.getRuntime().freeMemory()`.
  B) Surround the `StringTokenizer` and `nextToken()` calls with a `try/catch` block to handle the exception gracefully and return a `400 Bad Request`.
  C) Convert the `GET` method to a `POST` method in the HTML code.
  D) Use a Prepared Statement to sanitize the `StringTokenizer` input.
  
  **Question 4 (Fill in the Blank / PHP)**
  To fix an SQL injection vulnerability, a developer decides to use a Prepared Statement. Fill in the missing PHP method name (marked by `_______`) that replaces the `?` placeholders with the actual user input safely as literal data.
  ```php
  $stmt = $conn->prepare("SELECT * FROM credential WHERE name = ? AND Password = ?");
  $stmt->_______("ss", $input_uname, $hashed_pwd);
  $stmt->execute();
  ```
  
  **Question 5 (True/False)**
  When fixing the `/dev/random` Denial of Service bug in the Simple Web Server (SWS), using the Java `File.length()` method to check if the requested file is too large is an effective defense, because the server will see that `/dev/random` contains infinite bytes and will reject the request.
  
  **Question 6 (Crypto Math)**
  You are performing a small RSA encryption operation by hand. 
  The public key is $(e, n) = (3, 15)$. 
  If you want to encrypt the plaintext message $M = 4$, what is the resulting ciphertext $C$? *(Remember: $C = M^e \pmod n$)*.
  
  **Question 7 (Logic / Stack Setup)**
  You have successfully used pointer arithmetic to overwrite a function's Return Address so that it jumps to a new function: `void fun2(int a)`. 
  According to the 32-bit `cdecl` calling convention you learned in HW2, if your hijacked Return Address is currently sitting at `p[4]`, at which exact array index must you place the integer argument for `a` so that `fun2` can find it without crashing?
  A) `p[3]`
  B) `p[4]`
  C) `p[5]`
  D) `p[6]`
  
  **Question 8 (Crypto Math)**
  Alice and Bob are using the Diffie-Hellman key exchange. They agree on a public prime $p = 11$ and a public generator $g = 2$.
  Alice receives Bob's public transmission: $T_B = 5$.
  Alice's secret random number is $S_A = 3$. 
  Calculate the final Shared Secret Key ($K$) that Alice will derive.
  
  **Question 9 (Multiple Choice)**
  Which of the following describes the physical memory mechanism that allows a classic C buffer overflow to successfully overwrite the saved Instruction Pointer (`EIP`)?
  A) The stack grows downwards (high to low addresses), while string buffers are written upwards (low to high addresses).
  B) The stack grows upwards, while string buffers are written downwards.
  C) The heap grows downwards into the stack space.
  D) The `cdecl` calling convention places the local variables above the Return Address in memory.
  
  **Question 10 (True/False)**
  When crafting the shellcode payload for your Buffer Overflow project (Project 2), it was critical to ensure your payload string contained exactly one `\0` (null byte) at the very beginning so the `strcpy()` function would know where to start reading the malicious code.
  
  ---
  ---
- ### Answer Key & Explanations
  
  **1. Answer: `p[4]`**
  *Explanation:* You must do the math stepping up 4 bytes at a time.
  *   `p[0]` = `ebp - 12`
  *   `p[1]` = `ebp - 8`
  *   `p[2]` = `ebp - 4`
  *   `p[3]` = `ebp` (This is the Saved EBP)
  *   **`p[4]` = `ebp + 4` (This is the Return Address/EIP).**
  
  **2. Answer: `Admin' #`** (or `Admin' -- `)
  *Explanation:* As practiced in your SQLi Lab, the single quote (`'`) breaks you out of the string definition. The hash (`#`) or double-dash (`--`) comments out the remainder of the query (the password check). The resulting query executed by the server safely becomes `SELECT id FROM credential WHERE name='Admin'`. 
  
  **3. B) Surround the `StringTokenizer` and `nextToken()` calls with a `try/catch` block.**
  *Explanation:* As shown on slide 37 of the SWS lecture, the application crashes because `nextToken()` throws a `NoSuchElementException` when it hits an empty string. The programmer must catch this exception, return a `400 Bad Request`, and safely close the connection to keep the server alive.
  
  **4. Answer: `bind_param`**
  *Explanation:* The correct syntax is `$stmt->bind_param("ss", $input_uname, $hashed_pwd);`. This is the critical step in a Prepared Statement that tells the database engine to treat the inputs strictly as literal strings ("ss"), rendering any injected SQL commands completely harmless.
  
  **5. False.**
  *Explanation:* This is a highly specific "gotcha" from the SWS slides. In Linux, `/dev/random` is a special character device. If you ask Java for `File.length()`, the OS actually reports its size as **0 bytes** (because it doesn't exist as a physical file on the disk). The length check will pass (`0 < freeMemory`), but when the server tries to read it, it will stream infinite data, crashing the server. The only fix is to impose a hard `max_download_limit` counter inside the `while` loop.
  
  **6. Answer: $C = 4$**
  *Explanation:* 
  1. Formula: $C = M^e \pmod n$
  2. Plug in values: $C = 4^3 \pmod{15}$
  3. Exponentiation: $4 \times 4 \times 4 = 64$
  4. Modulo: $64 / 15 = 4$ with a remainder of $4$. 
  5. The ciphertext is $4$.
  
  **7. D) `p[6]`**
  *Explanation:* In the `cdecl` calling convention, a function expects the top of the stack (where the CPU jumped from) to contain a Return Address, and the argument to be placed *immediately after* that return address.
  *   `p[4]` = The hijacked Return Address (points to `fun2`).
  *   `p[5]` = The "Dummy" Return Address (where `fun2` will go when it calls `exit` or `ret`).
  *   **`p[6]` = The first argument (`a`).**
  
  **8. Answer: $K = 4$**
  *Explanation:* 
  1. Formula for the shared secret: $K = (T_B)^{S_A} \pmod p$
  2. Plug in the values Alice has: $K = 5^3 \pmod{11}$
  3. Exponentiation: $5 \times 5 \times 5 = 125$
  4. Modulo: $125 / 11 = 11$ with a remainder of $4$.
  5. The shared secret key is $4$.
  
  **9. A) The stack grows downwards (high to low addresses), while string buffers are written upwards (low to high addresses).**
  *Explanation:* This is the fundamental physical flaw of the x86 architecture that makes buffer overflows possible. Because the stack grows down, local variables (buffers) are placed at *lower* memory addresses than the Saved EIP. When `strcpy` writes into the buffer, it writes *upwards* towards the higher addresses, plowing straight through the local boundaries and overwriting the saved Return Address.
  
  **10. False.**
  *Explanation:* Your payload must **NOT** contain any `\0` (null bytes). The `strcpy()` function in C uses the null byte as the string terminator. If it encounters a `\0` anywhere in your shellcode, it stops copying immediately, and your payload will be truncated before it can successfully overwrite the Return Address!