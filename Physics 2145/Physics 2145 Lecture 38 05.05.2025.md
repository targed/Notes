# Lecture 38: Lenses and Mirrors - Calculations
  
  This lecture applies the lens/mirror equation and sign conventions reviewed in the previous lecture to perform quantitative calculations for image formation.
## Review: Terms and Sign Conventions
  
  It is essential to use the correct sign conventions when applying the equations:
  
  *   **Object Distance (s):** Positive for real objects (almost always).
  *   **Image Distance (s'):**
    *   Positive (+) for **real images** (formed on the side where light goes: opposite side for lens, same side for mirror).
    *   Negative (-) for **virtual images** (formed where light appears to come from: same side for lens, behind mirror).
  *   **Focal Length (f):**
    *   Positive (+) for **converging** elements (concave mirror, converging/convex lens).
    *   Negative (-) for **diverging** elements (convex mirror, diverging/concave lens).
  *   **Object Height (h):** Positive (+) if upright.
  *   **Image Height (h'):**
    *   Positive (+) if **upright**.
    *   Negative (-) if **inverted**.
  *   **Magnification (m):**
    *   $m = h'/h = -s'/s$
    *   Positive (+) m indicates an **upright** image.
    *   Negative (-) m indicates an **inverted** image.
    *   $|m| > 1$ indicates enlarged image.
    *   $|m| < 1$ indicates reduced image.
## Review: Lens/Mirror Equation and Magnification
  
  *   **Lens/Mirror Equation:** Relates object distance, image distance, and focal length.
  
    $\frac{1}{s} + \frac{1}{s'} = \frac{1}{f}$
  
  *   **Magnification:** Relates image size and orientation to object size and orientation, and also relates distances.
  
    $m = \frac{h'}{h} = -\frac{s'}{s}$
## Converging vs. Diverging Elements
  
  *   **Converging (Concave Mirror / Converging Lens):** Have a **positive focal length (f > 0)**. They can form both real and virtual images depending on the object position.
  *   **Diverging (Convex Mirror / Diverging Lens):** Have a **negative focal length (f < 0)**. They *always* form virtual, upright, reduced images of real objects.
## Review: Summary Table (Image Characteristics)
  
  | Type                       | Focal length f | Object distance s | Image distance s'  | Character | Orientation | Size     |
  | :------------------------- | :------------- | :---------------- | :----------------- | :-------- | :---------- | :------- |
  | **Concave Mirror / Converging Lens** | f > 0          | s > 2f            | f < s' < 2f        | real      | inverted    | reduced  |
  |                            |                | f < s < 2f        | s' > 2f            | real      | inverted    | enlarged |
  |                            |                | s < f             | s' < 0 (|s'| > s)  | virtual   | upright     | enlarged |
  | **Convex Mirror / Diverging Lens** | f < 0          | s > 0             | f < s' < 0 (|s'|<|f|) | virtual   | upright     | reduced  |
  
  **(Reminder: Understanding the equations and sign conventions is more useful than memorizing this table.)**
## Example 1: Concave Mirror
  
  **Problem:**
  
  An object is placed 30 cm in front of a concave mirror of focal length 20 cm. Find the image distance and magnification. Describe the image.
  
  **Solution:**
  
  1.  **Identify Given Values and Signs:**
    *   Object is placed in front: $s = +30$ cm
    *   Concave mirror (converging): $f = +20$ cm
  
  2.  **Find Image Distance (s'):** Use the lens/mirror equation:
    $\frac{1}{s} + \frac{1}{s'} = \frac{1}{f}$
    $\frac{1}{+30 \text{ cm}} + \frac{1}{s'} = \frac{1}{+20 \text{ cm}}$
    $\frac{1}{s'} = \frac{1}{20 \text{ cm}} - \frac{1}{30 \text{ cm}}$
    $\frac{1}{s'} = \frac{3}{60 \text{ cm}} - \frac{2}{60 \text{ cm}} = \frac{1}{60 \text{ cm}}$
    $s' = +60 \text{ cm}$
  
  3.  **Find Magnification (m):** Use the magnification formula:
    $m = -\frac{s'}{s} = -\frac{+60 \text{ cm}}{+30 \text{ cm}} = -2$
  
  4.  **Describe the Image:**
    *   $s' = +60$ cm: Since s' is positive, the image is **real**. It is located 60 cm in front of the mirror.
    *   $m = -2$:
        *   Since m is negative, the image is **inverted**.
        *   Since $|m| = 2 > 1$, the image is **enlarged** (twice the size of the object).
  
    *(This corresponds to the case f < s < 2f for a concave mirror).*
## Example 2: Diverging Lens
  
  **Problem:**
  
  An object is placed 30 cm in front of a diverging lens of focal length -20 cm. Find the image distance and magnification. Describe the image.
  
  **Solution:**
  
  1.  **Identify Given Values and Signs:**
    *   Object is placed in front: $s = +30$ cm
    *   Diverging lens: $f = -20$ cm
  
  2.  **Find Image Distance (s'):** Use the lens/mirror equation:
    $\frac{1}{s} + \frac{1}{s'} = \frac{1}{f}$
    $\frac{1}{+30 \text{ cm}} + \frac{1}{s'} = \frac{1}{-20 \text{ cm}}$
    $\frac{1}{s'} = -\frac{1}{20 \text{ cm}} - \frac{1}{30 \text{ cm}}$
    $\frac{1}{s'} = -\frac{3}{60 \text{ cm}} - \frac{2}{60 \text{ cm}} = -\frac{5}{60 \text{ cm}} = -\frac{1}{12 \text{ cm}}$
    $s' = -12 \text{ cm}$
  
  3.  **Find Magnification (m):** Use the magnification formula:
    $m = -\frac{s'}{s} = -\frac{-12 \text{ cm}}{+30 \text{ cm}} = +\frac{12}{30} = +0.4$
  
  4.  **Describe the Image:**
    *   $s' = -12$ cm: Since s' is negative, the image is **virtual**. It is located 12 cm from the lens on the *same side* as the object.
    *   $m = +0.4$:
        *   Since m is positive, the image is **upright**.
        *   Since $|m| = 0.4 < 1$, the image is **reduced** (0.4 times the size of the object).
  
    *(As expected for a diverging lens, the image is virtual, upright, and reduced).*
## Summary
  
  This lecture reinforced the use of the lens/mirror equation and magnification formula through specific numerical examples. Careful application of the sign conventions is critical to correctly determine the location, nature (real/virtual), orientation (upright/inverted), and size (enlarged/reduced) of images formed by lenses and mirrors.