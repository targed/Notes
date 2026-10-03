#### **1. The Computer Revolution**
  The driving force behind modern computing is **Moore’s Law**.
  *   **Slide Definition:** Integration density (number of transistors on a chip) doubles every two years.
  *   **Comprehensive Note:**
  *   **Origin:** Coined by Gordon Moore (Intel co-founder) in 1965.
  *   **Implication:** As transistors get smaller, they get faster and consume less power (historically). This allowed computers to move from building-sized to pocket-sized.
  *   **The "Slowing" aspect:** The slides mention it is slowing due to physical limitations. As transistors approach the size of atoms, we face issues like **heat dissipation** (thermal wall) and **quantum tunneling** (electricity leaking). This forces architects to stop just making clocks faster (frequency scaling) and start using **multicore** designs (parallelism) to keep improving performance.
#### **2. Classes of Computers**
  Computer architecture isn't "one size fits all." Design decisions depend on the intended use.
  
  | Class | Key Characteristics | Primary Design Constraints |
  | :--- | :--- | :--- |
  | **Desktop** | General purpose. Runs 3rd party software. | **Price/Performance ratio.** Must be affordable but fast enough to feel responsive. |
  | **Server** | Network-based. Runs large workloads (file storage, web hosting). | **Reliability, Availability, Scalability (RAS).** Throughput (transactions per second) is more important than single-task speed. |
  | **Supercomputer** | High-end scientific calc (weather forecasting, oil exploration). | **Floating-point performance.** Cost is rarely an object; speed is everything. Often built using thousands of server nodes. |
  | **Embedded** | Hidden inside other devices (car ABS, microwave, router). | **Power, Cost, & Real-time limits.** Often have strict hardware limits (memory/storage) and must react within a specific time frame. |
  | **Mobile (PMD)** | Personal Mobile Devices (Phones, Tablets). | **Battery Life (Energy efficiency).** Responsive UI and wireless connectivity are key. |
#### **3. The Post-PC Era**
  We have moved away from "one person, one desktop" to a model defined by **Cloud Computing** and **PMDs**.
  *   **PMD (Personal Mobile Device):** Replaces the PC for many users. The main challenge here is that they rely on batteries, so hardware must be aggressive about saving power (e.g., turning off parts of the chip when not in use).
  *   **Cloud Computing (WSC - Warehouse Scale Computers):**
    *   **SaaS (Software as a Service):** You don't install software; you access it (e.g., Google Docs, Netflix).
    *   **The Shift:** This moves the heavy processing power *off* your device and into massive datacenters (Amazon AWS, Google Cloud). This allows your phone to be "smart" without needing a supercomputer processor inside it.
#### **4. The Eight Great Ideas — ** *Critical Concept* ** **
  The slides list these icons, but exams usually ask you to identify or explain them. These are the guiding principles you will see in every chapter.
  
  1.  **Design for Moore’s Law:**
    *   *Concept:* It takes years to design a chip. Architects must design not for *today's* technology, but for the technology that will exist when the chip is mass-produced.
  2.  **Use Abstraction to Simplify Design:**
    *   *Concept:* Hiding details. A programmer writes high-level code (Python/C) and doesn't need to know how the electrons move. The hardware architect builds instructions (ADD, SUB) so the programmer doesn't worry about logic gates.
  3.  **Make the Common Case Fast:**
    *   *Concept:* Don't waste resources optimizing rare events. If a program does integer math 90% of the time and floating-point math 10% of the time, spend your money/silicon making the integer math unit faster.
  4.  **Performance via Parallelism:**
    *   *Concept:* Doing multiple things at once.
    *   *Example:* Multi-core processors (4 cores doing 4 different tasks) or distributed computing (clusters).
  5.  **Performance via Pipelining:**
    *   *Concept:* The "Assembly Line" approach. Don't wait for one instruction to finish completely before starting the next one.
    *   *Example:* While the CPU is *executing* instruction A, it is *decoding* instruction B and *fetching* instruction C.
  6.  **Performance via Prediction:**
    *   *Concept:* It is faster to guess and be wrong occasionally than to wait for a definite answer.
    *   *Example:* **Branch Prediction.** The CPU guesses an `if` statement will be true and starts working on that code immediately. If it was wrong, it throws the work away.
  7.  **Hierarchy of Memories:**
    *   *Concept:* Users want memory to be **fast**, **cheap**, and **huge**. You can't have all three in one component.
    *   *Solution:*
        *   **L1/L2 Cache:** Fast but small and expensive (SRAM).
        *   **RAM:** Medium speed, medium size (DRAM).
        *   **Disk/Flash:** Slow but huge and cheap.
  8.  **Dependability via Redundancy:**
    *   *Concept:* Things fail. We make them reliable by having backups.
    *   *Example:* RAID hard drives (data is written to two disks, so if one fails, the data is safe).
  
  ---
### **Student "Check Your Understanding"**
  *Try to answer these based on the notes above:*
  
  1.  Why is Moore's Law slowing down, and what design shift did this cause in modern processors?
  2.  If you are designing a computer for a satellite, which "Class of Computer" constraints apply, and what is likely the most critical constraint?
  3.  Explain the difference between **Parallelism** and **Pipelining** using a laundry analogy (Washing and Drying clothes).