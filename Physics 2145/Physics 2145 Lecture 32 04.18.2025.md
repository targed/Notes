# Lecture 32: Review for Test 3
  
  This lecture summarizes the key concepts and formulas related to magnetism and electromagnetic induction covered in previous lectures.
## 1. Permanent Magnets and Magnetic Poles
  
  *   **Poles:** Magnets have North (N) and South (S) poles.
  *   **Interaction:** Like poles repel, opposite poles attract.
  *   **No Monopoles:** Magnetic poles always exist in pairs (N and S). Isolated magnetic poles (monopoles) have not been observed. Breaking a magnet creates two smaller magnets, each with N and S poles.
  *   **Magnetic Field:** Magnets create a magnetic field ($\vec{B}$) in the space around them. Field lines point away from N poles and towards S poles outside the magnet.
## 2. Magnetic Fields Created by Currents
  
  Electric currents are a source of magnetic fields.
  
  *   **Long Straight Wire:**
    *   Magnitude: $B = \frac{\mu_0 I}{2\pi r}$
    *   Direction: Right-Hand Rule 1 (Thumb = Current I, Fingers curl in direction of B). Field lines are circles around the wire.
    *   $\mu_0 = 4\pi \times 10^{-7} \text{ T}\cdot\text{m/A}$ (permeability of free space).
  
  *   **Center of a Current Loop (N turns, Radius R):**
    *   Magnitude (at center): $B = \frac{\mu_0 N I}{2R}$
    *   Direction: Right-Hand Rule 2 (Fingers = Current I, Thumb = B direction at the center).
    *   Note: Field is only uniform at the very center.
  
  *   **Long Solenoid (n = turns per unit length):**
    *   Magnitude (inside): $B = \mu_0 n I$ (where $n = N/L$)
    *   Direction: Along the axis of the solenoid. Use RHR 2 (Fingers = current direction around coils, Thumb = B direction inside).
    *   Field inside is approximately uniform and strong.
    *   Field outside is very weak (approximately zero for an ideal long solenoid).
## 3. Magnetic Force on Moving Charges
  
  A magnetic field exerts a force on a moving charged particle.
  
  *   **Formula (Magnitude):** $F_B = |q|vB\sin\theta$
    *   $|q|$: magnitude of charge
    *   $v$: speed of charge
    *   $B$: magnetic field strength
    *   $\theta$: angle between velocity vector $\vec{v}$ and magnetic field vector $\vec{B}$.
  *   **Vector Form:** $\vec{F}_B = q(\vec{v} \times \vec{B})$
  *   **Direction: Right-Hand Rule (for Force):**
    1.  Fingers point along $\vec{v}$.
    2.  Curl fingers towards $\vec{B}$.
    3.  Thumb points in direction of $\vec{F}_B$ for a **positive** charge.
    4.  Force is **opposite** thumb direction for a **negative** charge.
  *   **Key Property:** $\vec{F}_B$ is always perpendicular to both $\vec{v}$ and $\vec{B}$. Magnetic force does **no work** and cannot change the particle's speed or kinetic energy, only its direction.
## 4. Motion of Charges in a Uniform Magnetic Field
  
  *   **Circular Motion:** If a charge *q* enters a uniform magnetic field $\vec{B}$ with velocity $\vec{v}$ perpendicular to $\vec{B}$ ($\theta = 90^\circ$), the magnetic force provides the centripetal force, causing uniform circular motion.
    *   $F_B = |q|vB$
    *   Centripetal Force = $mv^2/r$
    *   Equating: $|q|vB = \frac{mv^2}{r}$
    *   Radius of path: $r = \frac{mv}{|q|B}$
  
  *   **Application: Mass Spectrometer:** Uses magnetic fields to separate ions based on their mass-to-charge ratio ($m/|q|$). Ions are accelerated, enter a magnetic field, and follow circular paths with radii proportional to $\sqrt{m/|q|}$ (if accelerated through a potential) or $m/|q|$ (if velocity selected). Different masses land at different detector positions. $r = \frac{1}{B}\sqrt{\frac{2m\Delta V}{|q|}}$
## 5. Magnetic Force on Current-Carrying Wires
  
  A magnetic field exerts a force on a wire carrying current.
  
  *   **Straight Wire (Length L, Current I):**
    *   Magnitude: $F_{wire} = ILB\sin\theta$
    *   $\theta$: angle between the direction of current I and magnetic field $\vec{B}$.
    *   Vector Form: $\vec{F}_{wire} = I(\vec{L} \times \vec{B})$ (where $\vec{L}$ points along the wire in the direction of I).
    *   Direction: Use the same RHR as for the force on a positive charge (Fingers = I, Curl to B, Thumb = F).
  
  *   **Forces Between Parallel Currents:**
    *   Currents in the **same** direction: Wires **attract**.
    *   Currents in **opposite** directions: Wires **repel**.
## 6. Force and Torque on a Current Loop
  
  *   **Uniform Field:** A current loop in a uniform magnetic field generally experiences a **net torque** but often **zero net force**.
  *   **Magnetic Dipole Moment ($\vec{\mu}$):** A loop carrying current *I* with area *A* has a magnetic dipole moment.
    *   Magnitude: $\mu = NIA$ (for N turns)
    *   Direction: Perpendicular to the plane of the loop, given by RHR 2 (curl fingers with I, thumb points in direction of $\vec{\mu}$).
  *   **Torque Formula:**
    *   Magnitude: $\tau = \mu B \sin\phi$
    *   $\phi$: angle between the magnetic dipole moment $\vec{\mu}$ and the magnetic field $\vec{B}$.
    *   Vector Form: $\vec{\tau} = \vec{\mu} \times \vec{B}$
  *   **Effect:** The torque tends to rotate the loop to align its magnetic dipole moment $\vec{\mu}$ with the external magnetic field $\vec{B}$.
## 7. Magnetic Flux ($\Phi_B$)
  
  *   **Definition:** Measures the amount of magnetic field passing through a surface area.
  *   **Formula (Uniform B, Flat Area A):**
    $\Phi_B = BA\cos\theta$
    *   $\theta$: angle between the magnetic field $\vec{B}$ and the **normal** to the surface area.
  *   **Units:** Weber (Wb). 1 Wb = 1 T⋅m<sup>2</sup>.
## 8. Electromagnetic Induction
  
  A changing magnetic flux through a loop induces an emf (and potentially a current).
  
  *   **Faraday's Law:** Gives the magnitude of the induced emf (ℰ).
    *   Magnitude: $|ℰ| = N |\frac{d\Phi_B}{dt}|$ (Instantaneous)
    *   Magnitude: $|ℰ| = N |\frac{\Delta\Phi_B}{\Delta t}|$ (Average)
    *   N: number of turns in the coil.
    *   A flux change can be caused by changes in B, A, or θ.
  
  *   **Lenz's Law:** Determines the direction of the induced current.
    *   Statement: The induced current flows in a direction such that the magnetic field it creates **opposes the change** in magnetic flux that caused it.
    *   If flux increases, induced B opposes original B.
    *   If flux decreases, induced B reinforces original B.
    *   Use RHR 2 (Thumb = Induced B, Fingers = Induced I) to find current direction.
  
  This review covers the essential topics for Test 3. Ensure you understand the concepts, formulas, and especially the various Right-Hand Rules for determining directions. Good luck!