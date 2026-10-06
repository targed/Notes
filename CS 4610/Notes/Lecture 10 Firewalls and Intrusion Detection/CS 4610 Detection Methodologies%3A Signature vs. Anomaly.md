## I. Signature-Based IDS (Misuse Detection) (Slides 22–24, 27)
This is the traditional, most widely used method. It works exactly like classic Anti-Virus software. 

*   **The Mechanism:** The IDS maintains a database of specific patterns, rules, or "signatures" that belong to *known* attacks. It compares every network packet or system event against this database.
*   **Examples of Signatures (Slide 24):**
  *   **NOP Sleds (`0x90`):** As we learned in the Buffer Overflow module, attackers pad their payload with `0x90` (No Operation) bytes. If an IDS sees a network packet containing hundreds of `0x90` bytes in a row, it instantly flags it as a remote buffer overflow exploit.
  *   **TCP SYN Floods:** Seeing a massive number of TCP SYN packets coming from an IP address, but never seeing the follow-up ACK packets. This is a classic Denial of Service (DoS) signature.
  *   **Overly Long Strings:** Catching an HTTP GET request with a 10,000-character URL, which indicates an attempted buffer overflow.
*   **The Assessment (Pros & Cons):**
  *   *Pro:* **Low False Positive Rate.** If network traffic matches a highly specific attack signature, it is almost certainly an attack.
  *   *Con:* **High False Negative Rate.** It is completely blind to "Zero-Day" attacks. If a vulnerability was discovered this morning and has no signature in the database yet, the IDS will let it pass right through.
*   **The Arms Race & Signature Generation (Slide 27):**
  *   Because attackers know IDSes look for signatures, they use **obfuscation** and **polymorphism** (self-decrypting or mutating code) to change their signature every time they attack. 
  *   Defenders respond by setting up **Honeypots**—fake, vulnerable servers designed specifically to be hacked. The defenders watch the honeypot, capture the new attack payloads, automatically generate a new signature, and push it to IDSes worldwide.
- ## II. Anomaly-Based IDS (Slides 25, 28–32)
  Instead of looking for what is *bad*, an Anomaly-Based IDS looks for what is *not normal*.
  
  *   **The Mechanism:** 
    1.  **Training Phase:** The IDS watches the network/host for a period of weeks or months to build a statistical baseline of "normal" behavior.
    2.  **Detection Phase:** It compares live traffic against this baseline. If a statistically rare event or severe deviation occurs, it raises an alarm.
  *   **Examples of Anomaly Metrics (Slides 30–31):**
    *   *Resource Utilization:* CPU usage suddenly spiking to 100% at 3:00 AM.
    *   *Time/Location:* A user who normally logs in from the Missouri campus at 10:00 AM suddenly logs in from a foreign country at 4:30 AM.
    *   *System Call Sequences (Crucial!):* The IDS maps the normal behavior of a web server program. If the web server program suddenly makes an `execve()` system call to open `/bin/sh` (a root shell), the IDS detects a massive deviation and kills the process.
  *   **The Assessment (Pros & Cons):**
    *   *Pro:* **Low False Negative Rate.** It can catch brand new, never-before-seen Zero-Day exploits because the exploit will inherently cause the system to behave abnormally.
    *   *Con:* **High False Positive Rate.** "Normal" changes all the time. If the marketing team uploads a massive new video file, the sudden spike in bandwidth might trigger an alarm.
  *   **The Poisoning Risk (Slide 32):**
    *   If an attacker knows an IDS is in its "Training Phase," they can slowly and carefully introduce malicious traffic over time (like slowly boiling a frog). The IDS will incorporate this malicious activity into its baseline, effectively "learning" that the attack is normal behavior!
- ## III. Comparing the Two Approaches (Slides 26 & 33)
  
  Let's test our understanding using the examples from **Slide 26**:
  1.  **Password file modified:** *Signature/Misuse.* The `/etc/shadow` file should never be directly modified by a standard process. A simple rule can catch this.
  2.  **Four failed login attempts:** *Anomaly or Signature.* Can be a strict signature rule (e.g., "Alert on exactly >3 fails"), but is also a statistical anomaly compared to normal login behavior.
  3.  **User logs in at 4:30am from Lower Slobovia:** *Anomaly.* There is no "bad code" or "exploit" here to write a signature for. It is purely a behavioral deviation from the user's normal profile.
  4.  **UDP packet to port 1434 (Slammer Worm):** *Signature.* This is highly specific. Normal traffic rarely hits this specific database port with this specific UDP payload. 
  
  **The Conventional View (Slide 33):**
  Which is better? Neither works perfectly on its own. 
  Because Anomaly-Based IDS generates too many false positives (base-rate fallacy), modern security architectures **combine both**. They use Signature-Based IDS at the perimeter to cheaply and accurately block 99% of known "noise" and script-kiddies, and use Anomaly-Based IDS deeper inside the network to catch the sophisticated, stealthy 1% of attackers.
  
  ---
- ### Study Questions for Part 3
  1. **Detection Flaws:** You install a brand-new, perfectly updated Signature-Based NIDS. A nation-state attacker targets you with a Zero-Day buffer overflow. Does the NIDS catch it? Why or why not?
  2. **Anomaly Poisoning:** A company deploys an Anomaly-Based IDS and sets it to "Training Mode" for the month of October. Unbeknownst to them, a hacker had already breached their network in September and continues to exfiltrate data every night at 2:00 AM throughout October. How will the IDS react to the data exfiltration in November?
  3. **Metric Application:** Explain how monitoring "Short sequences of system calls" (Slide 31) can act as a powerful Anomaly-based defense against a Return-to-Libc attack.
  
  ---
-