# Lecture 1: Course Introduction - Coulomb's Law
## Semester Preview
  
  This lecture introduces the concept of electric charge and the force between charged particles, governed by Coulomb's Law.
## Charge
  
  *   **Definition:** There are two types of electric charge:
    *   **Positive**
    *   **Negative**
  *   **Interactions:**
    *   **Like charges repel** each other.
    *   **Unlike charges attract** each other.
  *   **Transfer:** Charge can be transferred from one object to another upon contact.
  *   **Conservation:** Charge cannot be created or destroyed; it is a conserved quantity.
  *   **Convention:** A glass rod rubbed with silk is defined as positively charged. Any object attracted to this charged rod is considered negatively charged.
## Atomic Structure
  
  *   **Atom:** Composed of a nucleus and surrounding electrons.
  *   **Nucleus:** Contains:
    *   **Protons:** Positively charged particles.
    *   **Neutrons:** Neutral particles (no charge).
  *   **Electrons:** Negatively charged particles that can move around the nucleus.
  *   **Quantization of Charge:**
    *   The charge of an electron is -e = -1.6 x 10<sup>-19</sup> Coulombs (C).
    *   The charge of a proton is +e = +1.6 x 10<sup>-19</sup> Coulombs (C).
    *   Charges can only exist in integer multiples of the elementary charge 'e'.
  *   **Elementary Charge:** e = 1.6 x 10<sup>-19</sup> C
## Insulators and Conductors
  
  *   **Insulator:** A material in which charges are immobile and cannot move freely.
  *   **Conductor:** A material that has mobile electrons, allowing for the flow of electric current.
## Force Between Charges: Coulomb's Law
  
  Coulomb's Law describes the force between two point charges:
  
  $F = k \frac{|q_1||q_2|}{r^2}$
  
  Where:
  
  *   $F$ is the magnitude of the electrostatic force between the charges.
  *   $k$ is Coulomb's constant, approximately equal to 9 x 10<sup>9</sup> N•m<sup>2</sup>/C<sup>2</sup>.
  *   $q_1$ and $q_2$ are the magnitudes of the charges.
  *   $r$ is the distance between the charges.
  
  **Key Features:**
  
  *   The force is inversely proportional to the square of the distance between the charges (an inverse-square law).
  *   The force is proportional to the product of the magnitudes of the charges.
  *   The force is attractive if the charges have opposite signs and repulsive if the charges have the same sign.
  
  **Comparison with Gravity:**
  Similar to Newton's Law of Universal Gravitation in terms of inverse square relationship.
## Multiple Charges
  
  *   **Superposition Principle:** When multiple charges are present, each charge exerts a force on every other charge as described by Coulomb's Law.
  *   **Net Force:** The net force on a particular charge is the vector sum of all the individual forces acting on it due to the other charges.
## Vector Nature of Forces
  
  *   **Vectors:** Electric forces are vector quantities, possessing both magnitude and direction.
  *   **Vector Addition:** To find the net force, we must perform vector addition, often using components.
## Example Problem
  
  **Problem:** Three charges are arranged in a line:
  
  *   q_1 = +1 nC, located at x = -1 cm
  *   q_2 = +2 nC, located at x = 0 cm
  *   q_3 = -2 nC, located at x = 1 cm
  
  Find the net force on q_2.
  
  **Solution**
  We denote the distance between charges q_i and q_j with r_ij. The force between the charges q_i and q_j is denoted with F_i on j.
  *   r_12 = 1cm
  *   r_23 = 1cm
  
  The force on q_2 due to q_1 is:
  
  $F_{1 \text{ on } 2} = k \frac{|q_1||q_2|}{r_{12}^2}$
  
  The force on q_2 due to q_3 is:
  
  $F_{3 \text{ on } 2} = k \frac{|q_2||q_3|}{r_{23}^2}$
  
  Since q_1 and q_2 are both positive, they will repel, and the force F_1 on 2 will be in the positive x direction. q_3 is negative, and q_2 is positive, so they will attract, and the force F_3 on 2 will also be in the positive x direction.
  Therefore, we can just add the magnitudes of these forces to find the net force on q_2, which will be in the x direction.
  
  $F_{net_x} = F_{1 \text{ on } 2_x} + F_{3 \text{ on } 2_x}$
  
  $F_{net_x} = k \frac{|q_1||q_2|}{r_{12}^2} + k \frac{|q_2||q_3|}{r_{23}^2}$
  $F_{net_x} = 9 \times 10^9 \frac{\text{Nm}^2}{\text{C}^2} \frac{(1 \times 10^{-9} \text{C})(2 \times 10^{-9}\text{C})}{(1 \times 10^{-2} \text{m})^2} + 9 \times 10^9 \frac{\text{Nm}^2}{\text{C}^2} \frac{(2 \times 10^{-9}\text{C})(2 \times 10^{-9} \text{C})}{(1 \times 10^{-2} \text{m})^2}$
  
  $F_{net_x} = 1.8 \times 10^{-4} \text{N} + 3.6 \times 10^{-4} \text{N}$
  $F_{net_x} = 5.4 \times 10^{-4} \text{N}$
  
  The net force on q_2 in the y direction is zero because the charges are aligned on the x-axis.
  
  $F_{net_y} = 0$
  Therefore, the net force on q_2 is $5.4 \times 10^{-4} \text{N}$ in the positive x-direction.
  
  **Reminder:**
  
  Vector addition can be performed by breaking vectors into their components along coordinate axes (e.g., x and y) and then adding the corresponding components.
  **Bio Application:** Hydrogen Bonds
  These occur when a partially positive hydrogen atom is attracted to a partially negative atom, like oxygen or nitrogen. These bonds are electrostatic in nature, much like the attraction between opposite charges described by Coulomb's Law. The force of attraction between the hydrogen and another atom can be understood in terms of Coulomb's Law, where the partial charges act similarly to q_1 and q_2.