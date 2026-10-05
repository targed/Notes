# Part 3: Weighted MinHashing & PolyMinHash
- ### 1. The Problem with Standard MinHash (Slide 24)
  Standard MinHash is designed for **Binary Sets** (e.g., Does this word exist in the document? Yes = 1, No = 0). 
  But what if the data isn't binary?
  *   **Histograms/Vectors:** A word might appear 5 times in Document A, and 12 times in Document B. These are **real-valued/weighted** features.
  *   **Geographic Shapes:** A polygon isn't a list of words; it is a continuous 2D space with an **Area**. 
  
  You cannot use standard MinHash on continuous areas or weighted frequencies. We need a new technique where the probability of a hash collision strictly equals the **Weighted Jaccard Similarity**.
- ### 2. The Solution: Rejection Sampling & The "Dart" Analogy (Slides 25-26)
  How do we hash something based on its "weight" or "area"? We use **Rejection Sampling** (throwing darts).
  
  Imagine a 1D vector mapped out as a colored bar (Slide 25). The "weight" of the feature is the green area.
  1. You randomly throw darts at the entire bar.
  2. If the dart hits the red area (empty space), you **reject** it and throw again.
  3. You count *how many darts you had to throw* before you finally hit the green area.
  4. **The Magic:** That count (the number of attempts) becomes your **Hash Value**!
- ### 3. PolyMinHash: Adapting Darts for 2D Polygons (Slides 18-20 & Paper)
  The researchers (including your professor!) adapted this 1D weighted dart method into a 2D geometric method for spatial databases (GIS).
  
  Here is the exact **PolyMinHash Algorithm** pipeline:
  
  *   **Step 1: Centering.** Shift every polygon so its exact center (centroid) is at coordinates `(0,0)`. 
    *   *Why?* So you can compare the shape of Lake Michigan to a similarly shaped lake in Africa, regardless of where they are on the globe.
  *   **Step 2: The Global MBR.** Draw a **Global Minimum Bounding Rectangle (MBR)**. This is a massive imaginary box that is big enough to entirely enclose any polygon in the database. This box is our "Dartboard."
  *   **Step 3: Throw Darts.** Use a pseudo-random number generator (with a fixed seed so it's reproducible) to generate random 2D coordinates `(x, y)` inside the Global MBR.
  *   **Step 4: Point-in-Polygon (PnP) Test.** Check if that `(x, y)` dart landed *inside* your specific polygon. 
    *   If it misses (lands outside), increment your attempt counter and throw again.
    *   If it hits (lands inside), **STOP**. 
  *   **Step 5: The Signature.** The number of attempts it took to hit the inside of the polygon is recorded as the **MinHash value**. Repeat this $k$ times with different seeds to build a signature array.
- ### 4. The Mathematical Proof (Slide 21 & Paper Theorem 1)
  Why does counting dart throws give us Jaccard Similarity?
  
  Imagine Polygon P and Polygon Q overlapping on the dartboard. We are throwing darts.
  *   We only stop and record a hash value when a dart finally hits *either* P or Q.
  *   What is the probability that they get the **exact same hash value**?
  *   They will only get the same hash value if the *very first dart to hit either of them* happens to land in the **Intersection** (the overlapping area where P and Q both exist).
  *   The total target area that stops the game is the **Union** (the combined area of P and Q).
  *   Therefore: $$ Pr[h(P) = h(Q)] = \frac{\text{Area}(P \cap Q)}{\text{Area}(P \cup Q)} = \text{Jaccard}(P, Q) $$
  
  This brilliantly proves that throwing random 2D darts perfectly mimics the Jaccard Similarity for complex geographic shapes!
  
  ---
- ### Part 3 Practice Questions (PolyMinHash Concepts)
  
  **Q1: The Hash Value Definition**
  In the PolyMinHash algorithm, when converting a 2D shape into a MinHash signature, what exactly *is* the integer stored in the signature array?
  A) The $(x, y)$ coordinates of the center of the polygon.
  B) The area of the polygon divided by the area of the MBR.
  C) The number of random attempts (dart throws) it took for a point to land inside the polygon.
  D) The number of vertices (corners) the polygon has.
  
  **Q2: The Centering Step**
  Before generating the hash, the PolyMinHash algorithm shifts every polygon so its centroid is at `(0,0)`. If you skipped this step, what would be the catastrophic result when comparing two identical shapes located in different cities?
  A) The algorithm would enter an infinite loop.
  B) The shapes would have a Jaccard Similarity of 0 because their areas would never intersect on the global dartboard.
  C) The MBR would be too small to fit the shapes.
  D) The shapes would cause a False Positive.
  
  **Q3: Rejection Sampling Efficiency**
  Suppose you have a Global MBR of size $100 \times 100$ (Area = 10,000). Polygon A is massive and takes up 5,000 area. Polygon B is tiny and takes up only 10 area. 
  Statistically, which polygon will result in a **larger** hash value (i.e., require more attempts/darts before a hit), and why?
  
  **Q4: The MBR Filter Optimization**
  In the PolyMinHash paper (Algorithm 1), before doing the expensive "Point-in-Polygon" math check, the code checks if the dart landed inside the polygon's *Local* MBR. Why do they do this?
  *(Hint: Think about computational cost. Is checking if a point is inside a simple rectangle faster than checking if it's inside a 50-sided irregular polygon?)*
  
  ---
-