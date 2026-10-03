## Introduction
  
  This lecture explores wave interference, focusing on standing waves and the superposition principle. We will cover:
  
  * Superposition of waves
  * Standing waves on a string
  * Interference
## Standing Waves
  
  * **Formation:** Standing waves are formed by the superposition (combination) of two waves traveling in opposite directions with the following properties:
    * Same amplitude
    * Same wavelength $\lambda$ (and thus same wave number $k$)
    * Same frequency $f$ (and thus same angular frequency $\omega$)
    * Consequently, same speed $v = \lambda f$
  * **Equations:**
    * $y_1(x, t) = A_0 \sin(kx - \omega t)$ (traveling in the positive x-direction)
    * $y_2(x, t) = A_0 \sin(kx + \omega t)$ (traveling in the negative x-direction)
  
  **Simulation:** http://www.walter-fendt.de/ph6en/standingwavereflection_en.htm
## Deriving the Standing Wave Equation
  
  * **Superposition principle:**  The displacement of the medium at any point is the sum of the displacements due to the individual waves:
    * $y(x,t) = y_1 + y_2$
  * **Trigonometric identity:**  $\sin a + \sin b = 2\sin\left(\frac{a+b}{2}\right)\cos\left(\frac{a-b}{2}\right)$
  * **Applying the identity:**
    * $y(x,t) = A_0\sin(kx - \omega t) + A_0\sin(kx + \omega t) = 2A_0 \sin(kx) \cos(\omega t)$
  
  * **Standing wave equation:** This equation represents a standing wave.  The $\sin(kx)$ term describes the spatial dependence, and the $\cos(\omega t)$ term describes the time dependence. Unlike a traveling wave, the spatial and temporal parts are separated.  All points oscillate with the same frequency, but the amplitude varies with position.
## Standing Wave on a String
  
  * **Boundary conditions:** Consider a string of length $L$ with fixed ends. The displacement at the ends must be zero:
    * $y(0, t) = 0$
    * $y(L, t) = 0$
  * **Applying boundary conditions:** The first condition, $y(0,t)=0$ is already satisfied since $\sin(0)=0$. The second condition leads to:
    * $y(L, t) = 2A_0 \sin(kL) \cos(\omega t) = 0$. Since this must be true at all times, $\sin(kL)=0$, which means:
    * $kL = n\pi$, where $n$ is an integer ($n = 1, 2, 3, ...$).
  * **Wavelengths:** Since $k = \frac{2\pi}{\lambda}$, we have $\lambda = \frac{2L}{n}$.
  * **Frequencies:** Using $v = f\lambda$, we get $f = \frac{nv}{2L}$.
  
  **(Diagram of a standing wave on a string with fixed ends)**
## Fundamental Frequency and Harmonics
  
  * **$n=1$: Fundamental frequency (first harmonic):** The lowest possible frequency for a standing wave on the string: $f_1 = \frac{v}{2L}$. The corresponding wavelength is $\lambda_1 = 2L$.
  
  * **$n=2$: Second harmonic (first overtone):** $f_2 = \frac{2v}{2L} = \frac{v}{L}$, $\lambda_2 = L$.
  * **$n=3$: Third harmonic (second overtone):** $f_3 = \frac{3v}{2L}$, $\lambda_3 = \frac{2L}{3}$.
  * **General case:** The $n$th harmonic has frequency $f_n = \frac{nv}{2L} = nf_1$.
  
  
  **(Diagram showing the first three harmonics on a string with fixed ends.)**
## Examples
  
  **Example 1:** A wire has a length of 8 m. The speed of waves on the wire is 240 m/s. What is the fundamental frequency?
  
  **(Solution: $f_1 = \frac{v}{2L} = \frac{240}{2(8)} = 15$ Hz)**
  
  **Example 2:** A particular guitar string has a mass of 3.0 grams and a length of 0.75 m. A standing wave on the string has the shape shown in the figure (e.g., the second harmonic). The wave has a frequency of 1200 Hz.
  
  a) What is the speed of the wave?
  b) What is the tension in the string?
  c) The wave on the string produces a sound wave. Does the sound wave have the same frequency, wavelength, or speed as the wave on the string?
  
  **(Solutions will be discussed in the lecture.)**
## Interference
  
  * **Definition:** Interference occurs when two or more traveling waves superimpose.
  * **Types:**
    * **Constructive interference:** The waves add up to create a larger amplitude.  Occurs when the waves are in phase.
    * **Destructive interference:** The waves cancel each other out, resulting in a smaller amplitude (or zero amplitude if they have equal amplitudes). Occurs when the waves are out of phase.
  
  **(Diagram illustrating constructive and destructive interference)**
## Interference and Path Length
  
  * **Two sources:** Consider two sources emitting waves in phase. How do the waves combine at a point P?
  * **Path length difference ($\Delta L$):**  The difference in the distances the waves travel from each source to point P.
  * **Constructive interference:** If the path length difference is an integer multiple of the wavelength ($\Delta L = n\lambda$, where $n$ is an integer), the waves arrive at P in phase, resulting in constructive interference.
  * **Destructive interference:** If the path length difference is a half-integer multiple of the wavelength ($\Delta L = (n + \frac{1}{2})\lambda$), the waves arrive at P out of phase, resulting in destructive interference.
  
  **(Diagram illustrating path length difference)**
## Example: Radio Transmitters
  
  **Problem:** Two radio transmitters (A and B) are 12 m apart. They are driven by the same oscillator (i.e., they emit in phase) and generate waves of wavelength 2 m. How do the waves interfere at point P, which is 16 m directly in front of source A?
  
  **(Diagram of the setup with sources A and B and point P.)**
  
  **(Solution will be worked out in lecture. It involves calculating the path length difference and determining if it corresponds to constructive or destructive interference.)**
## Example: Loudspeakers
  
  **Problem:** Two loudspeakers emit waves of frequency 172 Hz. The speed of sound is 344 m/s. You are 8 m from speaker A. How close can you get to speaker B and still have destructive interference?
  
  **(Diagram of the loudspeakers and the listener.)**
  
  **(Solution will be worked out in lecture. It involves using the frequency and wave speed to find the wavelength, then using the condition for destructive interference to find the minimum distance to speaker B.)**