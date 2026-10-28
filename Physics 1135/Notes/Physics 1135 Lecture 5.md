# Lecture 5: Newton's 1st and 2nd Laws
## Introduction
  
  This lecture introduces Newton's laws of motion, which form the foundation of classical mechanics. We will explore:
  
  * Newton's 1st and 2nd Law
  * Inertia
  * Relationship between forces and acceleration
  * Procedure for solving force problems
## The "Natural" State of Motion
  
  * **Aristotle's view:** Objects naturally tend to be at rest.
  * **Galileo's view:** Objects in motion tend to stay in motion with constant velocity unless acted upon by a force. Galileo's experiments with inclined planes provided evidence for this idea.
## Newton's 1st Law - Law of Inertia
  
  * **Statement:** Every body continues in its state of rest or of uniform speed in a straight line unless acted upon by a nonzero net force.
  * **Mathematical representation:**  If $\sum \vec{F} = 0$, then $\vec{v} = \text{constant}$.
  * **Inertia:** The tendency of an object to resist changes in its motion. Inertia is related to an object's mass.
  * **Important Note:** Velocity is a vector, meaning it has both magnitude (speed) and direction. Therefore, a change in either speed or direction constitutes a change in velocity and requires a net force.
### Example: Newton's 1st Law Applied to Cats
  
  * A cat at rest will remain at rest unless acted upon by an external force.  This illustrates the concept of inertia.
## Forces
  
  * **Definition:** A force is a push or pull on an object.
  * **Characteristics:**
    * A force is a vector, having both magnitude and direction.
    * A force acts on an object.
    * A force requires an agent – something that exerts the force.
    * A force can be either a contact force (direct physical contact) or a long-range force (acting over a distance, like gravity).
### Examples of Forces
  
  * **Gravity (weight):**  The force exerted by Earth on objects with mass.
  * **Spring force:**  The force exerted by a compressed or stretched spring.
  * **Tension:**  The force exerted by a rope, string, or cable when pulled taut.
  * **Friction:** A force that opposes motion between surfaces in contact.
  * **Push/Pull:**  Forces exerted by direct physical contact.
  * **Electromagnetic forces:** Forces arising from electric and magnetic interactions.
## Discussion Question
  
  You throw a small ball straight up. Disregarding air resistance, what forces are acting on the ball until it returns to the ground?
  
  * **A) a constant downward force of gravity only.**
  * B) its weight vertically downward along with a steadily decreasing upward force.
  * C) a steadily decreasing upward force from the moment it leaves the hand until it reaches the highest point, beyond which there is a steadily increasing downward force of gravity.
  
  **Correct Answer:** A.  Once the ball leaves your hand, the only force acting on it is gravity.
## Changes in Velocity
  
  * **Constant velocity:** If an object's velocity is constant in magnitude and direction, its acceleration is zero ($\vec{a} = 0$).
  * **Force required:** Changes in velocity, such as:
    * Stopping or starting an object
    * Changing direction of motion
    * Increasing or decreasing speed
    All require a force.
## Inertia
  
  * **Observation:** Objects with greater weight are harder to accelerate.
  * **Intrinsic resistance:** Even in the absence of gravity (e.g., in deep space), objects still have an intrinsic resistance to acceleration.
  * **Definition of inertia:** This resistance to changes in motion is called inertia.
  * **Definition of mass:**  The quantity of inertia (resistance to acceleration) is called **mass** ($m$).
## Newton's 2nd Law
  
  * **Statement:** If a net external force acts on a body, the body accelerates. The acceleration is directly proportional to the net force and inversely proportional to the mass.
  * **Mathematical representation:** $\vec{a} = \frac{\vec{F}_{net}}{m} = \frac{\sum \vec{F}}{m}$ 
  * **Equivalent form:**  $\sum \vec{F} = m\vec{a}$  (for objects with constant mass)
  * **Unit of force:** Newton (N) = kg m/s<sup>2</sup>
## Component Version of Newton's 2nd Law
  
  * **Vector equation:**  $\vec{F}_{net} = \sum \vec{F} = m\vec{a}$ 
  * **Component equations:**
    * $\sum F_x = ma_x$
    * $\sum F_y = ma_y$
  * **Orthogonal axes:**  Because the x and y axes are orthogonal (perpendicular), we can separately equate the x-components and the y-components of the forces and acceleration.
## Weight Force
  
  * **Earth's gravitational field:** Earth exerts a force on all objects with mass.
  * **Weight at Earth's surface:** $\vec{F}_{grav} = m\vec{g}$
  * **Acceleration due to gravity:** $\vec{g} = (g, \text{ down})$ with magnitude $g = 9.8 \text{ m/s}^2$
  * **Definition of weight:** The gravitational force on an object is called its **weight** ($\vec{W}$):
    * $\vec{W} = (mg, \text{ down})$ 
  
  **Important:** The mass ($m$) in the weight equation and the mass ($m$) in Newton's second law are the same. This equivalence of inertial mass and gravitational mass is a fundamental principle in physics.
## Object in Free Fall
  
  * **Only force acting:** If the gravitational force is the only force acting on an object, then:
    * $\vec{F}_{grav} = m\vec{g} = m\vec{a} \implies \vec{a} = \vec{g}$
  * **Free fall acceleration independent of mass:**  This means that the acceleration of an object in free fall is independent of its mass.
  * **Gravity always present:**  Even if an object is not in free fall, the force of gravity still acts on it.
## Can We Feel Gravity?
  
  * **No direct sensation of gravitational force:** We do not directly feel the gravitational force itself.
  * **Normal force:**  We feel a normal force from the floor or seat that supports us.
  * **Spring scale:**  A spring scale measures the normal force.
  * **Zero acceleration:** If an object is at rest or moving with constant velocity ($\vec{a} = 0$), the normal force is equal in magnitude to the object's weight: $N = Mg$.
## Apparent Weight
  
  * **Upward acceleration:**  In an elevator accelerating upwards, the normal force (apparent weight) is greater than the actual weight. This makes us feel heavier.
  * **Downward acceleration:**  In an elevator accelerating downwards, the normal force is less than the actual weight. This makes us feel lighter.
  * **Free fall:**  If the elevator cable breaks (free fall), the normal force becomes zero ($N = 0$). We experience a sensation of weightlessness. (However, the impact at the bottom would be very bad!)
## Inertial Reference Frames
  
  * **Definition:** An inertial reference frame is a coordinate system in which Newton's laws are valid.
  * **Constant velocity frames:**  Reference frames moving at constant velocity are inertial reference frames.
  * **Accelerating frames:**  Reference frames that are accelerating are **not** inertial reference frames.
### Example: Airplane
  
  * **Cruising at constant velocity:**  A ball on the floor remains at rest relative to the airplane. This means the airplane is an inertial reference frame.
  * **Accelerating before takeoff:**  A ball on the floor rolls to the back of the plane, even though no horizontal force is acting on it from the plane's perspective. This means the accelerating airplane is **not** an inertial reference frame.
## Galilei Transformation
  
  The Galilei transformation relates the coordinates and velocities of objects in two inertial reference frames moving at a constant velocity relative to each other. 
  
  **Key points:**
  
  * Newton's laws have the same form in all inertial reference frames.
  * The transformation preserves the form of Newton's second law: $\vec{F} = m\vec{a}$ in both frames.
## Example Problem
  
  **Problem:**  A worker pushes a crate of mass $M$ on a level frictionless surface by applying a constant pushing force of magnitude $P$ at an angle $\theta$ with respect to the horizontal.
  
  **Task:** Derive expressions for the acceleration of the crate and the magnitude of the normal force acting on the crate, in terms of relevant system parameters.
  
  **(Solution will be derived during the lecture)**
## Summary of Problem-Solving Strategy (Litany) for Force Problems
  
  1. **Sketch:** Draw a clear diagram of the situation.
  2. **Free-body diagram:** Draw a separate diagram showing all the forces acting on the object of interest. Label each force.
  3. **Coordinate system:**  Choose an x-y coordinate system, aligning one axis with the direction of the known or assumed acceleration.
  4. **Vector components:** Resolve all forces into their x and y components.
  5. **Starting equation:**  Use Newton's second law in component form:
    * $\sum F_x = ma_x$
    * $\sum F_y = ma_y$
  6. **Sum of forces:** Write out the sum of the force components in each direction.
  7. **Solve symbolically:** Solve the equations for the unknown quantities (acceleration, normal force, etc.) in terms of the given variables.
  
  
  This problem-solving strategy (Litany) will be crucial for solving force problems throughout the course.