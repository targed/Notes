## I. The Core Problem: Multiprogramming (Slide 3)
In the early days of computing, a computer ran one program at a time. If that program crashed, the computer halted. Today, modern operating systems use **Multiprogramming** (or multitasking), allowing dozens of processes to run simultaneously.

*   **The Risk:** Because multiple processes (like your web browser, a background updater, and a password manager) are running on the same physical CPU and sharing the same RAM, a bug or malicious exploit in one program could theoretically read or overwrite the memory of another.
*   **Resources Requiring Protection:**
  *   **Memory:** Preventing Process A from reading Process B's passwords.
  *   **Shared I/O Devices:** Preventing a malicious app from silently turning on your webcam or intercepting keystrokes intended for another app.
  *   **Networks & Shared Data:** Ensuring one user cannot delete another user's files on a shared server.
- ## II. The Four Methods of Separation (Slide 4)
  How do we keep these processes apart? Security architects use four primary methods:
  
  1.  **Physical Separation:**
    *   *Concept:* Different processes run on completely different, physically disconnected hardware.
    *   *Example:* A military base having one computer network for "Unclassified" work and a completely separate, unplugged network for "Top Secret" work (Air-gapping).
    *   *Pros/Cons:* The most secure method, but extremely expensive and highly inconvenient.
  2.  **Temporal (Time) Separation:**
    *   *Concept:* Processes run on the same hardware, but at different times.
    *   *Example:* User A uses the terminal from 9 AM to 12 PM. At 12 PM, the memory is wiped clean, and User B logs in.
    *   *Pros/Cons:* Prevents concurrent memory reading, but highly inefficient for modern rapid-task-switching CPUs.
  3.  **Logical Separation:**
    *   *Concept:* The Operating System uses software and hardware tricks to create an *illusion* of isolation.
    *   *Example:* Virtual Memory. As we learned in the previous module, every process thinks it has its own 4GB of RAM (Process Address Space). The OS legally restricts them from crossing into each other's space.
  4.  **Cryptographic Separation:**
    *   *Concept:* Processes might share the exact same physical space, but the data is scrambled with math.
    *   *Example:* Two users saving files on the same hard drive. User A's files are encrypted with their password; even if User B accesses the raw bytes, they just see garbage.
- ## III. Degrees of Separation (Slide 5)
  Separation is not a simple "On/Off" switch. It exists on a spectrum depending on the needs of the system:
  
  *   **None:** MS-DOS. Every program had full access to the entire computer. A single buggy game could crash the whole PC.
  *   **Complete Isolation:** The processes are completely oblivious to one another. (Think Virtual Machines running on a Hypervisor).
  *   **Binary Sharing:** Simple "Public" vs. "Private" folders.
  *   **Limited Sharing (Granular):** Using Access Control Lists (ACLs). E.g., "Process A can *Read* the file, but only Process B can *Write* to it."
  *   **Usage Control:** This is the most advanced. It dictates not just *who* can access the resource, but *how* they can use it.
    *   *Expanded Context:* Think of Digital Rights Management (DRM). You are authorized to open a PDF, but the Usage Control prevents you from clicking "Print" or "Copy Text."
- ## IV. Granularity of Separation (Slide 6)
  At what level do we draw the boundaries? 
  *   **OS Level:** Separating entirely different operating systems on one server using Virtual Machines (VMware, VirtualBox).
  *   **Program Level:** Separating distinct processes (e.g., keeping Spotify's memory separate from Microsoft Word's memory).
  *   **Parts of a Program (Sub-process Level):** This is highly relevant today. Modern browsers like Chrome use "Sandboxing." If one specific tab (part of the program) crashes or gets infected by a malicious script, the boundary ensures it doesn't crash the other tabs or the main browser process.
  
  ---
- ### Study Questions for Part 1
  1.  **Method Identification:** A bank requires its database administrators to use a specific, dedicated laptop that has no Wi-Fi or internet connection to make changes to the core ledger. Which of the four "Methods of Separation" does this represent?
  2.  **Multiprogramming Risks:** Why did the shift from single-tasking computers to multiprogramming computers make memory protection an absolute necessity?
  3.  **Usage Control vs. Access Control:** You share a Google Doc with a peer. They are allowed to view the document, but you disable their ability to download or screenshot it. Is this an example of Limited Sharing (Access Control) or Usage Control? Why?
  
  ---