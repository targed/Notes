## Introduction
  
  This lecture introduces wave motion, covering the basic properties of waves, including traveling waves, sinusoidal waves, and the Doppler effect.  We will explore:
  
  * Longitudinal and transverse waves
  * Traveling waves
  * Wave length, frequency, and speed
  * Distinction between wave speed and speed of a particle
  * Speed of a wave on a string
  * Doppler effect
  
  **Simulations:**  The Physlet simulations used in this lecture are from Davidson College: http://webphysics.davidson.edu/Applets/Applets.html
## What is a Wave?
  
  * **Definition:** A wave is a self-propagating disturbance in a medium.
  * **Medium's role:** The material of the medium does *not* move along with the wave. Instead, particles of the medium are momentarily displaced from their equilibrium positions and then return to them.  The disturbance, or wave, is what travels.
  
  **(Demonstrations showing wave motion. Examples: Slinky, rope, water waves.)**
## Transverse and Longitudinal Waves
  
  * **Transverse wave:** Particle displacement is *perpendicular* to the direction of wave propagation. (Examples: waves on a string, water waves)
  * **Longitudinal wave:** Particle displacement is *parallel* to the direction of wave propagation. (Example: sound waves)
  
  **Simulation:** http://webphysics.davidson.edu/physlet_resources/bu_semester1/c20_translong.html
## Periodic Waves
  
  * **Sine waves:** We will focus on sine-shaped waves (sinusoidal waves) because any periodic function can be expressed as a superposition of sine functions (Fourier analysis).
  
  **(Diagram of a complex periodic wave)**
  
  * **Fourier analysis:**  Building complex waveforms using the superposition of sine waves.
  
  **Simulation:** http://webphysics.davidson.edu/physlet_resources/bu_semester1/c22_squarewave_sim.html
## Sinusoidal Waves
  
  * **Snapshot at a fixed time:** A sinusoidal wave can be described by the equation:
   $y(x) = A\sin(kx + \phi)$
    * $y(x)$: Displacement of the medium at position $x$.
    * $A$: Amplitude (maximum displacement).
    * $k$: Wave number (related to the wavelength).
    * $\phi$: Phase constant.
  
  **(Diagram of a sinusoidal wave showing the wavelength and amplitude)**
## Wavelength and Wave Number
  
  * **Wavelength ($\lambda$):** The distance between two consecutive points on a wave that are in phase (e.g., two crests or two troughs).
  * **Relationship between $y(x)$ and $y(x+\lambda)$:** $y(x + \lambda) = y(x)$ (since the wave repeats every wavelength).
  * **Deriving wave number:**
    * $A\sin(k(x + \lambda) + \phi) = A\sin(kx + \phi)$
    * $k(x + \lambda) + \phi = kx + \phi + 2\pi$
    * $k\lambda = 2\pi$
    * $k = \frac{2\pi}{\lambda}$
  
  **(Diagram of a sinusoidal wave illustrating the wavelength)**
## Time Dependence at a Fixed Position
  
  * **Particle located at $x=0$:** The time dependence of the wave is given by:
   $y(t) = A\sin(\omega t + \phi)$
    * $\omega$: Angular frequency.
  
  * **Angular frequency, period, frequency**: $\omega = \frac{2\pi}{T} = 2\pi f$.  Where $T$ is the period and $f$ is the frequency.
  
  **Simulation:**  http://webphysics.davidson.edu/physlet_resources/bu_semester1/c20_wavelength_period.html
## Traveling Wave
  
  * **Moving wave:** Combining the spatial and temporal dependence, we get the equation for a traveling wave:
   $y(x, t) = A\sin(k(x \mp vt) + \phi)$ which simplifies to $y(x, t) = A\sin(kx \mp \omega t + \phi)$ since $v = \frac{\omega}{k}$.
  
  * **Wave speed ($v$):** The speed at which the wave propagates through the medium.  It is related to the angular frequency and wave number: $v = \frac{\omega}{k}$.
  
  * **Sign convention:**
    * Use $-\omega$ for a wave moving in the *positive* x-direction ($v_x = +v$).
    * Use $+\omega$ for a wave moving in the *negative* x-direction ($v_x = -v$).
## Wave Speed and Direction
  
  * **Equation:**  $y(x, t) = A\sin(kx \mp \omega t + \phi)$
  * **Wave speed:** $v = \frac{\omega}{k} = \lambda f = \frac{\lambda}{T}$  The wave travels one wavelength during one period.
## Transverse Velocity
  
  * **Particle velocity ($v_y$):**  The velocity of a particle in the medium as it oscillates. This is *not* the same as the wave speed. The particle motion is perpendicular to the direction of wave propagation for a transverse wave.
  * **Calculating $v_y$:**  For a particle at a fixed position $x$:
  $v_y = \frac{\partial y}{\partial t} = \pm A\omega \cos(kx \mp \omega t + \phi)$
  * **Caution:** $v_y$ (particle velocity) is not equal to $v_x$ (wave speed).
## Maximum Transverse Speed
  
  * **Maximum value of cosine:** The cosine function varies between -1 and +1.
  * **Maximum transverse speed:** The maximum transverse speed of a particle in the medium is given by $|v_{y_{max}}| = A\omega$.
## Example: Traveling Wave
  
  **Problem:** If $y(x,t) = 3\sin(2x + 8t + \frac{1}{4}\pi)$ (in SI units):
  
  1. What is the speed and direction of this traveling wave?
  2. What is the maximum speed of a particle in the medium?
  
  **Solution:**
  
  1. **Wave speed and direction:** Comparing the given equation to $y(x,t) = A\sin(kx \mp \omega t + \phi)$, we identify $k=2$ m$^{-1}$ and $\omega = 8$ s$^{-1}$.  Since the sign in front of $\omega$ is positive, the wave is traveling in the negative x-direction. The speed is $v = \frac{\omega}{k} = \frac{8}{2} = 4$ m/s.
  2. **Maximum particle speed:** The maximum speed of a particle is $A\omega = (3)(8) = 24$ m/s.
## Speed of a Transverse Wave on a String
  
  * **Formula:** $v = \frac{\omega}{k} = \sqrt{\frac{F_T}{\mu}}$
    * $F_T$: Tension in the string
    * $\mu$: Linear mass density (mass per unit length) of the string.
  
  * **Caution:** The speed of a transverse wave on a string is *not* the same as the transverse speed of a particle in the string (as discussed earlier).
## Doppler Effect
  
  * **Definition:** The change in frequency of a wave for an observer moving relative to the source of the wave.
  * **Formula:** $f_o = \frac{v - v_{Ox}}{v - v_{Sx}} f_s$
    * $f_o$: Observed frequency
    * $f_s$: Source frequency
    * $v$: Speed of the wave in the medium
    * $v_{Ox}$: x-component of the observer's velocity (positive if moving towards the source).
    * $v_{Sx}$: x-component of the source's velocity (positive if moving towards the observer).
  
  **(Diagram illustrating the Doppler effect)**
  
  * **Simulation:** http://webphysics.davidson.edu/physlet_resources/bu_semester1/c21_doppler.html
## Example: Doppler Effect
  
  **Problem:** You are exploring a planet in a very fast ground vehicle. The speed of sound in the planet's atmosphere is 250 m/s. You are driving straight toward a cliff wall at 50 m/s. In panic, you blow your emergency horn to warn the cliff wall (or your friend standing near it). If the frequency of your horn is 1000 Hz, what is the frequency your friend will hear? What is the frequency of the sound you hear reflected off the cliff before you crash into it?
  
  **(Solution will be discussed during the lecture.)**
## Beats
  
  * **Definition:** When two waves with slightly different frequencies ($f_1$ and $f_2$) interfere, they produce beats, which are periodic variations in amplitude.
  * **Beat frequency:** $f_{beat} = |f_1 - f_2|$
  * **Simulation:** http://webphysics.davidson.edu/physlet_resources/bu_semester1/c22_beats.html
  * **Demo:** 440 Hz and 441 Hz tones played together, demonstrating beats.