## Introduction
  
  This lecture reviews key concepts and problem-solving strategies in preparation for Test 3. Example problems will be worked out to illustrate these concepts.
## Test Structure
  
  * **Format:** In-person exam
  * **Time:** 60 minutes
  * **Content:**
    * 5 multiple-choice questions (10 points each)
    * 3 worked-out problems (50 points each, partial credit awarded)
  * **Materials:** Equation sheet stapled to exam package
  * **Test Rooms:** [Link to Test Room Assignments](replace with actual link)
## Concepts: Rotation
  
  * **Relationship between angular and linear quantities:** $v = \omega R$, $a_{tan} = \alpha R$, $a_{rad} = \omega^2 R$.
  * **Rolling without slipping:** $v_{CM} = \omega R$, $a_{CM} = \alpha R$.  The point of contact has zero instantaneous velocity.
  * **Moment of inertia:** $I = \sum m_n r_n^2$. Understanding how mass distribution affects the moment of inertia. Parallel axis theorem: $I_q = I_{CM} + Md^2$.
  * **Rotational kinetic energy:** $K_{rot} = \frac{1}{2}I\omega^2$
  * **Torque:** $\vec{\tau} = \vec{r} \times \vec{F}$.  Understanding how force, lever arm, and angle affect torque.
  * **Angular dynamics:** $\sum \tau_z = I\alpha_z$ (Newton's second law for rotation)
  * **Static equilibrium:** $\sum \vec{F} = 0$ and $\sum \vec{\tau} = 0$.  Choosing a convenient pivot point for torque calculations.
  * **Angular momentum conservation:** If $\sum \vec{\tau}_{ext} = 0$, then $\vec{L}_i = \vec{L}_f$.
## Concepts: Oscillation
  
  * **Simple Harmonic Motion (SHM):**
    * **Position, velocity, and acceleration:**  $x = A\cos(\omega t + \phi)$, $v = -A\omega\sin(\omega t + \phi)$, $a = -A\omega^2\cos(\omega t + \phi)$.
    * **Differential equation:** $\frac{d^2x}{dt^2} = -\omega^2 x$
  
  * **Period of a simple pendulum (small oscillations):**  $T = 2\pi\sqrt{\frac{L}{g}}$
  * **Period of a mass on a spring:** $T = 2\pi\sqrt{\frac{m}{k}}$
  * **Physical pendulum (small oscillations):** $T = 2\pi\sqrt{\frac{I}{mgD}}$
  * **Energetics of oscillations:**  $E = K + U = \frac{1}{2}mv^2 + \frac{1}{2}kx^2$ (for a mass on a spring). Conservation of energy.
## How to Identify Problem Types
  
  * **Static equilibrium:**  Object is at rest or moving with constant velocity (no linear or angular acceleration).  Apply $\sum \vec{F} = 0$ and $\sum \vec{\tau} = 0$.
  * **Dynamics (with external forces/torques):**
    * Use $\sum \vec{F} = m\vec{a}$ and $\sum \vec{\tau} = I\vec{\alpha}$ to find acceleration and angular acceleration.
    * Use energy/work methods to find speed if appropriate (especially if forces/torques are not constant).
    * Use kinematics equations *only* if forces/torques are constant.
  * **Rotational collisions/impulses (no external torques):**  Angular momentum is conserved ($\vec{L}_i = \vec{L}_f$). Mechanical energy may change.
## Energy Problems
  
  * **Identify motion:**
    * **Only translating:** $K = K_{trans} = \frac{1}{2}Mv^2$
    * **Only rotating:** $K = K_{rot} = \frac{1}{2}I\omega^2$
    * **Both rotating and translating:** $K = K_{trans} + K_{rot} = \frac{1}{2}Mv^2 + \frac{1}{2}I\omega^2$
  * **No slipping:** Relate linear and angular velocity: $v = \omega R$
  * **Identify other energies:** Gravitational potential energy, spring potential energy, etc.
  
  **(Diagrams illustrating different types of motion: ball rolling down an incline, box suspended from a pulley, cylinder rolling on a surface connected to a hanging box.)**
## Example 1: Rolling Pumpkin
  
  **Problem:** You have a pumpkin of mass $M$ and radius $R$. The pumpkin is not uniform inside, so you do not know its moment of inertia. To determine the moment of inertia, you roll the pumpkin down an incline that makes an angle $\theta$ with the horizontal. The pumpkin starts from rest and rolls without slipping. When it has descended a vertical height $H$, it has acquired a speed $V = \sqrt{\frac{5}{4}gH}$. Use energy methods to derive an expression for the moment of inertia of the pumpkin.
  
  **(Solution will be derived in lecture using conservation of energy and the relationship between linear and angular velocity for rolling without slipping.)**
## Forces and Torques (Review)
  
  * **Extended free-body diagram:**  Show forces and where they act on the object.
  * **Equations of motion:**
    * Rotation: $\sum \tau_z = I\alpha_z$
    * Translation: $\sum \vec{F} = m\vec{a}$
    * Combined motion: Use both equations.
  * **No slipping:**  Relate angular and linear acceleration: $a = \alpha R$.
## Example 2: Yo-yo
  
  **Problem:** A yo-yo shaped device (moment of inertia about center is $I$) is mounted on a horizontal frictionless axle through its center and used to lift a load of mass $M$. The outer radius of the device is $R$, the radius of the hub is $r$. A constant horizontal force of magnitude $P$ is applied to a rope wrapped around the outside of the device. The box, which is suspended from a rope wrapped around the hub, accelerates upwards. The ropes do not slip. Derive an expression for the acceleration of the box.
  
  **(Diagram of the yo-yo.)**
  
  **(Solution will be derived during the lecture using Newton's 2nd Law for rotation and translation.)**
## Example 3: Pumpkin Throwing Contest
  
  **Problem:** In a pumpkin throwing contest, a small pumpkin of mass $m$ is moving horizontally with speed $v$ when it hits a vertical pole of length $H$ and mass $M$ that is pivoted at a hinge at its foot. The pumpkin hits the pole a distance $d$ from its upper end and becomes impaled on a long nail sticking out of the pole. The pumpkin is small enough to be treated as a point mass. Derive an expression for the angular speed of the system after the collision.
  
  **(Diagram of the pumpkin and pole.)**
  
  **(Solution will be derived in lecture using conservation of angular momentum.)**
## A Statics Example
  
  **Problem:** A box of weight ½$W$ hangs from the top end of a uniform post that is pivoted on the ground at an angle $\theta$ with respect to the vertical. A horizontal rope is tied to the post a quarter of the way from the top end. The length of the post is $L$ and its weight is $W$. The tension in the horizontal rope is $2W$. Derive an expression for angle $\theta$ in terms of system parameters. Simplify your answer.
  
  **(Diagram of the post, box, and rope.)**
  
  **(Solution will be derived in lecture using static equilibrium conditions:  $\sum \vec{F} = 0$ and $\sum \vec{\tau} = 0$.)**