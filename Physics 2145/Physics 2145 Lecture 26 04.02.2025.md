# Lecture 26: Magnetic Force (Continued) - Forces on Currents and Torques on Loops
  
  This lecture extends the concept of magnetic force from single moving charges to macroscopic currents in wires and loops.
## Review: Force on Moving Charges
  
  Recall from the previous lecture, the magnetic force ($\vec{F}_B$) on a single charge *q* moving with velocity $\vec{v}$ in a magnetic field $\vec{B}$ is given by:
  
  $\vec{F}_B = q(\vec{v} \times \vec{B})$
  
  The magnitude is:
  
  $F_B = |q|vB\sin\theta$
  
  Where $\theta$ is the angle between $\vec{v}$ and $\vec{B}$. The direction is determined by the Right-Hand Rule.
## Force on a Straight Current-Carrying Wire
  
  *   **Concept:** A current in a wire consists of many moving charges. If a magnetic field exerts a force on individual moving charges, it must also exert a net force on the wire carrying the current.
  *   **Derivation (Conceptual):**
    *   Consider a segment of wire of length *L* carrying current *I*.
    *   The current *I* is due to charge carriers (e.g., electrons with charge *q* each) moving with an average drift velocity *v<sub>d</sub>*.
    *   The total charge ΔQ moving through the segment in time Δt is ΔQ = IΔt.
    *   The time it takes for charge to travel length L is Δt = L/v<sub>d</sub>.
    *   The force on a single charge carrier is $F_{single} = q v_d B \sin\theta$.
    *   The total number of charge carriers (N) in the segment is related to the total charge: ΔQ = Nq.
    *   The total force on the wire segment is the sum of forces on all charge carriers: $F_{wire} = N F_{single} = N q v_d B \sin\theta = (Nq) v_d B \sin\theta = (\Delta Q) v_d B \sin\theta$
    *   Substituting $\Delta Q = I \Delta t = I (L/v_d)$:
        $F_{wire} = (I L/v_d) v_d B \sin\theta = ILB\sin\theta$
  
  *   **Formula (Magnitude):** The magnitude of the magnetic force ($F_{wire}$) on a straight wire of length *L* carrying current *I*, placed in a uniform magnetic field *B*, is:
  
    $F_{wire} = ILB\sin\theta$
  
    Where $\theta$ is the angle between the direction of the current (*I*) and the magnetic field ($\vec{B}$).
  
  *   **Vector Form:**
  
    $\vec{F}_{wire} = I(\vec{L} \times \vec{B})$
  
    Where $\vec{L}$ is a vector whose magnitude is the length of the wire segment and whose direction points *along the wire in the direction of the conventional current I*.
  
  *   **Direction: Right-Hand Rule (for Force on Current):**
    1.  Point the fingers of your right hand in the direction of the **conventional current (I)** (or the direction of the $\vec{L}$ vector).
    2.  Curl your fingers towards the direction of the **magnetic field ($\vec{B}$)**.
    3.  Your thumb will point in the direction of the **magnetic force ($\vec{F}_{wire}$)** on the wire.
## Forces Between Parallel Currents
  
  *   **Setup:** Consider two long, parallel wires carrying currents I<sub>1</sub> and I<sub>2</sub>, separated by a distance *d*.
  *   **Mechanism:**
    *   Wire 1 creates a magnetic field (B<sub>1</sub>) at the location of wire 2.
    *   Wire 2, carrying current I<sub>2</sub>, experiences a force due to the magnetic field B<sub>1</sub>.
    *   Similarly, wire 2 creates a field B<sub>2</sub> at wire 1, and wire 1 experiences a force due to B<sub>2</sub>.
  *   **Result:**
    *   **Currents in the Same Direction:** The wires **attract** each other.
        *   (Wire 1 creates B<sub>1</sub> into the page at wire 2. Using RHR for force on wire 2 (I<sub>2</sub> up, B<sub>1</sub> in), F<sub>1 on 2</sub> points towards wire 1).
    *   **Currents in Opposite Directions:** The wires **repel** each other.
        *   (Wire 1 creates B<sub>1</sub> into the page at wire 2. Using RHR for force on wire 2 (I<sub>2</sub> down, B<sub>1</sub> in), F<sub>1 on 2</sub> points away from wire 1).
## Example: Square Current Loop
  
  *   **Setup:** Consider a square loop of wire carrying current placed in a uniform magnetic field.
  *   **Analysis (Conceptual):**
    *   Calculate the force on each of the four straight segments of the loop using $\vec{F} = I(\vec{L} \times \vec{B})$.
    *   The direction of the force on each segment depends on the orientation of that segment relative to the magnetic field.
    *   Depending on the orientation of the loop, the forces on opposite sides might cancel out, or they might create a net torque.
## Force and Torque on a Current Loop
  
  *   **Setup:** Consider a rectangular loop of wire (sides *a* and *b*) carrying current *I* placed in a uniform magnetic field $\vec{B}$. Let the plane of the loop make an angle with the magnetic field.
  *   **Forces on Sides:**
    *   The forces on the sides of the loop parallel to the axis of rotation (sides of length *a*) will be equal and opposite, directed perpendicular to the field and the current. These forces create a **torque**.
    *   The forces on the sides perpendicular to the axis of rotation (sides of length *b*) will be equal and opposite, acting along the same line (if the field is uniform), resulting in **zero net force** and **zero net torque** from these two sides.
  *   **Torque Calculation:**
    *   Force on one side of length *a*: $F = I a B$ (assuming the field is perpendicular to this side for maximum torque).
    *   The perpendicular distance from the axis of rotation to the line of action of this force is $(b/2)\sin\phi$, where $\phi$ is the angle between the normal to the loop and the magnetic field.
    *   Torque due to one side: $\tau_1 = F \times (\text{lever arm}) = (IaB)(\frac{b}{2}\sin\phi)$
    *   Total torque (from both sides): $\tau = 2 \tau_1 = IabB\sin\phi$
  *   **Area:** The area of the loop is A = ab.
  *   **Torque Formula:**
  
    $\tau = IAB\sin\phi$
  
    Where $\phi$ is the angle between the **normal** to the plane of the loop and the magnetic field $\vec{B}$.
  
  *   **Magnetic Dipole Moment ($\vec{\mu}$):**
    *   A current loop acts like a magnetic dipole.
    *   The magnetic dipole moment is a vector defined as:
  
        $\vec{\mu} = IA\hat{n}$
  
        Where:
        *   $I$ is the current.
        *   $A$ is the area enclosed by the loop.
        *   $\hat{n}$ is a unit vector **normal** (perpendicular) to the plane of the loop, with its direction determined by another Right-Hand Rule: Curl the fingers of your right hand in the direction of the current; your thumb points in the direction of $\vec{\mu}$ (and $\hat{n}$).
    *   For a loop with N turns: $\vec{\mu} = NIA\hat{n}$
    *   Units: Ampere-meter<sup>2</sup> (A⋅m<sup>2</sup>)
  
  *   **Torque in Terms of Dipole Moment:** The torque formula can be written compactly using the magnetic dipole moment:
  
    $\tau = \mu B \sin\phi$
  
    Or in vector form:
  
    $\vec{\tau} = \vec{\mu} \times \vec{B}$
  
  *   **Behavior:** The torque tends to **align** the magnetic dipole moment vector ($\vec{\mu}$) with the external magnetic field vector ($\vec{B}$). The torque is maximum when $\vec{\mu}$ is perpendicular to $\vec{B}$ ($\phi = 90^\circ$) and zero when $\vec{\mu}$ is parallel or anti-parallel to $\vec{B}$ ($\phi = 0^\circ$ or $180^\circ$).
## Summary
  
  This lecture established the formula for the magnetic force on a current-carrying wire ($F = ILB\sin\theta$) and its direction using the Right-Hand Rule. We saw that parallel currents attract if in the same direction and repel if in opposite directions. We then analyzed the forces on a current loop in a uniform magnetic field, finding that while the net force is often zero, there is generally a net torque. This torque was expressed as $\tau = IAB\sin\phi$, and introduced the concept of the magnetic dipole moment ($\vec{\mu} = IA\hat{n}$), allowing the torque to be written as $\vec{\tau} = \vec{\mu} \times \vec{B}$. This torque acts to align the loop's magnetic moment with the external field.