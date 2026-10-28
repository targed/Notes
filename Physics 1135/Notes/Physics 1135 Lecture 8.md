# Lecture 8: Circular Motion
## Introduction
  
  This lecture explores the dynamics of circular motion, focusing on:
  
  * Uniform and non-uniform circular motion
  * Centripetal acceleration
  * Problem solving with Newton's 2nd Law for circular motion
## Effect of Force Components (Review)
  
  Recall from Lecture 3 that components of force parallel and perpendicular to velocity have different effects:
  
  * **Force parallel to velocity ($F_{||}$):**  Causes a change in the magnitude of the velocity vector (speed).
  * **Force perpendicular to velocity ($F_{\perp}$):** Causes a change in the direction of the velocity vector.
## Uniform Circular Motion
  
  * **Definition:** Motion in a circle with **constant speed**.
  * **Caution:** Even though the speed is constant, the velocity is not constant because the direction is changing. Therefore, there is acceleration in uniform circular motion.
## Centripetal Acceleration
  
  * **Centripetal acceleration ($a_c$):** The acceleration directed towards the center of the circle that keeps an object moving in a circular path.
  * **Formula:** $a_c = \frac{v^2}{R}$ 
    * $v$: Speed of the object
    * $R$: Radius of the circle
  * **Direction:** Always points towards the center of the circle.
## Non-Uniform Circular Motion
  
  * **Definition:** Motion in a circle with **non-constant speed**.
  * **Two components of acceleration:**
    * **Centripetal acceleration ($a_c$):**  Towards the center of the circle, changes the direction of velocity.
    * **Tangential acceleration ($a_{tan}$):**  Tangential to the circle, changes the speed of the object.
  * **Formulas:**
    * $a_c = \frac{v^2}{R}$ (where $v$ is the instantaneous speed, which can change)
    * $a_{tan} = \frac{dv}{dt}$
## Forces Create Centripetal Acceleration
  
  * **Cause of centripetal acceleration:**  The acceleration towards the center of the circle must be caused by a **net force** that is also directed towards the center.
  * **Centripetal force:**  The net force that causes centripetal acceleration.
  * **Newton's 2nd Law for circular motion:**  $\sum F_r = ma_c = m\frac{v^2}{R}$ 
    * $\sum F_r$: The sum of the radial forces (forces acting along the radius, towards the center).
## Example: Carousel
  
  **Simulation:** http://www.walter-fendt.de/ph1i1e/carousel.htm
  
  **(Discussion of the forces involved in the carousel will be covered during the lecture.)**
## Example: Ball in Vertical Circle
  
  **Problem:** A ball of mass $m$ at the end of a string of length $L$ is moving in a vertical circle. When it is at its lowest point, it has speed $V$. What is the tension in the string at that instant?
  
  **(Solution will be derived during the lecture.)**
## Example: Ball in Vertical Circle - Minimum Speed
  
  **Problem:** A ball of mass $m$ at the end of a string of length $L$ is moving in a vertical circle. What must be its minimum speed at the highest point for the ball to stay in the circle?
  
  **(Solution will be derived during the lecture.)**
## Demo: Twirling a Bucket of Water
  
  **(Live demonstration showing the application of centripetal force to keep water from spilling out of a bucket when twirled in a vertical circle.)**
## Pseudoforces
  
  * **Non-inertial reference frames:** In non-inertial (accelerating) reference frames, such as a rotating reference frame, there are apparent forces called **pseudoforces**.
  * **Types:**
    * **Centrifugal force:**  Appears to push an object outward from the center of rotation in a rotating reference frame.
    * **Coriolis force:**  Appears to act on objects moving within a rotating reference frame, causing deflection.
## Coriolis Force
  
  * **Cause:**  Due to Earth's rotation.
  * **Relevance:**  Significant for very large masses (air masses, ocean currents) that are moving.
  * **Effect:** Responsible for the formation of hurricanes and other large-scale weather patterns.
  * **Northern hemisphere:** Deflection to the right as seen in the direction of motion.
  * **Southern hemisphere:** Deflection to the left as seen in the direction of motion.
## Avoiding Rotating Reference Frames
  
  * **In this course:** We will **never** describe circular motion in a rotating coordinate system.
  * **Inertial reference frame:**  We will always attach our coordinate system to Earth (or another non-rotating frame) and treat it as an inertial reference frame.
  * **No centrifugal force:** By avoiding rotating reference frames, we do not need to consider the centrifugal force.
## Car in Flat Curve
  
  **Scenario:** A car is driving around a flat curve. The force of static friction between the tires and the road provides the centripetal force to keep the car moving in a circle.
  
  * **Free-body diagram:** The forces acting on the car are:
    * **Weight ($W$):** Acting vertically downward.
    * **Normal force ($N$):** Acting vertically upward.
    * **Static friction force ($f_s$):**  Acting horizontally towards the center of the curve.
  * **Newton's 2nd Law in component form:**
    * $\sum F_x = f_s = ma_c = m\frac{v^2}{R}$
    * $\sum F_y = N - W = 0 \implies N = mg$
  * **Maximum speed:** The maximum speed at which the car can go around the curve without skidding is determined by the maximum static friction force:
    * $f_s = f_{s_{max}} = \mu_s N = \mu_s mg$
    * $\mu_s mg = m\frac{v_{max}^2}{R}$
    * $v_{max} = \sqrt{\mu_s gR}$
## Car in Banked Curve
  
  **Scenario:** A car is driving around a curve that is banked (tilted) at an angle $\beta$. Banking the curve allows the car to go around the curve at a higher speed, even without friction.
  
  * **Free-body diagram:** The forces acting on the car are:
    * **Weight ($W$):** Acting vertically downward.
    * **Normal force ($N$):** Acting perpendicular to the surface of the banked curve.
  * **Components of normal force:**  The normal force can be resolved into components parallel and perpendicular to the horizontal.
  * **Newton's 2nd Law in component form:**
    * $\sum F_x = N \sin \beta = ma_c = m\frac{v^2}{R}$
    * $\sum F_y = N \cos \beta - W = 0 \implies N \cos \beta = mg$
  * **Design speed:** The speed at which the car can go around the curve without relying on friction is called the **design speed** ($v_D$).
    * Solving the equations above for $v_D$ gives: $v_D = \sqrt{Rg \tan \beta}$
## Car in Banked Curve with Friction
  
  * **Slower than design speed:** If the car is going slower than the design speed, static friction will act up the incline to prevent the car from sliding down.
  * **Faster than design speed:** If the car is going faster than the design speed, static friction will act down the incline to prevent the car from skidding up.
  
  **Homework:**  You will derive expressions for the minimum and maximum speeds in these scenarios in the homework.