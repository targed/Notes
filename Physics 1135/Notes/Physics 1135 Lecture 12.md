## Introduction
  
  This lecture explores potential energy diagrams, which provide a visual representation of the relationship between potential energy, force, and motion. We will cover:
  
  * Problems involving work done by "other" forces (non-conservative forces)
  * The relationship between force and potential energy
  * Potential energy diagrams and their interpretation
## Example with Other (Non-Conservative) Force
  
  **Problem:** A block of mass $M$ is at rest on an incline that makes an angle $\theta$ with the horizontal. It slides down a distance $L$ and then flies off the edge, which is a height $H$ above the ground. Throughout its motion, a constant vertical blowing force of magnitude $B$ is acting on the block.
  
  **(Diagram provided showing the incline, block, angle, distance $L$, height $H$, blowing force $B$, and final velocity $v_f$)**
  
  **Task:** Derive an expression for the speed with which the block hits the ground.
  
  **(Solution will be shown in lecture, emphasizing the inclusion of work done by the non-conservative blowing force in the energy conservation equation.)**
## Relationship Between Force and Potential Energy
  
  * **Potential energy difference and work:** Recall from Lecture 11 that the potential energy difference between two points is defined as the negative of the work done by a conservative force: 
    * $U(\vec{r}_B) - U(\vec{r}_A) = -W_{A \to B} = -\int_{\vec{r}_A}^{\vec{r}_B} \vec{F} \cdot d\vec{r}$
  
  * **Force in one dimension:** In one dimension, if $\vec{F} = F_x(x) \hat{i}$, then the potential energy difference is:
    * $\Delta U = -\int F_x dx$
    * and the force is the negative derivative of the potential energy: $F_x = -\frac{dU(x)}{dx}$
  
  * **Force in three dimensions:** If $U = U(x, y, z)$, then the force components are given by the partial derivatives of the potential energy:
    * $F_x = -\frac{\partial U(x, y, z)}{\partial x}$
    * $F_y = -\frac{\partial U(x, y, z)}{\partial y}$
    * $F_z = -\frac{\partial U(x, y, z)}{\partial z}$
  
  **Note on partial derivatives:**  The partial derivative $\frac{\partial}{\partial x}$ means that we treat $y$ and $z$ as constants and differentiate only with respect to $x$.
## Motion in a Potential Energy Well
  
  * **Scenario:** Consider motion in one dimension under the influence of a single conservative force with potential energy $U(x)$. Since $W_{other} = 0$, we have $E_f = E_i$, meaning the total mechanical energy is conserved.
  
  **(Diagram of a potential energy well with a horizontal line representing the total energy $E$)**
## Kinetic and Potential Energy in a Potential Energy Well
  
  * **Total energy:**  $E = K(x) + U(x)$
  * **Kinetic energy:** $K(x) = E - U(x)$
  * **Relationship between K and U:**
    * Small $U(x)$ implies large $K(x)$.
    * Large $U(x)$ implies small $K(x)$.
  * **Maximum kinetic energy:**  The kinetic energy is maximum where the potential energy is minimum.
  * **Turning points:** The points where the total energy $E$ intersects the potential energy curve $U(x)$ are called turning points. At these points, $U = E$, and $K = 0$.  The object momentarily stops and changes direction.
## Force and Potential Energy on a Diagram
  
  * **Force as negative slope:**  The force $F_x$ is the negative slope of the potential energy graph: $F_x = -\frac{dU(x)}{dx}$.
  * **Equilibrium points:**  Points where the force is zero ($F_x = 0$) correspond to points where the slope of the potential energy graph is zero (local minima or maxima).
  
  **(Diagram showing the force vectors on a potential energy curve.  Forces point in the direction of decreasing potential energy.)**
## Different Total Mechanical Energies
  
  **(Diagram showing a potential energy curve with two different total energy lines, $E_1$ and $E_2$. $E_2$ is higher and illustrates the concept of a potential barrier.)**
  
  * **Allowed regions of motion:** Motion is only possible where the total energy $E$ is greater than or equal to the potential energy $U(x)$.
  * **Forbidden regions:** Regions where $U(x) > E$ are forbidden because the kinetic energy would be negative, which is not physically possible.
  * **Potential barrier:**  A region of high potential energy that a particle with lower total energy cannot cross.
## Equilibrium
  
  * **Definition of equilibrium:** A point where the net force is zero: $F_x = 0$.
  * **Potential energy at equilibrium:** Equilibrium points correspond to local minima or maxima of the potential energy curve: $F_x = -\frac{dU(x)}{dx} = 0$.
  * **Types of equilibrium:**
    * **Stable equilibrium:**  A local minimum of $U(x)$. If the object is slightly displaced from this point, it will tend to return to it.
    * **Unstable equilibrium:**  A local maximum of $U(x)$. If the object is slightly displaced from this point, it will move further away from it.
## Example: Diatomic Molecule
  
  **Scenario:** A diatomic molecule consists of two atoms separated by a distance $r$.  
  
  **(Diagrams showing the force and potential energy as a function of the interatomic distance $r$)**
  
  * **Too close:** The force is repulsive, and the potential energy is high.
  * **Too far:** The force is attractive, and the potential energy increases with distance but approaches zero as $r \to \infty$.
  * **Equilibrium distance ($r_{eq}$):**  The distance at which the force is zero and the potential energy is minimum. This is the stable equilibrium separation of the atoms in the molecule.