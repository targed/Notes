# Lecture 17: Circuits - Series/Parallel Resistors, and Measurement
  
  This lecture builds on previous concepts, applying Kirchhoff's Laws to analyze circuits with resistors in series and parallel, and introduces the principles of measuring current and voltage.
## Review: Kirchhoff's Laws
  
  *   **Kirchhoff's Junction Rule (KJR):**
    *   The sum of currents entering a junction equals the sum of currents leaving the junction: ΣI_in = ΣI_out.
    *   Based on conservation of charge.
  
  *   **Kirchhoff's Loop Rule (KLR):**
    *   The sum of potential differences (voltage changes) around *any* closed loop in a circuit is zero: ΣΔV = 0.
    *   Based on the fact that electric potential energy is a function of position (conservative force).
## Resistors in Series
  
  *   **Connection:** Resistors are in series when they are connected end-to-end, forming a single path for current to flow.
  *   **Same Current:** The current (I) is the *same* through all resistors in series.  This follows directly from the conservation of current (Junction Rule).
  *   **Voltage Division:** The total voltage across the series combination (ΔV_total) is the *sum* of the voltages across the individual resistors:
  
    $\Delta V_{total} = \Delta V_1 + \Delta V_2 + \Delta V_3 + ...$
  
  *   **Equivalent Resistance (R_eq):** The equivalent resistance of resistors in series is the *sum* of the individual resistances:
  
    $R_{eq} = R_1 + R_2 + R_3 + ...$
  
    *   **Derivation:**
        *   Since the current is the same:  $I = ΔV_1/R_1 = ΔV_2/R_2$ = ...
        *   Total voltage: $ΔV_t = ΔV_1 + ΔV_2 + ... = IR_1 + IR_2 + ... = I(R_1 + R_2 + ...)$
        *   Equivalent resistance: $R_e = ΔV_t / I = R_1 + R_2 + ...$
  
  *   **Key Idea:**  Resistors in series *add* their resistances, making it more difficult for current to flow.
## Resistors in Parallel
  
  *   **Connection:** Resistors are in parallel when their "top" ends are connected together and their "bottom" ends are connected together, providing multiple paths for current to flow.
  *   **Same Voltage:** The potential difference (ΔV) is the *same* across all resistors in parallel.
  *   **Current Division:** The total current (I_total) entering the parallel combination is the *sum* of the currents through the individual resistors:
  
    $I_{total} = I_1 + I_2 + I_3 + ...$
  
  *   **Equivalent Resistance (R_eq):** The reciprocal of the equivalent resistance of resistors in parallel is the sum of the reciprocals of the individual resistances:
  
    $\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \frac{1}{R_3} + ...$
  
    *   **Derivation:**
        *   Same voltage: ΔV
        *   Total current: $I_{total} = I_1 + I_2 + ... = \frac{\Delta V}{R_1} + \frac{\Delta V}{R_2} + ... = \Delta V (\frac{1}{R_1} + \frac{1}{R_2} + ...)$
        *   Equivalent resistance: $\frac{1}{R_{eq}} = \frac{I_{total}}{\Delta V} =  \frac{1}{R_1} + \frac{1}{R_2} + ...$
  
  *   **Key Idea:** Resistors in parallel provide *more* paths for current to flow, *decreasing* the overall resistance. The equivalent resistance of parallel resistors is *always* less than the smallest individual resistance.
## Practice Problem (Circuit Diagram Analysis)
  The lecture slide contains a practice problem with different resistor configurations. The full solution cannot be given without the image, but it will involve applying KVL and KCL.
## Example (Circuit Analysis - Not fully specified)
  
  The lecture includes an example problem, but the circuit diagram isn't fully described in the text. The general approach to solving such problems is:
  
  1.  **Simplify Series/Parallel Combinations:**  Identify groups of resistors that are purely in series or purely in parallel and replace them with their equivalent resistances.
  2.  **Apply Kirchhoff's Laws:** If you can't simplify the circuit completely using series/parallel combinations, use Kirchhoff's Junction and Loop Rules to set up a system of equations.
  3.  **Solve for Unknowns:** Solve the equations to find the unknown currents, voltages, or resistances.
## Measuring Current and Voltage
  
  *   **Ammeter (Measures Current):**
    *   **Connection:** An ammeter is connected in *series* with the component whose current you want to measure. This is because the current must flow *through* the ammeter.
    *   **Ideal Ammeter:** An ideal ammeter has *zero* resistance.  This ensures that inserting the ammeter into the circuit doesn't change the current being measured.  (Real ammeters have a very small resistance.)
  
  *   **Voltmeter (Measures Voltage):**
    *   **Connection:** A voltmeter is connected in *parallel* with the component whose voltage you want to measure. This is because the voltmeter measures the potential difference *across* the component.
    *   **Ideal Voltmeter:** An ideal voltmeter has *infinite* resistance. This ensures that no current flows through the voltmeter, so it doesn't affect the voltage being measured. (Real voltmeters have a very high resistance.)
  
  * **Measuring Current and Voltage Simultaneously**:
    *   **Correct Connection:**  Connect the voltmeter in *parallel* with the resistor and the ammeter in *series* with the resistor.
  * **Wrong Connection**: The slide contains an example of a wrong connection. Without seeing the diagram, we can deduce a likely scenario. It likely involves placing an ammeter in parallel. If the ammeter, which should be in series, is wrongly placed in parallel with a resistor, it will create a short circuit (a path of very low resistance). This is because an ideal ammeter has zero resistance. Almost all the current will flow through the ammeter, bypassing the resistor, and potentially damaging the ammeter and/or other circuit components.
## Summary
  
  This lecture reviewed Kirchhoff's Laws and applied them to circuits containing resistors in series and parallel.  It also explained the proper way to connect ammeters and voltmeters to measure current and voltage, emphasizing the ideal characteristics of these instruments (zero resistance for ammeters, infinite resistance for voltmeters). The key takeaway is understanding how to simplify resistor networks and use Kirchhoff's laws to determine unknown circuit parameters.