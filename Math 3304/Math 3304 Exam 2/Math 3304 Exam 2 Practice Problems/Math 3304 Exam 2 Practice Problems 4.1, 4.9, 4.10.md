### **Practice Problems**
#### **Problem 1: Simple Harmonic Motion (Metric)**
  A $2\text{-kg}$ mass is attached to a horizontal spring with stiffness $k = 72\text{ N/m}$ on a frictionless surface. The mass is pulled $0.5\text{ m}$ to the right of equilibrium and released with an initial velocity of $3\text{ m/s}$ to the left.
  1. Formulate and solve the initial value problem for the displacement $x(t)$.
  2. Express the solution in amplitude-phase form: $x(t) = A\cos(\omega_0 t - \phi)$.
  
  ---
#### **Problem 2: Undamped Free Vibrations (Imperial Units)**
  A weight of $64\text{ lb}$ stretches a vertical spring by $6\text{ in}$ upon coming to rest at equilibrium. The mass is pulled down $4\text{ in}$ below equilibrium and imparted with an upward velocity of $2\text{ ft/s}$. Assuming no damping and no external forces, find the equation of motion $x(t)$ (take downward as positive; use $g = 32\text{ ft/s}^2$).
  
  ---
#### **Problem 3: Classifying Damping Regimes**
  A mass of $1\text{ kg}$ is attached to a spring with spring constant $k = 25\text{ N/m}$. The system is submerged in a fluid providing a damping constant $b$.
  1. What value of $b$ produces a **critically damped** system?
  2. Classify the motion (overdamped, critically damped, or underdamped) if $b = 8\text{ N}\cdot\text{s/m}$.
  3. Classify the motion if $b = 14\text{ N}\cdot\text{s/m}$.
  
  ---
#### **Problem 4: Underdamped Motion (Envelope & Quasi-quantities)**
  A $1\text{-kg}$ mass is attached to a spring with stiffness $k = 25\text{ N/m}$. The damping constant is $b = 6\text{ N}\cdot\text{s/m}$. The mass is released from $1\text{ m}$ to the right of equilibrium from rest ($x(0) = 1, x'(0) = 0$).
  1. Find the equation of motion $x(t)$.
  2. Determine the damping factor (amplitude envelope), the quasiperiod $P$, and the quasifrequency $f_d$.
  
  ---
#### **Problem 5: Overdamped Motion (Passing Through Equilibrium)**
  The motion of an overdamped mass-spring system is governed by:
  $$x''(t) + 7x'(t) + 10x(t) = 0; \quad x(0) = 1, \quad x'(0) = -6$$
  1. Find the displacement $x(t)$.
  2. Determine whether the mass ever passes through the equilibrium position ($x = 0$), and if so, calculate the exact time $t > 0$ when this occurs.
  
  ---
#### **Problem 6: Critically Damped Motion (Maximum Displacement)**
  A mass of $4\text{ kg}$ is attached to a spring with stiffness $k = 36\text{ N/m}$. The system is known to be **critically damped**. The mass is launched from its equilibrium position ($x(0) = 0$) with an initial velocity of $8\text{ m/s}$ in the positive direction.
  1. Find the damping constant $b$.
  2. Find the equation of motion $x(t)$.
  3. Find the maximum displacement attained by the mass and the time at which it occurs.
  
  ---
#### **Problem 7: Pure Resonance in an Undamped System**
  An undamped mass-spring system has mass $m = 1\text{ kg}$ and spring constant $k = 9\text{ N/m}$. An external force $F(t) = 6\cos(3t)\text{ N}$ acts on the system. The mass is initially at rest at the equilibrium position ($x(0) = 0, x'(0) = 0$).
  1. Explain why pure resonance occurs.
  2. Solve for the displacement $x(t)$ for $t \ge 0$.
  
  ---
#### **Problem 8: The Phenomenon of Beats**
  An undamped system with $m = 1\text{ kg}$ and $k = 25\text{ N/m}$ is acted upon by an external periodic force $F(t) = 9\cos(4t)\text{ N}$. The mass starts from rest at equilibrium ($x(0) = 0, x'(0) = 0$).
  1. Find the displacement $x(t)$.
  2. Use the product-to-sum trigonometric identity to rewrite $x(t)$ in the form $2A\sin(\omega_{\text{envelope}} t)\sin(\omega_{\text{osc}} t)$ and identify the beat frequency.
  
  ---
#### **Problem 9: Forced Damped Motion (Transient vs. Steady-State)**
  A mass-spring-damper system is modeled by:
  $$x''(t) + 4x'(t) + 13x(t) = 40\cos(t)$$
  1. Find the transient solution $x_{\text{tr}}(t)$ and the steady-state solution $x_{\text{sp}}(t)$.
  2. Determine the amplitude of the steady-state oscillations.
  
  ---
#### **Problem 10: Comprehensive Word Problem**
  An $8\text{-lb}$ weight stretches a vertical spring $2\text{ ft}$ to its equilibrium position. The surrounding medium exerts a damping force numerically equal to $0.75$ times the instantaneous velocity. Starting at $t = 0$, an external vertical periodic force $F(t) = 5\sin(2t)\text{ lb}$ acts on the mass. The mass is pulled down $6\text{ in}$ from equilibrium and given an initial upward velocity of $1\text{ ft/s}$.
  1. Formulate the complete initial value problem (taking downward as positive; $g = 32\text{ ft/s}^2$).
  2. Determine the steady-state response $x_{\text{sp}}(t)$ of the system.
  
  ---
  ---
# **Solutions**
  
  ---
### **Solution 1**
  1. **Initial Value Problem:**
   * $m = 2\text{ kg}$, $b = 0$, $k = 72\text{ N/m}$
   * Differential equation: $2x'' + 72x = 0 \implies x'' + 36x = 0$
   * Natural frequency: $\omega_0 = \sqrt{36} = 6\text{ rad/s}$
   * General solution: $x(t) = c_1 \cos(6t) + c_2 \sin(6t)$
   * Initial conditions: $x(0) = 0.5 = \frac{1}{2}\text{ m}$, $x'(0) = -3\text{ m/s}$ (left is negative)
     * $x(0) = c_1 = \frac{1}{2}$
     * $x'(t) = -6c_1 \sin(6t) + 6c_2 \cos(6t) \implies x'(0) = 6c_2 = -3 \implies c_2 = -\frac{1}{2}$
   
   $$x(t) = \frac{1}{2}\cos(6t) - \frac{1}{2}\sin(6t)$$
  
  2. **Amplitude-Phase Form ($x(t) = A\cos(\omega_0 t - \phi)$):**
   * Amplitude: $A = \sqrt{c_1^2 + c_2^2} = \sqrt{\left(\frac{1}{2}\right)^2 + \left(-\frac{1}{2}\right)^2} = \sqrt{\frac{2}{4}} = \frac{\sqrt{2}}{2}\text{ m}$
   * Phase angle: $\tan\phi = \frac{c_2}{c_1} = \frac{-1/2}{1/2} = -1$. Since $c_1 > 0$ and $c_2 < 0$, $\phi$ is in Quadrant IV:
     $$\phi = -\frac{\pi}{4}$$
   
   $$x(t) = \frac{\sqrt{2}}{2}\cos\left(6t + \frac{\pi}{4}\right)$$
  
  ---
### **Solution 2**
  1. **Determine Parameters:**
   * Mass: $m = \frac{W}{g} = \frac{64\text{ lb}}{32\text{ ft/s}^2} = 2\text{ slugs}$
   * Equilibrium stretch: $\Delta L = 6\text{ in} = \frac{6}{12}\text{ ft} = \frac{1}{2}\text{ ft}$
   * Spring constant: $k = \frac{W}{\Delta L} = \frac{64}{1/2} = 128\text{ lb/ft}$
  
  2. **Differential Equation & Initial Conditions:**
   $$2x'' + 128x = 0 \implies x'' + 64x = 0 \implies \omega_0 = \sqrt{64} = 8\text{ rad/s}$$
   * Initial displacement: $4\text{ in}$ downward $\implies x(0) = \frac{4}{12} = \frac{1}{3}\text{ ft}$
   * Initial velocity: $2\text{ ft/s}$ upward $\implies x'(0) = -2\text{ ft/s}$
  
  3. **Solve the IVP:**
   $$x(t) = c_1 \cos(8t) + c_2 \sin(8t)$$
   * $x(0) = c_1 = \frac{1}{3}$
   * $x'(0) = 8c_2 = -2 \implies c_2 = -\frac{1}{4}$
  
   $$x(t) = \frac{1}{3}\cos(8t) - \frac{1}{4}\sin(8t)\text{ ft}$$
  
  ---
### **Solution 3**
  1. **Critical Damping:**  
   Occurs when the discriminant of $m r^2 + b r + k = 0$ is zero:
   $$b^2 - 4mk = 0 \implies b_{\text{crit}} = 2\sqrt{mk} = 2\sqrt{1 \times 25} = 10\text{ N}\cdot\text{s/m}$$
  
  2. **For $b = 8\text{ N}\cdot\text{s/m}$:**  
   $b < b_{\text{crit}}$ ($8 < 10$) $\implies$ **Underdamped**
  
  3. **For $b = 14\text{ N}\cdot\text{s/m}$:**  
   $b > b_{\text{crit}}$ ($14 > 10$) $\implies$ **Overdamped**
  
  ---
### **Solution 4**
  1. **Equation of Motion:**
   $$x'' + 6x' + 25x = 0$$
   Characteristic roots:
   $$r = \frac{-6 \pm \sqrt{36 - 100}}{2} = -3 \pm 4i$$
   General solution:
   $$x(t) = e^{-3t}\left(c_1 \cos(4t) + c_2 \sin(4t)\right)$$
   Apply $x(0) = 1, x'(0) = 0$:
   * $x(0) = c_1 = 1$
   * $x'(t) = -3e^{-3t}(c_1 \cos 4t + c_2 \sin 4t) + e^{-3t}(-4c_1 \sin 4t + 4c_2 \cos 4t)$
   * $x'(0) = -3c_1 + 4c_2 = 0 \implies 4c_2 = 3 \implies c_2 = \frac{3}{4}$
  
   $$x(t) = e^{-3t}\left(\cos(4t) + \frac{3}{4}\sin(4t)\right)$$
  
  2. **Quasi-quantities:**
   * Amplitude $A = \sqrt{1^2 + (3/4)^2} = \frac{5}{4}$
   * **Damping factor (envelope):** $\frac{5}{4}e^{-3t}$
   * **Quasifrequency (angular):** $\omega_d = 4\text{ rad/s}$
   * **Quasiperiod:** $P = \frac{2\pi}{\omega_d} = \frac{2\pi}{4} = \frac{\pi}{2}\text{ s}$
   * **Quasifrequency (cyclic):** $f_d = \frac{1}{P} = \frac{2}{\pi}\text{ Hz}$
  
  ---
### **Solution 5**
  id:: 6ad69e21-34f1-407e-96a9-ab79df3f64ca
  1. **Displacement $x(t)$:**
   Characteristic equation:
   $$r^2 + 7r + 10 = (r + 2)(r + 5) = 0 \implies r_1 = -2, \; r_2 = -5$$
   $$x(t) = c_1 e^{-2t} + c_2 e^{-5t}$$
   Apply initial conditions:
   * $x(0) = c_1 + c_2 = 1$
   * $x'(0) = -2c_1 - 5c_2 = -6$
   
   Multiply first equation by $2$ and add:
   $$-3c_2 = -4 \implies c_2 = \frac{4}{3}, \quad c_1 = 1 - \frac{4}{3} = -\frac{1}{3}$$
  
   $$x(t) = -\frac{1}{3}e^{-2t} + \frac{4}{3}e^{-5t}$$
  
  2. **Equilibrium Crossing:**  
   Set $x(t) = 0$:
   $$-\frac{1}{3}e^{-2t} + \frac{4}{3}e^{-5t} = 0 \implies 4e^{-5t} = e^{-2t}$$
   Multiply by $e^{5t}$:
   $$4 = e^{3t} \implies 3t = \ln(4) \implies t = \frac{\ln(4)}{3} = \frac{2\ln(2)}{3}\text{ s} \approx 0.462\text{ s}$$
   **Yes**, the mass passes through equilibrium once at $t = \frac{\ln(4)}{3}\text{ s}$.
  
  ---
### **Solution 6**
  1. **Damping Constant:**  
   For critical damping:
   $$b = 2\sqrt{mk} = 2\sqrt{4 \times 36} = 24\text{ N}\cdot\text{s/m}$$
  
  2. **Equation of Motion:**
   $$4x'' + 24x' + 36x = 0 \implies x'' + 6x' + 9x = 0$$
   $$(r + 3)^2 = 0 \implies r = -3 \quad (\text{multiplicity } 2)$$
   $$x(t) = (c_1 + c_2 t)e^{-3t}$$
   Apply $x(0) = 0, x'(0) = 8$:
   * $x(0) = c_1 = 0$
   * $x'(t) = c_2 e^{-3t} - 3c_2 t e^{-3t} \implies x'(0) = c_2 = 8$
  
   $$x(t) = 8t e^{-3t}$$
  
  3. **Maximum Displacement:**  
   Set $x'(t) = 0$:
   $$8(1 - 3t)e^{-3t} = 0 \implies 1 - 3t = 0 \implies t_{\text{max}} = \frac{1}{3}\text{ s}$$
   Evaluate $x(1/3)$:
   $$x_{\text{max}} = 8\left(\frac{1}{3}\right)e^{-3(1/3)} = \frac{8}{3e}\text{ m} \approx 0.981\text{ m}$$
  
  ---
### **Solution 7**
  1. **Why Resonance Occurs:**  
   The natural frequency of the undamped system is $\omega_0 = \sqrt{\frac{k}{m}} = \sqrt{\frac{9}{1}} = 3\text{ rad/s}$. The external force has frequency $\omega = 3\text{ rad/s}$. Because the driving frequency matches the natural frequency ($\omega = \omega_0$) in the absence of damping, **pure resonance** occurs.
  
  2. **Displacement $x(t)$:**
   $$x'' + 9x = 6\cos(3t)$$
   Complementary solution: $x_h(t) = c_1 \cos(3t) + c_2 \sin(3t)$.  
   Particular solution trial form:
   $$x_p(t) = t\left(A\cos(3t) + B\sin(3t)\right)$$
   Substituting into $x'' + 9x$:
   $$-6A\sin(3t) + 6B\cos(3t) = 6\cos(3t) \implies B = 1, \; A = 0$$
   $$x_p(t) = t\sin(3t)$$
   General solution:
   $$x(t) = c_1 \cos(3t) + c_2 \sin(3t) + t\sin(3t)$$
   Apply $x(0) = 0, x'(0) = 0$:
   * $x(0) = c_1 = 0$
   * $x'(0) = 3c_2 + 0 = 0 \implies c_2 = 0$
  
   $$x(t) = t\sin(3t)$$
  
  ---
### **Solution 8**
  1. **Find $x(t)$:**
   $$x'' + 25x = 9\cos(4t)$$
   $\omega_0 = 5$, $\omega = 4$.  
   Trial solution for $x_p(t) = A\cos(4t)$:
   $$-16A\cos(4t) + 25A\cos(4t) = 9\cos(4t) \implies 9A = 9 \implies A = 1$$
   General solution:
   $$x(t) = c_1 \cos(5t) + c_2 \sin(5t) + \cos(4t)$$
   Apply $x(0) = 0, x'(0) = 0$:
   * $x(0) = c_1 + 1 = 0 \implies c_1 = -1$
   * $x'(0) = 5c_2 = 0 \implies c_2 = 0$
  
   $$x(t) = \cos(4t) - \cos(5t)$$
  
  2. **Beat Representation:**  
   Using $\cos(\alpha) - \cos(\beta) = 2\sin\left(\frac{\beta - \alpha}{2}\right)\sin\left(\frac{\alpha + \beta}{2}\right)$ with $\alpha = 4t$ and $\beta = 5t$:
   $$x(t) = 2\sin\left(\frac{t}{2}\right)\sin\left(\frac{9t}{2}\right)$$
   * **Envelope frequency (beat frequency):** $\omega_{\text{beat}} = \frac{|\omega_0 - \omega|}{2} = \frac{1}{2}\text{ rad/s}$
   * **Fast oscillation frequency:** $\omega_{\text{osc}} = \frac{\omega_0 + \omega}{2} = \frac{9}{2}\text{ rad/s}$
  
  ---
### **Solution 9**
  1. **Transient and Steady-State Solutions:**
   * **Transient solution $x_{\text{tr}}(t)$:**  
     Solve $r^2 + 4r + 13 = 0 \implies r = -2 \pm 3i$:
     $$x_{\text{tr}}(t) = e^{-2t}\left(c_1 \cos(3t) + c_2 \sin(3t)\right)$$
     *(As $t \to \infty$, $x_{\text{tr}}(t) \to 0$)*.
  
   * **Steady-state solution $x_{\text{sp}}(t)$:**  
     Trial form: $x_{\text{sp}}(t) = A\cos(t) + B\sin(t)$.
     $$x' = -A\sin(t) + B\cos(t), \quad x'' = -A\cos(t) - B\sin(t)$$
     Substitute into $x'' + 4x' + 13x = 40\cos(t)$:
     $$(12A + 4B)\cos(t) + (-4A + 12B)\sin(t) = 40\cos(t)$$
     System:
     $$\begin{cases} 12A + 4B = 40 \implies 3A + B = 10 \\ -4A + 12B = 0 \implies A = 3B \end{cases}$$
     Substitute $A = 3B$:
     $$3(3B) + B = 10 \implies 10B = 10 \implies B = 1, \quad A = 3$$
     
     $$x_{\text{sp}}(t) = 3\cos(t) + \sin(t)$$
  
  2. **Steady-State Amplitude:**
   $$A_{\text{steady}} = \sqrt{A^2 + B^2} = \sqrt{3^2 + 1^2} = \sqrt{10}$$
  
  ---
### **Solution 10**
  1. **Formulate the Initial Value Problem:**
   * Weight $W = 8\text{ lb} \implies m = \frac{8}{32} = \frac{1}{4}\text{ slug}$
   * Stretch $\Delta L = 2\text{ ft} \implies k = \frac{8}{2} = 4\text{ lb/ft}$
   * Damping: $b = 0.75 = \frac{3}{4}\text{ lb}\cdot\text{s/ft}$
   * External Force: $F(t) = 5\sin(2t)$
   * Initial displacement: $6\text{ in}$ down $\implies x(0) = \frac{6}{12} = \frac{1}{2}\text{ ft}$
   * Initial velocity: $1\text{ ft/s}$ up $\implies x'(0) = -1\text{ ft/s}$
  
   $$\frac{1}{4}x'' + \frac{3}{4}x' + 4x = 5\sin(2t), \quad x(0) = \frac{1}{2}, \quad x'(0) = -1$$
   *(or cleared of fractions: $x'' + 3x' + 16x = 20\sin(2t)$)*
  
  2. **Steady-State Response:**
   Using $x'' + 3x' + 16x = 20\sin(2t)$, assume $x_{\text{sp}}(t) = A\cos(2t) + B\sin(2t)$:
   $$x_{\text{sp}}'' + 3x_{\text{sp}}' + 16x_{\text{sp}} = (12A + 6B)\cos(2t) + (-6A + 12B)\sin(2t) = 20\sin(2t)$$
   * $12A + 6B = 0 \implies B = -2A$
   * $-6A + 12B = 20 \implies -6A + 12(-2A) = 20 \implies -30A = 20 \implies A = -\frac{2}{3}$
   * $B = -2\left(-\frac{2}{3}\right) = \frac{4}{3}$
  
   $$x_{\text{sp}}(t) = -\frac{2}{3}\cos(2t) + \frac{4}{3}\sin(2t)\text{ ft}$$