# Lecture 15: Kirchhoff's Laws
  
  This lecture introduces Kirchhoff's Laws, which are fundamental tools for analyzing more complex circuits than the simple series circuits we've seen so far.
## Drawing Circuit Diagrams
  
  *   **Circuit Symbols:** Standardized symbols are used to represent circuit components in diagrams:
    *   **Battery:**  Longer line represents the positive terminal, shorter line is the negative terminal.
    *   **Resistor:**  A zigzag line.
    *   **Wire:**  A straight line (assumed to have negligible resistance).
    *   **Capacitor:** Two parallel lines of the same length
    *   **Junction:**  A point where three or more wires connect.
## Review: Key Concepts from Lecture 14
  
  *   **Conservation of Current:**  Current is the same at all points in a single, unbroken path (a current-carrying wire). Current is not "used up."
  *   **Kirchhoff's Junction Rule:** The sum of currents entering a junction equals the sum of currents leaving the junction.  This is a statement of charge conservation.
  *  **Ideal Wires**: Wires that have negligible resistance, so the potential does not change.
  *   **Resistors:**  $|\Delta V| = IR$ (Ohm's Law).  Current flows from higher potential to lower potential through a resistor.
## Kirchhoff's Laws
  
  Kirchhoff's Laws are two fundamental rules that govern the behavior of current and voltage in circuits:
  
  1.  **Kirchhoff's Junction Rule (KJR):**
    *   As stated above, the sum of currents entering a junction equals the sum of currents leaving the junction.
    *   Mathematically:  ΣI_in = ΣI_out
  
  2.  **Kirchhoff's Loop Rule (KLR):**
    *   The sum of the potential differences (voltage changes) around *any* closed loop in a circuit is zero.
    *   Mathematically:  ΣΔV = 0 (around any closed loop)
    *   **Physical Basis:** This is a consequence of the fact that electric potential energy is a function of position. If you return to the same point in a circuit, you must return to the same potential energy, and therefore the same potential.  Thus, the net change in potential energy (and therefore potential) around a closed loop must be zero.
## Analyzing Circuits with Kirchhoff's Laws
  
  **General Procedure:**
  
  1.  **Draw the Circuit Diagram:**  Clearly label all components (resistors, batteries, etc.) and their values.
  2.  **Assign Current Directions:**  Choose a direction for the current in each branch of the circuit.  Don't worry if you guess wrong; the math will correct it (you'll get a negative value for the current).
  3.  **Apply the Junction Rule:**  Write down equations for the junction rule at one or more junctions in the circuit.
  4.  **Apply the Loop Rule:**  Choose one or more closed loops in the circuit.  For each loop:
    *   **Choose a Starting Point and Direction:**  Pick any point on the loop to start and choose a direction (clockwise or counterclockwise) to go around the loop.
    *   **Sum the Potential Differences:**  As you go around the loop, add up the potential differences across each component, paying careful attention to the signs:
        *   **Battery:**  +ℰ if you go from the negative to the positive terminal; -ℰ if you go from positive to negative.
        *   **Resistor:**  -IR if you go through the resistor in the same direction as the *assumed* current; +IR if you go against the current.
    *   **Set the Sum to Zero:** ΣΔV = 0
  
  5.  **Solve the System of Equations:** You will now have a system of linear equations (from the junction and loop rules) that you can solve to find the unknown currents, voltages, or resistances.
## Example (From the Lecture Slide)
  The slide presents a circuit, but there is not enough information to completely solve the equations. We can set them up.
  We label the currents: I1, I2, I3
  We label the junctions: A, B.
  Juntion A: I1 + I3 = I2
  We pick two loops. Loop 1 is on the left. We can make an equation from it.
  emf1 - I1R1 - I2R2 = 0
  Loop 2 is on the right. 
  emf2 - I3R3 + I2R2 = 0
## Practice Problem (Q. 23.3)
  
  **Problem:**  Compare I_out and I_in. Rank the resistors.
  **Solution:**
  Kirchhoff's Junction rule states that the current in must be equal to the current out.
  $I_{in} = I_{out}$
## Practice Problems (Q. 23.4 and Q. 23.5)
  
  *23.4*
  R1 > R2. Which dissipates the larger amount of power, if they are in series?
  
  *   **Solution:**  Resistors in *series* have the same current (I) flowing through them.  The power dissipated by a resistor is given by P = I<sup>2</sup>R.  Since I is the same for both resistors, the resistor with the *larger* resistance (R_1) will dissipate more power.
  
  *23.5*
  R1> R2. Which dissipates the larger amount of power, if they are in parallel?
  *   **Solution:** Resistors in parallel have the same potential difference (ΔV).  The power dissipated in a resistor can be expressed as $P = \frac{|\Delta V|^2}{R}$.  Since the voltage is the same for both resistors, the resistor with the *smaller* resistance (R2) dissipates the greater amount of power.
## Summary
  
  Kirchhoff's Laws (Junction Rule and Loop Rule) are essential tools for analyzing complex circuits.  The Junction Rule is based on charge conservation, and the Loop Rule is based on the conservative nature of the electrostatic force (and the concept of potential). By applying these rules systematically, we can solve for unknown currents, voltages, and resistances in a wide variety of circuits.