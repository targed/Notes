### **Problem 1 (Direct Exam Match — Page 2 of your Past Exam)**
  Find the general solution for $t > 0$ of:
  $$y'' - \frac{4}{t}y' + \frac{6}{t^2}y = t + \frac{1}{t^2}$$
  given that $y_1(t) = t^2$ and $y_2(t) = t^3$ are linearly independent solutions of the associated homogeneous equation.
#### **Solution:**
  * **Step 0: Check Standard Form:**
  The coefficient of $y''$ is $1$. The equation is already in standard form, so:
  $$g(t) = t + \frac{1}{t^2} = t + t^{-2}$$
  * **Step 1: Compute the Wronskian:**
  $$W(y_1, y_2)(t) = \begin{vmatrix} t^2 & t^3 \\ 2t & 3t^2 \end{vmatrix} = (t^2)(3t^2) - (2t)(t^3) = 3t^4 - 2t^4 = \mathbf{t^4}$$
  * **Step 2: Find $u_1(t)$:**
  $$u_1'(t) = -\frac{y_2(t) g(t)}{W(t)} = -\frac{t^3 (t + t^{-2})}{t^4} = -\frac{t^4 + t}{t^4} = -\left(1 + \frac{1}{t^3}\right) = -1 - t^{-3}$$
  $$u_1(t) = \int (-1 - t^{-3})\,dt = -t - \frac{t^{-2}}{-2} = \mathbf{-t + \frac{1}{2t^2}}$$
  * **Step 3: Find $u_2(t)$:**
  $$u_2'(t) = \frac{y_1(t) g(t)}{W(t)} = \frac{t^2 (t + t^{-2})}{t^4} = \frac{t^3 + 1}{t^4} = \frac{1}{t} + t^{-4}$$
  $$u_2(t) = \int \left(\frac{1}{t} + t^{-4}\right)\,dt = \mathbf{\ln(t) - \frac{1}{3t^3}} \quad (t > 0)$$
  * **Step 4: Form the Particular Solution $y_p(t)$:**
  $$y_p(t) = u_1 y_1 + u_2 y_2 = \left(-t + \frac{1}{2t^2}\right)t^2 + \left(\ln(t) - \frac{1}{3t^3}\right)t^3$$
  $$y_p(t) = -t^3 + \frac{1}{2} + t^3 \ln(t) - \frac{1}{3} = t^3 \ln(t) - t^3 + \frac{1}{6}$$
  *Notice that $-t^3$ duplicates $c_2 y_2 = c_2 t^3$ and can be absorbed into the constant.*
  * **Step 5: Write the General Solution:**
  $$\mathbf{y(t) = c_1 t^2 + c_2 t^3 + t^3 \ln(t) + \frac{1}{6}}$$
  
  ---
### **Problem 2 (Review Sheet Match — Page 3, Problem 2)**
  Given that $y_1(t) = t$ and $y_2(t) = e^{-t}$ are solutions to $(t + 1)y'' + t y' - y = 0$ on $t > -1$, use Variation of Parameters to find the general solution of:
  $$(t + 1)y'' + t y' - y = (t + 1)^2, \quad t > -1$$
#### **Solution:**
  * **Step 0: MANDATORY Standard Form:**
  Divide the entire equation by $(t + 1)$:
  $$y'' + \frac{t}{t+1}y' - \frac{1}{t+1}y = \frac{(t+1)^2}{t+1} = t + 1 \implies \mathbf{g(t) = t + 1}$$
  * **Step 1: Compute the Wronskian:**
  $$W(y_1, y_2)(t) = \begin{vmatrix} t & e^{-t} \\ 1 & -e^{-t} \end{vmatrix} = t(-e^{-t}) - (1)(e^{-t}) = -t e^{-t} - e^{-t} = \mathbf{-(t + 1)e^{-t}}$$
  * **Step 2: Find $u_1(t)$:**
  $$u_1'(t) = -\frac{y_2 g}{W} = -\frac{e^{-t}(t + 1)}{-(t + 1)e^{-t}} = 1 \implies \mathbf{u_1(t) = t}$$
  * **Step 3: Find $u_2(t)$:**
  $$u_2'(t) = \frac{y_1 g}{W} = \frac{t(t + 1)}{-(t + 1)e^{-t}} = -\frac{t}{e^{-t}} = -t e^t$$
  Integrate by parts ($\int -t e^t dt$):
  $$\mathbf{u_2(t) = -t e^t + e^t = (1 - t)e^t}$$
  * **Step 4: Form $y_p(t)$:**
  $$y_p(t) = u_1 y_1 + u_2 y_2 = (t)(t) + (1 - t)e^t (e^{-t}) = t^2 + (1 - t)(1) = \mathbf{t^2 - t + 1}$$
  *Notice that $-t$ duplicates $c_1 y_1 = c_1 t$ and is absorbed.*
  * **Step 5: General Solution:**
  $$\mathbf{y(t) = c_1 t + c_2 e^{-t} + t^2 + 1}$$
  
  ---
### **Problem 3 (Review Sheet Match — Page 4, Problem 3)**
  Find the general solution for $t > 0$ of:
  $$t y'' + (1 - 2t)y' + (t - 1)y = t e^t$$
  given that $y_1(t) = e^t$ and $y_2(t) = e^t \ln(t)$ are linearly independent solutions of the homogeneous equation.
#### **Solution:**
  * **Step 0: Standard Form:** Divide by leading coefficient $t$:
  $$y'' + \left(\frac{1 - 2t}{t}\right)y' + \left(\frac{t - 1}{t}\right)y = \frac{t e^t}{t} = e^t \implies \mathbf{g(t) = e^t}$$
  * **Step 1: Compute the Wronskian:**
  $$y_1' = e^t, \qquad y_2' = e^t \ln(t) + \frac{e^t}{t}$$
  $$W = e^t\left(e^t \ln(t) + \frac{e^t}{t}\right) - (e^t)(e^t \ln(t)) = e^{2t}\ln(t) + \frac{e^{2t}}{t} - e^{2t}\ln(t) = \mathbf{\frac{e^{2t}}{t}}$$
  * **Step 2: Find $u_1(t)$:**
  $$u_1'(t) = -\frac{y_2 g}{W} = -\frac{(e^t \ln t)(e^t)}{\frac{e^{2t}}{t}} = -\frac{e^{2t}\ln(t)}{\frac{e^{2t}}{t}} = -t\ln(t)$$
  Integrate by parts:
  $$u_1(t) = -\int t\ln(t)\,dt = -\left[\frac{1}{2}t^2 \ln(t) - \frac{1}{4}t^2\right] = \mathbf{-\frac{1}{2}t^2 \ln(t) + \frac{1}{4}t^2}$$
  * **Step 3: Find $u_2(t)$:**
  $$u_2'(t) = \frac{y_1 g}{W} = \frac{(e^t)(e^t)}{\frac{e^{2t}}{t}} = \frac{e^{2t}}{\frac{e^{2t}}{t}} = t \implies \mathbf{u_2(t) = \frac{1}{2}t^2}$$
  * **Step 4: Form $y_p(t)$:**
  $$y_p(t) = \left(-\frac{1}{2}t^2 \ln(t) + \frac{1}{4}t^2\right)e^t + \left(\frac{1}{2}t^2\right)e^t \ln(t) = \mathbf{\frac{1}{4}t^2 e^t}$$
  * **Step 5: General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 e^t \ln(t) + \frac{1}{4}t^2 e^t}$$
  
  ---
### **Problem 4 (Cauchy-Euler with Logarithmic RHS)**
  Find the general solution for $t > 0$ of:
  $$t^2 y'' - t y' + y = 4t\ln(t)$$
  given that $y_1(t) = t$ and $y_2(t) = t\ln(t)$ are solutions of the homogeneous equation.
#### **Solution:**
  * **Step 0: Standard Form:** Divide by $t^2$:
  $$y'' - \frac{1}{t}y' + \frac{1}{t^2}y = \frac{4t\ln(t)}{t^2} \implies \mathbf{g(t) = \frac{4\ln(t)}{t}}$$
  * **Step 1: Compute the Wronskian:**
  $$W = \begin{vmatrix} t & t\ln(t) \\ 1 & \ln(t) + 1 \end{vmatrix} = t(\ln t + 1) - 1(t\ln t) = t\ln t + t - t\ln t = \mathbf{t}$$
  * **Step 2: Find $u_1(t)$:**
  $$u_1'(t) = -\frac{(t\ln t)\left(\frac{4\ln t}{t}\right)}{t} = -\frac{4(\ln t)^2}{t}$$
  Let $w = \ln(t) \implies dw = \frac{1}{t}dt$:
  $$u_1(t) = -4\int w^2\,dw = -\frac{4}{3}w^3 = \mathbf{-\frac{4}{3}(\ln t)^3}$$
  * **Step 3: Find $u_2(t)$:**
  $$u_2'(t) = \frac{t\left(\frac{4\ln t}{t}\right)}{t} = \frac{4\ln t}{t} \implies u_2(t) = 4\int w\,dw = 2w^2 = \mathbf{2(\ln t)^2}$$
  * **Step 4: Form $y_p(t)$:**
  $$y_p(t) = \left(-\frac{4}{3}(\ln t)^3\right)t + \left(2(\ln t)^2\right)(t\ln t) = -\frac{4}{3}t(\ln t)^3 + 2t(\ln t)^3 = \mathbf{\frac{2}{3}t(\ln t)^3}$$
  * **Step 5: General Solution:**
  $$\mathbf{y(t) = c_1 t + c_2 t\ln(t) + \frac{2}{3}t(\ln t)^3}$$
  
  ---
### **Problem 5 (The Classic Secant Forcing)**
  Find the general solution of:
  $$y'' + y = \sec(t), \quad -\frac{\pi}{2} < t < \frac{\pi}{2}$$
#### **Solution:**
  * **Homogeneous:** $r^2 + 1 = 0 \implies y_1 = \cos(t), \; y_2 = \sin(t)$.
  * **Wronskian:** $W = \cos^2(t) + \sin^2(t) = 1$.
  * **Standard Form:** Already standard, $g(t) = \sec(t) = \frac{1}{\cos(t)}$.
  * **Find $u_1(t)$:**
  $$u_1'(t) = -\frac{\sin(t)\sec(t)}{1} = -\tan(t) \implies u_1(t) = -\int \tan(t)\,dt = \mathbf{\ln|\cos(t)|}$$
  * **Find $u_2(t)$:**
  $$u_2'(t) = \frac{\cos(t)\sec(t)}{1} = \cos(t)\left(\frac{1}{\cos t}\right) = 1 \implies \mathbf{u_2(t) = t}$$
  * **Form $y_p(t)$:**
  $$y_p(t) = \cos(t)\ln|\cos(t)| + t\sin(t)$$
  * **General Solution:**
  $$\mathbf{y(t) = c_1\cos(t) + c_2\sin(t) + \cos(t)\ln|\cos(t)| + t\sin(t)}$$
  
  ---
### **Problem 6 (The Classic Tangent Forcing)**
  Find the general solution of:
  $$y'' + y = \tan(t), \quad -\frac{\pi}{2} < t < \frac{\pi}{2}$$
#### **Solution:**
  * $y_1 = \cos(t), \; y_2 = \sin(t), \; W = 1, \; g(t) = \tan(t) = \frac{\sin(t)}{\cos(t)}$.
  * **Find $u_1(t)$:**
  $$u_1'(t) = -\frac{\sin(t)\tan(t)}{1} = -\frac{\sin^2(t)}{\cos(t)} = -\frac{1 - \cos^2(t)}{\cos(t)} = -\sec(t) + \cos(t)$$
  $$u_1(t) = \int (-\sec(t) + \cos(t))\,dt = \mathbf{-\ln|\sec(t) + \tan(t)| + \sin(t)}$$
  * **Find $u_2(t)$:**
  $$u_2'(t) = \frac{\cos(t)\tan(t)}{1} = \sin(t) \implies \mathbf{u_2(t) = -\cos(t)}$$
  * **Form $y_p(t)$:**
  $$y_p(t) = \left(-\ln|\sec t + \tan t| + \sin t\right)\cos(t) + (-\cos t)\sin(t) = \mathbf{-\cos(t)\ln|\sec(t) + \tan(t)|}$$
  * **General Solution:**
  $$\mathbf{y(t) = c_1\cos(t) + c_2\sin(t) - \cos(t)\ln|\sec(t) + \tan(t)|}$$
  
  ---
### **Problem 7 (Repeated Root Homogeneous with Rational-Exponential RHS)**
  Find the general solution of:
  $$y'' - 2y' + y = \frac{e^t}{t^2 + 1}$$
#### **Solution:**
  * **Homogeneous:** $r^2 - 2r + 1 = 0 \implies (r-1)^2 = 0 \implies y_1 = e^t, \; y_2 = t e^t$.
  * **Wronskian:** $W = e^t(e^t + t e^t) - e^t(t e^t) = e^{2t}$.
  * **Standard Form:** $g(t) = \frac{e^t}{t^2 + 1}$.
  * **Find $u_1(t)$:**
  $$u_1'(t) = -\frac{(t e^t)\left(\frac{e^t}{t^2 + 1}\right)}{e^{2t}} = -\frac{t}{t^2 + 1} \implies \mathbf{u_1(t) = -\frac{1}{2}\ln(t^2 + 1)}$$
  * **Find $u_2(t)$:**
  $$u_2'(t) = \frac{(e^t)\left(\frac{e^t}{t^2 + 1}\right)}{e^{2t}} = \frac{1}{t^2 + 1} \implies \mathbf{u_2(t) = \arctan(t)}$$
  * **Form $y_p(t)$:**
  $$\mathbf{y_p(t) = -\frac{1}{2}e^t \ln(t^2 + 1) + t e^t \arctan(t)}$$
  * **General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 t e^t - \frac{1}{2}e^t \ln(t^2 + 1) + t e^t \arctan(t)}$$
  
  ---
### **Problem 8 (Constant Coefficient with Leading Coefficient $\neq 1$)**
  Find the general solution of:
  $$4y'' + 36y = 12\csc(3t), \quad 0 < t < \frac{\pi}{3}$$
#### **Solution:**
  * **Step 0: MANDATORY Standard Form:** Divide by $4$:
  $$y'' + 9y = \frac{12}{4}\csc(3t) = 3\csc(3t) \implies \mathbf{g(t) = 3\csc(3t)}$$
  * **Step 1: Homogeneous Solution & Wronskian:**
  $$r^2 + 9 = 0 \implies r = \pm 3i \implies y_1 = \cos(3t), \quad y_2 = \sin(3t)$$
  $$W = \cos(3t)(3\cos 3t) - (-3\sin 3t)(\sin 3t) = 3(\cos^2 3t + \sin^2 3t) = \mathbf{3}$$
  * **Step 2: Find $u_1(t)$:**
  $$u_1'(t) = -\frac{\sin(3t) \cdot 3\csc(3t)}{3} = -\sin(3t)\left(\frac{1}{\sin 3t}\right) = -1 \implies \mathbf{u_1(t) = -t}$$
  * **Step 3: Find $u_2(t)$:**
  $$u_2'(t) = \frac{\cos(3t) \cdot 3\csc(3t)}{3} = \frac{\cos(3t)}{\sin(3t)} = \cot(3t) \implies \mathbf{u_2(t) = \frac{1}{3}\ln|\sin(3t)|}$$
  * **Step 4: Form $y_p(t)$:**
  $$y_p(t) = -t\cos(3t) + \frac{1}{3}\sin(3t)\ln|\sin(3t)|$$
  * **Step 5: General Solution:**
  $$\mathbf{y(t) = c_1\cos(3t) + c_2\sin(3t) - t\cos(3t) + \frac{1}{3}\sin(3t)\ln|\sin(3t)|}$$
  
  ---
### **Problem 9 (Exponential Denominator)**
  Find the general solution of:
  $$y'' - y = \frac{1}{1 + e^{-t}}$$
#### **Solution:**
  * **Homogeneous:** $r^2 - 1 = 0 \implies y_1 = e^t, \; y_2 = e^{-t}$.
  * **Wronskian:** $W = e^t(-e^{-t}) - (e^t)(e^{-t}) = -1 - 1 = \mathbf{-2}$.
  * **Standard Form:** $g(t) = \frac{1}{1 + e^{-t}} = \frac{e^t}{e^t + 1}$.
  * **Find $u_1(t)$:**
  $$u_1'(t) = -\frac{e^{-t}\left(\frac{1}{1 + e^{-t}}\right)}{-2} = \frac{1}{2}\left(\frac{e^{-t}}{1 + e^{-t}}\right)$$
  Let $w = 1 + e^{-t} \implies dw = -e^{-t}dt$:
  $$\mathbf{u_1(t) = -\frac{1}{2}\ln(1 + e^{-t})}$$
  * **Find $u_2(t)$:**
  $$u_2'(t) = \frac{e^t\left(\frac{e^t}{e^t + 1}\right)}{-2} = -\frac{1}{2}\frac{e^{2t}}{e^t + 1}$$
  Let $u = e^t + 1 \implies e^t = u - 1, \; du = e^t dt$:
  $$\int \frac{e^t \cdot e^t}{e^t + 1}\,dt = \int \frac{u - 1}{u}\,du = \int \left(1 - \frac{1}{u}\right)du = u - \ln|u| = (e^t + 1) - \ln(e^t + 1)$$
  $$\mathbf{u_2(t) = -\frac{1}{2}\left[e^t - \ln(e^t + 1)\right]}$$
  * **Form $y_p(t)$ and Simplify:**
  Recall $\ln(1 + e^{-t}) = \ln\left(\frac{e^t + 1}{e^t}\right) = \ln(e^t + 1) - t$:
  $$u_1(t) = -\frac{1}{2}\ln(e^t + 1) + \frac{1}{2}t$$
  $$y_p = \left(-\frac{1}{2}\ln(e^t + 1) + \frac{1}{2}t\right)e^t - \frac{1}{2}e^{-t}\left(e^t - \ln(e^t + 1)\right)$$
  $$y_p(t) = \frac{1}{2}t e^t - \frac{1}{2} - \frac{1}{2}(e^t - e^{-t})\ln(e^t + 1)$$
  * **General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 e^{-t} + \frac{1}{2}t e^t - \frac{1}{2}(e^t - e^{-t})\ln(1 + e^t) - \frac{1}{2}}$$
  
  ---
### **Problem 10 (Full Initial Value Problem)**
  Solve the IVP using Variation of Parameters:
  $$y'' - 2y' + y = \frac{e^t}{t}, \quad t > 0, \quad y(1) = 0, \; y'(1) = 0$$
#### **Solution:**
  * **Homogeneous:** $r^2 - 2r + 1 = 0 \implies y_1 = e^t, \; y_2 = t e^t$.
  * **Wronskian:** $W = e^{2t}$.
  * **Standard Form:** $g(t) = \frac{e^t}{t}$.
  * **Find $u_1$ and $u_2$:**
  $$u_1'(t) = -\frac{(t e^t)\left(\frac{e^t}{t}\right)}{e^{2t}} = -1 \implies \mathbf{u_1(t) = -t}$$
  $$u_2'(t) = \frac{(e^t)\left(\frac{e^t}{t}\right)}{e^{2t}} = \frac{1}{t} \implies \mathbf{u_2(t) = \ln(t)} \quad (t > 0)$$
  * **Form $y_p(t)$:**
  $$y_p(t) = (-t)e^t + (\ln t)(t e^t) = -t e^t + t e^t \ln(t)$$
  *Absorb $-t e^t$ into $c_2 t e^t$.*
  * **Full General Solution:**
  $$y(t) = c_1 e^t + c_2 t e^t + t e^t \ln(t)$$
  * **Apply Initial Conditions at $t = 1$:**
  * $y(1) = 0$:
    $$0 = c_1 e^1 + c_2(1)e^1 + (1)e^1 \ln(1) = (c_1 + c_2)e \implies \mathbf{c_2 = -c_1}$$
  * Compute $y'(t)$:
    $$y'(t) = c_1 e^t + c_2(e^t + t e^t) + \left[ (e^t + t e^t)\ln(t) + (t e^t)\left(\frac{1}{t}\right) \right]$$
  * $y'(1) = 0$:
    $$0 = c_1 e + c_2(2e) + \left[ 0 + e \right] = c_1 e + 2c_2 e + e$$
    Substitute $c_2 = -c_1$:
    $$c_1 e + 2(-c_1)e + e = 0 \implies -c_1 e + e = 0 \implies \mathbf{c_1 = 1} \implies \mathbf{c_2 = -1}$$
  * **Final Particular Solution:**
  $$\mathbf{y(t) = e^t - t e^t + t e^t \ln(t) = e^t\left(1 - t + t\ln(t)\right)}$$
  
  ---
### Variation of Parameters Quick Self-Check
  * [x] Did you divide by the coefficient in front of $y''$ before writing down $g(t)$?
  * [x] Did you double-check the signs: $u_1' = \mathbf{-}\frac{y_2 g}{W}$ and $u_2' = \mathbf{+}\frac{y_1 g}{W}$?
  * [x] Did you check if any part of $y_p$ duplicates $y_1$ or $y_2$ so you can absorb it into $c_1$ or $c_2$?
  
  ---
  
  Whenever you're ready, let me know which topic we should generate 10 practice problems for next (e.g., **Cauchy-Euler & Reduction of Order**, **Mass-Spring Systems**, or **Laplace Transforms**)!