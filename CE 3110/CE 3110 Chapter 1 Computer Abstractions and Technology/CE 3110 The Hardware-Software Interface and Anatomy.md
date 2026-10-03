#### **1. The Translation Hierarchy**
  Computers only understand electricity (on/off). They do **not** understand C++, Java, or Python. We need a chain of translation to get there.
  
  *   **High-Level Language (HLL):** (e.g., C, Java, Python)
  *   *Purpose:* Productivity and Portability. You write code once, and it can run on an Intel chip or an ARM chip (with recompilation).
  *   *Notation:* Uses mathematical formulas (`a + b`) and English words (`if`, `while`).
  *   **Assembly Language:**
  *   *Purpose:* A textual representation of the machine's specific instructions.
  *   *Notation:* Mnemonics like `ADD`, `SUB`, `LW` (Load Word).
  *   *Relationship:* Usually **1-to-1 mapping** with machine code. One line of Assembly = One machine instruction.
  *   **Machine Code (Hardware Representation):**
  *   *Purpose:* The only language the hardware actually executes.
  *   *Notation:* Binary (bits). Zeros and Ones (Low voltage/High voltage).
  
  **The Translation Tools (Memorize these definitions):**
  *   **Compiler:** Translates High-Level Language $\rightarrow$ Assembly Language.
  *   **Assembler:** Translates Assembly Language $\rightarrow$ Machine Code (Binary).
  *   *Note:* The slides show `Linker` concepts implicitly. A **Linker** combines multiple machine code files into one executable application.
#### **2. The Five Classic Components**
  Slide 12 shows "The BIG Picture." Every computer, from a supercomputer to a smart-watch, consists of these **five components** (often called the Von Neumann Architecture):
  
  1.  **Input:** Feeds data into the system (Mouse, Keyboard, Mic, Touchscreen).
  2.  **Output:** Delivers results to the user (Display, Speaker, Printer).
  3.  **Memory:** Stores data and instructions.
  4.  **Datapath:** (Part of the Processor)
  5.  **Control:** (Part of the Processor)
  
  **Important Equation:**
  $$ \text{Processor (CPU)} = \text{Datapath} + \text{Control} $$
#### **3. Inside the Processor**
  The CPU is the "brain," but it has two distinct personalities:
  
  *   **The Datapath (The Brawn):**
    *   Performs the actual arithmetic operations.
    *   Contains the **ALU** (Arithmetic Logic Unit), registers, and internal buses.
    *   *Think of it as:* The workers on a factory floor doing the physical assembly.
  *   **The Control (The Brain):**
    *   Tells the Datapath what to do.
    *   It decodes the instructions and turns switches on/off to route data correctly.
    *   *Think of it as:* The manager holding the clipboard yelling "You add these numbers, you store that result."
  *   **Cache Memory (Slide 16):**
    *   Located *inside* the processor package.
    *   **SRAM (Static RAM):** Very fast, very expensive. Used to hold data the CPU needs *right now*.
#### **4. Memory & Storage Hierarchy**
  The slides distinguish between "Main Memory" and "Secondary Memory." This is a trade-off between speed and permanence.
  
  | Type | Component | Technology | Characteristics |
  | :--- | :--- | :--- | :--- |
  | **Volatile** | **Main Memory (DRAM)** | Dynamic RAM | Data is **lost** when power is cut. Fast access. Where programs live *while* they are running. |
  | **Non-Volatile** | **Secondary Storage** | Magnetic Disk (HDD) | Spinning platters. Slow, cheap, huge capacity. Keeps data without power. |
  | **Non-Volatile** | **Flash Memory** | EEPROM/NAND | SSDs. Faster than magnetic disk, more durable (no moving parts), but more expensive per GB. |
#### **5. I/O Mechanics**
  *   **The Optical Mouse:** An example of how analog worlds meet digital.
    *   It uses a tiny camera (DSP - Digital Signal Processor) to take pictures of the surface 1,500+ times a second. It compares picture A to picture B to calculate movement $(\Delta x, \Delta y)$.
  *   **The Display (LCD):**
    *   **Frame Buffer:** A dedicated chunk of memory that stores the image to be shown.
    *   **Raster Scan:** The hardware reads the Frame Buffer row-by-row and updates the pixels on the screen. To change the image on screen, the CPU writes new bits to the Frame Buffer memory.
    *   *Color Depth:* 24-bit color is standard (8 bits Red + 8 bits Green + 8 bits Blue).
  
  ---
### **Student "Check Your Understanding"**
  *Try to answer these based on Section 2:*
  
  1.  If you write a program in C, which software tool translates it into Assembly? Which tool translates that into Binary?
  2.  What are the two sub-components that make up the **Processor (CPU)**? What is the function of each?
  3.  Why do we need both DRAM (Main Memory) and Hard Disks (Secondary Memory)? Why can't we just use Hard Disks for everything?