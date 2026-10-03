# Lecture 14: Current, Resistance, and Ohm's Law
## Current
  
  *   **Definition:** Electric current (I) is the rate of flow of electric charge through a conductor.  It represents the motion of charges.
  *   **Charge Carriers:** In metals, the charge carriers are free electrons.
  *   **Cause:** Current is created by a potential difference (voltage) due to an electric field.  This is *different* from the electrostatic equilibrium case, where the electric field inside a conductor is zero. Now, charges are *moving*.
  
  *   **Conservation of Current:**
    *   Current is the *same* at all points in a current-carrying wire. Current is not "used up" as it flows through a circuit.
    *   This is a consequence of charge conservation – charge cannot be created or destroyed.
    *   **Kirchhoff's Junction Rule:** The sum of currents entering a junction (a point where wires connect) equals the sum of currents leaving the junction. "What goes in must come out."
  
  *   **Direction of Current:**
    *   **Conventional Current:** Defined as the direction of flow of *positive* charge.
    *   **Electron Flow:** In metals, the actual charge carriers are *negative* electrons, which move in the *opposite* direction to the conventional current.
  
  *   **Units:**
    *   SI unit of current is the Ampere (A).
    *   1 Ampere = 1 Coulomb/second (1 A = 1 C/s)
## Batteries
  
  *   **Function:**  Batteries are sources of potential difference (voltage) in a circuit. They maintain a potential difference between their terminals.
  *   **Mechanism:** Chemical reactions within the battery separate positive and negative charges, creating the potential difference.
    *   **Electrolyte:** A conducting solution or paste.
    *   **Electrodes:**  Two different materials (often metals) that interact with the electrolyte.
  *   **Electromotive Force (emf, ℰ):**
    *   The work done per unit charge by the chemical reactions in the battery:
  
        $ℰ = \frac{W_{chem}}{q}$
  
    *   Ideally, the terminal voltage (ΔV_bat) of a battery is equal to its emf:
  
        $\Delta V_{bat} = ℰ$
        
  *   **Internal Resistance (Real Batteries):**  Real batteries have a small internal resistance, which we will usually ignore in this course.  This internal resistance causes the terminal voltage to be slightly less than the emf when current is flowing.
  * **Batteries in Series**
  If batteries are connected in series, then the voltage increases: $\Delta V = \Delta V_1 + \Delta V_2$
## Simple Circuits
  
  *   **Components:**  A simple circuit typically consists of a battery (providing the voltage), a resistor (representing a device that uses electrical energy), and connecting wires.
  * **Current Equation**: $I = \frac{\Delta V_{chem}}{R}$
## Resistance and Resistivity
  
  *   **Resistance (R):** A measure of how difficult it is for current to flow through a material or device.
    *   **Relationship to Voltage and Current:**  For many materials, the current (I) through a device is proportional to the applied voltage (ΔV) across it.
  
        $R = \frac{|\Delta V|}{I}$  (This defines resistance)
  
    *   **Units:** SI unit of resistance is the Ohm (Ω).
        *   1 Ohm = 1 Volt/Ampere (1 Ω = 1 V/A)
  
  *   **Resistivity (ρ):**  A *material property* that describes how resistive a material is, independent of its shape or size.
    *   **Resistance of a Wire:** The resistance of a wire (or other object with uniform cross-section) is given by:
  
        $R = \frac{\rho L}{A}$
  
        Where:
  
        *   R: Resistance
        *   ρ: Resistivity
        *   L: Length of the wire
        *   A: Cross-sectional area of the wire
  
    *   **Units:** SI unit of resistivity is the Ohm-meter (Ω⋅m).
  
  * **Resistance is a device property**
## Example: Nichrome Wire
  
  **Problem:**
  
  A Nichrome wire is 15 cm (0.15 m) long.  When a potential difference of 1.5 V is applied across it, the current through the wire is 2.0 A.  What is the diameter of the wire? (Resistivity of Nichrome, ρ = 1.5 x 10<sup>-6</sup> Ωm)
  
  **Solution:**
  
  1.  **Calculate Resistance:**  Use the definition of resistance:
  
    $R = \frac{|\Delta V|}{I} = \frac{1.5 \text{ V}}{2.0 \text{ A}} = 0.75 \text{ }\Omega$
  
  2.  **Relate Resistance to Resistivity:**
  
    $R = \frac{\rho L}{A}$
  
  3.  **Solve for Area:**
  
    $A = \frac{\rho L}{R} = \frac{(1.5 \times 10^{-6} \text{ }\Omega \cdot \text{m})(0.15 \text{ m})}{0.75 \text{ }\Omega} = 3.0 \times 10^{-7} \text{ m}^2$
  
  4.  **Find Diameter:**  The cross-sectional area of a wire is a circle (A = πr<sup>2</sup> = π(d/2)<sup>2</sup> = πd<sup>2</sup>/4).
  
    $d^2 = \frac{4A}{\pi}$
    $d = \sqrt{\frac{4A}{\pi}} = \sqrt{\frac{4(3.0 \times 10^{-7} \text{ m}^2)}{\pi}} \approx 6.18 \times 10^{-4} \text{ m} = 0.618 \text{ mm}$
## Ohm's Law
  
  *   **Statement:** For *ohmic* materials, the voltage across a resistor is directly proportional to the current through it, and the constant of proportionality is the resistance:
  
    $|\Delta V| = IR$
  
  *   **Ohmic Materials:** Materials that obey Ohm's Law (i.e., have a constant resistance over a wide range of voltages).  Many metals are ohmic at constant temperature.
  *   **Non-Ohmic Materials:**  Materials and devices that *do not* obey Ohm's Law.  Examples include:
    *   Batteries
    *   Semiconductors (diodes, transistors)
    *   Capacitors
## Current Flow Through a Resistor
  
  *   **Ideal Wires:** We often assume that connecting wires have zero resistance ("ideal wires").
  *   **Reality:** In reality, wires have some resistance, but it's usually much smaller than the resistance of other circuit elements (like light bulbs, heaters, etc.).
  * **Example:** Flashlight R_bulb=3Ω, while R_wire = 0.01Ω.
## Energy and Power
  
  *   **Power (P):** The rate at which energy is transferred or transformed.
    *   **Units:** SI unit of power is the Watt (W).  1 Watt = 1 Joule/second (1 W = 1 J/s)
  
  *   **Power Dissipated in a Resistor:**  When current flows through a resistor, electrical energy is converted into thermal energy (heat). The power dissipated is:
  
    $P = I|\Delta V| = I^2R = \frac{|\Delta V|^2}{R}$ (These are equivalent forms, derived using Ohm's Law)
  
  *   **Energy (E):** Energy is power multiplied by time:
  
    $E = Pt$
  
  *    **Kilowatt-hour (kWh):** A common unit of energy used by electric companies.  1 kWh = (1000 W)(3600 s) = 3.6 x 10<sup>6</sup> J
## Example: Electric Heater
  
  **Problem:**
  
  An electric heater draws 15.0 A on a 120 V line.
  
  a) How much power does it use?
  b) How much energy does the heater use per hour?
  c) How much does it cost to run it for 3 hours/day for 30 days if the electric company charges 7.9 cents per kWh?
  
  **Solution:**
  
  a) **Power:**
  
    $P = I|\Delta V| = (15.0 \text{ A})(120 \text{ V}) = 1800 \text{ W} = 1.8 \text{ kW}$
  
  b) **Energy per Hour:**
  
    $E = Pt = (1800 \text{ W})(1 \text{ hour}) = (1800 \text{ J/s})(3600 \text{ s}) = 6.48 \times 10^6 \text{ J}$
    Or, in kWh:  E = (1.8 kW)(1 h) = 1.8 kWh
  
  c) **Cost:**
  
    *   Total energy used: (1.8 kWh/day) * (3 h/day) * (30 days) = 162 kWh
    *   Cost: (162 kWh) * ($0.079/kWh) = $12.798, or $12.80
## Summary
  
  This lecture introduced the fundamental concepts of current, resistance, and Ohm's Law. We learned how to calculate resistance from resistivity, how to analyze simple circuits, and how to calculate power and energy in electrical circuits. We also saw how batteries provide the driving force (emf) for current flow.