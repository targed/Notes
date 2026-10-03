## Introduction
  
  This lecture introduces the first law of thermodynamics, a statement of energy conservation for thermodynamic systems. We will cover:
  
  * Thermodynamic work
  * 1st Law of Thermodynamics
  * Equation of state of the ideal gas
  * Isochoric, isobaric, and isothermal processes in ideal gases
## Thermodynamic Work
  
  * **Work done by a gas:**  Consider a gas expanding against a piston. The gas exerts a force ($F = PA$) on the piston over a distance $dx$.  $P$ is pressure and $A$ is the area of the piston.
  * **Infinitesimal work:**  $dW = Fdx = PAdx = PdV$
    * $dV$: Change in volume
  * **Total work:** $W = \int_{V_i}^{V_f} p(V, T) dV$ (The work done *by* the gas)
  
  **(Diagram of gas expanding against a piston)**
## Work and p-V Curve
  
  * **Work as area:** The work done by the gas is represented by the area under the pressure-volume (p-V) curve.
  * **Sign of work:**
    * **Expansion ($V_f > V_i$):** Work done by the gas is positive ($W > 0$).
    * **Compression ($V_f < V_i$):** Work done by the gas is negative ($W < 0$).
  
  **(p-V diagrams showing expansion and compression)**
## First Law of Thermodynamics
  
  * **Internal energy:** The internal energy ($U$) of a system can change due to:
    * Heat flow in or out of the system ($Q$)
    * Work done on or by the system ($W$)
  * **1st Law:** The change in internal energy of a system is equal to the heat added to the system minus the work done *by* the system:
    * $\Delta U = Q - W$
  
  * **Sign conventions:**
    * $Q > 0$: Heat flows *into* the system.
    * $Q < 0$: Heat flows *out of* the system.
    * $W > 0$: Work is done *by* the system.
    * $W < 0$: Work is done *on* the system.
  
  
  * **State variables:** Internal energy is a state variable, meaning its value depends only on the current state of the system (defined by temperature, pressure, volume, etc.) and not on the path taken to reach that state.
  * **Path dependence of work and heat:** While $\Delta U$ is path-independent, the work done ($W$) and heat transferred ($Q$) *do* depend on the path (process) taken.
## Internal Energy
  
  * **Definition:** Internal energy ($U$) is the sum of the microscopic kinetic and potential energies of all the particles in the system.
  * **State variable:** $U$ depends only on the state of the system, characterized by state variables like temperature, pressure, volume, and phase.
## Ideal Gas
  
  * **Assumptions:**
    * Particles do not interact with each other (except during brief elastic collisions).
    * Collisions between particles and with the walls of the container are perfectly elastic.
  * **Real gases:** Real gases can be approximated as ideal gases at low densities, low pressures, and high temperatures.
  * **Equation of state:** $pV = nRT$
    * $p$: Pressure
    * $V$: Volume
    * $n$: Number of moles
    * $R$: Universal gas constant (8.315 J/(mol K))
    * $T$: Absolute temperature (in Kelvin)
  
  * **Internal energy of an ideal gas:**
    * Since ideal gas particles don't interact, the internal energy depends only on the kinetic energy of the particles, which is directly proportional to the temperature.
    * $U = U(T)$
## Important Processes for the Ideal Gas
  
  * **Isochoric (constant volume):** $V = \text{constant}$. Vertical line on a p-V diagram.
  * **Isobaric (constant pressure):** $p = \text{constant}$. Horizontal line on a p-V diagram.
  * **Isothermal (constant temperature):** $T = \text{constant}$.  $pV = \text{constant}$. Curve on a p-V diagram.
  
  **(p-V diagrams illustrating isochoric, isobaric, and isothermal processes)**
## Adiabatic Process
  
  * **Definition:** An adiabatic process is one in which no heat is exchanged between the system and its surroundings ($Q = 0$).
  * **Equation:** $pV^\gamma = \text{constant}$
    * $\gamma = \frac{c_p}{c_v}$ (ratio of specific heats)
    * $\gamma = 1.67$ for monoatomic gases
    * $\gamma = 1.40$ for diatomic gases
  * **p-V curve:** Steeper than an isothermal curve.
  
  
  **(p-V diagram illustrating an adiabatic process)**
## Isochoric Process (Ideal Gas)
  
  * **Constant volume:** $V = \text{constant}$, so $dV = 0$.
  * **Work done:** $W = \int pdV = 0$ (No work is done in an isochoric process.)
  * **Heat:** $Q = nc_v\Delta T$
    * $c_v$: Specific heat at constant volume
    * $c_v = \frac{3}{2}R$ for monoatomic gases
    * $c_v = \frac{5}{2}R$ for diatomic gases
  * **1st Law:** $\Delta U = Q - W = nc_v\Delta T$
  
  **(p-V diagram for an isochoric process)**
## Isobaric Process (Ideal Gas)
  
  * **Constant pressure:** $p = \text{constant}$.
  * **Work done:** $W = \int pdV = p\Delta V$
  * **Heat:** $Q = nc_p\Delta T$
    * $c_p$: Specific heat at constant pressure
    * $c_p = \frac{5}{2}R$ for monoatomic gases
    * $c_p = \frac{7}{2}R$ for diatomic gases
  * **1st Law:** $\Delta U = Q - W = nc_p\Delta T - p\Delta V$
  
  **(p-V diagram for an isobaric process)**
## Isothermal Process (Ideal Gas)
  
  * **Constant temperature:** $T = \text{constant}$.
  * **Internal energy:**  Since $U = U(T)$ for an ideal gas, $\Delta U = 0$ in an isothermal process.
  * **Work done:** $W = \int pdV = nRT\int \frac{dV}{V} = nRT \ln\frac{V_f}{V_i}$
  * **1st Law:** $\Delta U = Q - W = 0 \implies Q = W = nRT \ln\frac{V_f}{V_i}$
  
  **(p-V diagram for an isothermal process)**
## Isobaric vs. Isochoric Process
  
  * **Comparison:** Consider two processes between the same two temperatures.  
  * **Change in internal energy is the same:** Since $\Delta U$ is path-independent and depends only on the temperature change. For both processes $\Delta U = n c_v \Delta T$, where $c_v$ is independent of volume and pressure and depends on temperature.
  * **Isobaric process requires more heat:**  In an isobaric process, some of the heat added goes into doing work by the gas, whereas in an isochoric process, all the heat goes into increasing the internal energy.
  * **Relationship between specific heats:**  $c_p - c_v = R$
  
  **(p-V diagram comparing isobaric and isochoric processes)**
## Difference Between $c_v$ and $c_p$
  
  * **Relationship:** $c_p - c_v = R$
  * **Values:**
    * **Monoatomic gas:** $c_v = \frac{3}{2}R$, $c_p = \frac{5}{2}R$
    * **Diatomic gas:** $c_v = \frac{5}{2}R$, $c_p = \frac{7}{2}R$
## Monoatomic vs. Diatomic Gas
  
  * **Degrees of freedom:** Each degree of freedom in the kinetic energy contributes $\frac{1}{2}R$ to the specific heat.
  * **Monoatomic gas:** 3 translational degrees of freedom ($x, y, z$).
  * **Diatomic gas:** 3 translational degrees of freedom and 2 rotational degrees of freedom (rotation about two axes perpendicular to the bond axis). Rotation about the bond axis has negligible contribution due to a very small moment of inertia about that axis.
  
  **(Diagram illustrating degrees of freedom)**
## Adiabatic Process (Detailed)
  
  * **No heat transfer:** $Q=0$
  * **First law:** $\Delta U = -W$
  * **Change in internal energy:** $dU = nc_v dT$
  * **Work:** $dW = pdV$
  * **Combining and using the ideal gas law:**
    * $nc_v dT = -pdV = -\frac{nRT}{V}dV$
    * $c_v\frac{dT}{T} = -R\frac{dV}{V}$
  * **Integrating:** $c_v\ln\frac{T_f}{T_i} = -R\ln\frac{V_f}{V_i}$
  * **Relationship between $T$ and $V$:**  Using $c_p - c_v = R$ and $\gamma = \frac{c_p}{c_v}$, we can derive: $TV^{\gamma - 1} = \text{constant}$ and $pV^\gamma = \text{constant}$.
  
  
  **(p-V diagram illustrating adiabatic process and relevant equations)**