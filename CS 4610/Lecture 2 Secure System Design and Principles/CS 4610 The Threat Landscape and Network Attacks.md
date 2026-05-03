## I. Categorizing Threats
The slides list several specific ways attackers disrupt or exploit systems.

**1. Defacement**
*   **What it is:** Digital graffiti. Replacing a legitimate website’s content with a political or social message.
*   **Goal:** Reputation damage, not necessarily financial gain.

**2. Infiltration**
*   **What it is:** Gaining unauthorized access to internal resources (backend databases, file systems).
*   **Key Vector:** **SQL Injection (SQLi)**. This is when an attacker inputs database commands (like `DROP TABLE`) into a login box, and the system executes them.
*   **Consequence:** Data theft or modification (Read/Write access).

**3. Phishing vs. Pharming**
*   *These are often confused. Here is the distinction:*
*   **Phishing (Social Engineering):**
  *   **Vector:** Email/SMS.
  *   **Mechanism:** Tricking the *user* into clicking a fake link (e.g., `evil-site.com` that looks like `bank.com`).
  *   **Defense:** User education and spam filters.
*   **Pharming (Infrastructure Attack):**
  *   **Vector:** **DNS Cache Poisoning**.
  *   **Mechanism:** The attacker hacks the DNS server (the internet's phonebook). When the user types `www.google.com`, the corrupted DNS server sends them to the attacker’s IP address.
  *   **Danger:** The user typed the correct URL, but still ended up at the wrong place. User education doesn't help here; server security does.

**4. Click Fraud**
*   **Concept:** "Serverless Denial of Wallet."
*   **Mechanism:** Attackers use bots to click on a competitor's Pay-Per-Click (PPC) ads.
*   **Result:** The competitor loses their advertising budget paying for fake clicks, and legitimate customers never see the ad.

**5. Insider Threats**
*   **Concept:** The "Bad Apple." An employee with legitimate access (like a Database Admin) abuses that access.
*   **Why it's dangerous:** Firewalls stop outsiders. They do not stop someone who has a key card and a login.
- ## II. Denial of Service (DoS)
  **Goal:** Compromise **Availability**.
  
  **Types:**
  1.  **DoS:** Single attacker crashes a target.
  2.  **DDoS (Distributed):** Using a botnet (thousands of zombies) to flood a target.
  3.  **ReDoS (Regular Expression DoS):** *A very specific, modern attack.*
    *   **How it works:** Regex engines in programming often use "backtracking." If you write a poorly designed Regex (like `^(a+)+$`) and the attacker sends a long string like `aaaaaaaaaaaaaaaaaaaaX`, the CPU has to check every possible combination of "a"s.
    *   **Result:** Exponential processing time ($O(2^n)$). Sending one small string can freeze the server's CPU at 100% for hours.
- ## III. Network Attacks: IP Spoofing
  **Context:** The internet was built on trust (University/Military nodes). It was not built to verify identity at the packet level.
  
  **1. IP Whitelisting**
  *   **Defense:** A firewall rule that says "Only accept packets from IP `192.168.1.5` (Alice)."
  *   **The Flaw:** The "From" address in an IP packet is just text written by the sender.
  
  **2. IP Spoofing Attack**
  *   **Attack:** "Eve" sends a packet to the server but writes "Alice's" IP address in the "From" field. The firewall sees "Alice" and lets it in.
  
  **3. The Solution: Nonces & TCP**
  *   **Nonce:** A "Number used ONCE." A random number generated for a specific session.
  *   **How TCP prevents spoofing:**
    *   TCP requires a "Handshake" (SYN -> SYN-ACK -> ACK).
    *   When the server replies to the spoofed packet, it sends the **SYN-ACK** (containing a random **Sequence Number**) to *Alice* (the real owner of the IP), not Eve.
    *   Eve cannot see this reply (because she is not Alice).
    *   To complete the connection, Eve must guess the random number. If the number is truly random (has high entropy), she will fail.
  *   **UDP Vulnerability:** UDP is "connectionless." There is no handshake. It is much easier to spoof UDP because the attacker doesn't need to see the server's reply to send a malicious payload.
  
  ---
- ### Study Questions for Part 1
  1.  **Phishing vs. Pharming:** If you receive an email claiming to be your bank with a link to `http://banc-of-america.com`, is this Phishing or Pharming? Why?
  2.  **ReDoS:** Why is a Regular Expression vulnerability considered a "Denial of Service" attack rather than an "Infiltration" attack?
  3.  **Spoofing:** Why is it harder to spoof an IP address using TCP compared to UDP? (Hint: Think about the 3-way handshake and where the reply packet goes).
  
  ---