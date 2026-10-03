## Introduction
  
  This lecture delves deeper into gravitational potential energy, extending the concept to scenarios relevant for space travel. We will cover:
  
  * Universal gravitational potential energy
  * Space travel problems
  * Escape speed
  * Orbital energy
  * Multiple objects (gravitational potential energy in systems with more than two objects)
## Gravitational Potential Energy
  
  * **Gravitational force:** $F_{grav} = -\frac{GmM}{r^2}$ (attractive force)
  * **Conservative force:** Gravity is a conservative force, so the work it does is path-independent.
  * **Potential energy difference:** The difference in gravitational potential energy between two points is:
   $U_B - U_A = -W_{A \to B} = -\int_{\vec{r}_A}^{\vec{r}_B} \vec{F} \cdot d\vec{r}$
## Deriving Gravitational Potential Energy
  
  * **Work done by gravity:**
   $W = \int_{\vec{r}_A}^{\vec{r}_B} \vec{F} \cdot d\vec{r} = \int_{r_A}^{r_B} F_r dr = \int_{r_A}^{r_B} -\frac{GmM}{r^2} dr$
  * **Evaluating the integral:**
   $W = \left[ \frac{GmM}{r} \right]_{r_A}^{r_B} = \frac{GmM}{r_B} - \frac{GmM}{r_A}$
  * **Potential energy difference:**
   $U_B - U_A = -W = -\left( \frac{GmM}{r_B} - \frac{GmM}{r_A} \right) = GmM \left( \frac{1}{r_A} - \frac{1}{r_B} \right)$
  * **Reference point:** We choose the reference point to be at infinity ($r_0 = \infty$), where the potential energy is zero ($U(r_0 = \infty) = 0$).
  * **Gravitational potential energy:**
   $U_{grav} = -\frac{GmM}{r}$ (negative because gravity is attractive)
## Potential Energy Diagram
  
  **(Diagrams showing potential energy $U$ as a function of distance $r$ for different total energies $E$)**
  
  * **$E < 0$:** Bound orbits (elliptical or circular). The object is trapped in the gravitational well.
  * **$E = 0$:** Parabolic orbit. The object has just enough energy to escape to infinity with zero velocity.
  * **$E > 0$:** Hyperbolic trajectory. The object has more than enough energy to escape, and its speed approaches a non-zero value as $r \to \infty$.
## Escape Condition
  
  * **Total energy:** $E = \frac{1}{2}mv^2 - \frac{GmM}{r}$
  * **Escape condition:**  To escape the gravitational pull of a mass $M$, an object must have enough kinetic energy to overcome the negative gravitational potential energy. This occurs when the total energy $E$ is greater than or equal to zero.
## Escape Speed
  
  * **Definition:** The minimum speed an object must have at a distance $R$ from a central mass $M$ to escape to infinity.
  * **Derivation:**  We set the total energy at the initial position equal to the total energy at infinity:
    * $E_i = E_f$
    * $\frac{1}{2}mv_{esc}^2 - \frac{GmM}{R} = \frac{1}{2}m(0)^2 - \frac{GmM}{\infty} = 0$
    * Solving for $v_{esc}$, we get: $v_{esc} = \sqrt{\frac{2GM}{R}}$
## Example: Escape Speed from Earth
  
  * Using the values for Earth's mass ($M_E = 5.97 \times 10^{24} \text{ kg}$) and radius ($R_E = 6.38 \times 10^6 \text{ m}$), and the gravitational constant $G$, we find the escape speed from Earth's surface to be approximately 11,200 m/s.
## Example: Escape Speed from Orbit
  
  * **Escape from Sun's gravity:** Calculate the escape speed needed to leave the solar system from Earth's orbit. Use the mass of the Sun and the distance between Earth and the Sun.
## Orbital Energy
  
  * **Total energy of a satellite:** For a satellite in a circular orbit of radius $R$ around a planet of mass $M$:
   $E = K + U = \frac{1}{2}mv^2 - \frac{GmM}{R}$
  * **Speed of satellite:** In a circular orbit, the gravitational force provides the centripetal force, so $v^2 = \frac{GM}{R}$.
  * **Substituting for $v^2$:**
   $E = \frac{1}{2}m\left(\frac{GM}{R}\right) - \frac{GmM}{R} = -\frac{GmM}{2R}$
  * **Orbital energy is negative:**  The negative orbital energy indicates that the satellite is bound to the planet.
## Satellite Motion (Review)
  
  *(Review of the equations for satellite motion and centripetal force, as covered in Lecture 13)*
## Multiple Objects
  
  * **Gravitational potential energy for multiple objects:**  The total gravitational potential energy of a system of multiple objects is the sum of the potential energies due to every pair of objects.
  * **Example:** For three objects with masses $M_1$, $M_2$, and $m$, and distances $r_1$ and $r_2$:
   $U_g = U_{g1} + U_{g2} = -\frac{GM_1m}{r_1} - \frac{GM_2m}{r_2}$
## Example with Multiple Objects
  
  **Problem:** A planet has mass $4M$ and radius $2r$. Its moon has mass $M$ and radius $r$. The centers of the planet and moon are a distance $9r$ apart. A shuttle of mass $m$ is a distance $4r$ away from the center of the planet and is moving with speed $V$. What is the total mechanical energy of the shuttle?
  
  **(Diagram provided with planet, moon, and shuttle positions and parameters.)**
  
  **(Solution will be derived in lecture, adding kinetic and potential energies.)**
## Work Done by Engines
  
  **Problem:** If the shuttle was initially at rest at position X (closer to the planet), how much work did the engines do to bring it to its current position and speed?
  
  **(Diagram of the same setup as in the previous example, marking the initial position X.)**
  
  **(Solution will be discussed, using the work-energy theorem: the work done by the engines is equal to the change in the shuttle's mechanical energy.)**