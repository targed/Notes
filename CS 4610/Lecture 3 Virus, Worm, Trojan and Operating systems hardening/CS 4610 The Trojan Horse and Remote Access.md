## I. The Trojan Definition
*   **The Disguise:** A Trojan presents itself as a desirable or benign program (e.g., a free game, a "PC optimizer," or a screensaver).
*   **The Activation:** Unlike a worm, a Trojan **must be run by the user**. The user actively gives the program permission to execute, which often bypasses many security warnings.
*   **Non-Replicating:** Trojans do not typically "infect" other files or crawl across a network like viruses or worms. If Bob downloads a Trojan, Alice is safe unless she also downloads it herself.
*   **The "Payload":** The Trojan is just the delivery vehicle. Once executed, it performs its real mission:
  *   Stealing/Deleting data.
  *   Monitoring system activity (Spyware).
  *   **Installing a Backdoor:** This turns the victim's computer into a "Zombie" or "Bot" that the attacker can control remotely.
- ## II. Case Study: Back Orifice 2000 (BO2k)
  Created by the hacker group "Cult of the Dead Cow," BO2k was designed to prove how insecure Windows 9x and NT were at the time. It is a classic **RAT (Remote Access Trojan)**.
- ### 1. Dual-Use Nature
  *   The creators called it a "Remote Administration tool."
  *   *Security Concept:* Many hacking tools are "dual-use." A system administrator might use BO2k to fix a computer from home, while an attacker uses it to steal passwords. The tool doesn't care who is holding the controls.
  *   **Network Signature:** By default, it listens on **TCP 54320** or **UDP 54321**. (In a modern environment, a firewall would be configured to block these specific ports immediately).
- ### 2. Persistence: Surviving a Reboot
  An attacker doesn't want to lose access when the victim turns off their computer. They need **Persistence**.
  *   **The Windows Registry:** The Registry is a database that stores settings for the OS and apps. 
  *   **The Hook:** BO2k modifies the "Run" key in the registry:
    *   `HKEY_LOCAL_MACHINE\software\Microsoft\Windows\CurrentVersion\Run` (or similar).
  *   By adding its executable (`vmgr.exe`) to this key, the attacker ensures that Windows automatically launches the Trojan every single time the computer starts up.
- ### 3. Total System Takeover
  The power of a RAT like BO2k is that it gives the attacker a "Virtual Seat" in front of your computer.
  *   **Privacy Violations:** It can remotely turn on microphones and cameras to spy on the physical room.
  *   **Credential Theft:**
    *   **Keystroke Logging:** Recording every letter typed (capturing usernames, passwords, and private messages).
    *   **SAM Database Dump:** In Windows, passwords aren't stored as text; they are stored as "hashes" in the **Security Account Manager (SAM)** database. BO2k can steal this file so the attacker can use a tool like "L0phtCrack" to guess the passwords offline.
  *   **System Manipulation:** The attacker can edit files, reboot the machine, or even "play" with the user by opening/closing the CD tray or typing messages into dialog boxes.
  
  ---
- ### The "Shadow" Registry
  The slides mention `vmgr.exe`. Attackers often name their malicious files to look like legitimate system processes. For example, a user might see `svchost.exe` (legitimate) and `svch0st.exe` (malicious) in their task manager and never notice the difference. This is a form of **obfuscation**.
  
  ---
- ### Study Questions
  1.  **Categorization:** If a piece of malware spreads itself via the network (Worm) but its primary purpose is to allow an attacker to remotely control the computer (RAT), is it a Worm or a Trojan? (Hint: It can be both—this is called **Multipartite** or "Blended Threat" malware).
  2.  **Persistence:** If you find a suspicious file on your computer and delete it, but it reappears the next time you boot up, where should you look to find the "instructions" that are bringing it back?
  3.  **AuthN Protection:** How does "Keystroke Logging" render even the most complex, 20-character password useless? (Think back to the "Something you Know" factor from Module 1).
  
  ---