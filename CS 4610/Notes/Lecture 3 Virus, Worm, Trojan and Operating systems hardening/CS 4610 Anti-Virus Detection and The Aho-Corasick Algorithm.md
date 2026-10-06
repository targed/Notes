## I. Three Approaches to Static Detection
1.  **Scanners (Signatures):** The most common method. It looks for a specific "string of bits" (a fingerprint) that is known to belong to a specific virus.
2.  **Heuristics:** Instead of looking for a specific name, it looks for **suspicious patterns** (e.g., "This program is trying to write to the Boot Sector, and it's not a system utility—that’s suspicious").
3.  **Integrity Checkers:** It records the "Hash" or "Checksum" of all clean files. If a file's hash changes later, the AV knows it has been modified (likely infected).
- ## II. Signature Scanning: On-Demand vs. On-Access
  *   **On-Demand:** You manually tell the AV to "Scan my C: drive now."
  *   **On-Access (The "Real-time" Shield):** The AV sits in the background. Every time you open, download, or copy a file, the AV intercepts the action, scans the file, and only allows the action if the file is clean.
    *   *The Performance Problem:* There are hundreds of thousands of known virus signatures. If the AV checked every file against every signature one-by-one, the computer would be unusable.
- ## III. The Engine: Aho-Corasick Algorithm
  To solve the speed problem, AVs use the **Aho-Corasick algorithm**. It allows the software to search for thousands of different signatures **at the same time** in a single pass through the file.
- ### 1. Building the "Trie"
  The signatures are organized into a tree-like structure called a **Trie** (a prefix tree).
  *   Each node represents a state.
  *   Moving from one node to the next means you have matched a letter/bit of a signature.
  *   **Breadth-First Order:** Labels are added layer-by-layer from the root.
- ### 2. The Failure Function
  This is the "magic" of the algorithm. If you are searching for the word `HIPS` and you find `H-I-P` but the next letter is `T` (making `HIPT`), you don't have to go back to the very beginning. 
  *   The **Failure Function** tells the scanner exactly where to jump in the tree to resume the search without re-reading what it just saw.
  *   *Efficiency:* This makes the search **Linear**—the time it takes to scan a file depends only on the size of the file, not how many signatures you are looking for.
- ## IV. Improving Scan Performance
  Even with fast algorithms, "Grunt Scanning" (scanning every single byte of every single file) is too slow. AVs use shortcuts:
  *   **Scan the Entry Point:** Viruses like "Appended Viruses" (Slide 11) change the start of the program. AVs jump straight to that start point.
  *   **Targeted Scanning:** Only scan files that can actually be infected (e.g., `.EXE`, `.JS`, `.DOCM`). You don't usually need to scan a `.TXT` file for a virus.
  *   **Change Detection:** If a file hasn't been modified since the last scan (same size, same date), the AV skips it.
- ## V. The Disinfection Problem
  "Disinfection" is the attempt to return an infected file to its original state. 
  *   **The Hard Truth:** It is not always possible. If an **Overwriting Virus** replaced your original code with malicious code, that original code is gone forever.
  *   **The Solution:** In many cases, the AV will simply **Delete** or **Quarantine** the file.
    *   *The Risk:* If the infected file is a critical Windows system file, deleting it might crash the entire OS. This is why "Quarantine" exists—it moves the file to a "jail" where it can't run but can be recovered if needed.
  
  ---
- ### Study Questions
  1.  **Algorithm Logic:** Why is the **Aho-Corasick** algorithm better for an Anti-Virus than a standard "Find" function found in a text editor?
  2.  **Detection Trade-offs:** If a new "Zero-Day" virus (one that has no known signature yet) enters a system, which detection method is most likely to catch it? (Scanners, Heuristics, or Integrity Checkers?)
  3.  **The Entry Point:** Why is "Scanning the code entry point" (Slide 41) a highly effective way to catch **Parasitic Viruses**?
  
  ---