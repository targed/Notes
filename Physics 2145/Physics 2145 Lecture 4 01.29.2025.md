# Lecture 4: Electric Field and Force - Calculations, Forces, Torques, and Applications
## Review: Uniform Electric Field
  
  Recall from the last lecture that a uniform electric field can be created between two large parallel plates with opposite charges.
  
  *   **Field:** The electric field is constant in both magnitude and direction between the plates (edge effects are neglected when the size of the plates is much larger than their separation). The horizontal components cancel out, and only the vertical components remain.
  *   **Magnitude:**
  
  $E = \frac{\sigma}{\epsilon_0} = \frac{Q}{\epsilon_0 A}$
  
  Where:
  
  *   $E$: Electric field strength
  *   $\sigma$: Surface charge density (Q/A)
  *   $Q$: Charge on one plate
  *   $A$: Area of one plate
  *   $\epsilon_0$: Permittivity of free space (8.85 x 10<sup>-12</sup> C<sup>2</sup>/N•m<sup>2</sup>)
## Example: Electric Field Calculation
  
  **Problem:**
  
  Two point charges are arranged as follows:
  
  *   q_1 = +1.0 nC at (0, 10 cm)
  *   q_2 = -1.0 nC at (0, 0)
  
  Find the net electric field at point P located at (5.0 cm, 5.0 cm).
  
  **Solution:**
  
  **Step 1: Draw the electric field vectors.**
  
  *   **E_1:** The electric field due to q_1 at point P will point away from q_1 (since it's positive) and will have a direction towards the bottom left of point P.
  *   **E_2:** The electric field due to q_2 at point P will point towards q_2 (since it's negative) and will have a direction towards the bottom right of point P.
  
  **Step 2: Calculate the magnitudes of the electric fields.**
  
  *   **Distance r_1:** The distance between q_1 and P is 5 cm.
    $r_1 = 5.0 \text{ cm} = 0.05 \text{ m}$
    $E_1 = k \frac{|q_1|}{r_1^2} = (9 \times 10^9 \text{ N}\cdot\text{m}^2/\text{C}^2) \frac{1.0 \times 10^{-9} \text{ C}}{(0.05 \text{ m})^2} = 3600 \text{ N/C}$
  
  *   **Distance r_2:** The distance between q_2 and P is:
    $r_2 = \sqrt{(5 \text{ cm})^2 + (5 \text{ cm})^2} = \sqrt{50} \text{ cm} = 0.05\sqrt{2} \text{ m}$
    $E_2 = k \frac{|q_2|}{r_2^2} = (9 \times 10^9 \text{ N}\cdot\text{m}^2/\text{C}^2) \frac{1.0 \times 10^{-9} \text{ C}}{(0.05\sqrt{2} \text{ m})^2} = (9 \times 10^9 \text{ N}\cdot\text{m}^2/\text{C}^2) \frac{1.0 \times 10^{-9} \text{ C}}{(0.005 \text{ m}^2)} = 1800 \text{ N/C}$
  
  **Step 3: Calculate the x and y components of each electric field.**
  
  *   **E_1:**
    *   $E_{1x} = E_1 = 3600 \text{ N/C}$
    *   $E_{1y} = 0$
  *   **E_2:**
    The angle $\theta$ between E_2 and the negative x-axis can be found using the geometry of the triangle:
    $\tan(\theta) = \frac{5}{5} = 1$, so $\theta = 45^\circ$
    $E_{2x} = -E_2 \cos(\theta) = -1800\text{ N/C} \times \frac{1}{\sqrt{2}} = -1273 \text{ N/C}$
    $E_{2y} = -E_2 \sin(\theta) = -1800\text{ N/C} \times \frac{1}{\sqrt{2}} = -1273 \text{ N/C}$
  
  **Step 4: Calculate the net electric field components.**
  
  *   $E_{net,x} = E_{1x} + E_{2x} = 3600 \text{ N/C} - 1273 \text{ N/C} = 2327 \text{ N/C}$
  *   $E_{net,y} = E_{1y} + E_{2y} = 0 - 1273 \text{ N/C} = -1273 \text{ N/C}$
  
  **Step 5: Calculate the magnitude of the net electric field.**
  
  *   $E_{net} = \sqrt{E_{net,x}^2 + E_{net,y}^2} = \sqrt{(2327 \text{ N/C})^2 + (-1273 \text{ N/C})^2} \approx 2653 \text{ N/C}$
## Forces and Torques on Charges in Electric Fields
  
  **Force on a Charge in an Electric Field:**
  
  A charge *q* placed in an electric field *E* experiences a force given by:
  
  $\vec{F} = q\vec{E}$
  
  *   **Direction:**
    *   If *q* is positive, the force is in the same direction as the electric field.
    *   If *q* is negative, the force is in the opposite direction to the electric field.
  *   **Magnitude:** The magnitude of the force is *F* = |*q*|*E*.
## Examples
  
  **Example 1:**
  
  A protein molecule with a net charge of 30e is in an electric field of magnitude 1500 N/C. Calculate the magnitude of the force it experiences. (e = 1.6 x 10<sup>-19</sup> C)
  
  **Solution:**
  
  $F = |q|E = (30 \times 1.6 \times 10^{-19} \text{ C})(1500 \text{ N/C}) = 7.2 \times 10^{-15} \text{ N}$
  
  **Example 2:**
  
  The electric field in a cell membrane has a magnitude of 1.0 x 10<sup>7</sup> N/C. Calculate the magnitude of the force exerted on a Na<sup>+</sup> ion (which has a charge of +e).
  
  **Solution:**
  
  $F = |q|E = (1.6 \times 10^{-19} \text{ C})(1.0 \times 10^7 \text{ N/C}) = 1.6 \times 10^{-12} \text{ N}$
## Electric Dipole in a Uniform Electric Field
  
  **Electric Dipole:**
  
  *   Consists of two equal and opposite charges (+q and -q) separated by a distance *L*.
  *   **Dipole Moment (p):** A vector quantity that points from the negative charge to the positive charge and has a magnitude of *p* = *qL*.
  
  **Behavior in a Uniform Electric Field:**
  
  *   **Net Force:** The net force on the dipole is zero because the forces on the two charges are equal in magnitude and opposite in direction.
  *   **Torque:** The electric field exerts a torque on the dipole, tending to align it with the field.
  *   The magnitude of the torque is given by $\tau = pE \sin(\theta)$, where $\theta$ is the angle between the dipole moment vector and the electric field vector.
  *   **Equilibrium:**
    *   **Stable Equilibrium:** When the dipole moment is aligned with the electric field ($\theta$ = 0°), the torque is zero, and the dipole is in stable equilibrium.
    *   **Unstable Equilibrium:** When the dipole moment is anti-aligned with the electric field ($\theta$ = 180°), the torque is zero, but the dipole is in unstable equilibrium.
## Applications
### Cathode Ray Tube (CRT)
  
  *   **Older technology** used in televisions and computer monitors.
  *   **Principle:** Electrons are emitted from a heated cathode, accelerated by an electric field, and then deflected by electric or magnetic fields to strike a phosphorescent screen, creating an image.
### Flow Cytometry
  
  *   **Technique** used to analyze cells and particles.
  *   **Principle:** Cells are suspended in a fluid and passed through a laser beam. The scattered light and fluorescence from the cells are measured, providing information about their size, shape, and internal structure.
  *   **Electric Fields:** Electric fields can be used to sort cells based on their charge.
## Summary
  
  This lecture focused on calculating electric fields from multiple charges and understanding the forces and torques experienced by charges and dipoles in electric fields. We also explored two practical applications, the cathode ray tube and flow cytometry, that utilize these principles.