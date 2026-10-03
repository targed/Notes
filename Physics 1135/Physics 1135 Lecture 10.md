# Lecture 10: Work and Kinetic Energy
## Introduction
  
  This lecture introduces the concepts of work and kinetic energy and their relationship through the work-kinetic energy theorem. We will cover:
  
  * Kinetic energy
  * Vector dot product (scalar product)
  * Definition of work done by a force on an object
  * Work-kinetic energy theorem
## Kinetic Energy
  
  * **Definition:** Kinetic energy ($K$) is the energy of motion. It is a scalar quantity, meaning it has magnitude but no direction.
  * **Formula:** $K = \frac{1}{2}mv^2$
    * $m$: Mass of the object
    * $v$: Speed of the object
  * **Units:** Joule (J) =  kg m<sup>2</sup>/s<sup>2</sup> = N m
  
  **Note:** Kinetic energy does not have components because it is a scalar quantity.
## Effect of Force Components (Review)
  
  * **Force parallel to velocity ($F_{||}$):** Changes the magnitude of the velocity vector (speed) and therefore changes the kinetic energy of the object.
  * **Force perpendicular to velocity ($F_{\perp}$):** Changes only the direction of the velocity vector, but **does not** change the kinetic energy.
## Dot Product (Scalar Product)
  
  * **Motivation:** To determine the work done by a force, we need a way to quantify how much of a force acts parallel to the displacement of an object. This is where the dot product comes in.
  * **Definition:** The dot product of two vectors $\vec{A}$ and $\vec{B}$ is:
    * $\vec{A} \cdot \vec{B} = AB \cos \theta$
    * where $A$ and $B$ are the magnitudes of the vectors, and $\theta$ is the angle between them.
  * **Result:** The dot product is a **scalar** quantity.
  * **Geometric Interpretation:**
    * $\vec{A} \cdot \vec{B} = A_{||B} B = A B_{||A}$
    * The dot product selects the component of one vector that is parallel to the other vector.
## Dot Product: Special Cases
  
  * **Parallel vectors ($\theta = 0^\circ$):**  $\vec{A} \cdot \vec{B} = AB \cos 0^\circ = AB$
  * **Anti-parallel vectors ($\theta = 180^\circ$):** $\vec{A} \cdot \vec{B} = AB \cos 180^\circ = -AB$
  * **Perpendicular vectors ($\theta = 90^\circ$):**  $\vec{A} \cdot \vec{B} = AB \cos 90^\circ = 0$
## Properties of the Dot Product
  
  * **Commutative:**  $\vec{A} \cdot \vec{B} = \vec{B} \cdot \vec{A}$
  * **Distributive:** $\vec{A} \cdot (\vec{B} + \vec{C}) = \vec{A} \cdot \vec{B} + \vec{A} \cdot \vec{C}$
  * **Same unit vectors:** $\hat{i} \cdot \hat{i} = \hat{j} \cdot \hat{j} = \hat{k} \cdot \hat{k} = 1$
  * **Orthogonal unit vectors:** $\hat{i} \cdot \hat{j} = \hat{i} \cdot \hat{k} = \hat{j} \cdot \hat{k} = 0$
  * **Dot product in components:**  $\vec{A} \cdot \vec{B} = A_x B_x + A_y B_y + A_z B_z$
## Work
  
  * **Definition:** Work ($W$) is done on an object by a force $\vec{F}$ as the object moves along a path $\vec{r}(t)$ from an initial position to a final position. 
  * **Formula (General Case):** 
    *  $W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F}(\vec{r}) \cdot d\vec{r}$ 
    * This integral calculates the work done by a force that can vary in both magnitude and direction along the path.
  * **Key Points:**
    * Work is a scalar quantity.
    * The dot product selects the component of the force parallel to the path (i.e., parallel to the velocity), which is responsible for changing the kinetic energy.
    * The force can vary in both magnitude and direction along the path.
## Work by a Constant Force
  
  * **Simplified Formula:** If the force is constant in both magnitude and direction, the work formula simplifies to:
    * $W = \vec{F} \cdot \vec{D}$ 
    * where $\vec{D} = \vec{r}_f - \vec{r}_i$ is the total displacement vector.
  * **Caution:** This simplified formula only applies when the force is constant in both magnitude and direction.
## Force Perpendicular to Path
  
  * **Zero work:** If the force is perpendicular to the path at every point, then the work done by the force is zero.
    * $W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F}(\vec{r}) \cdot d\vec{r} = \int_{\vec{r}_i}^{\vec{r}_f} F(r) \cos 90^\circ dr = 0$
## Work Done by a Spring
  
  * **Spring force:** The force exerted by a spring is given by Hooke's law: 
    * $F_s = -kx$ 
    * where $k$ is the spring constant, and $x$ is the displacement from the equilibrium position (stretch or compression).
  * **Work done by a spring:**
    * $W_s = \int_{x_i}^{x_f} \vec{F}_s \cdot d\vec{x} = \int_{x_i}^{x_f} -kx dx = -\frac{1}{2}k(x_f^2 - x_i^2)$
## Net Work
  
  * **Net work ($W_{net}$):**  The total work done on an object by all the forces acting on it.
  * **Two ways to calculate:**
    1. **Work done by the net force:** Calculate the work done by the net force on the object as it moves along the path.
    2. **Sum of individual works (easier):**  $W_{net} = \sum W_n$ (the sum of the work done by all the individual forces)
## Work-Kinetic Energy Theorem
  
  * **Statement:** The net work done on an object is equal to the change in its kinetic energy.
  * **Formula:** $(W_{net})_{i \to f} = \Delta K = K_f - K_i = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2$
## Sign of Work
  
  * **Positive work:**  If the force has a component in the direction of displacement, the work done is positive, and the kinetic energy increases ($\Delta K > 0$, $v_f > v_i$). The object speeds up.
  * **Negative work:** If the force has a component opposite to the direction of displacement, the work done is negative, and the kinetic energy decreases ($\Delta K < 0$, $v_f < v_i$). The object slows down.
## Example Problem
  
  **Problem:** A block of mass $M$ is pulled by a force of magnitude $P$ directed at an angle $\theta$ above the horizontal a distance $D$ over a rough horizontal surface with a coefficient of friction $\mu$. Determine the change in the block's kinetic energy.
  
  **(Diagram provided showing the block, force, angle, distance, and coefficient of friction.)**
  
  **(Solution will be derived during the lecture, using the work-kinetic energy theorem and calculating the work done by each individual force.)**