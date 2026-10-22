### 1. Derivation of the Governing Equation of Motion
  
  Consider a body of mass $m$ attached to a vertical flexible spring suspended from a rigid support.
  
  ```
     Rigid Support
        //////////
           | 
           |  Unstretched spring (length l)
           |
        ---+---
           | 
           |  Static elongation L  (due to gravity pulling weight W = mg)
           |
        =======  Equilibrium Position (x = 0)
           |
           |  Displacement x(t) from equilibrium
           v  [DOWNWARD is chosen as POSITIVE: x > 0]
        +-----+
        |  m  |
        +-----+
  ```
#### **Step A: Forces Acting on the Mass**
  1. **Gravity (Weight):** Pulls downward $\implies F_g = +mg$.
  2. **Spring Restoring Force ($F_s$):** By Hooke’s Law, the spring exerts a force opposing the total elongation $(L + x)$:
   $$F_s = -k(L + x) = -kL - kx$$
   where $k > 0$ is the **spring stiffness constant**.
  3. **Damping Force ($F_d$):** Air resistance or a dashpot opposes motion (resists velocity $x'$):
   $$F_d = -b x' \quad \text{(or } -\gamma x'\text{)}$$
   where $b \ge 0$ is the **damping coefficient**.
  4. **External Driving Force ($F_{\text{ext}}$):** An external force applied to the mass, $F_{\text{ext}}(t)$.
  
  ---
#### **Step B: Applying Newton’s Second Law**
  $$\sum F = m a = m x''(t)$$
  $$mg + F_s + F_d + F_{\text{ext}}(t) = m x''$$
  $$mg - k(L + x) - b x' + F_{\text{ext}}(t) = m x''$$
  $$m x'' + b x' + kx = \underbrace{(mg - kL)}_{= 0} + F_{\text{ext}}(t)$$
#### **Step C: The Static Equilibrium Cancellation**
  Before any motion begins, when the mass is hung on the spring and comes to rest at static equilibrium, the sum of forces is zero:
  $$\sum F_{\text{static}} = mg - kL = 0 \implies \mathbf{mg = kL \iff k = \frac{mg}{L} = \frac{\text{Weight}}{L}}$$
  Because the weight $mg$ exactly cancels the static spring tension $kL$, gravity drops out of the equation entirely!
#### **The Master Equation of Mechanical Motion:**
  $$\mathbf{m x''(t) + b x'(t) + k x(t) = F_{\text{ext}}(t)}$$
  $$\text{with initial conditions: } x(0) = x_0, \quad x'(0) = v_0$$
  
  ---
### 2. Units & Conversions: The #1 Source of Exam Errors
  
  Engineering problems use two different unit systems. Mixing them up causes immediate errors:
  
  | Quantity | SI (Metric) System | English (US Customary) System |
  |---|---|---|
  | **Displacement $x$, Elongation $L$** | Meters ($\text{m}$) | **Feet ($\text{ft}$)** *(Convert inches: $L = \frac{\text{in}}{12}$)* |
  | **Mass $m$** | Kilograms ($\text{kg}$) | **Slugs** $\left(m = \frac{\text{Weight}}{g} = \frac{W}{32}\right)$ |
  | **Weight $W$** | Newtons ($\text{N} = \text{kg}\cdot\text{m/s}^2$) | **Pounds ($\text{lb}$)** |
  | **Gravity $g$** | $9.8\text{ m/s}^2$ | **$32\text{ ft/s}^2$** |
  | **Spring Constant $k$** | $\text{N/m}$ | $\text{lb/ft}$ |
  | **Damping Constant $b$** | $\text{N}\cdot\text{s/m}$ (or $\text{kg/s}$) | $\text{lb}\cdot\text{s/ft}$ |
  
  > ⚠️ **CRUCIAL EXAM WARNING ON "POUNDS":**
  > In the English system, "pounds" ($\text{lb}$) is **weight/force**, **NOT mass**! 
  > If a problem says: *"a 8 lb weight is attached to a spring"*, then:
  > * $W = 8\text{ lb}$
  > * Mass is **$m = \frac{W}{g} = \frac{8}{32} = \frac{1}{4}\text{ slug}$**! 
  > If you put $m = 8$ in your differential equation, the problem will be marked wrong.
  
  ---
### 3. Classification of Free Vibrations ($F_{\text{ext}} = 0$)
  
  When there is no external driving force, the equation is homogeneous:
  $$m x'' + b x' + k x = 0$$
  Characteristic equation:
  $$m r^2 + b r + k = 0 \implies r = \frac{-b \pm \sqrt{b^2 - 4mk}}{2m}$$
  
  ---
#### **Category 1: Undamped Free Vibrations ($b = 0$) — Simple Harmonic Motion**
  $$m x'' + k x = 0 \implies x'' + \left(\frac{k}{m}\right)x = 0$$
  
  Define the **natural angular frequency**:
  $$\mathbf{\omega_0 = \sqrt{\frac{k}{m}}} \quad (\text{units: rad/s})$$
  
  * **Roots:** $r = \pm i\omega_0$
  * **General Solution:**
  $$\mathbf{x(t) = c_1 \cos(\omega_0 t) + c_2 \sin(\omega_0 t)}$$
  * **Natural Frequency (Cycles per second / Hertz):**
  $$f_0 = \frac{\omega_0}{2\pi}$$
  * **Period (Time for one complete cycle):**
  $$T = \frac{1}{f_0} = \frac{2\pi}{\omega_0}$$
  
  ---
#### **The Amplitude-Phase Form: $A\sin(\omega_0 t + \phi)$**
  While $c_1\cos(\omega_0 t) + c_2\sin(\omega_0 t)$ is easy to write, engineers prefer rewriting it as a **single sinusoid** to clearly identify the peak physical amplitude $A$ and phase shift $\phi$:
  
  $$x(t) = A\sin(\omega_0 t + \phi)$$
  
  Using the angle-addition identity $\sin(\alpha + \beta) = \sin\alpha\cos\beta + \cos\alpha\sin\beta$:
  $$A\sin(\omega_0 t + \phi) = \underbrace{(A\sin\phi)}_{c_1}\cos(\omega_0 t) + \underbrace{(A\cos\phi)}_{c_2}\sin(\omega_0 t)$$
  
  Matching coefficients:
  $$c_1 = A\sin\phi, \qquad c_2 = A\cos\phi$$
  
  * **Amplitude $A$:**
  $$c_1^2 + c_2^2 = A^2(\sin^2\phi + \cos^2\phi) = A^2 \implies \mathbf{A = \sqrt{c_1^2 + c_2^2}}$$
  * **Phase Angle $\phi$:**
  $$\frac{c_1}{c_2} = \frac{A\sin\phi}{A\cos\phi} = \tan\phi \implies \mathbf{\tan\phi = \frac{c_1}{c_2}}$$
  
  *(Alternative cosine form: $x(t) = A\cos(\omega_0 t - \delta)$ where $\tan\delta = \frac{c_2}{c_1}$).*
  
  ---
#### **Category 2: Damped Free Vibrations ($b > 0$)**
  The discriminant $\Delta = b^2 - 4mk$ dictates the physical behavior:
  
  ```
  DAMPING REGIMES:
  
  b^2 - 4mk > 0  ====> OVERDAMPED        (Heavy damping; sluggish return, no oscillation)
  b^2 - 4mk = 0  ====> CRITICALLY DAMPED (Fastest return to equilibrium without oscillating)
  b^2 - 4mk < 0  ====> UNDERDAMPED       (Light damping; oscillatory decay to zero)
  ```
##### **A. Overdamped ($b^2 - 4mk > 0 \iff b > 2\sqrt{mk}$)**
  * Roots: Two distinct negative real numbers ($r_1 < 0, \; r_2 < 0$).
  $$x(t) = c_1 e^{r_1 t} + c_2 e^{r_2 t}$$
  * **Motion:** The system is so sluggish that it slowly creeps back to equilibrium without ever oscillating. It can cross $x = 0$ at most once (depending on initial velocity).
##### **B. Critically Damped ($b^2 - 4mk = 0 \iff b = 2\sqrt{mk}$)**
  * Roots: One repeated negative real root $r = -\frac{b}{2m} < 0$.
  $$x(t) = (c_1 + c_2 t)e^{rt}$$
  * **Motion:** Represents the threshold of oscillation. It returns to equilibrium **faster than any overdamped system** without oscillating. (Used in car shock absorbers and automatic door closers).
##### **C. Underdamped ($b^2 - 4mk < 0 \iff b < 2\sqrt{mk}$)**
  * Roots: Complex conjugates $r = \alpha \pm i\beta$:
  $$\alpha = -\frac{b}{2m} < 0, \qquad \beta = \frac{\sqrt{4mk - b^2}}{2m} > 0$$
  * **General Solution:**
  $$\mathbf{x(t) = e^{\alpha t}\left(c_1\cos(\beta t) + c_2\sin(\beta t)\right) = A e^{-\frac{b}{2m}t}\sin(\beta t + \phi)}$$
  * **Motion:** Oscillates back and forth with diminishing amplitude inside an exponentially decaying envelope $\pm A e^{\alpha t}$.
  * **Quasi-frequency:** $\beta = \frac{\sqrt{4mk - b^2}}{2m} < \omega_0$ (damping slows down oscillation).
  * **Quasi-period:** $T_d = \frac{2\pi}{\beta}$.
  
  ---
### 4. Forced Vibrations & Resonance (§4.10)
  
  Now consider an external periodic force:
  $$m x'' + b x' + k x = F_0 \cos(\omega t)$$
  By linearity:
  $$x(t) = \underbrace{x_h(t)}_{\text{Transient Solution}} + \underbrace{x_p(t)}_{\text{Steady-State Solution}}$$
#### A. The Damped Forced Case ($b > 0$)
  * As $t \to \infty$, the damping causes the homogeneous part to die out:
  $$\lim_{t \to \infty} x_h(t) = 0 \implies \mathbf{x_h(t) \text{ is called the TRANSIENT solution.}}$$
  * The particular solution $x_p(t)$ does **not** die out; it oscillates forever at the driving frequency $\omega$:
  $$\mathbf{x_p(t) \text{ is called the STEADY-STATE solution.}}$$
  
  ---
#### B. The Undamped Forced Case ($b = 0$) & Pure Resonance
  $$m x'' + k x = F_0 \cos(\omega t) \iff x'' + \omega_0^2 x = \frac{F_0}{m}\cos(\omega t)$$
  
  * **Case 1: Driving Frequency $\neq$ Natural Frequency ($\omega \neq \omega_0$)**
  $$x(t) = c_1\cos(\omega_0 t) + c_2\sin(\omega_0 t) + \frac{F_0}{m(\omega_0^2 - \omega^2)}\cos(\omega t)$$
  * Motion remains bounded for all time. If $\omega \approx \omega_0$, this produces the phenomenon of **beats** (waxing and waning loudness).
  
  ---
#### **Case 2: Driving Frequency = Natural Frequency ($\omega = \omega_0$) $\implies$ RESONANCE!**
  *(Filling in the full mathematical derivation from Pages 3 & 4 of your handwritten notes)*
  
  When $\omega = \omega_0$, the forcing term $F_0\cos(\omega_0 t)$ is an exact match for the complementary solution $x_h = c_1\cos(\omega_0 t) + c_2\sin(\omega_0 t)$.
  
  By the **Method of Undetermined Coefficients Duplication Rule**:
  $$x_p(t) = t\left(A\cos(\omega_0 t) + B\sin(\omega_0 t)\right) = At\cos(\omega_0 t) + Bt\sin(\omega_0 t)$$
  
  Differentiating via Product Rule:
  $$x_p'(t) = A\cos(\omega_0 t) - A\omega_0 t\sin(\omega_0 t) + B\sin(\omega_0 t) + B\omega_0 t\cos(\omega_0 t)$$
  $$\begin{aligned}
  x_p''(t) &= -A\omega_0\sin(\omega_0 t) - A\omega_0\left[\sin(\omega_0 t) + \omega_0 t\cos(\omega_0 t)\right] \\
  &\quad + B\omega_0\cos(\omega_0 t) + B\omega_0\left[\cos(\omega_0 t) - \omega_0 t\sin(\omega_0 t)\right] \\
  &= -2A\omega_0\sin(\omega_0 t) - A\omega_0^2 t\cos(\omega_0 t) + 2B\omega_0\cos(\omega_0 t) - B\omega_0^2 t\sin(\omega_0 t)
  \end{aligned}$$
  
  Substitute into $m x_p'' + k x_p = F_0\cos(\omega_0 t)$:
  $$\begin{aligned}
  & m\left[-2A\omega_0\sin(\omega_0 t) - A\omega_0^2 t\cos(\omega_0 t) + 2B\omega_0\cos(\omega_0 t) - B\omega_0^2 t\sin(\omega_0 t)\right] \\
  + & k\left[At\cos(\omega_0 t) + Bt\sin(\omega_0 t)\right] = F_0\cos(\omega_0 t)
  \end{aligned}$$
  
  Group terms:
  $$(-2Am\omega_0)\sin(\omega_0 t) + (2Bm\omega_0)\cos(\omega_0 t) + \underbrace{(-Am\omega_0^2 + kA)}_{= 0 \text{ since } \omega_0^2 = k/m} t\cos(\omega_0 t) + \underbrace{(-Bm\omega_0^2 + kB)}_{= 0} t\sin(\omega_0 t) = F_0\cos(\omega_0 t)$$
  
  Match coefficients:
  * For $\sin(\omega_0 t)$: $-2Am\omega_0 = 0 \implies \mathbf{A = 0}$
  * For $\cos(\omega_0 t)$: $2Bm\omega_0 = F_0 \implies \mathbf{B = \frac{F_0}{2m\omega_0}}$
#### **The Resonant Particular Solution:**
  $$\mathbf{x_p(t) = \frac{F_0}{2m\omega_0} t \sin(\omega_0 t)}$$
  
  ```
  x(t) ^           Linear Envelope: +(F0 / 2m*w0) * t
      |        /\          /\
      |       /  \        /  \
      |      /    \  /\  /    \
  ----+-----/------\/--\/------\-----------------------------> t
      |    /        /\  /\      \
      |   /        /  \/  \      \
      |  /        /        \      \
                 Linear Envelope: -(F0 / 2m*w0) * t
  ```
  
  > **Physical Significance of Pure Resonance:**
  > Because of the multiplying factor of $t$, the amplitude grows without bound:
  > $$\lim_{t \to \infty} |x_p(t)| = \infty$$
  > Real physical springs will stretch beyond their elastic limit and break (or shatter, as in the Tacoma Narrows Bridge collapse).
  
  ---
### 5. Fully Worked Case Studies from Your Handwritten Notes
  
  ---
#### **Example 1: Setting up an English Units IVP (Notes PDF 3, Page 2)**
  *Problem Statement:* A spring hanging vertically from a rigid support is stretched $6\text{ inches}$ by an $8\text{ lb}$ weight. The mass is in a medium with a damping constant of $0.25\text{ lb}\cdot\text{s/ft}$ and is acted upon by an external downward force of $4\cos(2t)\text{ lb}$. The mass is displaced an additional $2\text{ feet}$ in the **upward** direction and then **released**. Write, but do not solve, the initial value problem.
  
  * **Step 1: Compute Mass $m$:**
  $$m = \frac{W}{g} = \frac{8\text{ lb}}{32\text{ ft/s}^2} = \mathbf{\frac{1}{4}\text{ slug}}$$
  * **Step 2: Convert Elongation and Find $k$:**
  $$L = 6\text{ inches} = \frac{6}{12}\text{ ft} = 0.5\text{ ft}$$
  $$k = \frac{W}{L} = \frac{8\text{ lb}}{0.5\text{ ft}} = \mathbf{16\text{ lb/ft}}$$
  * **Step 3: Damping and Forcing:**
  $$b = 0.25, \qquad F_{\text{ext}}(t) = 4\cos(2t)$$
  * **Step 4: Initial Conditions (Direction convention: Down is $+$):**
  * Displaced $2\text{ ft}$ **upward** $\implies x(0) = \mathbf{-2}$
  * "Released" from rest $\implies x'(0) = \mathbf{0}$
  * **Final Formulated IVP:**
  $$\mathbf{\frac{1}{4}x'' + 0.25x' + 16x = 4\cos(2t), \quad x(0) = -2, \; x'(0) = 0}$$
  
  ---
#### **Example 2: Metric Units & Amplitude-Phase Form (Notes PDF 3, Pages 3–5)**
  *Problem Statement:* A $\frac{1}{8}\text{ kg}$ mass is attached to a horizontal spring with stiffness $k = 16\text{ N/m}$. The mass is displaced $\frac{1}{2}\text{ m}$ to the right of equilibrium and given an outward velocity (to the right) of $\sqrt{2}\text{ m/s}$. Neglecting damping and external forces, determine:
  1. Angular frequency $\omega_0$, natural frequency $f_0$, and period $T$.
  2. The equation of motion $x(t)$.
  3. Amplitude $A$ and phase angle $\phi$ in the form $x(t) = A\sin(\omega_0 t + \phi)$.
  
  * **Part 1: Frequency and Period:**
  $$\omega_0 = \sqrt{\frac{k}{m}} = \sqrt{\frac{16}{1/8}} = \sqrt{128} = \sqrt{64 \cdot 2} = \mathbf{8\sqrt{2}\text{ rad/s}}$$
  $$f_0 = \frac{\omega_0}{2\pi} = \frac{8\sqrt{2}}{2\pi} = \mathbf{\frac{4\sqrt{2}}{\pi}\text{ Hz}}$$
  $$T = \frac{1}{f_0} = \frac{2\pi}{\omega_0} = \frac{2\pi}{8\sqrt{2}} = \mathbf{\frac{\sqrt{2}\pi}{8}\text{ seconds}}$$
  
  * **Part 2: Initial Value Problem & General Solution:**
  $$\frac{1}{8}x'' + 16x = 0 \implies x(t) = c_1\cos(8\sqrt{2}t) + c_2\sin(8\sqrt{2}t)$$
  * Use $x(0) = \frac{1}{2}$:
    $$c_1 = \mathbf{\frac{1}{2}}$$
  * Compute velocity $x'(t)$:
    $$x'(t) = -8\sqrt{2}c_1\sin(8\sqrt{2}t) + 8\sqrt{2}c_2\cos(8\sqrt{2}t)$$
  * Use $x'(0) = \sqrt{2}$:
    $$8\sqrt{2}c_2 = \sqrt{2} \implies c_2 = \mathbf{\frac{1}{8}}$$
  * Equation of motion:
    $$\mathbf{x(t) = \frac{1}{2}\cos(8\sqrt{2}t) + \frac{1}{8}\sin(8\sqrt{2}t)}$$
  
  * **Part 3: Amplitude & Phase Angle:**
  * **Amplitude:**
    $$A = \sqrt{c_1^2 + c_2^2} = \sqrt{\left(\frac{1}{2}\right)^2 + \left(\frac{1}{8}\right)^2} = \sqrt{\frac{1}{4} + \frac{1}{64}} = \sqrt{\frac{17}{64}} = \mathbf{\frac{\sqrt{17}}{8}\text{ m}}$$
  * **Phase Angle:**
    $$\tan\phi = \frac{c_1}{c_2} = \frac{1/2}{1/8} = 4$$
    Since both $c_1 > 0$ and $c_2 > 0$, $\phi$ is in **Quadrant I**:
    $$\phi = \arctan(4) \approx \mathbf{1.326\text{ radians}}$$
  * **Single Sinusoid Form:**
    $$\mathbf{x(t) = \frac{\sqrt{17}}{8}\sin(8\sqrt{2}t + 1.326)}$$
  
  ---
### Mechanical Vibrations Summary & Exam Checklist
  * [x] **Mass in English Units:** Always use $m = \frac{\text{Weight}}{32}$.
  * [x] **Stretch $L$ in Feet:** If stretch is in inches, divide by 12 before computing $k = \frac{W}{L}$.
  * [x] **Sign Conventions:** If downward is positive:
  * "Lifted upward" $\implies x(0) < 0$.
  * "Displaced downward" $\implies x(0) > 0$.
  * "Released from rest" $\implies x'(0) = 0$.
  * [x] **Damping Regimes:**
  * $b^2 - 4mk > 0 \implies$ Overdamped (exponentials).
  * $b^2 - 4mk = 0 \implies$ Critically damped ($(c_1 + c_2 t)e^{rt}$).
  * $b^2 - 4mk < 0 \implies$ Underdamped ($e^{\alpha t}$ times sinusoids).
  * [x] **Resonance Condition:** Occurs when undamped system ($b = 0$) is driven at $\omega = \omega_0 = \sqrt{k/m}$, producing $x_p = \frac{F_0}{2m\omega_0} t\sin(\omega_0 t)$.
  
  ---