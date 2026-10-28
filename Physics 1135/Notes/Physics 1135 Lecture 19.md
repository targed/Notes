## Introduction
  
  This lecture reviews key concepts and problem-solving strategies in preparation for Test 2.  We will work through example problems covering the topics since Test 1.
## Concepts: Work and Energy
  
  * **Definition and sign of work:** Work is done when a force acts on an object that undergoes a displacement. Work is positive when the force has a component in the direction of displacement, negative when the force opposes the displacement, and zero when the force is perpendicular to the displacement.
  * **Force perpendicular to the path:** Does zero work.
  * **Conservative force:** Work is independent of the path taken; only the initial and final positions matter.
  * **Force component as negative derivative of potential energy:** $F_x = -\frac{dU}{dx}$
  * **Potential energy diagrams:** Relate force, potential energy, and kinetic energy.
  * **Energy problems:** Applying the work-energy theorem and conservation of mechanical energy.
## Concepts: Universal Gravitation
  
  * **Free-fall acceleration:** $g = \frac{GM}{R^2}$
  * **Satellite motion:**  Gravitational force provides the centripetal force.  Kepler's 3rd law.
  * **Escape speed:** $v_{esc} = \sqrt{\frac{2GM}{R}}$
  * **Space travel:** Problems involving gravitational potential energy and energy conservation.
## Concepts: Momentum and Impulse
  
  * **Impulse:** $\vec{J} = \int \vec{F} dt = \Delta \vec{p}$ (Impulse equals the change in momentum)
  * **Collisions:**
    * **Inelastic:** Kinetic energy is not conserved.
    * **Perfectly inelastic:** Objects stick together after the collision.
    * **Elastic:** Kinetic energy is conserved.
  * **Center of mass motion under external forces:** $\sum \vec{F}_{ext} = M_{tot} \vec{a}_{CM}$
  * **Momentum conservation:** Problems involving collisions and explosions where the total momentum of the system is conserved in the absence of external forces.
## Concepts: Static Fluids
  
  * **Pressure increase with depth:** $p = p_0 + \rho gh$
  * **Pascal's principle:** Pressure applied to a confined fluid is transmitted undiminished throughout the fluid.
  * **Buoyancy:** Archimedes' principle: The buoyant force on a submerged object equals the weight of the fluid displaced by the object. $B = \rho_{fluid} V_{disp} g$.
## Example 1: Spring, Friction, and Incline
  
  **Problem:** A block of mass $M$ is pushed against a spring with an unknown spring constant, compressing it a distance $L$. When the block is released from rest, it travels a distance $d$ on a frictionless horizontal surface and then up a rough incline that has a coefficient of kinetic friction $\mu$ with the block. The incline makes an angle $\theta$ above the horizontal. When the block reaches height $H$ on the incline, its speed is $V$. Derive an expression for the force constant $k$ of the spring in terms of system parameters.
  
  **(Diagram showing the spring, block, incline, distances, angle, and coefficient of friction.)**
  
  **(This is a multi-step problem involving energy conservation and work done by non-conservative forces.  The solution steps are as follows):**
  
  1. **Initial energy stored in the spring:**  $U_{spring} = \frac{1}{2}kL^2$
  2. **Energy at the bottom of the incline:**  The potential energy is zero. The kinetic energy is equal to the initial spring potential energy (since there's no friction on the horizontal surface): $K = \frac{1}{2}MV_1^2 = \frac{1}{2}kL^2$ (where $V_1$ is the speed at the bottom of the incline).
  3. **Work done by friction on the incline:**  $W_{friction} = -f_k \Delta x = -\mu N \frac{H}{\sin\theta} = -\mu Mg \cos\theta \frac{H}{\sin\theta} = -\mu MgH \cot\theta$
  4. **Energy at height $H$ on the incline:** The block has both potential energy ($U_{grav} = MgH$) and kinetic energy ($K = \frac{1}{2}MV^2$).
  5. **Applying the work-energy theorem:** $\Delta E = W_{friction}$. The change in mechanical energy from the bottom of the incline to height $H$ is equal to the work done by friction:
    * $(\frac{1}{2}MV^2 + MgH) - (\frac{1}{2}MV_1^2) = -\mu MgH \cot\theta$
  6. **Solving for $k$:** Substituting $V_1$ from step 2 and solving for $k$, we get an expression for the spring constant in terms of the given system parameters.
## Example 2: Gravitational Forces and Acceleration
  
  **Problem:** Planet A has mass $4M$ and radius $2R$. Planet B has mass $3M$ and radius $R$. They are separated by center-to-center distance $8R$. A rock of mass $m$ is placed halfway between their centers at point O and released from rest. (Ignore any motion of the planets.)
  
  **(Diagram showing planets A and B, the rock at point O, and the distances.)**
  
  **Tasks:**
  
  a) Derive an expression for the magnitude and direction of the acceleration of the rock at the moment it is released.
  b) Derive an expression, in terms of relevant system parameters, for the speed with which the rock crashes into a planet.
  
  **(Solution steps will be discussed in lecture.)**
  
  **Part a):**
  
  1. **Gravitational forces:** Calculate the gravitational force on the rock from each planet using Newton's law of universal gravitation.
  2. **Net force:**  Find the net force on the rock by vector addition (since the forces are in opposite directions, we subtract their magnitudes).
  3. **Acceleration:** Use Newton's second law ($\vec{F}_{net} = m\vec{a}$) to find the acceleration of the rock.
  
  
  **Part b):** This can be solved using energy conservation:
  
  1. **Initial energy:** The rock starts at rest, so its initial kinetic energy is zero.  Calculate the initial gravitational potential energy due to both planets: $U_i = -\frac{G(4M)m}{4R} - \frac{G(3M)m}{4R}$.
  2. **Final energy:** When the rock crashes into a planet, we can assume its final potential energy is dominated by the potential energy due to that planet (since the distance to the other planet will be much larger). The final kinetic energy is $\frac{1}{2}mv^2$.
  3. **Energy conservation:** Set $E_i = E_f$ and solve for the final speed $v$.
## Example 3: 2D Collision on a Frictionless Surface
  
  **Problem:** Bilbo and Thorin slide on a frictionless, horizontal, frozen pond. Thorin (mass $M$) is initially moving eastwards with speed $v_{Ti}$. Bilbo (mass $m$) is initially sliding northward. They collide, and after the collision, Thorin is moving with speed $v_{Tf}$ at angle $\theta$ north of east, while Bilbo is moving at angle $\phi$ south of east. 
  
  **(Diagrams showing the collision before and after, with velocities and angles labeled.)**
  
  **Tasks:**
  
  a) Derive expressions for the speed of Bilbo before and after the collision.
  b) Derive an expression for the average force exerted on Thorin by Bilbo in unit vector notation, if the two are in contact for a time span $\Delta t$.
  
  **(Solutions will be derived during the lecture. This problem involves conservation of momentum in two dimensions and the impulse-momentum theorem.)**
  
  
  **Part a):**
  
  1. **Momentum conservation:** Write the equations for conservation of momentum in the x and y directions.
  2. **Solve for Bilbo's velocities:** Use the equations to solve for the unknown speeds $v_{Bi}$ (initial speed of Bilbo) and $v_{Bf}$ (final speed of Bilbo).
  
  
  **Part b):**
  
  1. **Change in Thorin's momentum:** Calculate the change in Thorin's momentum vector: $\Delta \vec{p}_T = \vec{p}_{Tf} - \vec{p}_{Ti}$.
  2. **Impulse-momentum theorem:** The average force exerted on Thorin by Bilbo is related to the change in Thorin's momentum by: $\vec{F}_{avg} \Delta t = \Delta \vec{p}_T$.
  3. **Solve for average force:** Solve for $\vec{F}_{avg}$ and express it in unit vector notation.