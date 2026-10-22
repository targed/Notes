### **Problem 1 (Order 3: Real Roots with Exponential $y_p$)**
  We are looking for a third-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$0, 2,$ and $-2$**, and a particular solution is **$y_p(t) = e^{3t}$**.
  1. Write the three linear factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $r$, $(r - 2)$, $(r + 2)$
  * **Characteristic Equation:** $r(r - 2)(r + 2) = r(r^2 - 4) = \mathbf{r^3 - 4r = 0}$
  * **Homogeneous ODE:** $\mathbf{y''' - 4y' = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p(t) = e^{3t} \implies y_p' = 3e^{3t}, \quad y_p''' = 27e^{3t}$$
  $$g(t) = y_p''' - 4y_p' = 27e^{3t} - 4(3e^{3t}) = (27 - 12)e^{3t} = 15e^{3t}$$
  * **Final ODE:** $\mathbf{y''' - 4y' = 15e^{3t}}$
  
  ---
### **Problem 2 (Order 4: Real and Imaginary Roots with Trig $y_p$)**
  We are looking for a fourth-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$2, -2, 3i,$ and $-3i$**, and a particular solution is **$y_p(t) = \cos(t)$**.
  1. Write the three irreducible real factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $(r - 2)$, $(r + 2)$, and $(r^2 + 9)$ *(from $(r - 3i)(r + 3i)$)*
  * **Characteristic Equation:** $(r^2 - 4)(r^2 + 9) = \mathbf{r^4 + 5r^2 - 36 = 0}$
  * **Homogeneous ODE:** $\mathbf{y'''' + 5y'' - 36y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = \cos(t) \implies y_p'' = -\cos(t), \quad y_p'''' = \cos(t)$$
  $$g(t) = y_p'''' + 5y_p'' - 36y_p = \cos(t) + 5(-\cos t) - 36\cos(t) = (1 - 5 - 36)\cos(t) = -40\cos(t)$$
  * **Final ODE:** $\mathbf{y'''' + 5y'' - 36y = -40\cos(t)}$
  
  ---
### **Problem 3 (Order 3: Repeated Real Root with Polynomial $y_p$)**
  We are looking for a third-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$1$ (multiplicity $2$) and $-3$**, and a particular solution is **$y_p(t) = t^2$**.
  1. Write the prime factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $(r - 1)^2$ and $(r + 3)$
  * **Characteristic Equation:** $(r^2 - 2r + 1)(r + 3) = r^3 + 3r^2 - 2r^2 - 6r + r + 3 = \mathbf{r^3 + r^2 - 5r + 3 = 0}$
  * **Homogeneous ODE:** $\mathbf{y''' + y'' - 5y' + 3y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = t^2 \implies y_p' = 2t, \quad y_p'' = 2, \quad y_p''' = 0$$
  $$g(t) = (0) + (2) - 5(2t) + 3(t^2) = \mathbf{3t^2 - 10t + 2}$$
  * **Final ODE:** $\mathbf{y''' + y'' - 5y' + 3y = 3t^2 - 10t + 2}$
  
  ---
### **Problem 4 (Order 4: Two Pairs of Pure Imaginary Roots)**
  We are looking for a fourth-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$i, -i, 2i,$ and $-2i$**, and a particular solution is **$y_p(t) = e^{-t}$**.
  1. Write the two irreducible real factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $(r^2 + 1)$ and $(r^2 + 4)$
  * **Characteristic Equation:** $(r^2 + 1)(r^2 + 4) = \mathbf{r^4 + 5r^2 + 4 = 0}$
  * **Homogeneous ODE:** $\mathbf{y'''' + 5y'' + 4y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = e^{-t} \implies y_p'' = e^{-t}, \quad y_p'''' = e^{-t}$$
  $$g(t) = e^{-t} + 5(e^{-t}) + 4(e^{-t}) = (1 + 5 + 4)e^{-t} = 10e^{-t}$$
  * **Final ODE:** $\mathbf{y'''' + 5y'' + 4y = 10e^{-t}}$
  
  ---
### **Problem 5 (Order 3: Complex Roots with Non-Zero Real Part)**
  We are looking for a third-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$0$ and $1 \pm 2i$**, and a particular solution is **$y_p(t) = t^2 + 1$**.
  1. Write the irreducible real factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** 
  * Root $0 \implies r$
  * Roots $1 \pm 2i \implies (r - 1)^2 + 2^2 = \mathbf{r^2 - 2r + 5}$
  * **Characteristic Equation:** $r(r^2 - 2r + 5) = \mathbf{r^3 - 2r^2 + 5r = 0}$
  * **Homogeneous ODE:** $\mathbf{y''' - 2y'' + 5y' = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = t^2 + 1 \implies y_p' = 2t, \quad y_p'' = 2, \quad y_p''' = 0$$
  $$g(t) = y_p''' - 2y_p'' + 5y_p' = (0) - 2(2) + 5(2t) = \mathbf{10t - 4}$$
  * **Final ODE:** $\mathbf{y''' - 2y'' + 5y' = 10t - 4}$
  
  ---
### **Problem 6 (Order 4: Double Zero Root and Imaginary Roots)**
  We are looking for a fourth-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$0$ (multiplicity $2$) and $\pm \sqrt{3}i$**, and a particular solution is **$y_p(t) = e^{2t}$**.
  1. Write the irreducible real factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $r^2$ and $(r^2 + 3)$
  * **Characteristic Equation:** $r^2(r^2 + 3) = \mathbf{r^4 + 3r^2 = 0}$
  * **Homogeneous ODE:** $\mathbf{y'''' + 3y'' = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = e^{2t} \implies y_p'' = 4e^{2t}, \quad y_p'''' = 16e^{2t}$$
  $$g(t) = y_p'''' + 3y_p'' = 16e^{2t} + 3(4e^{2t}) = (16 + 12)e^{2t} = 28e^{2t}$$
  * **Final ODE:** $\mathbf{y'''' + 3y'' = 28e^{2t}}$
  
  ---
### **Problem 7 (Order 3: Triple Root at $-1$)**
  We are looking for a third-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has root **$-1$ with multiplicity $3$**, and a particular solution is **$y_p(t) = 2\sin(t)$**.
  1. Write the prime factor of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factor:** $(r + 1)^3$
  * **Characteristic Equation:** $(r + 1)^3 = \mathbf{r^3 + 3r^2 + 3r + 1 = 0}$
  * **Homogeneous ODE:** $\mathbf{y''' + 3y'' + 3y' + y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = 2\sin(t), \quad y_p' = 2\cos(t), \quad y_p'' = -2\sin(t), \quad y_p''' = -2\cos(t)$$
  $$g(t) = (-2\cos t) + 3(-2\sin t) + 3(2\cos t) + (2\sin t) = 4\cos(t) - 4\sin(t)$$
  * **Final ODE:** $\mathbf{y''' + 3y'' + 3y' + y = 4\cos(t) - 4\sin(t)}$
  
  ---
### **Problem 8 (Order 4: Repeated Complex Roots)**
  We are looking for a fourth-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$\pm i$ (each with multiplicity $2$)**, and a particular solution is **$y_p(t) = 3e^{-2t}$**.
  1. Write the irreducible real factor of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factor:** $(r^2 + 1)^2$
  * **Characteristic Equation:** $(r^2 + 1)^2 = \mathbf{r^4 + 2r^2 + 1 = 0}$
  * **Homogeneous ODE:** $\mathbf{y'''' + 2y'' + y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = 3e^{-2t} \implies y_p'' = 12e^{-2t}, \quad y_p'''' = 48e^{-2t}$$
  $$g(t) = 48e^{-2t} + 2(12e^{-2t}) + 3e^{-2t} = (48 + 24 + 3)e^{-2t} = 75e^{-2t}$$
  * **Final ODE:** $\mathbf{y'''' + 2y'' + y = 75e^{-2t}}$
  
  ---
### **Problem 9 (Order 4: Fourth-Order Binomial with Constant $y_p$)**
  We are looking for a fourth-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$3, -3, 3i,$ and $-3i$**, and a particular solution is the constant **$y_p(t) = 5$**.
  1. Write the three irreducible real factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $(r - 3)$, $(r + 3)$, and $(r^2 + 9)$
  * **Characteristic Equation:** $(r^2 - 9)(r^2 + 9) = \mathbf{r^4 - 81 = 0}$
  * **Homogeneous ODE:** $\mathbf{y'''' - 81y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = 5 \implies y_p'''' = 0$$
  $$g(t) = y_p'''' - 81y_p = 0 - 81(5) = -405$$
  * **Final ODE:** $\mathbf{y'''' - 81y = -405}$
  
  ---
### **Problem 10 (Order 3: Mixed Roots with Product $y_p$)**
  We are looking for a third-order inhomogeneous linear differential equation with constant coefficients and leading coefficient $1$. The characteristic equation has roots **$1, -1,$ and $3$**, and a particular solution is **$y_p(t) = t e^{2t}$**.
  1. Write the three linear factors of the characteristic equation.
  2. Write the characteristic equation.
  3. Write the associated homogeneous differential equation.
  4. Write the complete unknown inhomogeneous differential equation.
#### **Solution:**
  * **Factors:** $(r - 1)$, $(r + 1)$, and $(r - 3)$
  * **Characteristic Equation:** $(r^2 - 1)(r - 3) = \mathbf{r^3 - 3r^2 - r + 3 = 0}$
  * **Homogeneous ODE:** $\mathbf{y''' - 3y'' - y' + 3y = 0}$
  * **Right-hand side $g(t)$:**
  $$y_p = t e^{2t}$$
  $$y_p' = e^{2t} + 2t e^{2t} = (2t + 1)e^{2t}$$
  $$y_p'' = 2e^{2t} + 2(2t + 1)e^{2t} = (4t + 4)e^{2t}$$
  $$y_p''' = 4e^{2t} + 2(4t + 4)e^{2t} = (8t + 12)e^{2t}$$
  Substitute into the ODE:
  $$g(t) = (8t + 12)e^{2t} - 3(4t + 4)e^{2t} - (2t + 1)e^{2t} + 3(t e^{2t})$$
  $$g(t) = \left[(8 - 12 - 2 + 3)t + (12 - 12 - 1)\right]e^{2t} = \mathbf{-(3t + 1)e^{2t}}$$
  * **Final ODE:** $\mathbf{y''' - 3y'' - y' + 3y = -(3t + 1)e^{2t}}$