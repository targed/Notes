## I. Definition and "The Why"
*   **The Standalone Nature:** A worm does not need to attach to an existing program. It is its own executable process.
*   **No User Intervention:** This is the most dangerous aspect. A user doesn't have to click a link or open an email. If your computer is on and connected to the network, and it has an unpatched vulnerability, the worm can force its way in.
*   **Worm vs. Virus (The Comparison):**
  *   **Viruses** typically use **Social Attacks** (tricking Alice into running a "cool" program).
  *   **Worms** use **Technical Attacks** (exploiting a bug in the Operating System or a network service).
*   **Primary Goal:** While viruses often try to steal or delete data, worms often aim for **Denial of Service (DoS)**. By replicating exponentially, they saturate network bandwidth and crash servers.
- ## II. The Worm Lifecycle (Expanded Context)
  A successful worm follows a four-step loop:
  1.  **Scanning:** The worm generates random IP addresses to find "live" computers on the internet.
  2.  **Exploitation:** It tries to use a specific exploit (like a **Buffer Overflow**) against a service running on those IPs (like a web server or a DNS server).
  3.  **Self-Copying (Propagation):** Once it gains access, it sends a copy of its own code over the network to the victim.
  4.  **Execution:** It runs itself on the new machine and starts the loop again from Step 1.
- ## III. Case Study 1: The Lion Worm (Linux)
  The Lion worm is a classic example of a multi-stage technical attack on Linux systems.
  *   **The Exploit:** It targets a **Buffer Overflow** in **BIND** (Berkeley Internet Name Domain), which is the software that runs the DNS system.
  *   **The Privilege:** Because DNS is a core system service, Lion gains **Root Access** (the highest possible permission level).
  *   **The Theft:** It targets the most sensitive files in Linux:
    *   `/etc/passwd`: List of user accounts.
    *   `/etc/shadow`: The file containing the **encrypted hashes of user passwords**.
  *   **The Exfiltration:** It emails these files to a specific address (`huckit@china.com`), allowing the attacker to crack the passwords at their leisure.
- ## IV. Case Study 2: The Code Red Infection (Windows)
  Code Red (July 2001) changed how the world viewed cybersecurity because of its speed and its unique "fileless" nature.
  
  *   **Fileless Malware:** Most malware saves a file to the hard drive (e.g., `virus.exe`). Code Red **never touched the hard drive**. It existed only in the computer's **RAM (Memory)**. 
    *   *Security Implication:* If you reboot the computer, the worm is gone. However, as soon as you reconnect to the internet, you are likely reinfected within minutes.
  *   **The "Logic Bomb" Schedule:** Code Red behaved differently based on the calendar:
    *   **Days 1–19:** Propagation Phase. It spawns 99 "threads" (mini-programs) to scan random IP addresses and spread.
    *   **Days 20–27:** Attack Phase. Every infected computer on Earth coordinated to launch a **DDoS attack** against the White House web server's IP address.
    *   **Day 28+:** It goes into a dormant/sleep state.
  *   **Defacement:** It hooked the web server's functions to replace legitimate pages with a "Hacked By Chinese!" message.
  *   **The Impact:** It infected **250,000 systems in just 9 hours**. This speed is only possible because no humans were involved in the spread. It cost businesses billions in lost productivity and "cleaning" costs.
  
  ---
- ### Study Questions
  1.  **Infrastructure Risk:** Why did the Lion worm target **BIND** (DNS software) specifically? What is the advantage for an attacker to control a DNS server compared to a single user's desktop?
  2.  **The "Fileless" Threat:** From an Anti-Virus perspective, why is a "fileless" worm like Code Red harder to detect than a traditional virus? (Hint: Refer back to "Static Detection" of the previous module).
  3.  **Network Math:** If a worm infects one computer, and that computer infects two more every hour, how many computers are infected after 24 hours? This illustrates the "Exponential Spread" mentioned in the slides.
  
  ---