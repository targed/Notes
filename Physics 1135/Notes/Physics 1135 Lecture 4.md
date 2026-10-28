# Lecture 4: Motion in Two Dimensions
## Introduction
  
  This lecture expands on the concepts of 2-D kinematics introduced in Lecture 3, focusing on:
  
  * Equations for 2-D kinematics at constant acceleration
  * Projectile motion
  * Problem-solving techniques
## Kinematics Equations (Review)
  
  Recall the kinematics equations for constant acceleration from Lecture 2. These equations apply to both horizontal and vertical motion independently in 2-D:
  
  |**Horizontal Motion** | **Vertical Motion**|
  |------- | --------|
  |$x = x_0 + v_{0x}t + \frac{1}{2}a_x t^2$ | $y = y_0 + v_{0y}t + \frac{1}{2}a_y t^2$|
  |$v_x = v_{0x} + a_x t$ | $v_y = v_{0y} + a_y t$|
  |$v_x^2 = v_{0x}^2 + 2a_x(x - x_0)$ | $v_y^2 = v_{0y}^2 + 2a_y(y - y_0)$|
  
  **Note:** These are the "official starting equations" for solving kinematics problems with constant acceleration.
## Projectile Motion
  
  * **Definition:** Projectile motion is the motion of an object under the influence of gravity only (neglecting air resistance).
  * **Assumptions:**
    * The only force acting on the object is gravity (neglecting air resistance).
    * The acceleration due to gravity is constant and directed downward ($\vec{a} = -g \hat{j}$).
  * **Independence of motion:** The horizontal and vertical motions are independent of each other.
### Effect of Gravity on Velocity
  
  * **Horizontal velocity ($v_x$):** Remains constant throughout the motion ($v_x = v_{0x}$) because there is no horizontal acceleration ($a_x = 0$).
  * **Vertical velocity ($v_y$):** Changes linearly with time due to the constant downward acceleration due to gravity ($v_y = v_{0y} - gt$).
  
  **Important:** The equations $v_x = v_{0x}$ and $v_y = v_{0y} - gt$ are **not** the starting equations for projectile motion problems. They are derived from the more general constant acceleration equations.
## Projectile Motion: Simulation
  
  A helpful simulation to visualize projectile motion can be found at:
  
  http://www.walter-fendt.de/ph14e/projectile.htm
## Free-Fall Trajectory
  
  * **Trajectory:** The path followed by a projectile is called its trajectory. It is a parabolic curve.
  * **Deriving the trajectory equation:** We can eliminate time ($t$) from the horizontal and vertical position equations to find an equation for the trajectory $y(x)$.
    1. **Horizontal motion:** $x = v_{0x}t$ (assuming $x_0 = 0$)
    2. **Solve for $t$:** $t = \frac{x}{v_{0x}}$
    3. **Vertical motion:** $y = v_{0y}t - \frac{1}{2}gt^2$ (assuming $y_0 = 0$)
    4. **Substitute $t$ from step 2:** $y = v_{0y}\left(\frac{x}{v_{0x}}\right) - \frac{1}{2}g\left(\frac{x}{v_{0x}}\right)^2$
    5. **Simplify:** $y = \left(\frac{v_{0y}}{v_{0x}}\right)x - \left(\frac{g}{2v_{0x}^2}\right)x^2$
  * **Parabolic form:** The trajectory equation is a quadratic equation in $x$, indicating a parabolic shape.
## Example: Projectile Motion with a Wall
  
  **Problem:** A child kicks a soccer ball from the ground level with an initial speed $v_0$ at an angle $\theta$ with respect to the horizontal. The ball hits a wall a distance $L$ away.
  
  **Tasks:**
  
  a) Complete the diagram with all information necessary to solve the parts below.
  b) Derive a symbolic expression for the time it takes the ball to reach the wall.
  c) Derive a symbolic expression for the height $H$ at which the ball hits the wall.
  d) The ball reaches its highest point before hitting the wall. Find the maximum height above the ground.
  
  **(Solutions will be derived during the lecture)**
## Example: Range of a Projectile
  
  **(This example will be covered in detail during the lecture)**
## Demo: The Hunter and the Monkey
  
  This demonstration illustrates the concept of projectile motion and the independence of horizontal and vertical motions.
  
  **Scenario:** A hunter aims directly at a monkey hanging from a tree. At the instant the hunter fires the dart, the monkey lets go of the branch.
  
  **Question:** Will the dart hit the monkey?
  
  **Hint:** The angle $\theta$ between the initial velocity and the horizontal is not given, but knowing the horizontal distance $D$ and the height $H$ will enable you to find $\sin \theta$ and $\cos \theta$.
  
  **(You will work this out in the Special Homework)**