## Introduction
  
  This lecture builds upon the concepts of linear momentum and energy conservation, introducing multi-step problems and the concept of center of mass motion. We will cover:
  
  * Multi-step problems involving momentum and energy
  * Elastic collisions
  * Center of mass motion
  * Rocket propulsion
## Momentum and Energy in Multi-Step Problems
  
  * **Quick collisions:**
    * Total linear momentum is conserved: $\vec{P}_f = \vec{P}_i$
    * Total mechanical energy is usually *not* conserved: $E_f \neq E_i$ (due to non-conservative forces like deformation)
  * **Before or after collision:** Mechanical energy may be conserved in the stages before or after a collision, or the change in mechanical energy can be determined by considering the work done by other forces (e.g., friction, external forces):
    * $E_f - E_i = W_{other}$
## Example: Ballistic Pendulum
  
  **Problem:** A bullet of mass $m$ and unknown speed is fired into a block of mass $M$ that is hanging from two cords. The bullet gets stuck in the block, and the block rises to a height $h$. What was the initial speed of the bullet?
  
  **(Diagram of the ballistic pendulum before and after the collision.)**
  
  **(This is a classic two-step problem.  The solution involves:**
  1. **Momentum conservation during the collision:** The total momentum of the bullet and block is conserved during the very short collision time.  Since the block is initially at rest, we have $mv_i = (m+M)v_f$, where $v_i$ is the initial speed of the bullet and $v_f$ is the speed of the combined bullet and block immediately after the collision.
  2. **Energy conservation after the collision:** Mechanical energy is conserved after the collision as the block swings upward (since we neglect air resistance and any friction at the pivot). The kinetic energy of the bullet+block system immediately after the collision is converted into gravitational potential energy at height $h$: $\frac{1}{2}(m+M)v_f^2 = (m+M)gh$.
  
  **By combining these two equations, we can solve for $v_i$.)**
## Energy in Collisions (Review)
  
  * **Inelastic collision:** Total mechanical energy is not conserved.
  * **Perfectly inelastic collision:** Objects stick together after the collision.
  * **Elastic collision:** Total mechanical energy is conserved.
## Elastic Collisions
  
  * **Conservation of mechanical energy:** In an elastic collision, only conservative forces act during the collision, so mechanical energy is conserved: $E_f = E_i$.
  * **Conservation of linear momentum:** Linear momentum is also conserved in an elastic collision: $\vec{P}_f = \vec{P}_i$.
## Example: Elastic Head-on Collision with Stationary Target
  
  **Problem:** Consider a one-dimensional elastic collision where mass $m_1$ with initial velocity $v_1$ collides with a stationary mass $m_2$ ($v_2 = 0$). Find the final velocities of both masses.
  
  **(Diagram showing the collision before and after, with velocities labeled.)**
  
  * **x-component of momentum conservation:** $m_1v_1 = m_1v_{f1x} + m_2v_{f2x}$
  * **Energy conservation:** $\frac{1}{2}m_1v_1^2 = \frac{1}{2}m_1v_{f1x}^2 + \frac{1}{2}m_2v_{f2x}^2$
  
  **(After some algebra, we find):**
  
  * $v_{f1x} = \frac{m_1 - m_2}{m_1 + m_2}v_1$
  * $v_{f2x} = \frac{2m_1}{m_1 + m_2}v_1$
## Special Cases of Elastic Collisions
  
  * **$m_1 << m_2$ (Ping-pong ball hits stationary cannonball):**  $v_{f1x} \approx -v_1$, $v_{f2x} \approx 0$ (The ping-pong ball bounces back with almost the same speed, and the cannonball barely moves.)
  * **$m_1 >> m_2$ (Cannonball hits stationary ping-pong ball):** $v_{f1x} \approx v_1$, $v_{f2x} \approx 2v_1$ (The cannonball continues with almost the same speed, and the ping-pong ball is launched forward with twice the speed of the cannonball.)
  * **$m_1 = m_2$ (Newton's cradle):** $v_{f1x} = 0$, $v_{f2x} = v_1$ (The first mass comes to rest, and the second mass moves with the initial velocity of the first mass.  This is the principle behind Newton's cradle.)
## General Elastic Collisions
  
  * **Two-dimensional, off-center collisions:** In more general cases, the objects may move away at angles after the collision.
  * **Three equations:**  For a two-dimensional elastic collision, we have three equations:
    * x-component of momentum conservation: $P_{fx} = P_{ix}$
    * y-component of momentum conservation: $P_{fy} = P_{iy}$
    * Conservation of mechanical energy: $E_f = E_i$
## Center of Mass: Definition
  
  * **Center of mass ($\vec{r}_{CM}$):** The weighted average position of all the mass in a system.
  * **Formula (for a system of particles):** 
    * $\vec{r}_{CM} = \frac{1}{M_{tot}}\sum_n m_n \vec{r}_n$
    * $x_{CM} = \frac{1}{M_{tot}}\sum_n m_n x_n$
    * $y_{CM} = \frac{1}{M_{tot}}\sum_n m_n y_n$
    * where $M_{tot}$ is the total mass of the system.
  * **Continuous object:** For a continuous object, the sum becomes an integral.
  * **Symmetry:** If an object has a line of symmetry, the center of mass lies on that line.
## Center of Mass and Momentum
  
  * **Total momentum and center of mass velocity:**  The total momentum of a system is related to the velocity of the center of mass:
    * $\vec{P} = M_{tot} \vec{v}_{CM}$
  * **Newton's 2nd Law for center of mass:**
    * $M_{tot}\vec{a}_{CM} = \frac{d\vec{P}}{dt} = \sum \vec{F}$
## Center of Mass and External Forces
  
  * **Internal forces cancel:** As seen before, internal forces cancel out in action-reaction pairs.
  * **Center of mass motion:** Only external forces affect the motion of the center of mass:
   $\sum \vec{F}_{ext} = M_{tot}\vec{a}_{CM}$
  
  
  **Demo:** Demonstrating center-of-mass motion.
## Discussion Question
  
  **(Refer to the truck and car collision example from Lecture 17)**
  
  Find the x-component of the velocity of the center of mass of the truck and car *before* the collision.
  
  **(Solution will be discussed in the lecture.)**
## Another Discussion Question
  
  You find yourself in the middle of a frictionless frozen lake. How do you get to the shore?
  
  **Answer:** Throw something away from the shore. By conservation of momentum, you will recoil in the opposite direction.  This is the same principle behind rocket propulsion.
  
  
  **Demo:** Demonstrating rocket motion with a rocket cart.