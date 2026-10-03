#### **1. Semiconductor Manufacturing**
  Computers are built on **Silicon**, a semiconductor. This means we can modify it to either conduct electricity or insulate against it, effectively creating tiny switches (transistors).
  
  **The Manufacturing Process (The "Pipeline"):**
  1.  **Ingot:** A giant sausage-shaped crystal of pure silicon.
  2.  **Wafer:** The ingot is sliced into thin, circular disks (like deli meat).
  3.  **Patterning:** Chemicals and light (photolithography) etch billions of transistors onto the wafer.
  4.  **Dies:** The wafer is chopped into small squares. Each square is a processor (a "die").
  5.  **Testing & Yield:**
  *   **Yield:** The percentage of dies on a wafer that actually work.
  *   *Why this matters:* Manufacturing is imperfect. Dust or chemical errors ruin chips. If a wafer holds 100 chips and only 50 work, the **Yield is 50%**. You have to throw the broken ones away, which drives up the cost of the working ones.
#### **2. The Most Important Abstraction: ISA**
  This is a guaranteed exam topic.
  
  *   **ISA (Instruction Set Architecture):**
    *   **Definition:** The "Contract" or "Interface" between the Hardware and the Software.
    *   It defines the instructions the processor understands (e.g., "Add registers," "Jump to address").
    *   *Significance:* It allows different hardware designers (Intel, AMD) to build different physical chips that run the exact same software, provided they implement the same ISA (like x86).
  *   **ABI (Application Binary Interface):**
    *   The ISA + The Operating System interfaces. This is what defines the portability of a compiled binary file.
#### **3. Defining Performance**
  What does it mean for a computer to be "fast"? The slides use an **Airplane Analogy** (Boeing 747 vs. Concorde) to distinguish between two types of speed.
  
  *   **Response Time (or Latency/Execution Time):**
    *   *Definition:* How long it takes to finish **one** specific task.
    *   *Airplane Analogy:* The **Concorde** is faster here. It flies from NY to London in 3 hours. (The Boeing takes 6.5 hours).
    *   *Computer Context:* Crucial for consumer PCs (how fast does Chrome open? How fast does the game load?).
  *   **Throughput (or Bandwidth):**
    *   *Definition:* The total amount of work done in a given amount of time.
    *   *Airplane Analogy:* The **Boeing 747** wins here. It moves 470 passengers in 6.5 hours. The Concorde only moves 100 passengers in 3 hours. The Boeing moves more people per hour total.
    *   *Computer Context:* Crucial for Servers (Google search servers don't care about one specific search speed as much as handling 5 billion searches per hour).
  
  **Key Relationship:**
  *   Replacing a processor with a faster one improves **both** Response Time and Throughput.
  *   Adding *more* processors (multicore) usually improves **Throughput** only (unless the program is specifically written to be parallel).
#### **4. Relative Performance Math**
  This is the first formula you must memorize.
  
  $$ \text{Performance} = \frac{1}{\text{Execution Time}} $$
  
  If we want to compare Computer X and Computer Y:
  $$ \frac{\text{Performance}_X}{\text{Performance}_Y} = \frac{\text{Execution Time}_Y}{\text{Execution Time}_X} = n $$
  *(Notice the flip: Time Y is on top because "Faster" means "Less Time".)*
  
  **The "Times Faster" Rule:**
  If Computer A runs a program in **10 seconds** and Computer B runs it in **15 seconds**:
  $$ \frac{15}{10} = 1.5 $$
  *   "Computer A is **1.5 times faster** than Computer B."
#### **5. Measuring Time**
  When we measure time, we have to be specific about *what* time we are counting.
  
  *   **Elapsed Time (Wall-Clock Time):**
    *   The time you feel waiting. It counts everything: CPU calculation, disk reading/waiting, network lag, and other programs running in the background.
    *   *Use case:* Determining total system performance.
  *   **CPU Time:**
    *   The time the CPU spends *only* on your specific program. It ignores waiting for I/O (disk/network) and ignores time spent running other people's apps.
    *   **User CPU Time:** Time spent running your code.
    *   **System CPU Time:** Time the OS spends doing things on your behalf (like printing to the screen).
  
  ---
### **Student "Check Your Understanding"**
  *Try to answer these based on Section 3:*
  
  1.  **Math Problem:** Computer A finishes a task in 5 seconds. Computer B is **2 times faster** than Computer A. How many seconds does Computer B take? (Careful!)
  2.  If you add a second processor to a web server, does it primarily improve **Response Time** (latency) or **Throughput**?
  3.  Why is the **ISA** called the "Interface" between hardware and software? (i.e., What does it allow software to do regarding hardware implementation?)