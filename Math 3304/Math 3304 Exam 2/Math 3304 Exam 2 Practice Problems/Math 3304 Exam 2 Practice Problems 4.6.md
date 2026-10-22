### **Practice Problems**
#### **Problem 1**
  Find the general solution to:
  $$y'' + y = \tan(t)$$
  
  ---
#### **Problem 2**
  Find the general solution to:
  $$y'' + 4y = \csc(2t)$$
  
  ---
#### **Problem 3**
  Find the general solution for $t > 0$:
  $$y'' - 2y' + y = \frac{e^t}{t}$$
  
  ---
#### **Problem 4**
  Find the general solution to:
  $$y'' + 3y' + 2y = \frac{1}{1 + e^t}$$
  
  ---
#### **Problem 5**
  Find the general solution to:
  $$y'' + y = \sec^2(t)$$
  
  ---
#### **Problem 6 (Cauchy–Euler)**
  Find the general solution for $t > 0$:
  $$t^2 y'' - 4ty' + 4y = t^4$$
  
  ---
#### **Problem 7 (Cauchy–Euler)**
  Find the general solution for $x > 0$:
  $$x^2 y'' - 2y = \frac{1}{x}$$
  
  ---
#### **Problem 8 (Variable Coefficients)**
  Given that $y_1(x) = e^x$ and $y_2(x) = x + 1$ form a fundamental solution set to the associated homogeneous equation, find the general solution for $x > 0$:
  $$x y'' - (x + 1)y' + y = x^2 e^{2x}$$
  
  ---
#### **Problem 9 (Initial Value Problem)**
  Solve the initial value problem:
  $$y'' + 4y' + 4y = \frac{e^{-2t}}{t^2 + 1}; \quad y(0) = 0, \quad y'(0) = 0$$
  
  ---
#### **Problem 10 (Third-Order Equation)**
  Find the general solution to:
  $$y''' + y' = \tan(t)$$
  
  ---
  ---
# **Solutions**
  
  ---
### **Solution 1**
  $$y'' + y = \tan(t)$$
  
  1. **Homogeneous Solution:**  
   $r^2 + 1 = 0 \implies r = \pm i$  
   $$y_1 = \cos(t), \quad y_2 = \sin(t)$$
   $$y_h(t) = c_1 \cos(t) + c_2 \sin(t)$$
  
  2. **Wronskian:**  
   $$W(t) = \begin{vmatrix} \cos(t) & \sin(t) \\ -\sin(t) & \cos(t) \end{vmatrix} = \cos^2(t) + \sin^2(t) = 1$$
  
  3. **Parameters $u_1$ and $u_2$ ($g(t) = \tan(t)$):**  
   $$u_1'(t) = -\frac{\sin(t)\tan(t)}{1} = -\frac{\sin^2(t)}{\cos(t)} = \frac{\cos^2(t) - 1}{\cos(t)} = \cos(t) - \sec(t)$$
   $$u_1(t) = \sin(t) - \ln|\sec(t) + \tan(t)|$$
  
   $$u_2'(t) = \frac{\cos(t)\tan(t)}{1} = \sin(t)$$
   $$u_2(t) = -\cos(t)$$
  
  4. **Particular Solution:**  
   $$y_p(t) = \left[\sin(t) - \ln|\sec(t) + \tan(t)|\right]\cos(t) + \left[-\cos(t)\right]\sin(t) = -\cos(t)\ln|\sec(t) + \tan(t)|$$
  
  **General Solution:**
  $$y(t) = c_1 \cos(t) + c_2 \sin(t) - \cos(t)\ln|\sec(t) + \tan(t)|$$
  
  ---
### **Solution 2**
  $$y'' + 4y = \csc(2t)$$
  
  1. **Homogeneous Solution:**  
   $r^2 + 4 = 0 \implies r = \pm 2i$  
   $$y_1 = \cos(2t), \quad y_2 = \sin(2t)$$
   $$y_h(t) = c_1 \cos(2t) + c_2 \sin(2t)$$
  
  2. **Wronskian:**  
   $$W(t) = \begin{vmatrix} \cos(2t) & \sin(2t) \\ -2\sin(2t) & 2\cos(2t) \end{vmatrix} = 2\cos^2(2t) + 2\sin^2(2t) = 2$$
  
  3. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(t) = -\frac{\sin(2t)\csc(2t)}{2} = -\frac{1}{2} \implies u_1(t) = -\frac{1}{2}t$$
   $$u_2'(t) = \frac{\cos(2t)\csc(2t)}{2} = \frac{1}{2}\cot(2t) \implies u_2(t) = \frac{1}{4}\ln|\sin(2t)|$$
  
  4. **Particular Solution:**  
   $$y_p(t) = -\frac{1}{2}t \cos(2t) + \frac{1}{4}\sin(2t)\ln|\sin(2t)|$$
  
  **General Solution:**
  $$y(t) = c_1 \cos(2t) + c_2 \sin(2t) - \frac{1}{2}t \cos(2t) + \frac{1}{4}\sin(2t)\ln|\sin(2t)|$$
  
  ---
### **Solution 3**
  $$y'' - 2y' + y = \frac{e^t}{t}, \quad t > 0$$
  
  1. **Homogeneous Solution:**  
   $r^2 - 2r + 1 = (r - 1)^2 = 0 \implies r = 1$ (multiplicity 2)  
   $$y_1 = e^t, \quad y_2 = t e^t$$
   $$y_h(t) = c_1 e^t + c_2 t e^t$$
  
  2. **Wronskian:**  
   $$W(t) = \begin{vmatrix} e^t & t e^t \\ e^t & (1 + t)e^t \end{vmatrix} = e^{2t}(1 + t) - t e^{2t} = e^{2t}$$
  
  3. **Parameters $u_1$ and $u_2$ ($g(t) = \frac{e^t}{t}$):**  
   $$u_1'(t) = -\frac{(t e^t)\left(\frac{e^t}{t}\right)}{e^{2t}} = -1 \implies u_1(t) = -t$$
   $$u_2'(t) = \frac{e^t\left(\frac{e^t}{t}\right)}{e^{2t}} = \frac{1}{t} \implies u_2(t) = \ln(t)$$
  
  4. **Particular Solution:**  
   $$y_p(t) = (-t)(e^t) + (\ln t)(t e^t) = -t e^t + t e^t \ln(t)$$  
   *(The term $-t e^t$ is absorbed into the complementary term $c_2 t e^t$)*.
  
  **General Solution:**
  $$y(t) = c_1 e^t + c_2 t e^t + t e^t \ln(t)$$
  
  ---
### **Solution 4**
  $$y'' + 3y' + 2y = \frac{1}{1 + e^t}$$
  
  1. **Homogeneous Solution:**  
   $r^2 + 3r + 2 = (r + 1)(r + 2) = 0 \implies r_1 = -1, \; r_2 = -2$  
   $$y_1 = e^{-t}, \quad y_2 = e^{-2t}$$
   $$y_h(t) = c_1 e^{-t} + c_2 e^{-2t}$$
  
  2. **Wronskian:**  
   $$W(t) = \begin{vmatrix} e^{-t} & e^{-2t} \\ -e^{-t} & -2e^{-2t} \end{vmatrix} = -2e^{-3t} - (-e^{-3t}) = -e^{-3t}$$
  
  3. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(t) = -\frac{e^{-2t}\left(\frac{1}{1 + e^t}\right)}{-e^{-3t}} = \frac{e^t}{1 + e^t} \implies u_1(t) = \ln(1 + e^t)$$
   $$u_2'(t) = \frac{e^{-t}\left(\frac{1}{1 + e^t}\right)}{-e^{-3t}} = -\frac{e^{2t}}{1 + e^t} = -\frac{e^t(1 + e^t - 1)}{1 + e^t} = -e^t + \frac{e^t}{1 + e^t}$$
   $$u_2(t) = -e^t + \ln(1 + e^t)$$
  
  4. **Particular Solution:**  
   $$y_p(t) = e^{-t}\ln(1 + e^t) + e^{-2t}\left[-e^t + \ln(1 + e^t)\right]$$
   $$y_p(t) = (e^{-t} + e^{-2t})\ln(1 + e^t) - e^{-t}$$  
   *(The term $-e^{-t}$ is absorbed into $c_1 e^{-t}$)*.
  
  **General Solution:**
  $$y(t) = c_1 e^{-t} + c_2 e^{-2t} + (e^{-t} + e^{-2t})\ln(1 + e^t)$$
  
  ---
### **Solution 5**
  $$y'' + y = \sec^2(t)$$
  
  1. **Homogeneous Solution:**  
   $$y_1 = \cos(t), \quad y_2 = \sin(t), \quad W(t) = 1$$
  
  2. **Parameters $u_1$ and $u_2$ ($g(t) = \sec^2(t)$):**  
   $$u_1'(t) = -\sin(t)\sec^2(t) = -\tan(t)\sec(t) \implies u_1(t) = -\sec(t)$$
   $$u_2'(t) = \cos(t)\sec^2(t) = \sec(t) \implies u_2(t) = \ln|\sec(t) + \tan(t)|$$
  
  3. **Particular Solution:**  
   $$y_p(t) = (-\sec(t))\cos(t) + \ln|\sec(t) + \tan(t)|\sin(t) = -1 + \sin(t)\ln|\sec(t) + \tan(t)|$$
  
  **General Solution:**
  $$y(t) = c_1 \cos(t) + c_2 \sin(t) - 1 + \sin(t)\ln|\sec(t) + \tan(t)|$$
  
  ---
### **Solution 6 (Cauchy–Euler)**
  $$t^2 y'' - 4ty' + 4y = t^4, \quad t > 0$$
  
  1. **Homogeneous Solution:**  
   $r(r - 1) - 4r + 4 = r^2 - 5r + 4 = (r - 1)(r - 4) = 0 \implies r_1 = 1, \; r_2 = 4$  
   $$y_1 = t, \quad y_2 = t^4$$
   $$y_h(t) = c_1 t + c_2 t^4$$
  
  2. **Standard Form:**  
   Divide by $t^2$:
   $$y'' - \frac{4}{t}y' + \frac{4}{t^2}y = t^2 \implies g(t) = t^2$$
  
  3. **Wronskian:**  
   $$W(t) = \begin{vmatrix} t & t^4 \\ 1 & 4t^3 \end{vmatrix} = 4t^4 - t^4 = 3t^4$$
  
  4. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(t) = -\frac{t^4(t^2)}{3t^4} = -\frac{1}{3}t^2 \implies u_1(t) = -\frac{1}{9}t^3$$
   $$u_2'(t) = \frac{t(t^2)}{3t^4} = \frac{1}{3t} \implies u_2(t) = \frac{1}{3}\ln(t)$$
  
  5. **Particular Solution:**  
   $$y_p(t) = \left(-\frac{1}{9}t^3\right)(t) + \left(\frac{1}{3}\ln t\right)(t^4) = -\frac{1}{9}t^4 + \frac{1}{3}t^4 \ln(t)$$  
   *(The term $-\frac{1}{9}t^4$ is absorbed into $c_2 t^4$)*.
  
  **General Solution:**
  $$y(t) = c_1 t + c_2 t^4 + \frac{1}{3}t^4 \ln(t)$$
  
  ---
### **Solution 7 (Cauchy–Euler)**
  $$x^2 y'' - 2y = \frac{1}{x}, \quad x > 0$$
  
  1. **Homogeneous Solution:**  
   $r(r - 1) + 0r - 2 = r^2 - r - 2 = (r - 2)(r + 1) = 0 \implies r_1 = 2, \; r_2 = -1$  
   $$y_1 = x^2, \quad y_2 = x^{-1}$$
   $$y_h(x) = c_1 x^2 + c_2 x^{-1}$$
  
  2. **Standard Form:**  
   Divide by $x^2$:
   $$y'' - \frac{2}{x^2}y = x^{-3} \implies g(x) = x^{-3}$$
  
  3. **Wronskian:**  
   $$W(x) = \begin{vmatrix} x^2 & x^{-1} \\ 2x & -x^{-2} \end{vmatrix} = -1 - 2 = -3$$
  
  4. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(x) = -\frac{x^{-1}(x^{-3})}{-3} = \frac{1}{3}x^{-4} \implies u_1(x) = -\frac{1}{9}x^{-3}$$
   $$u_2'(x) = \frac{x^2(x^{-3})}{-3} = -\frac{1}{3x} \implies u_2(x) = -\frac{1}{3}\ln(x)$$
  
  5. **Particular Solution:**  
   $$y_p(x) = \left(-\frac{1}{9}x^{-3}\right)(x^2) + \left(-\frac{1}{3}\ln x\right)(x^{-1}) = -\frac{1}{9x} - \frac{\ln(x)}{3x}$$  
   *(The term $-\frac{1}{9x}$ is absorbed into $c_2 x^{-1}$)*.
  
  **General Solution:**
  $$y(x) = c_1 x^2 + c_2 x^{-1} - \frac{\ln(x)}{3x}$$
  
  ---
### **Solution 8 (Variable Coefficients)**
  $$x y'' - (x + 1)y' + y = x^2 e^{2x}, \quad x > 0$$
  $$\text{Given: } y_1(x) = e^x, \quad y_2(x) = x + 1$$
  
  1. **Standard Form:**  
   Divide by $x$:
   $$y'' - \left(\frac{x + 1}{x}\right)y' + \frac{1}{x}y = x e^{2x} \implies g(x) = x e^{2x}$$
  
  2. **Wronskian:**  
   $$W(x) = \begin{vmatrix} e^x & x + 1 \\ e^x & 1 \end{vmatrix} = e^x(1) - (x + 1)e^x = -x e^x$$
  
  3. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(x) = -\frac{(x + 1)(x e^{2x})}{-x e^x} = (x + 1)e^x$$
   $$u_1(x) = \int (x + 1)e^x \, dx = x e^x$$
  
   $$u_2'(x) = \frac{e^x(x e^{2x})}{-x e^x} = -e^{2x}$$
   $$u_2(x) = \int -e^{2x} \, dx = -\frac{1}{2}e^{2x}$$
  
  4. **Particular Solution:**  
   $$y_p(x) = (x e^x)(e^x) + \left(-\frac{1}{2}e^{2x}\right)(x + 1) = x e^{2x} - \frac{1}{2}x e^{2x} - \frac{1}{2}e^{2x} = \frac{1}{2}(x - 1)e^{2x}$$
  
  **General Solution:**
  $$y(x) = c_1 e^x + c_2(x + 1) + \frac{1}{2}(x - 1)e^{2x}$$
  
  ---
### **Solution 9 (Initial Value Problem)**
  $$y'' + 4y' + 4y = \frac{e^{-2t}}{t^2 + 1}; \quad y(0) = 0, \quad y'(0) = 0$$
  
  1. **Homogeneous Solution:**  
   $$r^2 + 4r + 4 = 0 \implies r = -2 \text{ (multiplicity 2)}$$
   $$y_1(t) = e^{-2t}, \quad y_2(t) = t e^{-2t}$$
  
  2. **Wronskian:**  
   $$W(t) = \begin{vmatrix} e^{-2t} & t e^{-2t} \\ -2e^{-2t} & (1 - 2t)e^{-2t} \end{vmatrix} = e^{-4t}$$
  
  3. **Parameters $u_1$ and $u_2$:**  
   $$u_1'(t) = -\frac{(t e^{-2t})\left(\frac{e^{-2t}}{t^2 + 1}\right)}{e^{-4t}} = -\frac{t}{t^2 + 1} \implies u_1(t) = -\frac{1}{2}\ln(t^2 + 1)$$
   $$u_2'(t) = \frac{e^{-2t}\left(\frac{e^{-2t}}{t^2 + 1}\right)}{e^{-4t}} = \frac{1}{t^2 + 1} \implies u_2(t) = \arctan(t)$$
  
  4. **Particular Solution:**  
   $$y_p(t) = -\frac{1}{2}e^{-2t}\ln(t^2 + 1) + t e^{-2t}\arctan(t) = e^{-2t}\left[t \arctan(t) - \frac{1}{2}\ln(t^2 + 1)\right]$$
  
  5. **Apply Initial Conditions:**  
   Notice that $y_p(0) = 0$ and $y_p'(0) = 0$. Since the initial conditions are homogeneous ($y(0) = 0, y'(0) = 0$), $c_1 = 0$ and $c_2 = 0$.
  
  **Solution:**
  $$y(t) = e^{-2t}\left[t \arctan(t) - \frac{1}{2}\ln(t^2 + 1)\right]$$
  
  ---
### **Solution 10 (Third-Order Equation)**
  $$y''' + y' = \tan(t)$$
  
  1. **Homogeneous Solution:**  
   $$r(r^2 + 1) = 0 \implies r = 0, \quad r = \pm i$$
   $$y_1 = 1, \quad y_2 = \cos(t), \quad y_3 = \sin(t)$$
   $$y_h(t) = c_1 + c_2 \cos(t) + c_3 \sin(t)$$
  
  2. **Wronskian $W(t)$:**  
   $$W(t) = \begin{vmatrix} 1 & \cos(t) & \sin(t) \\ 0 & -\sin(t) & \cos(t) \\ 0 & -\cos(t) & -\sin(t) \end{vmatrix} = 1\left(\sin^2(t) + \cos^2(t)\right) = 1$$
  
  3. **Compute $W_1, W_2, W_3$:**  
   $$W_1(t) = \begin{vmatrix} 0 & \cos(t) & \sin(t) \\ 0 & -\sin(t) & \cos(t) \\ 1 & -\cos(t) & -\sin(t) \end{vmatrix} = 1$$
   $$W_2(t) = \begin{vmatrix} 1 & 0 & \sin(t) \\ 0 & 0 & \cos(t) \\ 0 & 1 & -\sin(t) \end{vmatrix} = -\cos(t)$$
   $$W_3(t) = \begin{vmatrix} 1 & \cos(t) & 0 \\ 0 & -\sin(t) & 0 \\ 0 & -\cos(t) & 1 \end{vmatrix} = -\sin(t)$$
  
  4. **Integrate Parameters ($g(t) = \tan(t)$):**  
   $$u_1(t) = \int \tan(t) \, dt = \ln|\sec(t)|$$
   $$u_2(t) = \int -\cos(t)\tan(t) \, dt = \int -\sin(t) \, dt = \cos(t)$$
   $$u_3(t) = \int -\sin(t)\tan(t) \, dt = \int \left(\cos(t) - \sec(t)\right) dt = \sin(t) - \ln|\sec(t) + \tan(t)|$$
  
  5. **Particular Solution:**  
   $$y_p(t) = (1)\ln|\sec(t)| + \cos^2(t) + \sin^2(t) - \sin(t)\ln|\sec(t) + \tan(t)|$$
   $$y_p(t) = \ln|\sec(t)| + 1 - \sin(t)\ln|\sec(t) + \tan(t)|$$  
   *(The constant $+1$ is absorbed into the arbitrary constant $c_1$)*.
  
  **General Solution:**
  $$y(t) = c_1 + c_2 \cos(t) + c_3 \sin(t) + \ln|\sec(t)| - \sin(t)\ln|\sec(t) + \tan(t)|$$