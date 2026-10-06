## I. The "Defense in Depth" Approach
The slides state that security is holistic, requiring three specific layers: **Physical**, **Technological**, and **Policies**.
*   **Expanded Concept:** In the industry, this is often called **Defense in Depth**. The idea is that no single layer is perfect. If an attacker bypasses the physical lock, the password policy should stop them. If they steal a password, the network firewalls should restrict their access.
*   **The Weakest Link Principle:** A system is only as secure as its weakest layer. You can have the best encryption in the world (Technological), but if an employee writes the password on a sticky note (Policy failure) or throws a hard drive in the trash (Physical failure), the encryption is useless.
- ## II. Layer 1: Physical Security
  *   **The Goal:** Prevent physical contact with the hardware.
  *   **The Golden Rule:** "If an attacker can physically touch your computer, it is no longer your computer." With physical access, an attacker can boot from a USB drive, bypass OS passwords, or remove the hard drive entirely.
  *   **Dumpster Diving:** The practice of sifting through trash to find manuals, org charts, IP address lists, or sticky notes with passwords. This is often the "Reconnaissance" phase of an attack.
- ## III. Layer 2: Technological Security 
  This is subdivided into three distinct layers that interact with each other.
- ### A. Application Security
  This focuses on the software code itself (where you will be working for your projects).
  *   **"Interpret Data Robustly":** This is a polite way of saying **Input Validation**.
    *   *Why it matters:* Most hacks (like Buffer Overflows or SQL Injection) happen because the application trusted the user's input too much. If an app expects a name but gets a line of malicious code, and it tries to "interpret" that code, the system is compromised.
  *   **Identity Verification:** Ensuring the user is who they say they are (Authentication) before showing them data.
  *   **The Architecture (Diagram):**
    *   User $\rightarrow$ Firewall $\rightarrow$ Web Server $\rightarrow$ Database.
    *   *Note:* The Database is usually kept behind the Web Server, never exposed directly to the internet. This is "Protection via Separation" (a concept covered in Week 6).
- ### B. OS & Network Security
  *   **The OS Kernel:** The kernel is the core of the OS that has complete control over everything.
    *   *The Risk:* Since applications run *on top* of the OS, if the OS has a vulnerability, every application running on it is at risk.
    *   *Mitigation:* **Patching.** However, this creates the **"Patch Gap"**—the dangerous time window between a vulnerability being discovered and the patch being installed.
  *   **Network Security:**
    *   **Firewall:** Filters traffic (e.g., "Allow traffic on port 80, block everything else").
    *   **IDS (Intrusion Detection System):** A specialized tool that watches network traffic for suspicious patterns (like a burglar alarm).
  *   **CVE (Common Vulnerabilities and Exposures):**
    *   *Definition:* A global dictionary/database of known security flaws. (e.g., CVE-2021-44228).
    *   *Usage:* Security professionals check the CVE list daily to see if the software they use has a newly discovered flaw.
- ## IV. Layer 3: Policies & Procedures
  *   **The Human Factor:** Technology cannot patch human psychology.
  *   **Social Engineering:** Manipulating people into breaking security procedures.
    *   *Example:* An attacker calls an employee pretending to be "IT Support" and asks for their password to "fix a glitch."
    *   *Mitigation:* "Paranoia and Vigilance." Training employees to verify identity before releasing information.
- ## V. The Cast of Characters
  In Cryptography and Security scenarios, we use standard names to describe roles. This prevents confusion (instead of saying "Person A" and "Person B").
  
  *   **Alice & Bob:** The "Good Guys." They want to communicate securely.
  *   **Trent:** The **T**rusted Third Party.
    *   *Expanded Context:* Trent represents a central authority everyone trusts, like a Certificate Authority (CA) that issues SSL certificates for websites, or a Key Distribution Center (KDC) in corporate networks.
  *   **The Attackers (Crucial Distinction):**
    *   **Eve (The Eavesdropper):**
        *   *Behavior:* **Passive.** She listens to the wire but does not touch the data.
        *   *Threat:* She violates **Confidentiality**.
        *   *Difficulty:* Very hard to detect (since she isn't changing anything).
    *   **Mallory (The Malicious Attacker):**
        *   *Behavior:* **Active.** She intercepts the message, modifies it, and sends it on.
        *   *Threat:* She violates **Integrity**.
        *   *Difficulty:* Easier to detect (if using integrity checks), but significantly more dangerous because she can change the content of a transaction.
  
  ---
- ### Study Questions for Part 2
  1.  ** Scenario:** You have a perfectly coded web application (App Security) running on a fully patched Windows server (OS Security). An attacker calls the receptionist, claims to be the CEO, and asks for the admin password. The receptionist gives it to them. Which layer of the Holistic Security model failed?
  2.  Why is **Mallory** considered an "Active" attacker while **Eve** is "Passive"? Which one is harder to catch, and why?
  3.  The slides mention **CVE**. If you are a system administrator, why is it important to monitor `cve.mitre.org`?
  
  ---