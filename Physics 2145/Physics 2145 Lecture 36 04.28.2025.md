# Lecture 36: Ray Optics
  
  This lecture introduces the Ray Model of Light, which describes light propagation in terms of straight-line paths called rays. This model is useful for understanding reflection and refraction.
## Ray Model of Light
  
  *   **Basic Assumptions:**
    1.  **Straight Line Travel:** Light travels in straight lines (rays) in a uniform medium.
    2.  **Rays Can Cross:** Light rays can intersect without affecting each other.
    3.  **Travel Until Interaction:** Light rays travel indefinitely unless they interact with matter (reflection, refraction, absorption).
    4.  **Objects as Sources:** Objects are treated as sources of light rays. If we see an object, it's either emitting light (like the sun or a lamp) or reflecting light from another source.
    5.  **Rays from Every Point:** Light rays originate from every point on an object and travel outward in all directions.
  *   **Ray Diagrams:** Diagrams used to visualize the path of light. Typically, only a few representative rays are drawn (e.g., from the top and bottom of an object) to understand image formation or path changes.
## Reflection
  
  *   **Definition:** Reflection occurs when light bounces off the surface of a material.
  *   **Terminology:**
    *   **Incident Ray:** The incoming light ray.
    *   **Reflected Ray:** The outgoing light ray after bouncing off the surface.
    *   **Normal:** A line drawn perpendicular to the surface at the point of incidence.
    *   **Angle of Incidence ($\theta_i$):** The angle between the incident ray and the normal.
    *   **Angle of Reflection ($\theta_r$):** The angle between the reflected ray and the normal.
  *   **Law of Reflection:** The angle of incidence equals the angle of reflection.
  
    $\theta_i = \theta_r$
  
    *   **Important:** Angles are always measured relative to the **normal** to the surface, not the surface itself.
    *   The incident ray, the reflected ray, and the normal all lie in the same plane.
## Refraction
  
  *   **Definition:** Refraction is the bending of light rays as they pass from one transparent medium into another.
  *   **Cause:** Refraction occurs because the speed of light is different in different media (characterized by the refractive index, *n*).
  *   **Terminology:**
    *   **Angle of Incidence ($\theta_a$ or $\theta_1$):** The angle between the incident ray (in medium *a*) and the normal.
    *   **Angle of Refraction ($\theta_b$ or $\theta_2$):** The angle between the refracted ray (in medium *b*) and the normal.
    *   **Refractive Index ($n_a$, $n_b$):** The refractive indices of the two media.
## Snell's Law
  
  Snell's Law relates the angles of incidence and refraction to the refractive indices of the two media.
  
  *   **Formula:**
  
    $n_a \sin\theta_a = n_b \sin\theta_b$
  
    Or, using alternative notation:
  
    $n_1 \sin\theta_1 = n_2 \sin\theta_2$
  
  *   **Interpretation:**
    *   If light enters a medium with a *higher* refractive index ($n_b > n_a$), the ray bends *towards* the normal ($\theta_b < \theta_a$). (e.g., air to water).
    *   If light enters a medium with a *lower* refractive index ($n_b < n_a$), the ray bends *away* from the normal ($\theta_b > \theta_a$). (e.g., water to air).
    *   If light enters perpendicular to the surface ($\theta_a = 0^\circ$), then $\sin\theta_a = 0$, which means $\sin\theta_b = 0$ and $\theta_b = 0^\circ$. The ray is **not bent** (it doesn't change direction), although its speed and wavelength change. Some light is still reflected.
  
  *   **Caution:** Remember that angles are always measured from the **normal**.
## Example (Conceptual, using slide diagrams)
  
  *   **Air to Water:**
    *   $n_a \approx 1.00$ (air), $n_b \approx 1.33$ (water). $n_b > n_a$.
    *   Snell's Law: $1.00 \sin\theta_a = 1.33 \sin\theta_b$.
    *   Since $1.33 > 1.00$, we must have $\sin\theta_b < \sin\theta_a$, which means $\theta_b < \theta_a$. The ray bends **towards** the normal.
  *   **Water to Air:**
    *   $n_a \approx 1.33$ (water), $n_b \approx 1.00$ (air). $n_a > n_b$.
    *   Snell's Law: $1.33 \sin\theta_a = 1.00 \sin\theta_b$.
    *   Since $1.00 < 1.33$, we must have $\sin\theta_b > \sin\theta_a$, which means $\theta_b > \theta_a$. The ray bends **away** from the normal.
## Total Internal Reflection (TIR)
  
  *   **Condition:** TIR can only occur when light travels from a medium with a *higher* refractive index ($n_1$) to a medium with a *lower* refractive index ($n_2$) (i.e., $n_1 > n_2$).
  *   **Mechanism:** As the angle of incidence ($\theta_1$) in the denser medium increases, the angle of refraction ($\theta_2$) in the less dense medium also increases, bending further away from the normal ($n_1 \sin\theta_1 = n_2 \sin\theta_2$).
  *   **Critical Angle ($\theta_C$):** There is a specific angle of incidence, called the critical angle, at which the angle of refraction reaches its maximum possible value, $\theta_2 = 90^\circ$. At this angle, the refracted ray skims along the boundary between the two media.
    *   Setting $\theta_2 = 90^\circ$ in Snell's Law:
        $n_1 \sin\theta_C = n_2 \sin(90^\circ)$
        $n_1 \sin\theta_C = n_2 (1)$
    *   **Formula for Critical Angle:**
  
        $\sin\theta_C = \frac{n_2}{n_1}$   (where $n_1 > n_2$)
  
  *   **Total Internal Reflection:** If the angle of incidence $\theta_1$ is *greater* than the critical angle ($\theta_1 > \theta_C$), Snell's Law would require $\sin\theta_2 > 1$, which is impossible. In this case, **no light is refracted** into the second medium. All the light is **reflected** back into the first medium, obeying the law of reflection ($\theta_i = \theta_r$). This phenomenon is called total internal reflection.
  
  *   **Illustration:**
    1.  Ray incident normally ($\theta_1=0$): Passes straight through (some reflection occurs).
    2.  Increasing $\theta_1$: Ray bends away from normal ($\theta_2 > \theta_1$).
    3.  $\theta_1 = \theta_C$: Refracted ray travels along the boundary ($\theta_2 = 90^\circ$).
    4.  $\theta_1 > \theta_C$: No refracted ray; all light is reflected back into medium 1.
  
  *   **Applications:** Fiber optics, prisms in binoculars and cameras, diamond sparkle.
## Summary
  
  The ray model treats light as straight lines. Reflection follows the law $\theta_i = \theta_r$. Refraction, the bending of light between media, is governed by Snell's Law: $n_1 \sin\theta_1 = n_2 \sin\theta_2$. Angles are measured relative to the normal. When light travels from a denser medium ($n_1$) to a less dense medium ($n_2$), total internal reflection occurs if the angle of incidence exceeds the critical angle, $\theta_C$, where $\sin\theta_C = n_2/n_1$.