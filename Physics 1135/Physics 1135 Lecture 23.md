## Introduction
  
  This lecture introduces the concept of angular momentum, the rotational analog of linear momentum. We will cover:
  
  * Angular momentum of a point mass
  * Angular momentum of a rigid rotating object
  * Conservation of angular momentum
## Translation vs. Rotation
  
  * **Linear momentum ($\vec{p}$):** Fundamental quantity for translation.  Forces change linear momentum.
  * **Angular momentum ($\vec{L}$):** Fundamental quantity for rotation. Torques change angular momentum.
  * **Definition of angular momentum:** $\vec{L} = \vec{r} \times \vec{p} = \vec{r} \times m\vec{v}$
    * $\vec{r}$: Position vector from the reference point to the object.
    * $\vec{p}$: Linear momentum of the object.
## Angular Momentum of a Particle
  
  * **Formula:** $\vec{L} = \vec{r} \times \vec{p} = \vec{r} \times m\vec{v}$
  * **Magnitude:**  $L = r_\perp mv = rmv_\perp = rmv\sin\theta$
    * $r_\perp$: The component of $\vec{r}$ perpendicular to $\vec{v}$ (sometimes called the *moment arm* for angular momentum).
    * $v_\perp$: The component of $\vec{v}$ perpendicular to $\vec{r}$.
    * $\theta$: Angle between $\vec{r}$ and $\vec{v}$.
  * **Direction:** Determined by the right-hand rule:
    * Thumb: $\vec{r}$
    * Index finger: $\vec{p}$ (or $\vec{v}$)
    * Middle finger: $\vec{L}$
  * **Geometrically:**  The direction of $\vec{L}$ is perpendicular to the plane formed by $\vec{r}$ and $\vec{p}$ (or $\vec{v}$).
  
  
  **(Diagram illustrating the angular momentum of a particle)**
## Angular Momentum of a Rigid Object
  
  * **System of particles:** For a system of particles rotating about a common axis:
    * $\vec{L} = \sum_n \vec{L}_n = \sum_n \vec{r}_n \times \vec{p}_n$
  
  * **Rigid object rotating about z-axis:**
    * $\sum_n L_n = \sum_n r_{n\perp} m_n v_n = \sum_n r_n m_n (\omega r_n) = \omega \sum_n m_n r_n^2$
    * $L_z = I\omega$  (where $I = \sum_n m_n r_n^2$ is the moment of inertia about the z-axis)
  
  
  * **Direction of $\vec{L}$:** The total angular momentum $\vec{L}$ is in the same direction as the angular velocity $\vec{\omega}$ *only* for rotations about a symmetry axis.  This is not generally true for rotations about other axes.
  
  **(Diagram of a rotating rigid object)**
## Angular Momentum Conservation
  
  * **Newton's 2nd Law for rotation (single object):** $\sum \tau_z = \frac{dL_z}{dt} = \frac{d(I\omega)}{dt} = I\alpha_z$
  * **Newton's 2nd Law for rotation (system):**  $\sum \vec{\tau} = \sum \vec{\tau}_{ext} = \frac{d\vec{L}}{dt}$ (internal torques cancel due to action-reaction pairs).
  * **Conservation of angular momentum:** If the net external torque acting on a system is zero ($\sum \vec{\tau}_{ext} = 0$), then the total angular momentum of the system is conserved:
    * $\frac{d\vec{L}}{dt} = 0$
    * $\vec{L}_i = \vec{L}_f$
  
  * **Comparison to linear momentum:**  This is analogous to the conservation of linear momentum: If $\sum \vec{F}_{ext,x} = 0$, then $P_{ix} = P_{fx}$.
## Demonstrations
  
  * **Changing moment of inertia:** Demonstrations of how changing the distribution of mass (and therefore the moment of inertia) affects the angular velocity for a fixed angular momentum. Examples:
    * Person on a rotating platform bringing weights closer to their body (decreasing $I$, increasing $\omega$).
    * Person on a rotating stool holding a spinning bicycle wheel and flipping the wheel's orientation (changing $\vec{L}$, causing the stool to rotate).
  
  
  **(Diagrams and equations illustrating the demonstrations.)**  $L_i = I_i \omega_i = I_f \omega_f = L_f$
## Example 1: Ball Striking a Door
  
  **Problem:** A ball of mass $m$ and speed $V$ strikes a door at an angle $\theta$ and bounces off at a right angle with 1/4 its original speed. What is the final angular speed of the door after the collision?
  
  **(Diagram showing the ball striking the door and the hinge.)**
  
  **(Solution will be discussed during the lecture, using conservation of angular momentum about the hinge.)**
## Example 2: Putty Sticking to a Door
  
  **Problem:**  A ball (from the previous example) is made of putty and sticks to the door after the collision. What is the final angular speed of the door with the ball stuck on?
  
  **(Diagram showing the putty sticking to the door.)**
  
  **(Solution will be discussed during the lecture, again using conservation of angular momentum.)**
## Example 3: Merry-Go-Round and Child
  
  **Problem:** A merry-go-round (solid disk of mass $M$ and radius $R$) is rotating on frictionless bearings about a vertical axis through its center. It rotates clockwise with angular speed $\omega$. A child of mass ½$M$ is initially sitting at the outer edge of the merry-go-round. When the child jumps off tangentially to the circumference, the merry-go-round reverses its rotation and now rotates with the same angular speed $\omega$ in the opposite direction (counterclockwise). Derive an expression for the speed (relative to the ground) with which the child jumps off.
  
  **(Diagrams showing the merry-go-round and child before and after the jump.)**
  
  
  **(Solution will be derived during the lecture, using conservation of angular momentum about the axis of rotation.)**
## Kepler's 2nd Law Revisited
  
  Kepler's second law (equal areas swept out in equal times) can be explained using conservation of angular momentum.
  
  * **Torque due to gravity:** The torque on a planet due to the Sun's gravity is zero because the force is always directed along the line connecting the Sun and the planet.
  * **Conservation of angular momentum:** This means that the planet's angular momentum is constant: $\vec{L} = \vec{r} \times m\vec{v} = \text{constant}$.
  * **Relating to area swept out:** The magnitude of angular momentum is  $L = rmv\sin\theta = rmv_{\perp} = r^2m\omega$. The rate of change of area swept out by the planet's position vector is $\frac{dA}{dt} = \frac{1}{2}r^2\omega = \frac{L}{2m}$. Since $L$ and $m$ are constant, $\frac{dA}{dt}$ is constant, which means equal areas are swept out in equal times.
  
  **(Diagram illustrating Kepler's 2nd law and the relationship to angular momentum)**