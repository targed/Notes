## Introduction
  
  This lecture introduces the concept of torque, which is the rotational analog of force.  We will cover:
  
  * Cross product (mathematical tool for calculating torque)
  * Torque definition and properties
  * Relationship between torque and angular acceleration
  * Problem-solving involving torque
## What Causes Rotation?
  
  * **Demo:** Bolt and wrench demonstration.  Illustrates how the same force applied at different distances from the pivot point (or at different angles) produces different rotational effects.
  * **Requirements for rotation:**
    * Force
    * Distance from the pivot point (moment arm or lever arm)
    * Perpendicular component of the force (only the component of force perpendicular to the lever arm causes rotation)
## Vector Cross Product: Magnitude
  
  * **Definition:** The vector cross product, denoted by $\vec{A} \times \vec{B} = \vec{C}$, is a vector operation that produces a vector $\vec{C}$ perpendicular to both $\vec{A}$ and $\vec{B}$.
  * **Magnitude:** The magnitude of the cross product is given by:
    * $C = AB\sin\theta = A_{\perp B}B = AB_{\perp A}$
    * where $\theta$ is the angle between $\vec{A}$ and $\vec{B}$, and $A_{\perp B}$ ($B_{\perp A}$) is the component of $\vec{A}$ ($\vec{B}$) perpendicular to $\vec{B}$ ($\vec{A}$).
  
  **(Diagram illustrating the components perpendicular to each vector.)**
## Vector Cross Product: Direction
  
  * **Direction:** The direction of the cross product vector $\vec{C}$ is determined by the right-hand rule:
    * Point the fingers of your right hand in the direction of vector $\vec{A}$.
    * Curl your fingers towards vector $\vec{B}$.
    * Your thumb points in the direction of $\vec{C}$.
  
  **(Diagram illustrating the right-hand rule for the cross product)**
## Torque
  
  * **Definition:** Torque ($\vec{\tau}$) is the rotational analog of force. It is the tendency of a force to cause rotation about a specific point or axis.
  * **Formula:** $\vec{\tau} = \vec{r} \times \vec{F}$
    * $\vec{r}$:  The position vector from the pivot point to the point where the force is applied (moment arm vector).
    * $\vec{F}$: The applied force vector.
  * **Magnitude:** $|\vec{\tau}| = rF\sin\theta = rF_\perp = r_\perp F$
    * $r_\perp = r\sin\theta$:  The perpendicular distance from the pivot point to the line of action of the force (moment arm).
    * $F_\perp = F\sin\theta$: The component of the force perpendicular to the moment arm.
  
  
  **(Diagram illustrating torque: moment arm, line of action, and perpendicular component of the force)**
## Direction of Torque
  
  * **Right-hand rule:** The direction of the torque vector is determined by the right-hand rule, similar to the cross product:
    * Point your thumb in the direction of $\vec{r}$.
    * Point your index finger in the direction of $\vec{F}$.
    * Your middle finger points in the direction of $\vec{\tau}$.
  * **Simplified rule:**
    * If the force tends to produce rotation in the positive z-direction (counterclockwise), the torque is positive: $\tau_z = +rF\sin\theta$.
    * If the force tends to produce rotation in the negative z-direction (clockwise), the torque is negative: $\tau_z = -rF\sin\theta$.
  * **Curved arrow notation:** We can use a curved arrow to indicate the direction of rotation and the sign of the torque.
  
  **(Diagram illustrating the direction of torque)**
## Angular Acceleration of a Rigid Object
  
  * **Rigid object:** An object that does not deform when a force is applied.
  * **Rotation about z-axis:** Consider a rigid object that can rotate about the z-axis.  $I_z$ is its moment of inertia about the z-axis.
  * **Newton's 2nd Law for rotation:**  $\sum \tau_z = I_z \alpha_z$ (The net torque about the z-axis is equal to the moment of inertia about the z-axis times the angular acceleration about the z-axis.)
  * **Comparison to linear motion:** This is analogous to Newton's 2nd Law for linear motion: $\sum F_x = ma_x$.
## Problem Solving with Torque
  
  To solve problems involving torque and rotational motion:
  
  1. **Begin with an extended free-body diagram:** Show all forces acting on the object and their points of application. Indicate the distances from the pivot point (moment arms).
  2. **Identify the axis of rotation:**  Choose a positive direction for rotation.
  3. **Calculate individual torques:**  Calculate the torque due to each force, including its sign based on the direction of rotation.
  4. **Apply Newton's 2nd Law for rotation:**  $\sum \tau_z = I_z \alpha_z$.
  5. **Solve for the unknown quantity:** Solve for the angular acceleration or other desired quantity.
## Example 1: Rotating Bar
  
  **Problem:** A uniform bar of length $L$ and mass $M$ can freely rotate about a frictionless horizontal axis O at its end. The bar is initially in a horizontal position, released from rest, and swings down under the influence of gravity. What is the initial angular acceleration of the bar just after it is released from rest?  $I_{bar} = \frac{1}{3}ML^2$ about O.
  
  **(Diagram showing the bar pivoting at point O.)**
  
  **(Solution will be derived during the lecture, using Newton's 2nd Law for rotation and considering the torque due to gravity.)**
## Example 2: Rolling Without Slipping
  
  **Problem:** An object of mass $M$, radius $R$, and moment of inertia $I$ is rolling without slipping down an incline that makes an angle $\theta$ with the horizontal. Derive an expression for the object's linear acceleration.
  
  **(Diagram of the object rolling down the incline.)**
  
  **(Solution will be derived in lecture, using both Newton's 2nd Law for linear motion and Newton's 2nd Law for rotation, as well as the relationship between linear and angular acceleration for rolling without slipping.)**
## Example 3: Coupled Objects with Rotation
  
  **Problem:** A small disk of radius $r$ is glued onto a large disk of radius $R$ that is mounted on a fixed axle through its center. The combined moment of inertia of the disks is $I$. A string is wrapped around the edge of the small disk, and a box of mass $m$ is tied to the end of the string. The string does not slip on the disk.
  
  **(Diagram of the coupled system)**
  
  **Task:** Find the acceleration of the box after it is released from rest.
  
  **(Solution will be derived in the lecture using Newton's second law for both linear and rotational motion and considering the tension in the string and the torque on the disk.)**