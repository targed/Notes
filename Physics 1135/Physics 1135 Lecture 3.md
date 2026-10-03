# Lecture 3: Vectors and 2-D Kinematics
## Introduction
  
  This lecture introduces the concept of **vectors**, which are essential for describing motion in two dimensions (2-D). We will cover:
  
  * Definition of vectors
  * Unit vector notation, components, magnitude, and direction
  * Addition and subtraction of vectors
  * Position, velocity, and acceleration in 2-D
  * Separation of motion in x and y directions
## Vectors
  
  * **Definition:** A vector is a quantity that has both **magnitude** (size) and **direction**. It is often represented graphically by an arrow.
  * **Representation:**
    * The **length** of the arrow represents the magnitude of the vector.
    * The **direction** of the arrow represents the direction of the vector.
  * **Notation:**
    * Vectors are typically denoted by a letter with an arrow above it (e.g., $\vec{A}$) or in boldface (e.g., **A**).
    * The magnitude of a vector is denoted by the letter without the arrow or in absolute value bars (e.g., $A$ or $|\vec{A}|$).
## Unit Vectors
  
  * **Definition:** Unit vectors are vectors with a magnitude of 1 that point along the coordinate axes.
  * **Standard unit vectors:**
    * $\hat{i}$: Unit vector in the positive x-direction.
    * $\hat{j}$: Unit vector in the positive y-direction.
  * **Representation:** Unit vectors are often drawn as shorter arrows with a hat or caret symbol above them.
## Unit Vector Notation
  
  * **Components:** Any vector can be expressed as a sum of its components along the coordinate axes.
  * **Unit vector notation:** A vector $\vec{A}$ can be written in unit vector notation as:
    * $\vec{A} = A_x \hat{i} + A_y \hat{j}$
    * Where $A_x$ and $A_y$ are the components of $\vec{A}$ along the x and y axes, respectively.
## Vector Components
  
  * **Finding components:** The components of a vector can be found using trigonometry if the magnitude and direction of the vector are known.
  * **Example:** For a vector $\vec{A}$ with magnitude $A$ and angle $\theta$ measured counterclockwise from the positive x-axis:
    * $A_x = A \cos \theta$
    * $A_y = A \sin \theta$
  * **Note:** The signs of the components depend on the quadrant in which the vector lies.
## Magnitude and Direction from Components
  
  * **Magnitude:** The magnitude of a vector can be found using the Pythagorean theorem:
    * $A = \sqrt{A_x^2 + A_y^2}$
  * **Direction:** The direction of a vector can be found using the inverse tangent function:
    * $\theta = \tan^{-1} \left( \frac{A_y}{A_x} \right)$
  * **Note:**  Care must be taken to determine the correct quadrant for the angle based on the signs of the components.
## Vector Addition and Subtraction
  
  * **Graphical method:** Vectors can be added or subtracted graphically using the head-to-tail method or the parallelogram method.
  * **Component method:** Vectors can be added or subtracted by adding or subtracting their corresponding components.
  * **Addition:** 
    * $\vec{C} = \vec{A} + \vec{B} = (A_x + B_x) \hat{i} + (A_y + B_y) \hat{j}$
  * **Subtraction:**
    * $\vec{D} = \vec{A} - \vec{B} = (A_x - B_x) \hat{i} + (A_y - B_y) \hat{j}$
## Position, Velocity, and Acceleration in 2-D
  
  * **Position vector:** The position of a particle in 2-D can be represented by a position vector $\vec{r}$:
    * $\vec{r} = x \hat{i} + y \hat{j}$
  * **Velocity vector:** The velocity of a particle is the rate of change of its position vector:
    * $\vec{v} = \frac{d\vec{r}}{dt} = \frac{dx}{dt} \hat{i} + \frac{dy}{dt} \hat{j} = v_x \hat{i} + v_y \hat{j}$
  * **Acceleration vector:** The acceleration of a particle is the rate of change of its velocity vector:
    * $\vec{a} = \frac{d\vec{v}}{dt} = \frac{dv_x}{dt} \hat{i} + \frac{dv_y}{dt} \hat{j} = a_x \hat{i} + a_y \hat{j}$
## Separation of Motion in x and y Directions
  
  * **Independence:**  In projectile motion (where only gravity acts), the horizontal and vertical motions are independent of each other.
  * **Constant velocity in x:** If there is no horizontal acceleration ($a_x = 0$), the horizontal velocity remains constant ($v_x = v_{0x}$).
  * **Constant acceleration in y:** The vertical motion is subject to a constant downward acceleration due to gravity ($a_y = -g$).
## Demonstrations
  
  * **Vertical launch from a moving car:** Shows that the horizontal motion of the ball is independent of its vertical motion.
  * **Simultaneously dropped and horizontally launched balls:**  Demonstrates that both balls fall at the same rate vertically, regardless of their horizontal motion.
## Projectile Motion
  
  * **Definition:** Projectile motion is the motion of an object under the influence of gravity only (neglecting air resistance).
  * **Constant acceleration:** The acceleration is constant and directed downward: $\vec{a} = -g \hat{j}$.
  * **Effect on velocity:**
    * **Horizontal velocity:** Remains constant ($v_x = v_{0x}$).
    * **Vertical velocity:** Changes linearly with time ($v_y = v_{0y} - gt$).
  
  **Note:** The equations for projectile motion will be discussed in more detail in a later lecture. They are not the same as the constant acceleration equations from Lecture 2, although they are related.