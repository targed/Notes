# Lecture 2: Motion in One Dimension
## Motion in One Dimension (Continued)
- This lecture builds upon the concepts introduced in Lecture 1, focusing on motion along a straight line.
- We will explore:
  - Equations for constant acceleration
  - Free fall
  - Problem-solving strategies for kinematics
## Velocity and Acceleration (Review)
- Recall the definitions of velocity and acceleration from Lecture 1:
- **Instantaneous Velocity:**
  - $v_x = \frac{dx}{dt}$ (the rate of change of position with respect to time)
- **Instantaneous Acceleration:**
  - $a_x = \frac{dv_x}{dt} = \frac{d^2x}{dt^2}$ (the rate of change of velocity with respect to time)
## Constant Acceleration
- When acceleration is constant, we can derive several useful equations to describe motion:
- **Constant a_{x} on an a_{x} vs. t graph:**
  - A horizontal line indicates constant acceleration.
- **Constant a_{x} on a v_{x} vs. t graph:**
  - A straight line with a constant slope (equal to $a_x$) represents constant acceleration.
- **Constant a_{x} on an x vs. t graph:**
  - A parabolic curve indicates constant acceleration. The slope of the tangent line at any point gives the instantaneous velocity, which changes linearly with time.
### Deriving Equations for Constant Acceleration
- Starting with the definition of acceleration, we can integrate to find equations for velocity and position:
  - 1. **Acceleration:** $a_x = \frac{dv_x}{dt}$
  - 2. **Rearrange:** $dv_x = a_x dt$
  - 3. **Integrate both sides:** $\int_{v_{0x}}^{v_x(t)} dv_x = \int_{t_0}^{t} a_x dt$
  - 4. **Evaluate the integrals (assuming $t_0 = 0$ for simplicity):** $v_x - v_{0x} = a_x t$
  - 5. **Rearrange to solve for velocity:** $v_x = v_{0x} + a_x t$
- Similarly, we can integrate the velocity equation to find the position equation:
  - 1. **Velocity:** $v_x = \frac{dx}{dt}$
  - 2. **Rearrange:** $dx = v_x dt$
  - 3. **Substitute the velocity equation:** $dx = (v_{0x} + a_x t) dt$
  - 4. **Integrate both sides:** $\int_{x_0}^{x} dx = \int_{0}^{t} (v_{0x} + a_x t) dt$
  - 5. **Evaluate the integrals:** $x - x_0 = v_{0x}t + \frac{1}{2}a_x t^2$
  - 6. **Rearrange to solve for position:** $x = x_0 + v_{0x}t + \frac{1}{2}a_x t^2$
- Another useful equation can be derived by eliminating time from the velocity and position equations:
  - $v_x^2 = v_{0x}^2 + 2a_x(x - x_0)$
## Constant Acceleration: Starting Equations
- These equations are fundamental for solving problems involving constant acceleration:
- |Horizontal Motion|Vertical Motion|
  |--|--|
  |$x = x_0 + v_{0x}t + \frac{1}{2}a_x t^2$	|$y = y_0 + v_{0y}t + \frac{1}{2}a_y t^2$|
  |$v_x = v_{0x} + a_x t$|$v_y = v_{0y} + a_y t$|
  |$v_x^2 = v_{0x}^2 + 2a_x(x - x_0)$|$v_y^2 = v_{0y}^2 + 2a_y(y - y_0)$|
- **Note:** These equations will be provided on the equation sheet for homework and exams.
## Free Fall
- **Definition:** An object is in free fall when the only force acting on it is gravity.
- **Constant downward acceleration:** Objects in free fall experience a constant downward acceleration due to gravity, denoted by $g$.
- **Magnitude of g:**  $g = 9.8 \text{ m/s}^2$ (approximately)
- **Sign of a_{y}:**  In a coordinate system where the positive y-direction is upward, the acceleration due to gravity is negative: $a_y = -g = -9.8 \text{ m/s}^2$.
- **Air resistance:**  In the absence of air resistance, all objects fall with the same acceleration, regardless of their mass.
- **Demonstration:** The Apollo 15 Feather and Hammer experiment demonstrates this principle on the Moon.
## Example Problem
- **Problem:** A person stands on top of a building of height 80m. They throw a ball straight up with an initial speed of 20 m/s so that it just misses the edge of the building when coming down. (Use $g = 10 \text{ m/s}^2$ for simplicity). 
  
  **Calculate:**
  
  a) The time it takes for the ball to reach its highest point.
  b) The height of the highest point above the ground.
  c) The velocity with which the ball hits the ground.
  
  **(Solution will be worked out on the board during the lecture)**
## Summary of Problem-Solving Strategy (Litany)
  
  1. **Complete diagram:**
   * Draw the initial velocity and acceleration vectors.
   * Draw the coordinate axis, including the origin.
   * Indicate and label the initial and final positions.
  2. **Starting equation:** Choose the appropriate equation from the constant acceleration equations based on the given information and what you need to find.
  3. **Replace generic quantities:** Substitute the known values from the problem into the chosen equation.
  4. **Derive symbolic answer:** Solve the equation algebraically for the unknown quantity.
  5. **Calculate numerical answer:** Plug in the numerical values and calculate the final answer, including units.
  
  
  This problem-solving strategy will be applied to various examples throughout the course.