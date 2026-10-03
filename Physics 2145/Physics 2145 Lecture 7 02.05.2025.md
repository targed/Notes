# Lecture 7: Electric Potential (Continued)
## Parallel Plate Capacitor (Review and Expansion)
  
  Recall from previous lectures:
  
  *   **Uniform Field:** A parallel plate capacitor creates a uniform electric field between its plates (neglecting edge effects).
  *   **Field Strength:**  The magnitude of the electric field is given by:
  
    $E = \frac{\sigma}{\epsilon_0} = \frac{Q}{\epsilon_0 A}$
  
    Where:
  
    *   $E$: Electric field strength
    *   $\sigma$: Surface charge density (Q/A)
    *   $Q$: Charge on one plate
    *   $A$: Area of one plate
    *   $\epsilon_0$: Permittivity of free space (8.85 x 10<sup>-12</sup> C<sup>2</sup>/N•m<sup>2</sup>)
  
  *   **Potential Difference:** The potential difference (ΔV) between the plates, if *d* is the separation between the plates, is:
  
    $\Delta V = Ed$
  
    This comes from the fact that the work done per unit charge to move a charge from one plate to the other is the force per unit charge (E) times the distance (d).
## Equipotential Surfaces
  
  *   **Definition:** An *equipotential surface* is a surface on which the electric potential (V) is constant at every point.  Moving a charge *along* an equipotential surface requires *no* work because the potential difference is zero.
  *    Since $\Delta V = 0$, then W = 0.
  *   **Equipotential Surfaces for a Capacitor:** For a parallel plate capacitor, the equipotential surfaces are *planes parallel to the plates*.  Each plane between the plates represents a different constant value of V.
  
  *   **Relationship to Electric Field Lines:**
    *   **Perpendicularity:** Electric field lines are always *perpendicular* to equipotential surfaces. This is because the electric field points in the direction of the greatest *decrease* in potential.  If there were a component of the electric field along an equipotential surface, it would do work on a charge moving along that surface, which contradicts the definition of an equipotential.
    *   **Direction:** The electric field vector points from regions of *higher* potential to regions of *lower* potential.
    *   **Field Strength:** The electric field is *stronger* where the equipotential surfaces are *closer together*. This is because a given potential difference occurs over a shorter distance, implying a larger electric field strength (since E ≈ ΔV/Δx).
  *   **Constant Electric Field:** If the electric field is constant, the distances between the equipotential surfaces is constant.
## Example: Electron Moving Between Capacitor Plates
  
  **Problem:**
  
  The potential difference between two plates of a parallel plate capacitor is 3000 V. An electron is launched from the negative plate with an initial speed of 1.5 x 10<sup>7</sup> m/s.  Determine:
  
  a) The speed with which the electron strikes the positive plate.  (Derive a symbolic answer first, then calculate the numerical value.)
  b) The electron's change in kinetic energy in electron volts.
  
  **Solution (a):**
  
  1.  **Energy Conservation:** The total energy of the electron is conserved.  The initial energy is the sum of its initial kinetic energy and its initial electric potential energy.  The final energy is the sum of its final kinetic energy and its final electric potential energy.
  
    $K_i + U_i = K_f + U_f$
    $\frac{1}{2}mv_i^2 + qV_i = \frac{1}{2}mv_f^2 + qV_f$
  
  2.  **Rearrange for Final Velocity:** We want to find v_f, so we rearrange the equation:
  
    $\frac{1}{2}mv_f^2 = \frac{1}{2}mv_i^2 + q(V_i - V_f)$
    $v_f^2 = v_i^2 + \frac{2q(V_i - V_f)}{m}$
    $v_f = \sqrt{v_i^2 + \frac{2q(V_i - V_f)}{m}}$
  
  3.  **Define Potentials:** Let's define the potential of the negative plate (where the electron starts) as V_i = 0 V. Then the potential of the positive plate is V_f = +3000 V. The potential difference, ΔV, is:
  
    $\Delta V = V_f - V_i = 3000 \text{ V}$
  
  4. **Simplify:** substitute into velocity equation
  $v_f = \sqrt{v_i^2 - \frac{2q\Delta V}{m}}$
  
  5.  **Plug in Values:**  We have:
    *   $v_i = 1.5 x 10^7 m/s$
    *   $q = -1.602 x 10^-19 C$ (charge of an electron)
    *   $m = 9.109 x 10^-31 kg$ (mass of an electron)
    *   $ΔV = 3000 V$
  
    $v_f = \sqrt{(1.5 \times 10^7 \text{ m/s})^2 + \frac{2(-1.602 \times 10^{-19} \text{ C})(-3000 \text{ V})}{9.109 \times 10^{-31} \text{ kg}}}$
    $v_f = \sqrt{2.25 \times 10^{14} \text{ m}^2/\text{s}^2 + 1.054 \times 10^{15}\text{ m}^2/\text{s}^2}$
    $v_f = \sqrt{1.279 \times 10^{15} \text{ m}^2/\text{s}^2} \approx 3.58 \times 10^7 \text{ m/s}$
  
  **Solution (b):**
  
  1.  **Change in Kinetic Energy (ΔK):** The change in kinetic energy is equal to the negative of the change in potential energy:
  $\Delta K = -\Delta U$
  $\Delta K = -q \Delta V$
  
  2.  **In electron volts:** Since the charge is negative for an electron:
  
    $\Delta K = -(-e)(3000 \text{ V}) = 3000 \text{ eV}$
  
    The electron *gains* 3000 eV of kinetic energy.
## Connecting Potential and Field
  
  *   **General Relationship:** The electric field is related to the *rate of change* of the electric potential with respect to position.  In one dimension:
  
    $E_x = -\frac{dV}{dx}$
  
    This means the electric field points in the direction of the most rapid *decrease* in electric potential. The negative sign indicates that the field points from higher to lower potential.
  *   **Vector Form (3D):** In three dimensions, the electric field is the negative gradient of the potential:
  
    $\vec{E} = -\nabla V = -\left( \frac{\partial V}{\partial x}\hat{i} + \frac{\partial V}{\partial y}\hat{j} + \frac{\partial V}{\partial z}\hat{k} \right)$
  
    Where $\nabla$ is the gradient operator.
## Potential and Field for Important Cases (Summary)
  
  | Charge Configuration       | Electric Field (E)                                                               | Electric Potential (V)                                                            |
  | :-------------------------- | :--------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------- |
  | Point Charge               | $E = k\frac{|q|}{r^2}$ (radially outward/inward)                                      | $V = k\frac{q}{r}$                                                                 |
  | Uniformly Charged Sphere    | Outside:  $E = k\frac{|q|}{r^2}$ (like point charge); Inside: E = 0                   | Outside: $V = k\frac{q}{r}$; Inside: V = constant (same as surface potential)       |
  | Parallel Plate Capacitor | $E = \frac{\sigma}{\epsilon_0} = \frac{Q}{\epsilon_0 A}$ (uniform between plates)      | $V = Ed$ (linear change between plates)                                            |
  | Infinitely long line of charge | $E = \frac{2k\lambda}{r}$| $V = -2k\lambda ln(r) + C$                                   |
## Conductors in Electrostatic Equilibrium (Revisited)
  
  *   **Equipotential Surface:** The surface of a conductor in electrostatic equilibrium is an *equipotential surface*. This means the electric potential is constant everywhere on the surface.
  *   **Zero Field Inside:** The electric field inside the conductor is zero.
  *   **Perpendicular Field Outside:** The electric field just outside the conductor is perpendicular to the surface.
  * The electric field is stronger where equipotentials are closer together.
## Summary
  
  This lecture continued the discussion of electric potential, focusing on parallel-plate capacitors and equipotential surfaces.  We explored the important relationship between the electric field and the electric potential, and reviewed the potential and field for key charge configurations.  Finally, we reiterated the key properties of conductors in electrostatic equilibrium in terms of equipotentials.