# Lecture 19: RC Circuits
  
  This lecture introduces RC circuits, which contain both resistors (R) and capacitors (C).  These circuits exhibit time-dependent behavior, as the capacitor charges or discharges through the resistor.
## Discharging a Capacitor
  
  *   **Circuit Setup:** A capacitor (initially charged with charge Q<sub>0</sub> and voltage V<sub>0</sub>) is connected in series with a resistor (R) and a switch. When the switch is closed, the capacitor begins to discharge through the resistor.
  *   **Kirchhoff's Loop Rule:** Applying Kirchhoff's Loop Rule (ΣΔV = 0) to the circuit during discharge:
  
    $\frac{q}{C} - iR = 0$
  
    Where:
  
    *   q is the charge on the capacitor at time t.
    *   C is the capacitance.
    *   i is the current in the circuit at time t.
    *   R is the resistance.
  
    Note that the voltage across the capacitor is q/C, and the voltage across the resistor is -iR (negative because the current flows in the direction of decreasing potential).
  
  *   **Differential Equation:** Since current is the rate of change of charge (i = dq/dt), and the charge is *decreasing* during discharge, we have i = -dq/dt.  Substituting this into the loop rule equation:
  
    $\frac{q}{C} + R\frac{dq}{dt} = 0$
  
    $\frac{dq}{dt} = -\frac{1}{RC}q$
    This means the instantaneous rate of change of charge is proportional to the charge.
  
  *   **Solution (Charge vs. Time):** The solution to this differential equation is an exponential decay:
  
    $q(t) = Q_0 e^{-t/RC}$
  
    Where:
  
    *   q(t) is the charge on the capacitor at time t.
    *   Q<sub>0</sub> is the initial charge on the capacitor (at t = 0).
    *   R is the resistance.
    *   C is the capacitance.
    *   e is the base of the natural logarithm (approximately 2.718).
  
  *   **Current vs. Time:** The current during discharge is:
  
    $i(t) = -\frac{dq}{dt} = -\frac{d}{dt}(Q_0 e^{-t/RC}) = \frac{Q_0}{RC} e^{-t/RC} = I_0 e^{-t/RC}$
  
    Where:
  
    *    i(t) is the current at time t.
    *   $I_0 = \frac{Q_0}{RC} = \frac{V_0}{R}$ is the initial current (at t = 0), and V<sub>0</sub> is the initial voltage.
  
  *  **Time Constant (τ):**
    The product RC is called the *time constant* (τ) of the circuit:
     $\tau = RC$
    *   **Units:** The units of RC are seconds: (Ohms)(Farads) = seconds.
    *   **Significance:** The time constant represents the time it takes for the charge on the capacitor to decay to 1/e (approximately 37%) of its initial value.  It also represents the time it takes for the current to decay to 37% of its initial value.
  *   **Sample Plots:**  The lecture includes plots of q(t) and i(t) showing exponential decay.  After one time constant (t = RC), q(t) = Q<sub>0</sub>/e ≈ 0.37Q<sub>0</sub>, and i(t) = I<sub>0</sub>/e ≈ 0.37I<sub>0</sub>.
## Charging a Capacitor
  
  *   **Circuit Setup:** An uncharged capacitor (C) is connected in series with a resistor (R), a battery (with emf ℰ), and a switch. When the switch is closed, the capacitor begins to charge.
  *   **Kirchhoff's Loop Rule:** Applying Kirchhoff's Loop Rule to the circuit during charging:
  
    $ℰ - \frac{q}{C} - iR = 0$
  
    Where:
  
    *   ℰ is the emf of the battery.
    *   q is the charge on the capacitor at time t.
    *   C is the capacitance.
    *   i is the current in the circuit at time t.
    *   R is the resistance.
  * **Differential Equation:**
  $i = \frac{dq}{dt}$
  $\epsilon - \frac{q}{C} - \frac{dq}{dt}R = 0$
  $\frac{dq}{dt} = \frac{\epsilon}{R} - \frac{q}{RC} = -\frac{1}{RC}(q-C\epsilon)$
  
  *   **Solution (Charge vs. Time):** The solution to this differential equation is:
  
    $q(t) = Cℰ(1 - e^{-t/RC}) = Q_f(1 - e^{-t/RC})$
  
    Where:
  
    *   q(t) is the charge on the capacitor at time t.
    *   $Q_f = Cℰ$ is the final charge on the capacitor (at t = ∞).
    *    ℰ is the battery's emf
    *   R is the resistance.
    *   C is the capacitance.
    *   e is the base of the natural logarithm.
  
  *   **Current vs. Time:** The current during charging is:
  
    $i(t) = \frac{dq}{dt} = \frac{d}{dt}(Cℰ(1 - e^{-t/RC})) = \frac{ℰ}{R} e^{-t/RC} = I_0 e^{-t/RC}$
  
    Where:
     * i(t) is the current
     * $I_0 = \frac{ℰ}{R}$ is the initial current
  
  *   **Time Constant (τ):**  The time constant (τ = RC) has the same significance as in the discharging case.
    *   After one time constant (t = RC), the capacitor is charged to (1 - 1/e) ≈ 63% of its final charge (Q<sub>f</sub>).
    *   After one time constant (t = RC), the current has decayed to 1/e (≈ 37%) of its initial value (I<sub>0</sub>).
  
  *   **Sample Plots:**  The lecture includes plots of q(t) and i(t) showing the exponential approach to the final charge and the exponential decay of the current.
## Example 1: Charging Capacitor
  
  **Problem:**
  
  A 4.60 µF capacitor is initially uncharged. It is connected in series with a switch, a 7.50 kΩ resistor, and a 125 V battery (negligible internal resistance).
  
  a) Just after the switch is closed, what are:
    *   The voltage drop across the capacitor?
    *   The voltage drop across the resistor?
    *   The current through the resistor?
  
  b) At what time after the switch is closed will the current in the resistor be 10.0 mA?
  
  **Solution (a):**
  
  *   **Voltage Across Capacitor:** Just after the switch is closed (t = 0), the capacitor is uncharged (q = 0).  Therefore, the voltage across the capacitor is:
  
    $V_C = \frac{q}{C} = \frac{0}{C} = 0 \text{ V}$
  
  *   **Voltage Across Resistor:** Since the capacitor has 0 V across it, the entire battery voltage appears across the resistor (by KLR):
  
    $V_R = ℰ = 125 \text{ V}$
  
  *   **Current Through Resistor:**  Use Ohm's Law:
  
    $I = \frac{V_R}{R} = \frac{125 \text{ V}}{7.50 \times 10^3 \text{ }\Omega} = 0.0167 \text{ A} = 16.7 \text{ mA}$
  
  **Solution (b):**
  
  *   **Current Equation:**  We use the equation for the current during charging:
  
    $i(t) = I_0 e^{-t/RC}$
  
  *   **Initial Current:**  We found I<sub>0</sub> = 16.7 mA in part (a).
  
  *   **Solve for Time:**  We want to find *t* when i(t) = 10.0 mA:
  
    $10.0 \text{ mA} = (16.7 \text{ mA}) e^{-t/(RC)}$
    $\frac{10.0}{16.7} = e^{-t/(RC)}$
    $\ln(\frac{10.0}{16.7}) = -\frac{t}{RC}$
    $t = -RC \ln(\frac{10.0}{16.7})$
  
  * **Calculate RC (time constant)**
     $RC = (7.50 * 10^3 \Omega) * (4.60 * 10^{-6} F) = 0.0345 s$
  *   **Plug in Values:**
  
    $t = -(0.0345 \text{ s}) \ln(\frac{10.0}{16.7}) \approx 0.0176 \text{ s} = 17.6 \text{ ms}$
## Example 2: Discharging Capacitor
  
  **Problem:**
  
  A capacitor initially has a potential difference of 100 V. It is connected in series to a 10 kΩ resistor and a switch.  It begins to discharge when the switch is closed.  10 seconds after the switch is closed, the potential across the capacitor is 1 V.
  
  a) What is the capacitance of the capacitor?
  b) What is the current in the resistor 10 seconds after the switch is closed?
  
  **Solution (a):**
  
  *   **Voltage Equation:**  For a discharging capacitor, the voltage across the capacitor is:
  
    $V(t) = V_0 e^{-t/RC}$
  
  *   **Solve for C:** We are given V<sub>0</sub> = 100 V, V(10 s) = 1 V, R = 10 kΩ = 10<sup>4</sup> Ω, and t = 10 s.  We need to solve for C:
  
    $1 \text{ V} = (100 \text{ V}) e^{-(10 \text{ s})/(RC)}$
    $0.01 = e^{-10/RC}$
    $\ln(0.01) = -\frac{10}{RC}$
    $RC = -\frac{10}{\ln(0.01)}$
    $C = -\frac{10}{R\ln(0.01)} = -\frac{10 \text{ s}}{(10^4 \text{ }\Omega)\ln(0.01)} \approx 2.17 \times 10^{-4} \text{ F} = 217 \text{ }\mu\text{F}$
  
  **Solution (b):**
  
  *   **Current Equation:**
  
    $i(t) = I_0 e^{-t/RC}$
    where
    $I_0 = V_0/R$
  
  *   **Calculate I<sub>0</sub>:**
  
    $I_0 = \frac{100 \text{ V}}{10^4 \text{ }\Omega} = 0.01 \text{ A} = 10 \text{ mA}$
  * **Plug in Values:**
  We have calculated RC in the first part.
  $RC = -\frac{10}{\ln(0.01)} = 2.17 \times 10^{-4}F \times 10^4 \Omega \approx 2.17 s$
  
    $i(10 \text{ s}) = (10 \text{ mA}) e^{-10 \text{ s}/(2.17 \text{ s})} \approx 0.10 \text{ mA} $
    Or, since V(10s) = 1V:
     $I = \frac{V}{R} = \frac{1V}{10k\Omega} = 0.1mA$
## Summary
  
  This lecture introduced RC circuits and analyzed the time-dependent behavior of charging and discharging capacitors. The key equations are the exponential functions for charge and current, and the concept of the time constant (τ = RC) is crucial for understanding the timescale of these processes. The examples demonstrated how to apply these equations to solve problems involving RC circuits.