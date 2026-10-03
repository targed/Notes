# Lecture 25: Magnetic Force
  
  This lecture focuses on the force exerted by a magnetic field on a moving charged particle.
## Force on Moving Charges
  
  *   **Observation:** A magnetic field exerts a force on a charged particle *only if the particle is moving*. A stationary charge experiences no magnetic force.
  *   **Magnitude:** The magnitude of the magnetic force ($F_B$) on a charge *q* moving with velocity *v* in a magnetic field *B* is given by:
  
    $F_B = |q|vB\sin\theta$
  
    Where:
    *   $F_B$ is the magnitude of the magnetic force (in Newtons, N).
    *   $|q|$ is the magnitude of the charge (in Coulombs, C).
    *   $v$ is the speed of the particle (in meters/second, m/s).
    *   $B$ is the magnitude of the magnetic field strength (in Tesla, T).
    *   $\theta$ is the angle between the velocity vector ($\vec{v}$) and the magnetic field vector ($\vec{B}$).
  
  *   **Important Cases:**
    *   **Maximum Force:** The force is maximum when the velocity is perpendicular to the magnetic field ($\theta = 90^\circ$, $\sin 90^\circ = 1$). $F_{max} = |q|vB$.
    *   **Zero Force:** The force is zero if the velocity is parallel or anti-parallel to the magnetic field ($\theta = 0^\circ$ or $\theta = 180^\circ$, $\sin 0^\circ = \sin 180^\circ = 0$).
  
  *   **Vector Form:** The magnetic force can be expressed using the vector cross product:
  
    $\vec{F}_B = q(\vec{v} \times \vec{B})$
## Direction of Force: Right-Hand Rule (RHR) for Magnetic Force
  
  1.  **Point Fingers:** Point the fingers of your right hand in the direction of the velocity vector ($\vec{v}$).
  2.  **Curl Fingers:** Curl your fingers towards the direction of the magnetic field vector ($\vec{B}$).
  3.  **Thumb Points:** Your thumb will point in the direction of the magnetic force ($\vec{F}_B$) **if the charge *q* is positive**.
  4.  **Negative Charge:** If the charge *q* is **negative**, the force is in the *opposite* direction to where your thumb points.
  
  **Key Feature:** The magnetic force is always **perpendicular** to *both* the velocity vector ($\vec{v}$) and the magnetic field vector ($\vec{B}$). This means the magnetic force does **no work** on the charged particle, as the force is always perpendicular to the displacement. Consequently, the magnetic force **cannot change the kinetic energy or speed** of the particle; it can only change its direction.
## Examples (Conceptual)
  
  *   An electron moving horizontally enters a region with a vertical magnetic field pointing upwards. What is the direction of the force?
    *   $\vec{v}$ is horizontal, $\vec{B}$ is up.
    *   Using RHR: Point fingers horizontally, curl towards up. Thumb points out of the page.
    *   Since the charge is *negative* (electron), the force is *opposite* the thumb direction, i.e., **into the page**.
  *   A proton moves parallel to a magnetic field. What is the force?
    *   $\theta = 0^\circ$.
    *   $F_B = |q|vB\sin 0^\circ = 0$. The force is **zero**.
## Uniform Circular Motion (Review)
  
  *   **Definition:** Motion in a circle at a constant *speed*.
  *   **Velocity:** Even though the speed is constant, the *velocity* is **not** constant because the direction is continuously changing.
  *   **Acceleration:** Since the velocity is changing, there must be an acceleration. This acceleration is called **centripetal acceleration** ($\vec{a}_c$).
  *   **Centripetal Acceleration:**
    *   Directed towards the **center** of the circle.
    *   Magnitude: $a_c = \frac{v^2}{r}$
    *   Where *v* is the speed and *r* is the radius of the circle.
  *   **Centripetal Force:** According to Newton's Second Law ($\vec{F}=m\vec{a}$), this acceleration requires a net force directed towards the center of the circle. This force is called the centripetal force.
## Circular Motion in a Uniform Magnetic Field
  
  *   **Condition:** If a charged particle enters a uniform magnetic field with its velocity *perpendicular* to the field ($\theta = 90^\circ$), the magnetic force will act as a centripetal force.
  *   **Force:** $F_B = |q|vB\sin 90^\circ = |q|vB$.
  *   **Force Direction:** The force is always perpendicular to the velocity.
  *   **Result:** The particle will undergo uniform circular motion. The magnetic force provides the necessary centripetal force.
  
  *   **Equating Forces:**
    *   Magnetic Force = Centripetal Force
    *   $|q|vB = m\frac{v^2}{r}$
  
  *   **Radius of Circular Path:** Solving for the radius *r*:
  
    $r = \frac{mv}{|q|B}$
  
    Where:
    *   $m$ is the mass of the particle.
    *   $v$ is the speed of the particle.
    *   $|q|$ is the magnitude of the charge.
    *   $B$ is the magnetic field strength.
  
  *   **Key Insight:** The radius of the circular path is proportional to the particle's momentum (*mv*) and inversely proportional to its charge and the magnetic field strength.
## Application: Mass Spectrometer
  
  *   **Purpose:** A device used to measure the masses of ions and determine the relative abundance of isotopes.
  *   **Principle:**
    1.  **Ionization:** The sample is ionized (atoms or molecules are given a net charge, usually positive).
    2.  **Acceleration:** The ions are accelerated through a potential difference (ΔV) to give them a known kinetic energy.
        *   Conservation of energy: $|q|\Delta V = \frac{1}{2}mv^2$
        *   Solving for speed: $v = \sqrt{\frac{2|q|\Delta V}{m}}$
    3.  **Velocity Selector (Optional but common):** Ions pass through a region with crossed electric and magnetic fields. Only ions with a specific velocity ($v = E/B_{selector}$) pass through undeflected.
    4.  **Magnetic Deflection:** The ions enter a region with a uniform magnetic field ($\vec{B}$) perpendicular to their velocity. They follow semicircular paths.
    5.  **Radius Dependence:** The radius of the path depends on the mass-to-charge ratio (m/q) of the ion:
        $r = \frac{mv}{|q|B} = \frac{m}{|q|B}\sqrt{\frac{2|q|\Delta V}{m}} = \frac{1}{B}\sqrt{\frac{2m\Delta V}{|q|}}$
    6.  **Detection:** The ions strike a detector at different positions depending on their path radii. By measuring the radius (or the position where they hit the detector), one can determine the mass-to-charge ratio. If the charge *q* is known (often +e), the mass *m* can be calculated.
  
  *   **Separation:** Ions with different masses will follow paths of different radii and strike the detector at different locations, allowing for separation and identification.
## Summary
  
  This lecture introduced the magnetic force on moving charged particles, described by $F_B = |q|vB\sin\theta$. The direction of this force is given by the right-hand rule and is always perpendicular to both velocity and the magnetic field. This force does no work but changes the direction of the particle. When a charge moves perpendicular to a uniform magnetic field, it undergoes uniform circular motion with a radius $r = mv/|q|B$. This principle is the basis for the mass spectrometer, a device used to separate and identify ions based on their mass-to-charge ratio.