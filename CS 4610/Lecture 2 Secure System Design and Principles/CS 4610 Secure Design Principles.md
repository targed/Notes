## I. The "Bolt-On" Fallacy
*   **The Golden Rule:** Security must be designed *in*, not added *on*.
*   **Why "Adding it later" fails:**
  *   If the underlying architecture assumes trust (like the early Internet or early Windows), adding a password prompt on top is like putting a padlock on a cardboard box. An attacker can just cut through the box.
*   **Case Study: Windows 98**
  *   Windows 98 had user passwords, but the OS wasn't designed with true access control at the file system level.
  *   *The Exploit:* Boot into **"Diagnostic Mode" (Safe Mode)** by pressing F8. This mode bypassed the login screen entirely, giving anyone full access to the hard drive.
  *   *Lesson:* The security was a superficial layer, not baked into the kernel.
*   **"Security or Bust":**
  *   Companies now realize that shipping insecure code destroys brand reputation.
  *   *Example:* Microsoft delayed the .NET server in 2002 because it didn't meet security standards. This was a major shift in the industry from "Ship it fast" to "Ship it secure."
- ## II. Architectural Metaphors: The Turtle Shell
  *   **Definition:** A network architecture with a hard outer boundary (Firewall) but a soft, unprotected interior.
  *   **The Assumption:** "Bad guys are outside, good guys are inside."
  *   **Why it fails:**
    1.  **Insider Threats:** If the attacker is already an employee (the "soft center"), the firewall does nothing.
    2.  **Phishing/Malware:** If a user inside the shell clicks a malicious link, the malware runs *inside* the perimeter.
    3.  **Zero-Day Attacks:** If an attacker finds a way through the firewall (the shell cracks), there are no internal defenses to stop them.
  *   **Modern Alternative:** While not explicitly named in the slides, the opposite of this is **Zero Trust Architecture** (Verify every request, even if it comes from inside).
- ## III. The Convenience vs. Security Trade-off
  *   **The Curve:** Generally, as Security increases, Convenience decreases.
    *   *Low Security:* No password. (High Convenience).
    *   *High Security:* 20-character password, retinal scan, 5-minute timeout. (Low Convenience).
  *   **The User Behavior Risk:** If you make security *too* inconvenient, users will bypass it.
    *   *Example:* Forcing users to change passwords every week leads to users writing the password on a sticky note attached to the monitor. The strict policy actually *lowered* security.
  *   **The Goal:** Technologies that increase security with minimal user friction (e.g., Biometrics—unlocking a phone with a face scan is more secure *and* easier than a PIN).
- ## IV. Data Validation: The Credit Card Example
  You must validate data before processing it. The slides use Credit Cards (CC) to show two different types of checks:
  
  **1. Validity Check (Mod 10 / Luhn Algorithm)**
  *   **Goal:** Catch typos.
  *   **Mechanism:** A mathematical formula (double every second digit, sum them up, check if divisible by 10).
  *   **Security Level:** Low. Anyone can generate a number that passes this check. It just proves the number is *structurally* correct.
  
  **2. Security Check (CVC - Card Verification Code)**
  *   **Goal:** Prove possession.
  *   **Mechanism:** A 3-digit code printed on the back. It is *not* stored in the magnetic stripe data.
  *   **Security Level:** Higher. Prevents fraud where the attacker stole the card number (digital skim) but doesn't have the physical card.
- ## V. Writing Measurable Requirements
  "Make the system secure" is a bad requirement because it cannot be tested. You need **Specific, Measurable** requirements.
  
  *   **Bad:** "The system should be safe."
  *   **Good:**
    *   **Access Control:** "Only users with the 'Manager' role can view Salary tables."
    *   **Auditing:** "Every deletion of a record must be logged with a timestamp and user ID."
    *   **Availability:** "The system must handle 10,000 concurrent connections without crashing."
    *   **Encryption:** "All data traffic must use TLS 1.3."
  
  ---
- ### Study Questions for Part 2
  1.  **Architecture:** You are auditing a company that has a world-class firewall costing $100,000. However, once you connect to their Wi-Fi, you can access any server without a password. What architectural flaw is this?
  2.  **Trade-offs:** A company implements a security policy requiring employees to plug in a physical USB key every time they want to save a file. Productivity drops by 20%, and employees start emailing files to their personal Gmail to work faster. How does this illustrate the "Convenience vs. Security" concept?
  3.  **Requirements:** Convert this vague requirement into a measurable security requirement: "The website should stop hackers from stealing passwords." (Hint: Think about encryption standards or lockout policies).
  
  ---