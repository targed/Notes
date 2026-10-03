# Lecture 35: Wave Optics
  
  This lecture transitions from geometric optics (ray model) to wave optics, exploring phenomena that demonstrate the wave nature of light, particularly interference.
## Refractive Index (Review)
  
  *   **Definition:** The refractive index (*n*) of a medium describes how much the speed of light is reduced in that medium compared to its speed in a vacuum (*c*).
  *   **Formula:**
  
    $n = \frac{c}{v}$
  
    Where:
    *   *n* is the refractive index (dimensionless, n ≥ 1)
    *   *c* is the speed of light in vacuum ($c \approx 3.00 \times 10^8$ m/s)
    *   *v* is the speed of light in the medium.
  *   **Examples:** n ≈ 1.00 for vacuum/air, n ≈ 1.33 for water, n ≈ 1.5 for glass.
## Wavelength and Frequency in a Medium
  
  *   When light enters a medium from a vacuum (or air):
    *   The **frequency (f)** of the light wave **remains the same**. Frequency is determined by the source.
    *   The **speed (v)** of the light wave **decreases** ($v = c/n$).
    *   The **wavelength ($\lambda$)** of the light wave **decreases**.
  *   **Relationship:** The speed of a wave is given by $v = \lambda f$.
  *   **Wavelength in a Medium ($\lambda_n$):**
    *   In vacuum: $c = \lambda_{vac} f$
    *   In medium: $v = \lambda_n f$
    *   Since $v = c/n$: $\lambda_n f = (\lambda_{vac} f) / n$
    *   Therefore:
  
        $\lambda_n = \frac{\lambda_{vac}}{n}$
  
    The wavelength in a medium is shorter than the wavelength in a vacuum by a factor of *n*.
## Evidence for the Wave Nature of Light: Diffraction
  
  *   **Geometric Optics Limitation:** Geometric optics (rays) predicts that light passing through a narrow slit would produce a sharp shadow.
  *   **Wave Behavior (Diffraction):** Experiments show that light, like water waves, *spreads out* after passing through a narrow opening. This phenomenon is called **diffraction**.
  *   **Observation:** When light passes through a very narrow slit, the pattern observed on a screen behind it is not just a sharp image of the slit but a broader pattern with fringes.
  *   **Conclusion:** Diffraction provides strong evidence that light behaves like a wave.
## Interference Phenomena
  
  Interference occurs when two or more waves overlap in space.
  
  *   **Superposition Principle:** When waves overlap, the resulting displacement at any point is the vector sum of the displacements due to each individual wave.
  *   **Types of Interference:**
    *   **Constructive Interference:** Occurs when waves arrive *in phase* (crests meet crests, troughs meet troughs). The resulting amplitude is larger than the individual amplitudes.
    *   **Destructive Interference:** Occurs when waves arrive *out of phase* (crests meet troughs). The resulting amplitude is smaller than the individual amplitudes (can be zero if amplitudes are equal).
## Young's Double-Slit Interference Experiment (Thomas Young, ~1801)
  
  This classic experiment provided definitive evidence for the wave nature of light and allowed for the measurement of its wavelength.
  
  *   **Setup:**
    1.  Light (originally sunlight, now often a laser) passes through a single narrow slit (to ensure the light is coherent, though often omitted in simplified diagrams if using a laser).
    2.  This light then illuminates two closely spaced, parallel narrow slits (S1 and S2).
    3.  An observation screen is placed some distance away from the double slits.
  *   **Observation:** Instead of two bright lines corresponding to the slits, a pattern of alternating bright and dark bands (fringes) appears on the screen.
    *   **Bright Fringes:** Regions of constructive interference.
    *   **Dark Fringes:** Regions of destructive interference.
## Interference and Path Length Difference
  
  Consider two coherent sources (like the light emerging from slits S1 and S2, assuming the original light source is coherent) emitting waves in phase. We want to determine the type of interference at a point P on the screen.
  
  *   **Path Lengths:** Let L<sub>1</sub> be the distance from source S1 to point P, and L<sub>2</sub> be the distance from source S2 to point P.
  *   **Path Length Difference (ΔL):** The difference in the distances traveled by the two waves is:
  
    $\Delta L = |L_2 - L_1|$
  
  *   **Condition for Constructive Interference (Bright Fringes):** Constructive interference occurs when the path length difference is an integer multiple of the wavelength ($\lambda$):
  
    $\Delta L = m\lambda$   (where $m = 0, 1, 2, 3, ...$)
  
    This means the waves arrive at P in phase (shifted by a whole number of wavelengths).
    *   m = 0 corresponds to the central bright fringe (where ΔL = 0).
    *   m = 1 corresponds to the first-order bright fringes on either side, etc.
  
  *   **Condition for Destructive Interference (Dark Fringes):** Destructive interference occurs when the path length difference is a half-integer multiple of the wavelength:
  
    $\Delta L = (m + \frac{1}{2})\lambda$   (where $m = 0, 1, 2, 3, ...$)
  
    This means the waves arrive at P exactly out of phase (shifted by an integer number of wavelengths plus half a wavelength).
    *   m = 0 corresponds to the first dark fringes on either side ($\Delta L = \lambda/2$).
    *   m = 1 corresponds to the second dark fringes ($\Delta L = 3\lambda/2$), etc.
## Summary
  
  Wave optics considers the wave properties of light. Key concepts include the refractive index, the change in wavelength ($\lambda_n = \lambda_{vac}/n$) but not frequency when light enters a medium, and diffraction (spreading of waves) as evidence for light's wave nature. Young's double-slit experiment demonstrates interference, where waves from two slits superimpose. Constructive interference (bright fringes) occurs when the path length difference is $\Delta L = m\lambda$, and destructive interference (dark fringes) occurs when $\Delta L = (m + 1/2)\lambda$.