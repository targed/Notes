# Lecture 6: Electric Potential
## Potential Energy (Review)
  
  *   **General Concept:** Potential energy is energy associated with the position or configuration of an object within a system where a conservative force acts.  A conservative force is a force where the work done is independent of the path taken.
  *   **Gravitational Potential Energy:**  Near the Earth's surface, the change in gravitational potential energy (ΔU) is given by ΔU = mgh, where m is mass, g is the acceleration due to gravity, and h is the change in height.
## Electric Potential Energy
  
  Just as a mass in a gravitational field has gravitational potential energy, a charge in an electric field has *electric potential energy*.
  
  *   **Work and Potential Energy:** When a charged particle moves in an electric field, the electric force does work on the particle.  This work changes the electric potential energy of the particle.
  *   **Uniform Electric Field:** Consider a positive charge *q* moving a distance *d* in a uniform electric field *E*.  The force on the charge is *F* = *qE*. If the charge moves in the direction of the field, the work done by the electric field is *W* = *Fd* = *qEd*.
    *   The *change* in electric potential energy (ΔU) is the *negative* of the work done by the electric field: ΔU = -W = -qEd.  (If the displacement and field are in the same direction. If they are in opposite directions, ΔU = qEd).
    * We consider final potential energy minus the intitial potential energy:
    $U_f - U_i = -qEd$
  *   **General Case:** More generally, the change in electric potential energy for a charge *q* moving from point *a* to point *b* in *any* electric field is:
  
    ΔU = U_b - U_a = -W_a→b
  
    Where W_a→b is the work done by the electric field on the charge as it moves from *a* to *b*.
## Electric Potential (V)
  
  *   **Definition:** Electric potential (V) is defined as the electric potential energy *per unit charge*:
  
    $V = \frac{U}{q}$
  
    This represents the potential energy that a *unit* positive charge would have at a particular point in space.
  *   **Units:** The SI unit of electric potential is the Volt (V), where 1 Volt = 1 Joule/Coulomb (1 V = 1 J/C).
  *   **Scalar Quantity:** Electric potential is a *scalar* quantity (it has magnitude but no direction), unlike the electric field, which is a vector.
  *   **Potential Difference (ΔV):**  The difference in electric potential between two points *a* and *b* is:
  
    ΔV = V_b - V_a = (U_b/q) - (U_a/q) = (U_b - U_a)/q = -W_a→b/q
  
    The potential difference is the work done *per unit charge* to move a charge from point *a* to point *b*.
  * **Uniform Field:** In a uniform electric field E, the potential difference is given by:
    $\Delta V = -Ed$, if d is in the direction of E, or
    $\Delta V = Ed$, if d is opposite the direction of E.
## Electric Potential of a Point Charge
  
  The electric potential *V* at a distance *r* from a point charge *q* is given by:
  
  $V = k \frac{q}{r}$
  
  Where:
  
  *   *V* is the electric potential.
  *   *k* is Coulomb's constant (9 x 10<sup>9</sup> N•m<sup>2</sup>/C<sup>2</sup>).
  *   *q* is the charge of the point charge.
  *   *r* is the distance from the point charge to the point where the potential is being calculated.
  
  **Important Notes:**
  
  *   This formula assumes that the electric potential is zero at an infinite distance from the charge (V = 0 at r = ∞). This is a common convention.
  *   The potential is positive if *q* is positive and negative if *q* is negative.
## Potential Differences and Charge Separation
  
  Potential differences are created by separating positive and negative charges.  This separation requires work, which is stored as electric potential energy.  A common example is a battery, where chemical reactions separate charges, creating a potential difference between the terminals.
## Multiple Charges
  
  The electric potential due to a collection of point charges is the *scalar sum* of the potentials due to each individual charge:
  
  $V_{total} = V_1 + V_2 + V_3 + ... = k \frac{q_1}{r_1} + k \frac{q_2}{r_2} + k \frac{q_3}{r_3} + ... = k \sum_i \frac{q_i}{r_i}$
  
  Where:
  
  *   $q_i$ is the charge of the *i*-th point charge.
  *   $r_i$ is the distance from the *i*-th point charge to the point where the potential is being calculated.
## Energy Conservation
  
  The principle of conservation of energy applies to charged particles in electric fields. The total energy (kinetic energy + electric potential energy) of a charge remains constant if only conservative forces (like the electric force) are acting.
  
  *   **Total Energy (E):**  E = K + U, where K is kinetic energy (1/2 mv<sup>2</sup>) and U is electric potential energy.
  *   **Conservation:**  If a charge moves from point *a* to point *b*, then:
  
    K_a + U_a = K_b + U_b
  
    Or, equivalently:
  
    ΔK + ΔU = 0
    
    1/2mv_a<sup>2</sup> + qV_a = 1/2mv_b<sup>2</sup> + qV_b
## Example
  A proton is released from rest in a uniform electric field. It moves from a point with an electric potential of +200V to a point of 0V. If it's initial velocity is 0, what is it's final velocity?
  **Solution**
  Using energy conservation. We have the charge of a proton (q = 1.602 x 10<sup>-19</sup> C) and mass m = 1.672 x 10<sup>-27</sup> kg.
  1/2mv_a<sup>2</sup> + qV_a = 1/2mv_b<sup>2</sup> + qV_b
  
  Since $v_a = 0$,
  $qV_a = 1/2mv_b + qV_b$
  v_b<sup>2</sup> = 2(qV_a - qV_b)/m
  v_b = $\sqrt{2q(V_a - V_b)/m} = \sqrt{2 \times 1.602 \times 10^{-19} \text{ C} (200\text{ V} - 0\text{ V})/(1.672 \times 10^{-27} \text{ kg})} \approx 1.96 \times 10^5 \text{ m/s}$
## Electron Volt (eV)
  
  *   **Definition:** The electron volt (eV) is a unit of energy commonly used in atomic and nuclear physics. It is defined as the amount of kinetic energy gained (or lost) by an electron when it moves through a potential difference of 1 Volt.
  *   **Conversion:** 1 eV = 1.602 x 10<sup>-19</sup> J (This comes from the charge of an electron multiplied by 1 Volt).
  *   **Usefulness:** The eV is a convenient unit because it directly relates energy changes to potential differences in many situations involving electrons and other charged particles. For example, if an electron accelerates through a potential difference of 100V, we immediately know it gains 100eV of kinetic energy.
## Example
  
  What is the speed of an 8.7 MeV proton?
  
  **Solution:**
  
  1.  **Convert MeV to Joules:**
    8.  7 MeV = 8.7 x 10<sup>6</sup> eV
    8.  7 x 10<sup>6</sup> eV * (1.602 x 10<sup>-19</sup> J/eV) = 1.39374 x 10<sup>-12</sup> J
  
  2.  **Use Kinetic Energy Formula:**
    K = 1/2 mv<sup>2</sup>
    v = √(2K/m)
  
  3.  **Plug in Values:**
    v = √[(2 * 1.39374 x 10<sup>-12</sup> J) / (1.672 x 10<sup>-27</sup> kg)]
    v ≈ 4.08 x 10<sup>7</sup> m/s
## Summary
  
  This lecture introduced electric potential energy and electric potential, emphasizing their relationship and the concept of potential difference. We learned how to calculate the potential due to point charges and multiple charges, and how to apply energy conservation to charged particles moving in electric fields. Finally, we introduced the electron volt as a convenient unit of energy.