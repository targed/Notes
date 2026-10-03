## Introduction
  
  This lecture introduces the kinematics and energetics of rotating objects. We will cover:
  
  * Angular quantities
  * Rolling without slipping
  * Rotational kinetic energy
  * Moment of inertia
  * Energy problems involving rotation
## Angle Measurement in Radians
  
  * **Radians:** The standard unit for measuring angles in rotational motion.
  * **Definition:** $\theta (\text{in radians}) = \frac{s}{r}$
    * $s$: arc length of a circle
    * $r$: radius of the circle
  * **Complete circle:** $s = 2\pi r$, $\theta = 2\pi$ radians
  
  **(Diagram illustrating the definition of a radian)**
## Angular Kinematic Vectors
  
  * **Angular position ($\theta$):** Specifies the angular orientation of an object.
  * **Angular displacement ($\Delta \theta$):**  The change in angular position: $\Delta \theta = \theta_2 - \theta_1$.
  * **Angular velocity ($\omega_z$):** The rate of change of angular position:  $\omega_z = \frac{d\theta}{dt}$. The subscript $z$ indicates that the rotation is about the z-axis.
  * **Angular velocity vector ($\vec{\omega}$):** Points perpendicular to the plane of rotation. The direction is determined by the right-hand rule: curl the fingers of your right hand in the direction of rotation, and your thumb points in the direction of the angular velocity vector.
  
  **(Diagram showing the right-hand rule for angular velocity)**
## Angular Acceleration
  
  * **Angular acceleration ($\alpha_z$):** The rate of change of angular velocity: $\alpha_z = \frac{d\omega_z}{dt}$.
  * **Relationship between $\vec{\alpha}$ and $\vec{\omega}$:**
    * If $\vec{\alpha}$ and $\vec{\omega}$ are in the same direction, the rotation is speeding up.
    * If $\vec{\alpha}$ and $\vec{\omega}$ are in opposite directions, the rotation is slowing down.
  
  
  **(Diagram illustrating angular acceleration)**
## Angular Kinematics
  
  For constant angular acceleration, the kinematic equations are analogous to those for linear motion:
  
  | Linear Motion | Rotational Motion |
  |---|---|
  | $x = x_0 + v_{0x}t + \frac{1}{2}a_x t^2$ | $\theta = \theta_0 + \omega_{0z}t + \frac{1}{2}\alpha_z t^2$ |
  | $v_x = v_{0x} + a_x t$ | $\omega_z = \omega_{0z} + \alpha_z t$ |
  | $v_x^2 = v_{0x}^2 + 2a_x(x - x_0)$ | $\omega_z^2 = \omega_{0z}^2 + 2\alpha_z(\theta - \theta_0)$ |
## Relationship Between Angular and Linear Motion
  
  * **Linear velocity ($v$):** Tangent to the circular path: $v = \frac{ds}{dt} = \frac{d(r\theta)}{dt} = r\frac{d\theta}{dt} = r\omega$
  * **Tangential acceleration ($a_{tan}$):** The component of acceleration tangent to the circular path: $a_{tan} = \frac{dv}{dt} = r\frac{d\omega}{dt} = r\alpha$
  * **Radial acceleration ($a_{rad}$):** The component of acceleration towards the center of the circle (centripetal acceleration): $a_{rad} = \frac{v^2}{r} = \omega^2 r$
  
  **Important:** Angular speed ($\omega$) is the same for all points of a rigid rotating body.  Linear speed, however, increases with distance from the rotation axis.
  
  **(Diagram illustrating the relationship between linear and angular quantities)**
## Rolling Without Slipping
  
  * **Combined motion:** Rolling without slipping can be considered as a combination of rotation about the center of mass (CM) and translation of the CM.
  * **Velocity relationship:**  $v_{CM} = \omega R$ (where $R$ is the radius of the rolling object)
  * **No slipping condition:**  The velocity of the point of contact with the surface is zero: $v_{bottom} = 0$.
  
  **(Diagram illustrating rolling without slipping)**
## Rotational Kinetic Energy
  
  * **Definition:**  The kinetic energy associated with the rotation of an object.
  * **Derivation:** $K_{rotation} = \sum \frac{1}{2}m_n v_n^2 = \sum \frac{1}{2}m_n (\omega r_n)^2 = \frac{1}{2}\left(\sum m_n r_n^2\right)\omega^2$
  * **Formula:** $K_{rot} = \frac{1}{2}I\omega^2$
    * $I$: Moment of inertia
## Moment of Inertia
  
  * **Definition:** A measure of an object's resistance to rotational acceleration. It depends on the mass distribution and the axis of rotation.
  * **Formula:** $I = \sum m_n r_n^2$ (for a system of particles)
  * **Continuous objects:**  $I = \int r^2 dm$ (calculated using integration - *not covered in this course*)
  * **Table of moments of inertia:** Refer to Table p. 291 in your textbook for moments of inertia of common shapes.
## Properties of the Moment of Inertia
  
  1. **Axis dependence:** The moment of inertia depends on the axis of rotation. The same object will have different moments of inertia for different axes.
  2. **Mass distribution:** The farther the mass is from the axis of rotation, the greater the moment of inertia.
  3. **Axial symmetry:** The distribution of mass along the rotation axis does not affect the moment of inertia; only the radial distance matters.
  
  **(Examples illustrating the properties of moment of inertia)**
## Parallel Axis Theorem
  
  * **Statement:**  Relates the moment of inertia about an axis through the center of mass ($I_{CM}$) to the moment of inertia about a parallel axis ($I_q$) a distance $d$ away:
    * $I_q = I_{CM} + Md^2$
    * where $M$ is the total mass of the object.
  
  **(Example using the parallel axis theorem)**
## Rotation and Translation
  
  * **Total kinetic energy:**  For an object that is both rotating and translating, the total kinetic energy is:
    * $K = K_{trans} + K_{rot} = \frac{1}{2}Mv_{CM}^2 + \frac{1}{2}I\omega^2$
## Hoop-Disk Race
  
  **Demo:** Race of a hoop and a disk down an incline, illustrating the effect of different moments of inertia on rotational acceleration.
  
  **Example:** An object of mass $M$, radius $R$, and moment of inertia $I$ is released from rest and rolls down an incline that makes an angle $\theta$ with the horizontal. What is the speed when the object has descended a vertical distance $H$?
  
  **(Diagram showing the incline, object, height $H$, and angle $\theta$)**
  
  **(Solution will be derived during the lecture, using conservation of energy and the relationship between linear and angular speeds for rolling without slipping.)**
## Example with Coupled Objects
  
  **Problem:** A small disk of radius $r$ is glued onto a large disk of radius $R$ that is mounted on a fixed axle through its center. The combined moment of inertia of the disks is $I$. A string is wrapped around the edge of the small disk, and a box of mass $m$ is tied to the end of the string. The string does not slip on the disk. The box is released from rest. Find the speed of the box after it has descended a distance $d$.
  
  **(Diagram showing the disks, string, and box)**
  
  **(Solution will be derived using energy conservation. The potential energy lost by the falling box is converted into the rotational kinetic energy of the disks and the translational kinetic energy of the box.)**