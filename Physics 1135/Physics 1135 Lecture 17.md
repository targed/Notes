## Introduction
  
  This lecture introduces the concept of linear momentum, a fundamental quantity in physics that is conserved in the absence of external forces.  We will cover:
  
  * Definition of impulse and linear momentum
  * Systems of particles
  * Conservation of linear momentum
  * Explosions and collisions
  
  [Cats playing with Newton's cradle](replace with link if available - optional)
## Linear Momentum
  
  * **Definition:** Linear momentum ($\vec{p}$) is the product of an object's mass ($m$) and its velocity ($\vec{v}$).
  * **Formula:** $\vec{p} = m\vec{v}$
  * **Vector quantity:** Momentum is a vector, meaning it has both magnitude and direction.
  * **Newton's 2nd Law:** Newton's second law can be expressed in terms of momentum:
    * $\vec{F}_{net} = \frac{d\vec{p}}{dt}$ (The net force is the rate of change of momentum)
    * For constant mass: $\vec{F}_{net} = \frac{d(m\vec{v})}{dt} = m\frac{d\vec{v}}{dt} = m\vec{a}$ (which is the familiar form of Newton's 2nd Law)
## Impulse
  
  * **Definition:** Impulse ($\vec{J}$) is the change in momentum of an object caused by a force acting over a period of time.  It's also the area under the force vs time graph.
  * **Formula:** $\vec{J} = \int_{t_i}^{t_f} \vec{F} dt$
  * **Vector quantity:** Impulse is a vector.
  * **Average force:**  If the force is not constant, we can use the average force:
    * $\vec{J} = \vec{F}_{avg} \Delta t$
  
  **(Diagram showing the force vs. time graph and the area representing impulse.)**
## Change in Momentum and Impulse
  
  * **Derivation:** Starting from Newton's 2nd Law:
    * $\vec{F}_{net} = \frac{d\vec{p}}{dt}$
    * Integrating both sides with respect to time: $\int_{t_i}^{t_f} \vec{F}_{net} dt = \int_{t_i}^{t_f} \frac{d\vec{p}}{dt} dt$
    * $\vec{J}_{net} = \vec{p}_f - \vec{p}_i = \Delta \vec{p}$
  * **Impulse-momentum theorem:** The net impulse acting on an object is equal to the change in its momentum.
## Example: Kicking a Ball
  
  **Problem:** A soccer ball of mass $m$ is moving with speed $v_i$ in the positive x-direction. After being kicked by the player's foot, it moves with speed $v_f$ at an angle $\theta$ with respect to the *negative* x-axis. Calculate the impulse delivered to the ball by the player.
  
  **(Diagram showing the initial and final velocities of the ball.)**
  
  **(Solution will be shown in the lecture, using the impulse-momentum theorem and vector components.)**
## System of Particles
  
  * **Total momentum:** The total linear momentum ($\vec{P}$) of a system of particles is the vector sum of the individual momenta of each particle:
    * $\vec{P} = \sum_n \vec{p}_n = \sum_n m_n \vec{v}_n$
  * **Newton's 2nd Law for a system:**
    * $\vec{F}_{net} = \sum \vec{F} = \frac{d\vec{P}}{dt}$
    * The net external force acting on a system of particles is equal to the rate of change of the total momentum of the system.
## Internal and External Forces
  
  * **Internal forces:** Forces between particles *within* the system.
  * **External forces:** Forces exerted on particles in the system by objects *outside* the system.
  * **Action-reaction pairs:** Internal forces always occur in action-reaction pairs, and therefore they cancel each other out when considering the net force on the *system*.
  * **Net external force:** Only the external forces contribute to the change in the total momentum of the system:
    * $\sum \vec{F}_{ext} = \frac{d\vec{P}}{dt}$
    * $\vec{J}_{net_{ext}} = \vec{P}_f - \vec{P}_i = \Delta \vec{P}$
  
  
  **(Diagram illustrating internal and external forces on a system of particles.)**
## Conservation of Linear Momentum
  
  * **No external forces:** If no external forces act on a system of particles ($\sum \vec{F}_{ext} = 0$), then the total linear momentum of the system is conserved:
    * $\frac{d\vec{P}}{dt} = 0$
    * $\vec{P}_f = \vec{P}_i$
  * **Component-wise conservation:**  If momentum is conserved, then each component of the momentum is also conserved:
    * $P_{fx} = P_{ix}$
    * $P_{fy} = P_{iy}$
    * etc.
  * **Example:** Explosions (internal forces cause a change in the momenta of individual fragments, but the total momentum of the system remains constant).
## Example: Explosion
  
  **Problem:** A firecracker of mass $M$ is traveling with speed $V$ in the positive x-direction. It explodes into two fragments of equal mass. Fragment A moves away at an angle $\theta$ above the positive x-axis. Fragment B moves along the negative y-axis. Find the speeds of the fragments.
  
  **(Diagram showing the firecracker before and after the explosion, including the velocity vectors and angle.)**
  
  **(Solution will be demonstrated in lecture, using conservation of momentum.)**
## Summary of Litany for Momentum Problems
  
  1. **Draw before and after sketch:** Visualize the system before and after the event (collision, explosion, etc.).
  2. **Label masses and draw vectors:** Label the masses of all objects involved and draw momentum/velocity vectors.
  3. **Draw vector components:** Resolve vectors into their x and y components.
  4. **Starting equation:** Write down the appropriate starting equation (Newton's 2nd Law, impulse-momentum theorem, or conservation of momentum).
  5. **Conservation of momentum (if appropriate):** If no external forces act, apply the conservation of momentum principle.
  6. **Sum initial and final momenta:** Write down the expressions for the total initial and final momenta in each direction.
  7. **Express components:**  Write down the equations for conservation of momentum in each direction using the vector components.
  8. **Solve symbolically:** Solve the equations for the desired quantities.
## Short Collisions
  
  * **Definition:** Collisions that happen in a very short time.
  * **Dominating impulse:**  The forces between colliding objects deliver a large impulse during the short collision time.
  * **Negligible external impulse:** The impulse due to external forces (like friction or gravity) is negligible compared to the impulse due to the collision forces.
  * **Conservation of momentum (approximate):** Therefore, we can approximately consider the total momentum to be conserved during a short collision: $\vec{P}_f \approx \vec{P}_i$.
  
  **Example:** In a car crash, the collision forces between the cars dominate, and the effect of road friction during the collision is negligible. We can determine the momenta right after the collision, before the wrecks skid on the pavement, using conservation of momentum.
## Example: Collision
  
  **Problem:** A truck is moving with velocity $V_0$ along the positive x-direction. It is struck by a car, which had been moving towards it at an angle $\theta$ with respect to the x-axis. As a result of the collision, the car is brought to a stop, and the truck is moving in the negative y-direction. The truck is twice as heavy as the car. Derive an expression for the speed $V_f$ of the truck immediately after the collision.
  
  **(Diagrams showing the collision before and after, with velocity vectors and masses labeled.)**
  
  **(Solution will be demonstrated during the lecture, using conservation of momentum.)**
## Energy in Collisions
  
  * **Momentum conservation:** In a quick collision, the total linear momentum is conserved ($\vec{P}_f = \vec{P}_i$).
  * **Energy conservation (usually not):** Total mechanical energy is *usually not* conserved ($E_f \neq E_i$) due to non-conservative forces like deformation of the colliding objects.
  * **Inelastic collision:** A collision in which mechanical energy is not conserved.
  * **Perfectly inelastic collision:** A special case of an inelastic collision where the objects stick together after the collision.
  * **Elastic collision:** A collision in which mechanical energy *is* conserved.
  
  
  **Demo:** Demonstrating elastic and inelastic 1-D collisions on an air track.
## Fractional Change of Kinetic Energy
  
  * **Formula:** $\frac{\Delta K}{K_i} = \frac{K_f - K_i}{K_i} = \frac{K_f}{K_i} - 1$
  * **Inelastic collisions:**  There is a loss of kinetic energy in inelastic collisions due to deformation, heat generation, sound, etc.
  * **Explosions:** Chemical energy is released and converted into kinetic energy.