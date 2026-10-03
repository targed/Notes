# Lecture 11: Potential Energy
## Introduction
  
  This lecture introduces the concept of potential energy, which is associated with conservative forces. We will cover:
  
  * Conservative and non-conservative forces
  * Potential energy
  * Total mechanical energy
  * Energy conservation
## Conservative Forces
  
  * **Definition:** A force is called **conservative** if the work it does on an object as the object moves between two points is **independent of the path** taken.
  * **Key property:** The work done by a conservative force along any two paths between the same two points is the **same**.
  * **Mathematical representation:** $W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F}(\vec{r}) \cdot d\vec{r}$  (the work done depends only on the initial and final positions, not on the specific path).
## Example: Work Done by Gravity
  
  * **Work done by gravity:** $W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F}_{grav} \cdot d\vec{r} = -mg \hat{j} \cdot (\vec{r}_f - \vec{r}_i) = -mg(y_f - y_i)$
  * **Path independence:**  The work done by gravity depends only on the initial and final vertical positions ($y_i$ and $y_f$), not on the specific path taken.
  * **Conservative force:** Therefore, gravity is a conservative force.
## Properties of Conservative Forces
  
  * **Reverse path, negative work:** If the path is reversed, the work done by a conservative force changes sign: $W_{A \to B} = - W_{B \to A}$
  * **Closed path, zero work:** The work done by a conservative force over a closed path (returning to the starting point) is zero: $W_{A \to A} = \oint \vec{F} \cdot d\vec{r} = 0$
## Constant Forces are Conservative
  
  * **Work done by a constant force:** $W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F} \cdot d\vec{r} = \vec{F} \cdot \int_{\vec{r}_i}^{\vec{r}_f} d\vec{r} = \vec{F} \cdot (\vec{r}_f - \vec{r}_i) = \vec{F} \cdot \vec{D}$
  * **Path independence:** The work done by a constant force depends only on the initial and final positions, not on the path taken. Therefore, constant forces are conservative.
  * **Caution:**
    1.  **Constant in magnitude and direction:** For a force to be conservative, it must be constant in both magnitude and direction.
    2. **Not all conservative forces are constant:** Some conservative forces, like the spring force, are not constant.
## Non-Conservative Forces
  
  * **Path dependence:** If the work done by a force depends on the path taken between two points, the force is **non-conservative**.
  * **Examples:** Friction is a common example of a non-conservative force. The work done by friction depends on the length of the path taken. 
  * **Properties:**
    * **Different paths, different work:** Different paths between the same initial and final points result in different amounts of work.
    * **Closed path, non-zero work:** The work done by a non-conservative force over a closed path is generally not zero.
## Potential Energy Difference: Definition
  
  * **Work and position:** The work done by a conservative force depends only on the initial and final positions, not on the path. This means that each pair of points has a unique value of work associated with it.
  * **Definition of potential energy difference:**  The **difference in potential energy** of a conservative force $\vec{F}$ between positions $\vec{r}_A$ and $\vec{r}_B$ is defined as:
    * $\Delta U_{A \to B} = U(\vec{r}_B) - U(\vec{r}_A) = -W_{A \to B} = -\int_{\vec{r}_A}^{\vec{r}_B} \vec{F} \cdot d\vec{r}$
## Potential Energy: Reference Point
  
  * **Meaningful differences:** Only **differences** in potential energy are meaningful. The absolute value of potential energy at a single point is arbitrary.
  * **Choosing a reference point:**  We choose an arbitrary reference point $\vec{r}_0$ and assign it a value of potential energy $U_0$ that is convenient (often zero).
  * **Potential energy at a point:**  The potential energy at a point $\vec{r}$ is then defined as:
    * $U(\vec{r}) - U(\vec{r}_0) = -W_{r_0 \to r}$
    * $U(\vec{r}) = U(\vec{r}_0) - W_{r_0 \to r}$
## Potential Energy of Gravity
  
  * **Near Earth's surface:** The potential energy of gravity near Earth's surface is given by:
    * $U_{grav}(\vec{r}) - U_{grav}(\vec{r}_0) = -W_{grav_{r_0 \to r}} = -[-mg(y - y_0)] = mg(y - y_0)$
  * **Choosing the reference point:** We typically choose the ground level ($y_0 = 0$) as the reference point and assign it a potential energy of zero ($U_{grav}(y_0) = 0$).
  * **Potential energy of gravity (with y-axis up):** $U_{grav}(\vec{r}) = mgy$
## Potential Energy of a Spring Force
  
  * **From Lecture 10:** Recall that the work done by a spring force is $W_s = \frac{1}{2}k(x_i^2 - x_f^2)$.
  * **Potential energy difference:** $\Delta U_s = -W_s = -\frac{1}{2}k(x_i^2 - x_f^2) = \frac{1}{2}k(x_f^2 - x_i^2)$
  * **Choosing the reference point:** We choose the equilibrium position ($x = 0$) as the reference point and assign it a potential energy of zero ($U_s(x = 0) = 0$).
  * **Potential energy of a spring:**  $U_{spring} = \frac{1}{2}kx^2$
## Total Mechanical Energy
  
  * **Definition:** The total mechanical energy ($E$) of a system is the sum of its kinetic energy ($K$) and potential energy ($U$):
    * $E = K + U$ 
  * **Change in mechanical energy:** The change in total mechanical energy is equal to the work done by non-conservative forces:
    * $\Delta K = W_{net} = W_{conservative} + W_{other}$
    * $K_f - K_i + ( - W_{cons}) = W_{other}$
    * $K_f - K_i + (U_f - U_i) = W_{other}$
    * $(K_f + U_f) - (K_i + U_i) = W_{other}$
    * $E_f - E_i = W_{other}$
## Energy Conservation
  
  * **Conservative forces only:** If only conservative forces act on a system, the work done by other forces is zero ($W_{other} = 0$), and the total mechanical energy is conserved:
    * $E_f = E_i$
  * **Conservation principle:** This is a fundamental principle in physics known as the **conservation of mechanical energy**.
## Example: Ski Jumper
  
  **Problem:** In a new Olympic discipline, a ski jumper of mass $M$ is launched by means of a compressed spring of spring constant $k$. At the top of a frictionless ski jump at height $H$ above the ground, he is pushed against the spring, compressing it a distance $L$. When he is released from rest, the spring pushes him so he leaves the lower end of the ski jump with a speed $V$ at a positive angle $\theta$ with respect to the horizontal.
  
  **(Diagram provided showing the ski jump, spring, initial height $H$, compression distance $L$, launch speed $V$, angle $\theta$, and final height $D$.)**
  
  **Task:** Determine the height $D$ of the end of the ski jump in terms of the given system parameters.
  
  **(Solution will be derived during the lecture, using energy conservation.)**
## Tension in Coupled Objects
  
  * **Net work by tension:** The net work done by tension in a coupled system (objects connected by ropes or strings) is zero.
  * **Reason:**  The tension forces on the connected objects have equal magnitude but act in opposite directions, and the displacements of the objects are also equal in magnitude but opposite in direction.
## Example with Coupled Objects
  
  **Problem:** A block of mass $m$ is on a frictionless incline that makes an angle $\theta$ with the vertical. A light string attaches it to another block of mass $M$ that hangs over a massless frictionless pulley. The blocks are then released from rest, and the block of mass $M$ descends. 
  
  **(Diagram provided showing the incline, blocks, angle, and pulley.)**
  
  **Task:** What is the blocks' speed after they move a distance $D$?
  
  **(Solution will be derived during the lecture, using energy conservation and considering the work done by gravity on each block.)**