## Introduction
  
  This lecture introduces periodic motion, focusing on simple harmonic motion (SHM) as exemplified by a mass on a spring and various types of pendulums.  We will cover:
  
  * Motion of a mass at the end of a spring
  * Differential equation for simple harmonic oscillation
  * Amplitude, period, frequency, and angular frequency
  * Energetics of SHM
  * Simple pendulum
  * Physical pendulum
## Mass at the End of a Spring
  
  * **System:** A mass $m$ connected to a spring with spring constant $k$ on a frictionless surface.
  * **Restoring force:** The spring force always acts to restore the mass to its equilibrium position.
  * **Hooke's Law:**  $F_x = -kx$ (The force is proportional to the displacement $x$ from equilibrium and in the opposite direction.)
  * **Linear restoring force:** The force is linear in the displacement $x$.  This leads to simple harmonic motion.
  
  **(Diagram of a mass attached to a spring)**
## Spring Force (Review)
  
  * **Spring force:** $\vec{F}_s = -kx \hat{i}$
    * $x = l - l_{eq}$: Displacement from equilibrium (stretch or compression).  $l$ is the current length of the spring, and $l_{eq}$ is the equilibrium length.
    * $k$: Force constant (a measure of the spring's stiffness).
  
  * **Direction of force:**
    * If $x$ is positive (stretched spring), $F_x$ is negative (force points towards equilibrium).
    * If $x$ is negative (compressed spring), $F_x$ is positive (force points towards equilibrium).
  
  
  **(Diagrams of a stretched and compressed spring)**
## Differential Equation of a Simple Harmonic Oscillator (SHO)
  
  * **Newton's 2nd Law:**  $\sum F_x = ma_x$
  * **Substituting Hooke's Law:** $-kx = m \frac{d^2x}{dt^2}$
  * **Rearranging:** $\frac{d^2x}{dt^2} = -\frac{k}{m}x$
  * **Angular frequency ($\omega$):**  Defining $\omega^2 = \frac{k}{m}$ (where $\omega$ is the angular frequency), we get:
   $\frac{d^2x}{dt^2} = -\omega^2 x$
  * **Differential equation of SHO:** This second-order differential equation is the defining equation for simple harmonic motion.
## Solution to the SHO Equation
  
  * **General solution:** The general solution to the differential equation $\frac{d^2x}{dt^2} = -\omega^2 x$ is:
    $x = A \cos(\omega t + \phi)$
    * $A$: Amplitude (maximum displacement from equilibrium)
    * $\phi$: Phase constant (determined by the initial conditions)
  
  * **Verification:**  You can verify that this solution satisfies the differential equation by taking the second derivative of $x$ with respect to time.
## Amplitude
  
  * **Range of cosine function:**  The cosine function varies between -1 and +1.
  * **Displacement range:** Therefore, the displacement $x$ varies between $-A$ and $+A$.
  * **Amplitude ($A$):** The amplitude is the maximum displacement from equilibrium.
## Phase Constant
  
  * **Shifting the cosine function:** The phase constant $\phi$ shifts the cosine function horizontally, allowing us to describe motion with different starting points.
  * **$\phi = 0$:** If the motion starts at the maximum displacement ($x_0 = A$ at $t=0$), then $\phi = 0$.
  * **Examples:**
    * $x_0 = -A$:  Shift by $\pi$ (or $180^\circ$).
    * $x_0 = 0$: Shift by $\frac{\pi}{2}$ (or $90^\circ$).
  
  **(Graphs illustrating the effect of the phase constant)**
## Initial Conditions
  
  * **Determining A and $\phi$:** The amplitude $A$ and phase constant $\phi$ are determined by the initial conditions of the motion:
    * $x_0 = x(t=0)$ (Initial displacement)
    * $v_{x0} = v_x(t=0)$ (Initial velocity)
  
  * **Equations:**
    * $x_0 = A\cos\phi$
    * $v_{x0} = -A\omega\sin\phi$
  
  These two equations can be used to solve for $A$ and $\phi$.
## Position and Velocity
  
  * **Position:** $x = A\cos(\omega t + \phi)$
  * **Velocity:** $v_x = \frac{dx}{dt} = -A\omega\sin(\omega t + \phi)$
  * **At turning points:**  When the mass reaches its maximum displacement ($x = \pm A$), the velocity is zero ($v_x = 0$).
## Simulation
  
  **Simulation of SHM:** http://www.walter-fendt.de/ph14e/springpendulum.htm
## Period and Angular Frequency
  
  * **Period ($T$):** The time for one complete cycle of oscillation.
  * **Angular frequency ($\omega$):**  Related to the period by $\omega T = 2\pi$, so  $\omega = \frac{2\pi}{T}$.
  * **Frequency ($f$):** The number of cycles per second: $f = \frac{1}{T}$.
  * **Relationship between $\omega$ and $f$:** $\omega = 2\pi f$.
  
  **(Graph showing the position as a function of time for SHM)**
## Effect of Mass and Amplitude on Period
  
  * **Period formula:** Since $\omega = \sqrt{\frac{k}{m}}$ and $T = \frac{2\pi}{\omega}$, we have $T = 2\pi \sqrt{\frac{m}{k}}$.
  * **Mass dependence:** The period is proportional to the square root of the mass.  Larger mass means longer period.
  * **Spring constant dependence:** The period is inversely proportional to the square root of the spring constant. Stiffer spring (larger $k$) means shorter period.
  * **Amplitude independence:**  The amplitude $A$ does not appear in the period formula, which means the period is independent of the amplitude for SHM.
  
  **Demo:** Demonstrating the effect of mass and amplitude on the period of vertical springs.
## Energy in SHO
  
  * **Potential energy:** $U = \frac{1}{2}kx^2$
  * **Kinetic energy:** $K = \frac{1}{2}mv^2$
  * **Total mechanical energy:**  $E = K + U$
  * **At maximum displacement ($x = \pm A$):** $U = \frac{1}{2}kA^2$, $K = 0$, $E = \frac{1}{2}kA^2$
  * **At equilibrium ($x = 0$):** $U = 0$, $K = K_{max} = E$
  
  **(Energy diagram for SHO)**
## Kinetic and Potential Energy in SHO
  
  * **Kinetic energy:** $K = \frac{1}{2}mv^2 = \frac{1}{2}m(A\omega\sin(\omega t + \phi))^2 = \frac{1}{2}mA^2\omega^2\sin^2(\omega t + \phi)$
  * **Potential energy:** $U = \frac{1}{2}kx^2 = \frac{1}{2}k(A\cos(\omega t + \phi))^2 = \frac{1}{2}kA^2 \cos^2(\omega t + \phi)$
  * **Total energy:** $E = K_{max}\sin^2(\omega t + \phi) + U_{max}\cos^2(\omega t + \phi)$, where $K_{max} = \frac{1}{2}m(A\omega)^2$ and $U_{max} = \frac{1}{2}kA^2$.  Since $\omega = \sqrt{k/m}$, $K_{max} = U_{max}$. Since $\sin^2(\omega t+\phi)+\cos^2(\omega t+\phi) = 1$, this means $E=K_{max}=U_{max}$.
  
  **Simulation:** http://www.walter-fendt.de/ph14e/springpendulum.htm
## Example: Energy in SHM
  
  **Problem:** A block of mass $M$ is attached to a spring and executes SHM with amplitude $A$. At what displacement(s) $x$ from equilibrium does its kinetic energy equal twice its potential energy?
  
  **(Solution will be derived during the lecture using the expressions for kinetic and potential energy and the conservation of energy.)**
## Simple Harmonic Motion (SHO) - Summary
  
  * **Differential equation:** $\frac{d^2 x}{dt^2} = -\omega^2 x$
  * **General solution:** $x=A\cos(\omega t+\phi)$
  * **Period:** $T = \frac{2\pi}{\omega}$
## Simple Pendulum
  
  * **System:**  A point mass $m$ at the end of a massless string of length $L$.
  * **Displacement coordinate ($\theta$):** The angle (with sign) from the vertical equilibrium position.
  * **Restoring force:**  The component of gravity tangential to the arc of motion ($mg\sin\theta$) acts as the restoring force.
  
  **(Diagram of a simple pendulum)**
## Simple Pendulum - Differential Equation
  
  * **Torque:** $\sum \tau_z = I\alpha_z$
  * **Torque due to gravity:** $-mgL\sin\theta$
  * **Moment of inertia:** $I = mL^2$
  * **Angular acceleration:** $\alpha_z = \frac{d^2\theta}{dt^2}$
  * **Differential equation:** $-mgL\sin\theta = mL^2 \frac{d^2\theta}{dt^2}$
  * **Small angle approximation:** For small oscillations ($\theta << 1 \text{ radian}$), $\sin\theta \approx \theta$.
  * **Simplified differential equation:** $-\frac{g}{L}\theta = \frac{d^2\theta}{dt^2}$
  * **SHO equation:** This is the differential equation for simple harmonic motion with $\omega^2 = \frac{g}{L}$.
## Simple Pendulum Oscillations
  
  * **Solution:**  $\theta(t) = \theta_{max}\cos(\omega t + \phi)$
  * **Angular frequency:** $\omega = \sqrt{\frac{g}{L}}$
  * **Period:** $T = 2\pi\sqrt{\frac{L}{g}}$
  
  **Demo:** Demonstrating simple pendulums with different masses, lengths, and amplitudes.
## Simple Pendulum - Period
  
  * **Period formula:** $T = 2\pi\sqrt{\frac{L}{g}}$
  * **Mass independence:** The period is independent of the mass of the bob.
  * **Amplitude independence (for small oscillations):**  The period is approximately independent of the amplitude for small oscillations.
## Physical Pendulum
  
  * **System:** An extended object of mass $m$ that swings back and forth about an axis P that does not go through its center of mass (CM).
  * **Distance $D$:**  The distance between the pivot point P and the center of mass.
  * **Differential equation:**
    * $\sum \tau_z = I\alpha_z$
    * $-mgD\sin\theta = I\frac{d^2\theta}{dt^2}$
  * **Small angle approximation:** For small oscillations, $\sin\theta \approx \theta$.
  * **SHO equation:** $-\frac{mgD}{I}\theta = \frac{d^2\theta}{dt^2}$  (where $I$ is the moment of inertia about the pivot point P)
## Motion of the Physical Pendulum
  
  * **SHO:**  For small oscillations, the motion is simple harmonic.
  * **Angular frequency:** $\omega = \sqrt{\frac{mgD}{I}}$
  * **Period:** $T = 2\pi\sqrt{\frac{I}{mgD}}$
  * **Parallel axis theorem:** Often, the moment of inertia about the pivot point P is calculated using the parallel axis theorem:  $I_P = I_{CM} + mD^2$
  
  **Demo:**  Meter stick pivoted at different positions, demonstrating the effect of changing the pivot point on the period.
## Example: Physical Pendulum - Disk
  
  **Problem:** A uniform disk of mass $M$ and radius $R$ is pivoted at a point on the rim. Find the period for small oscillations.
  
  **(Diagram of the disk pivoted at the rim.)**
  
  **(Solution will be derived during the lecture using the formula for the period of a physical pendulum and the parallel axis theorem to find the moment of inertia about the pivot point.)**