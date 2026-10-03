# Lecture 37: Lenses and Mirrors
  
  This lecture applies the ray model of light to understand how images are formed by lenses and mirrors, introducing ray tracing techniques and the lens/mirror equation.
## Terms and Sign Conventions for Lenses and Mirrors
  
  Consistent sign conventions are crucial for using the lens and mirror equations correctly.
  
  *   **Object Distance (s):**
    *   Distance from the object to the lens/mirror vertex.
    *   **s is positive** for real objects (usually the case).
  *   **Image Distance (s'):**
    *   Distance from the image to the lens/mirror vertex.
    *   **s' is positive:** Real image. Formed on the side where light *goes* after interacting (opposite side of lens for lenses, same side as object for mirrors). Real images can be projected onto a screen.
    *   **s' is negative:** Virtual image. Formed on the side where light *appears to come from* (same side as object for lenses, behind the mirror for mirrors). Virtual images cannot be projected onto a screen but can be seen by looking into the lens/mirror.
  *   **Focal Length (f):**
    *   Distance from the vertex to the focal point (F).
    *   **f is positive:** Converging elements (converging lens, concave mirror).
    *   **f is negative:** Diverging elements (diverging lens, convex mirror).
    *   For spherical mirrors: $f = R/2$, where R is the radius of curvature.
  *   **Object Height (h):**
    *   Height of the object, measured perpendicular to the principal axis.
    *   **h is positive** if the object is upright (usually assumed).
  *   **Image Height (h'):**
    *   Height of the image, measured perpendicular to the principal axis.
    *   **h' is positive** if the image is upright (same orientation as the object).
    *   **h' is negative** if the image is inverted (opposite orientation to the object).
  *   **Magnification (m):**
    *   Ratio of image height to object height.
    *   **m = h'/h**
    *   Also related to distances: **m = -s'/s**
    *   **Sign of m:**
        *   Positive m: Image is upright (virtual images often are).
        *   Negative m: Image is inverted (real images formed by single element often are).
    *   **Magnitude of m:**
        *   |m| > 1: Image is enlarged.
        *   |m| < 1: Image is reduced.
        *   |m| = 1: Image is the same size as the object.
## The Lens/Mirror Equation
  
  This fundamental equation relates the object distance, image distance, and focal length for both lenses and spherical mirrors.
  
  *   **Equation:**
  
    $\frac{1}{s} + \frac{1}{s'} = \frac{1}{f}$
  
  *   **Usage:** Allows calculation of the image location (s') if the object location (s) and focal length (f) are known, or finding f if s and s' are known, etc. Remember to use the sign conventions consistently.
## Spherical Lenses
  
  Lenses refract light to form images.
  
  *   **Converging Lens (Convex Lens):**
    *   Thicker in the center than at the edges.
    *   Causes parallel rays to converge at a focal point (F).
    *   Has a **positive focal length (f > 0)**.
    *   Has focal points on both sides, equidistant from the lens center.
  *   **Diverging Lens (Concave Lens):**
    *   Thinner in the center than at the edges.
    *   Causes parallel rays to diverge *as if* they came from a focal point (F) on the same side as the incoming light.
    *   Has a **negative focal length (f < 0)**.
    *   Has focal points on both sides.
## Ray Tracing for Lenses
  
  Ray diagrams help visualize image formation. Use at least two (preferably three) principal rays:
  
  *   **Converging Lens:**
    1.  **P-Ray:** Ray parallel to the principal axis refracts through the far focal point (F).
    2.  **F-Ray:** Ray passing through the near focal point (F') refracts parallel to the principal axis.
    3.  **C-Ray:** Ray passing through the center of the lens continues undeviated.
    *   **Image Location:** The image forms where the refracted rays (or their backward extensions for virtual images) intersect.
    *   **Cases:**
        *   Object beyond 2f (s > 2f): Image is real, inverted, reduced (f < s' < 2f).
        *   Object between f and 2f (f < s < 2f): Image is real, inverted, enlarged (s' > 2f).
        *   Object inside f (s < f): Image is virtual, upright, enlarged (s' < 0).
  
  *   **Diverging Lens:**
    1.  **P-Ray:** Ray parallel to the principal axis refracts *as if* it came from the near focal point (F). Draw the refracted ray diverging, extending it backward through F.
    2.  **F-Ray:** Ray heading towards the far focal point (F') refracts parallel to the principal axis.
    3.  **C-Ray:** Ray passing through the center of the lens continues undeviated.
    *   **Image Location:** The image forms where the backward extensions of the refracted rays intersect.
    *   **Result:** For a diverging lens, the image is **always virtual, upright, and reduced**, regardless of the object distance (s' < 0, 0 < m < 1).
## Plane Mirrors
  
  *   **Image Formation:** Light rays from an object point (A) reflect off the mirror according to the law of reflection. The reflected rays appear to diverge from a point (A') located *behind* the mirror.
  *   **Image Characteristics:**
    *   **Virtual:** Image is behind the mirror (s' is negative).
    *   **Upright:** Image has the same orientation as the object (m is positive).
    *   **Same Size:** Image height equals object height (h' = h, |m| = 1).
    *   **Same Distance:** Image distance equals object distance (|s'| = s).
## Spherical Mirrors
  
  Mirrors with a reflecting surface shaped like a section of a sphere.
  
  *   **Terminology:**
    *   **Center of Curvature (C):** The center of the sphere from which the mirror is a part.
    *   **Radius of Curvature (R):** The radius of that sphere.
    *   **Vertex (V):** The geometric center of the mirror surface.
    *   **Principal Axis:** The line passing through C and V.
    *   **Focal Point (F):** Point where parallel rays converge (concave) or appear to diverge from (convex) after reflection. Located halfway between C and V.
    *   **Focal Length (f):** Distance from V to F. $f = R/2$.
  
  *   **Concave Mirror ("Converging"):**
    *   Reflecting surface curves inward (like a cave).
    *   Parallel rays converge at the focal point F.
    *   **Positive focal length (f > 0)**.
  *   **Convex Mirror ("Diverging"):**
    *   Reflecting surface curves outward.
    *   Parallel rays reflect *as if* they diverged from a focal point F behind the mirror.
    *   **Negative focal length (f < 0)**.
## Ray Tracing for Spherical Mirrors
  
  Use at least two (preferably three) principal rays:
  
  *   **Concave Mirror:**
    1.  **P-Ray:** Ray parallel to the axis reflects through the focal point (F).
    2.  **F-Ray:** Ray passing through the focal point (F) reflects parallel to the axis.
    3.  **C-Ray:** Ray passing through the center of curvature (C) reflects back along itself.
    4.  **V-Ray:** Ray striking the vertex (V) reflects symmetrically across the principal axis ($\theta_i = \theta_r$).
    *   **Image Location:** The image forms where the reflected rays (or their backward extensions) intersect.
    *   **Cases:**
        *   Object beyond C (s > R = 2f): Image is real, inverted, reduced (f < s' < 2f). (Telescope objective)
        *   Object between C and F (f < s < 2f): Image is real, inverted, enlarged (s' > 2f). (Microscope objective principle)
        *   Object inside F (s < f): Image is virtual, upright, enlarged (s' < 0). (Makeup/shaving mirror)
  
  *   **Convex Mirror:**
    1.  **P-Ray:** Ray parallel to the axis reflects *as if* it came from the focal point (F) behind the mirror.
    2.  **F-Ray:** Ray heading towards the focal point (F) behind the mirror reflects parallel to the axis.
    3.  **C-Ray:** Ray heading towards the center of curvature (C) behind the mirror reflects back along itself.
    4.  **V-Ray:** Ray striking the vertex (V) reflects symmetrically.
    *   **Image Location:** The image forms where the backward extensions of the reflected rays intersect.
    *   **Result:** For a convex mirror, the image is **always virtual, upright, and reduced**, regardless of object distance (s' < 0, 0 < m < 1).
## Applications
  
  *   **Concave Mirrors:** Shaving/makeup mirrors, solar cookers, reflecting telescopes, satellite dishes (for EM waves).
  *   **Convex Mirrors:** Passenger-side rear-view mirrors ("Objects in mirror are closer than they appear"), security/surveillance mirrors, Christmas tree ornaments.
## Summary Table (Image Characteristics)
  
  | Type                       | Focal length f | Object distance s | Image distance s'  | Character | Orientation | Size     |
  | :------------------------- | :------------- | :---------------- | :----------------- | :-------- | :---------- | :------- |
  | **Concave Mirror / Converging Lens** | f > 0          | s > 2f            | f < s' < 2f        | real      | inverted    | reduced  |
  |                            |                | f < s < 2f        | s' > 2f            | real      | inverted    | enlarged |
  |                            |                | s < f             | s' < 0 (|s'| > s)  | virtual   | upright     | enlarged |
  | **Convex Mirror / Diverging Lens** | f < 0          | s > 0             | f < s' < 0 (|s'|<|f|) | virtual   | upright     | reduced  |
  
  **(Note: It's better to understand the principles and use the equation/ray tracing than to memorize this table.)**
## Summary
  
  This lecture covered the formation of images by lenses and spherical mirrors using the ray model. Key tools include ray tracing with principal rays and the lens/mirror equation ($\frac{1}{s} + \frac{1}{s'} = \frac{1}{f}$) combined with magnification ($m = -s'/s$). Consistent use of sign conventions is essential for correct results. Converging elements (concave mirrors, convex lenses) have f > 0, while diverging elements (convex mirrors, concave lenses) have f < 0. The characteristics of the image (real/virtual, upright/inverted, enlarged/reduced) depend on the type of element and the object's position relative to the focal point.