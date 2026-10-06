## I. Host-Based IDS (HIDS) (Slides 35–38)
A Host-Based IDS is installed as a software agent directly on a single computer or server (the "host"). 

*   **How it works:** It integrates deeply with the Operating System. It audits highly specific, local activities that never cross a network cable. 
  *   *Examples:* Monitoring exactly which user opened a specific file, watching for changes to the Windows Registry, or tracking the sequence of system calls made by a running application.
*   **The Advantage:** **Ultimate Visibility.** If an attacker uses a zero-day exploit over an encrypted connection, a network monitor is blind. But the HIDS will instantly see the exploit when the compromised application suddenly tries to spawn a root shell (`/bin/sh`). 
*   **The Limitations:**
  1.  *Deployment Cost:* You must install and maintain the HIDS software on every single laptop, desktop, and server in the company.
  2.  *The Local View:* It only sees what happens on its specific machine. It cannot detect a widespread "port scan" hitting 50 different computers across the network.
  3.  *The Compromise Risk:* If the attacker gains "root" or "admin" access, they can simply turn the HIDS off or delete the alarm logs before anyone notices.
- ### The Ultimate HIDS Defeat: Rootkits (Slide 38)
  If an attacker compromises a host, their first priority is hiding from the HIDS and the system administrator. They do this by installing a **Rootkit**.
  *   **The Mechanism:** A rootkit replaces the fundamental binaries of the Operating System with "Trojan" versions. 
  *   **The Illusion:** When the admin types `ps` (list processes) or `netstat` (list network connections), they aren't running the real OS commands; they are running the hacker's trojan commands. The trojan commands look normal, but they intentionally filter out and hide the hacker's malware processes and network connections. The system is actively lying to the administrator.
- ## II. Network-Based IDS (NIDS) (Slides 39–42)
  A Network-Based IDS is a dedicated appliance (or software) placed at a strategic choke point on the network, usually sitting right behind the firewall.
  
  *   **How it works:** It is **passive**. It sits on the wire and reads a copy of every single packet flowing in and out of the network. It looks for protocol violations, port scans, and attack strings in the packet payloads.
  *   **The Advantage:** **Broad Coverage.** A single NIDS can protect thousands of computers simultaneously. It provides a macro-view of the network, easily spotting widespread, coordinated attacks.
  *   **Real-World Example - Snort (Slides 41-42):** Snort is the most famous open-source NIDS. It uses a massive rule-set of over 4,000 signatures. 
    *   *Snort Rule Syntax:* Has a **Header** (Action, Protocol, Source IP/Port $\rightarrow$ Dest IP/Port) and **Options** (the specific payload string to search for, and the alert message to generate).
- ### Limitations of NIDS (Slide 40)
  *   **Blind to Encryption:** If the attacker connects to an internal web server via HTTPS (TLS), the payload is encrypted. The NIDS only sees scrambled ciphertext and cannot scan for attack signatures.
  *   **Non-Network Attacks:** If an employee plugs a malicious USB drive directly into a server, the NIDS never sees the traffic. 
  *   **Bandwidth Overload:** A NIDS must process gigabits of traffic per second. 
    *   *The Attack:* An attacker floods the network with massive amounts of garbage data. The NIDS CPU maxes out and starts dropping packets because it can't keep up. The attacker *then* sends the real exploit, knowing the NIDS is too overwhelmed to scan it.
- ## III. NIDS Evasion Techniques (Slide 43)
  Attackers know NIDS exist, so they use clever network tricks to slip past the signature scanners.
  
  1.  **Fragmentation:** If the NIDS is looking for the signature `DROP TABLE`, the attacker splits the attack across two IP packets: `DROP T` in packet one, and `ABLE` in packet two. A simple NIDS scans each packet individually, sees no match, and lets both through. 
  2.  **Out-of-Order Delivery:** The attacker sends the `ABLE` packet first, and the `DROP T` packet second. If the NIDS only remembers the immediately previous packet, it fails to put the signature together.
  3.  **TCP Tricks (Overlapping Fragments/TTL Evasion):** The attacker sends a packet with a short Time-To-Live (TTL) that contains garbage data, and a second packet with the real exploit. The NIDS, sitting at the perimeter, sees and reassembles both, effectively scrambling the signature. However, the garbage packet's TTL expires before reaching the actual target server, so the target server only receives the pure exploit. *The NIDS and the target server see two completely different realities!*
- ## IV. Module Summary (Slide 44)
  *   **Firewalls** are the standard, baseline defense (packet filters are cheap and common).
  *   **IDS (both HIDS and NIDS)** are required to catch what the firewall misses. You need *both* for Defense in Depth.
  *   **The Arms Race:** Attackers constantly invent new evasion techniques (encryption, fragmentation), and defenders constantly update algorithms to catch them.
  *   **The Core Problem:** Alarm volume (the Base-Rate Fallacy) remains the hardest challenge for security administrators today.
  
  ---
- ### Study Questions for Part 4
  1. **Rootkits vs. HIDS:** Explain why a system administrator running the `ls` or `netstat` command on a compromised machine might falsely believe the system is perfectly secure, even if malware is actively exfiltrating data.
  2. **NIDS Evasion:** Explain how an attacker could successfully launch a Buffer Overflow attack containing a string of `0x90` NOP instructions against a server, even if a NIDS is explicitly configured to block packets containing `0x90` strings. (Hint: Think about TCP/IP fragmentation).
  3. **Deployment Strategy:** Your company recently mandated that all internal network traffic between workstations and servers must be encrypted using IPsec or TLS. Why might the security team suddenly argue that they need to invest heavily in Host-Based IDS (HIDS) software for every machine?
  
  ---