## Introduction
  
  This lecture introduces the second law of thermodynamics and discusses thermodynamic cycles, including the Carnot cycle. We will cover:
  
  * Thermodynamic cycles
  * 2nd Law of Thermodynamics
  * Carnot Cycle
## Thermodynamic Cycles
  
  * **Definition:** A thermodynamic cycle is a sequence of thermodynamic processes that returns a system to its initial state.
  * **Internal energy change:** Since the system returns to its initial state, the change in internal energy over a complete cycle is zero: $\Delta U = U_f - U_i = 0$.
  * **First Law of Thermodynamics for a cycle:**
    * $\Delta U = Q - W = 0$
    * $Q = W$ (The net heat added to the system over a cycle is equal to the net work done by the system.)
  
  
  **(p-V diagram of a thermodynamic cycle)**
## Work in Cycles
  
  * **Work done in a cycle:**  The net work done by the system during a cycle is equal to the area enclosed by the cycle on a p-V diagram.
  * **Sign of work:**
    * **Clockwise cycle:** Net work done by the gas is positive ($W_{net} > 0$).
    * **Counterclockwise cycle:** Net work done by the gas is negative ($W_{net} < 0$). This signifies work being done *on* the system.
  
  **(p-V diagrams illustrating clockwise and counterclockwise cycles)**
## Heat Engine
  
  * **Purpose:** A heat engine is a device that converts thermal energy (heat) into mechanical work.
  * **Process:**
    1. Absorbs heat ($Q_H > 0$) from a hot reservoir (at temperature $T_H$).
    2. Does work ($W$) on its surroundings.
    3. Releases heat ($Q_C < 0$) to a cold reservoir (at temperature $T_C$).
  * **Net heat:** $Q_{net} = Q_H + Q_C = Q_H - |Q_C|$.
  * **First law (for a cycle):** $W = Q_{net} = Q_H - |Q_C|$
  
  **(Diagram illustrating a heat engine)**
## Efficiency
  
  * **Definition:** Efficiency ($e$) of a heat engine is the ratio of the work output to the heat input:
    * $e = \frac{W_{out}}{Q_H} = \frac{Q_{net}}{Q_H} = \frac{Q_H - |Q_C|}{Q_H} = 1 - \frac{|Q_C|}{Q_H}$
  * **Maximum efficiency:**  A heat engine cannot convert all the input heat into work. Some heat must always be rejected to a cold reservoir. Therefore, the efficiency is always less than 1.
## 2nd Law of Thermodynamics
  
  The second law of thermodynamics places limitations on the efficiency of heat engines and the direction of heat flow.  There are several equivalent statements of the 2nd Law:
  
  * **Clausius statement:** Heat flows naturally from a hot object to a cold object. Heat will not flow spontaneously from a cold object to a hot object without some external work being done.
  * **Kelvin-Planck statement:** It is impossible to devise a cyclically operating device, the sole effect of which is to absorb energy in the form of heat from a single thermal reservoir and to deliver an equivalent amount of work. In simpler terms, no heat engine can be 100% efficient.
## Implications of the 2nd Law
  
  The Kelvin-Planck statement implies that for a heat engine to operate, it *must* reject some heat to a cold reservoir. A heat engine that converts all input heat into work is impossible.
  
  **(Diagram of an impossible heat engine violating the 2nd Law)**
## Carnot Cycle
  
  * **Definition:** The Carnot cycle is a theoretical thermodynamic cycle that represents the most efficient possible heat engine operating between two given temperatures.
  * **Processes:** The Carnot cycle consists of four reversible processes:
    1. **Isothermal expansion:** The system absorbs heat $Q_H$ from the hot reservoir at temperature $T_H$.
    2. **Adiabatic expansion:** The system expands further without exchanging heat with its surroundings.
    3. **Isothermal compression:** The system releases heat $Q_C$ to the cold reservoir at temperature $T_C$.
    4. **Adiabatic compression:** The system is compressed back to its initial state without exchanging heat.
  
  **(p-V diagram of the Carnot cycle)**
## Carnot Cycle Details
  
  * **1-2 Isothermal Expansion:** $\Delta U=0$,  $Q = W = nRT_H \ln \frac{V_2}{V_1}$ ($Q > 0$, heat flows in)
  * **2-3 Adiabatic Expansion:** $Q=0$, $\Delta U=-W$, $W=-nc_v(T_C-T_H)$
  * **3-4 Isothermal Compression:** $\Delta U=0$, $Q=W=nRT_C \ln \frac{V_4}{V_3}$ ($Q < 0$, heat flows out)
  * **4-1 Adiabatic Compression:** $Q=0$, $\Delta U = -W$, $W = -nc_v(T_H-T_C)$
## Efficiency of the Carnot Cycle
  
  * **Derivation:**
    * Using the equations from the previous section and the fact that $\frac{V_2}{V_1} = \frac{V_3}{V_4}$ for the adiabatic processes, we can derive the efficiency of the Carnot cycle:
    * $e_{Carnot} = 1 - \frac{|Q_C|}{Q_H} = 1 - \frac{T_C}{T_H}$
  * **Maximum efficiency:** The Carnot cycle has the maximum possible efficiency for any heat engine operating between temperatures $T_C$ and $T_H$.
## Carnot Cycle and Reversibility
  
  * **Reversible processes:** The Carnot cycle is composed of reversible processes.  This means that the cycle can be run in reverse to act as a refrigerator or heat pump.
  * **Maximum efficiency proof:** If a more efficient engine existed, it could be coupled with a reversed Carnot engine to create a device that transfers heat from a cold reservoir to a hot reservoir without any external work, violating the 2nd Law.  Therefore, the Carnot efficiency is the maximum possible efficiency.
## Carnot Efficiency
  
  * **Formula:** $e_{Carnot} = 1 - \frac{T_C}{T_H}$
  * **Maximum efficiency:** Represents the maximum efficiency of any heat engine operating between temperatures $T_C$ and $T_H$.