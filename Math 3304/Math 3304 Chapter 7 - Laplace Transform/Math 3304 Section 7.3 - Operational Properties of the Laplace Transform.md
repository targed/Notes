### 1. Conceptual Overview: Transforming Calculus into Algebra
  
  In Section 7.2, you learned the definition of the Laplace transform and how to transform basic functions. 
  
  Section 7.3 introduces the **operational properties** that give the Laplace transform its true power. These theorems establish three fundamental mathematical dualities between the **Time Domain ($t$)** and the **Frequency Domain ($s$)**:
  
  ```
  +-------------------------------------------------------------------------------------------------+
  |                                TIME DOMAIN vs. FREQUENCY DOMAIN                                 |
  |                                                                                                 |
  |       Operation in Time Domain (t)                      Equivalent in Frequency Domain (s)      |
  |  ---------------------------------------               ------------------------------------     |
  |   1. Differentiation:      d/dt [ f(t) ]       <===>    Multiplication by s  [ s F(s) - f(0) ]  |
  |   2. Damping Envelope:     e^(at) * f(t)       <===>    Horizontal Shift     [ F(s - a) ]       |
  |   3. Multiplied by time:   t * f(t)            <===>    Differentiation      [ -d/ds F(s) ]     |
  +-------------------------------------------------------------------------------------------------+
  ```
  
  ---
### 2. Property 1: Transforms of Derivatives
  
  This is the central theorem that allows us to solve differential equations. It converts derivatives into simple multiplications by powers of $s$.
  
  ---
#### **Theorem A: First Derivative Formula**
  If $f(t)$ is continuous and exponentially bounded, and $f'(t)$ is piecewise continuous on $[0, \infty)$:
  
  $$\mathbf{\mathcal{L}\{f'(t)\} = s F(s) - f(0)}$$
  
  where $F(s) = \mathcal{L}\{f(t)\}$.
#### **Theoretical Proof via Integration by Parts (Notes Page 2):**
  By definition:
  $$\mathcal{L}\{f'(t)\} = \int_0^\infty e^{-st} f'(t)\,dt$$
  Use Integration by Parts:
  $$\begin{aligned}
  u &= e^{-st} &\implies& du = -s e^{-st}\,dt \\
  dv &= f'(t)\,dt &\implies& v = f(t)
  \end{aligned}$$
  
  $$\begin{aligned}
  \int_0^\infty e^{-st} f'(t)\,dt &= \left[ e^{-st} f(t) \right]_0^\infty - \int_0^\infty \left(-s e^{-st}\right) f(t)\,dt \\
  &= \lim_{T \to \infty} \left[ e^{-sT} f(T) - e^0 f(0) \right] + s \int_0^\infty e^{-st} f(t)\,dt
  \end{aligned}$$
  
  Because $f(t)$ is of exponential order ($|f(t)| \le M e^{ct}$), for any $s > c$:
  $$\lim_{T \to \infty} e^{-sT} f(T) = 0$$
  Therefore:
  $$\mathcal{L}\{f'(t)\} = \left(0 - f(0)\right) + s \underbrace{\int_0^\infty e^{-st} f(t)\,dt}_{= F(s)} = \mathbf{s F(s) - f(0)} \quad \blacksquare$$
  
  ---
#### **Theorem B: Second Derivative Formula**
  To transform $f''(t)$, apply the first-derivative rule to the function $g(t) = f'(t)$:
  $$\mathcal{L}\{f''(t)\} = \mathcal{L}\{(f')'(t)\} = s \mathcal{L}\{f'(t)\} - f'(0)$$
  Substitute $\mathcal{L}\{f'(t)\} = s F(s) - f(0)$:
  $$= s\left[ s F(s) - f(0) \right] - f'(0)$$
  
  $$\mathbf{\mathcal{L}\{f''(t)\} = s^2 F(s) - s f(0) - f'(0)}$$
  
  ---
#### **Theorem C: General $n$-th Derivative Formula**
  Applying the rule repeatedly by mathematical induction yields the general result for an $n$-th order derivative:
  
  $$\mathbf{\mathcal{L}\{f^{(n)}(t)\} = s^n F(s) - s^{n-1}f(0) - s^{n-2}f'(0) - s^{n-3}f''(0) - \dots - f^{(n-1)}(0)}$$
#### **Notice the Distinct Sign and Power Pattern:**
  1. The powers of $s$ **decrease by 1** with each successive term: $s^n \to s^{n-1} \to s^{n-2} \to \dots \to s^0 = 1$.
  2. The order of derivatives in the initial conditions **increases by 1**: $f(0) \to f'(0) \to f''(0) \to \dots \to f^{(n-1)}(0)$.
  3. The very first term is positive; **every subsequent initial condition term is subtracted (negative)**.
  
  > ⚡ **CRITICAL EXAM TAKEAWAY:**
  > When using Laplace transforms, **initial conditions are incorporated at Step 1**! 
  > You do not have to solve for arbitrary constants $c_1, c_2$ at the very end like you did in Chapters 4 and 6.
  
  ---
### 3. Property 2: The First Translation (Frequency Shifting) Theorem
  
  What happens in the frequency domain when a function is multiplied by an exponential $e^{at}$ in the time domain?
  
  > **Theorem (First Translation / Shift on the $s$-axis — Notes Page 3):**
  > If $\mathcal{L}\{f(t)\} = F(s)$ for $s > c$, then for any real number $a$:
  > 
  > $$\mathbf{\mathcal{L}\{e^{at} f(t)\} = F(s - a), \quad s > c + a}$$
#### **Proof from Definition:**
  $$\mathcal{L}\{e^{at} f(t)\} = \int_0^\infty e^{-st} \left(e^{at} f(t)\right)\,dt = \int_0^\infty e^{-(s - a)t} f(t)\,dt$$
  Notice this is the exact definition of $F(s)$, with $s$ replaced everywhere by $(s - a)$:
  $$= F(s - a) \quad \blacksquare$$
  
  ---
#### **Immediate Applications of the Frequency Shift Theorem:**
  
  1. **Damped Powers of $t$ (Entry 8):**
   * Since $\mathcal{L}\{t^n\} = \frac{n!}{s^{n+1}}$, replacing $s \to s - a$ gives:
     $$\mathbf{\mathcal{L}\{e^{at} t^n\} = \frac{n!}{(s - a)^{n+1}}}$$
   * *Example:* $\mathcal{L}\{e^{-3t} t^2\} = \frac{2!}{(s - (-3))^{2+1}} = \frac{2}{(s + 3)^3}$.
  
  2. **Damped Cosine (Entry 10):**
   * Since $\mathcal{L}\{\cos(bt)\} = \frac{s}{s^2 + b^2}$, replacing $s \to s - a$ gives:
     $$\mathbf{\mathcal{L}\{e^{at} \cos(bt)\} = \frac{s - a}{(s - a)^2 + b^2}}$$
  
  3. **Damped Sine (Entry 9):**
   * Since $\mathcal{L}\{\sin(bt)\} = \frac{b}{s^2 + b^2}$, replacing $s \to s - a$ gives:
     $$\mathbf{\mathcal{L}\{e^{at} \sin(bt)\} = \frac{b}{(s - a)^2 + b^2}}$$
  
  ---
### 4. Property 3: Derivatives of Transforms (Multiplication by $t^n$)
  
  What happens when a function $f(t)$ is multiplied by $t$ or powers of $t$?
  
  > **Theorem (Differentiation in the $s$-Domain — Notes Page 4):**
  > If $\mathcal{L}\{f(t)\} = F(s)$, then:
  > 
  > $$\mathbf{\mathcal{L}\{t f(t)\} = -\frac{d}{ds}F(s) = -F'(s)}$$
  > 
  > In general, for any positive integer $n$:
  > $$\mathbf{\mathcal{L}\{t^n f(t)\} = (-1)^n \frac{d^n}{ds^n}F(s)}$$
  
  ---
#### **Theoretical Proof via Leibniz's Integral Rule (Notes Page 4):**
  $$\frac{d}{ds}F(s) = \frac{d}{ds}\left[ \int_0^\infty e^{-st} f(t)\,dt \right]$$
  Differentiating under the integral sign with respect to the parameter $s$:
  $$\frac{d}{ds}F(s) = \int_0^\infty \frac{\partial}{\partial s}\left(e^{-st}\right) f(t)\,dt = \int_0^\infty \left(-t e^{-st}\right) f(t)\,dt = -\int_0^\infty e^{-st} \left[t f(t)\right]\,dt$$
  Recognize that the remaining integral is precisely the definition of $\mathcal{L}\{t f(t)\}$:
  $$\frac{d}{ds}F(s) = -\mathcal{L}\{t f(t)\} \implies \mathbf{\mathcal{L}\{t f(t)\} = -\frac{d}{ds}F(s)} \quad \blacksquare$$
  
  ---
#### **Worked Example 1: Transforming $t\cos(bt)$ (Notes Page 4)**
  Find $\mathcal{L}\{t\cos(bt)\}$:
  1. Identify $f(t) = \cos(bt) \implies F(s) = \frac{s}{s^2 + b^2}$.
  2. Apply the theorem:
   $$\mathcal{L}\{t\cos(bt)\} = -\frac{d}{ds}\left[ \frac{s}{s^2 + b^2} \right]$$
  3. Use the **Quotient Rule**:
   $$\frac{d}{ds}\left[ \frac{s}{s^2 + b^2} \right] = \frac{(s^2 + b^2)\frac{d}{ds}(s) - s \frac{d}{ds}(s^2 + b^2)}{(s^2 + b^2)^2} = \frac{(s^2 + b^2)(1) - s(2s)}{(s^2 + b^2)^2} = \frac{s^2 + b^2 - 2s^2}{(s^2 + b^2)^2} = \frac{b^2 - s^2}{(s^2 + b^2)^2}$$
  4. Multiply by the negative sign:
   $$\mathcal{L}\{t\cos(bt)\} = -\left[ \frac{b^2 - s^2}{(s^2 + b^2)^2} \right] = \mathbf{\frac{s^2 - b^2}{(s^2 + b^2)^2}}$$
  
  ---
#### **Worked Example 2: Transforming $t\sin(bt)$ (Resonance Term)**
  Find $\mathcal{L}\{t\sin(bt)\}$:
  1. Identify $f(t) = \sin(bt) \implies F(s) = \frac{b}{s^2 + b^2} = b(s^2 + b^2)^{-1}$.
  2. Apply the theorem:
   $$\mathcal{L}\{t\sin(bt)\} = -\frac{d}{ds}\left[ b(s^2 + b^2)^{-1} \right]$$
  3. Differentiate using the Chain Rule:
   $$= -\left[ -b(s^2 + b^2)^{-2} \cdot (2s) \right] = \mathbf{\frac{2bs}{(s^2 + b^2)^2}}$$
  *(Notice that this matches the exact transform needed when undamped resonance occurs!).*
  
  ---
### 5. Connecting It All: Transforming an Initial Value Problem
  
  Let's see how these properties combine to transform a differential equation into a clean algebraic equation.
#### **Walkthrough: Transforming a 2nd-Order IVP**
  Apply the Laplace transform to the IVP:
  $$y'' + 4y = 6e^{2t}, \quad y(0) = 2, \quad y'(0) = -1$$
  
  1. **Take the Laplace transform of both sides:**
   $$\mathcal{L}\{y''\} + 4\mathcal{L}\{y\} = 6\mathcal{L}\{e^{2t}\}$$
  2. **Apply Derivative and Exponential Rules:**
   * $\mathcal{L}\{y''\} = s^2 Y(s) - s y(0) - y'(0)$
   * $\mathcal{L}\{y\} = Y(s)$
   * $\mathcal{L}\{e^{2t}\} = \frac{1}{s - 2}$
  3. **Substitute initial values $y(0) = 2$ and $y'(0) = -1$:**
   $$\left[ s^2 Y(s) - s(2) - (-1) \right] + 4 Y(s) = \frac{6}{s - 2}$$
   $$s^2 Y(s) - 2s + 1 + 4 Y(s) = \frac{6}{s - 2}$$
  4. **Group $Y(s)$ terms and move initial conditions to the right:**
   $$(s^2 + 4)Y(s) - (2s - 1) = \frac{6}{s - 2}$$
   $$\mathbf{(s^2 + 4)Y(s) = 2s - 1 + \frac{6}{s - 2}}$$
  5. **Notice the Structure:**
   * The coefficient of $Y(s)$ is **$s^2 + 4$**, which is the exact **characteristic polynomial** of the homogeneous equation!
   * Solving for $Y(s)$ requires only dividing by $(s^2 + 4)$:
     $$\mathbf{Y(s) = \frac{2s - 1}{s^2 + 4} + \frac{6}{(s^2 + 4)(s - 2)}}$$
  *(To finish the problem, you take the Inverse Laplace Transform of $Y(s)$ using the methods of Section 7.4).*
  
  ---
### Summary of Section 7.3 Operational Properties
  
  | Time Domain Operation | Frequency Domain Equivalent | Formula |
  |---|---|---|
  | **First Derivative** | Multiply by $s$, subtract initial value | $\mathcal{L}\{f'(t)\} = s F(s) - f(0)$ |
  | **Second Derivative** | Multiply by $s^2$, subtract initial values | $\mathcal{L}\{f''(t)\} = s^2 F(s) - s f(0) - f'(0)$ |
  | **$n$-th Derivative** | Polynomial in $s$ with initial conditions | $\mathcal{L}\{f^{(n)}\} = s^n F(s) - \sum_{k=1}^n s^{n-k} f^{(k-1)}(0)$ |
  | **Exponential Damping $e^{at}f(t)$** | Horizontal shift by $a$ in $s$ | $\mathcal{L}\{e^{at}f(t)\} = F(s - a)$ |
  | **Multiply by $t$** | Negative first derivative in $s$ | $\mathcal{L}\{t f(t)\} = -F'(s)$ |
  | **Multiply by $t^n$** | Alternating $n$-th derivative in $s$ | $\mathcal{L}\{t^n f(t)\} = (-1)^n F^{(n)}(s)$ |
  
  ---
### Section 7.3 Pro-Tips & Exam Traps Checklist
  * [x] **Watch the Minus Signs in Initial Conditions:** In $\mathcal{L}\{y''\} = s^2 Y(s) - s y(0) - y'(0)$, both initial condition terms have negative signs. If $y'(0) = -3$, then $-y'(0) = -(-3) = +3$.
  * [x] **Sign Flip in Shifting:** Multiplying by $e^{\mathbf{+a}t}$ causes a shift of $s \mathbf{- a}$. Multiplying by $e^{\mathbf{-a}t}$ causes a shift of $s \mathbf{+ a}$.
  * [x] **Remember the Minus Sign for $t f(t)$:** Do not forget the $(-1)$ when differentiating $F(s)$!
  * [x] **Characteristic Polynomial Check:** When transforming $a y'' + b y' + c y$, the coefficient multiplying $Y(s)$ must **always** be $a s^2 + b s + c$. If it isn't, you made a copying error.
  
  ---