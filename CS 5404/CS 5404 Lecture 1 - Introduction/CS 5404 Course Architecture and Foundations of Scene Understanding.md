## 1. Course Profile & Administrative Strategy (CS 5404)
### A. Core Metadata
  * **Course:** COMP SCI 5404 — Intro to Computer Vision
  * **Institution:** Missouri University of Science and Technology (Missouri S&T)
  * **Instructor:** Dr. Ce Zhou (`cezhou@mst.edu`)
  * **Lectures:** In-person, Computer Science Room 222
  * **Office Hours:** Thursday 3:30 PM – 5:00 PM (CS Room 311)
  * **Primary Reference:** Richard Szeliski, [*Computer Vision: Algorithms and Applications*](https://szeliski.org/Book/) (2nd ed., freely available online); secondary reference: Reinhard Klette, *Concise Computer Vision*.
  
  ---
### B. Prerequisite Mastery Map
  Computer vision lies at the intersection of applied mathematics and computer science. The slides highlight five prerequisites; here is how they directly map to upcoming course topics:
  
  ```
  Prerequisite          -> Direct Application in CS 5404
  ────────────────────────────────────────────────────────────────────────────
  Multivariable Calc    -> Image gradients, edge detection (Sobel/Canny), 
                         Taylor expansions for optical flow (Lucas-Kanade).
  Linear Algebra        -> Homogeneous coordinates, projective geometry, camera 
                         matrices (intrinsics/extrinsics), SVD for Essential/
                         Fundamental matrix estimation (Epipolar geometry).
  Probability & Stats   -> RANSAC (robust fitting), Bayesian filtering, Markov 
                         Random Fields, Gaussian noise models, maximum likelihood.
  Data Structures       -> KD-trees for nearest-neighbor feature matching, graph-cut 
                         segmentation, spatial partitioning (octrees, voxel grids).
  Scientific Python     -> Vectorized array manipulation in NumPy, OpenCV pipelines, 
                         matrix broadcast operations, and PyTorch tensors.
  ```
  
  ---
### C. Grade Optimization & Execution Strategy
  * **Total Base Points:** 100% + Up to 5% Extra Credit.
  
  ```
  ┌───────────────────────────────────────┬───────┬──────────────────────────────────────────┐
  │ Component                             │ Weight│ Operational Rules & Best Practices       │
  ├───────────────────────────────────────┼───────┼──────────────────────────────────────────┤
  │ 5 Take-Home Programming Projects      │ 50%   │ • 10% each. Groups ≤ 3.                  │
  │ (P1–P5)                               │       │ • All collaborator names must be listed. │
  │                                       │       │ • Vectorize in NumPy; avoid slow loops.  │
  ├───────────────────────────────────────┼───────┼──────────────────────────────────────────┤
  │ 5 Concept Assignments                 │ 20%   │ • 5% each; LOWEST SCORE DROPPED.         │
  │ (C1–C5)                               │       │ • Groups ≤ 3 discussion allowed, but     │
  │                                       │       │   WRITE-UPS MUST BE 100% INDEPENDENT.    │
  ├───────────────────────────────────────┼───────┼──────────────────────────────────────────┤
  │ Final Project (FP)                    │ 30%   │ • 20% Submission / Report, 10% Oral Pres.│
  │                                       │       │ • Solo or teams ≤ 3 (scaled rubric).     │
  │                                       │       │ • Open-ended: connect theory to new app. │
  ├───────────────────────────────────────┼───────┼──────────────────────────────────────────┤
  │ Bonus Points                          │ +5%   │ • +3% Full semester lecture attendance.  │
  │                                       │       │ • +2% Asking an insightful Q on pres day.│
  └───────────────────────────────────────┴───────┴──────────────────────────────────────────┘
  ```
#### Late Policy Nuance
  * **Grace Period:** Every assignment (except the Final Project) has a **2-day grace period with 0% penalty**. No email requests needed.
  * **Extended Penalty:** After day 2, a **10% per day penalty** applies up to day 7 (1 week total). Beyond day 7, the submission receives zero credit.
  * **Academic Integrity & Generative AI:** AI tools (ChatGPT, Copilot) are permitted **solely** for ideation, syntax clarification, and concept debugging. Code/text copy-pasting is flagged under university academic dishonesty policies resulting in an automatic zero.
  
  ---
## 2. Defining Computer Vision: The Inverse Problem
### A. The Core Objective
  > **Formal Definition:** Computer Vision is the automated extraction, analysis, and understanding of useful information from a single image or a sequence of images (video), reconstructing the 3D physical world and its semantic meaning from 2D projections.
### B. Computer Graphics vs. Computer Vision
  To understand why vision is difficult, compare it to its sister discipline, Computer Graphics:
  
  ```
         [3D World Model]
         • Geometry (meshes, voxels)
         • Physics (BRDF, reflectance)
         • Radiometry (light sources)
         • Camera dynamics (focal length, pose)
              │                    ▲
              │                    │
   GRAPHICS   │                    │   VISION
  (Forward    │                    │  (Inverse Problem:
   Problem)   │                    │   Lossy, Ill-Posed)
              ▼                    │
         [2D Image Plane]
         • Array of pixel intensities: I(x, y) = [R, G, B]
  ```
  
  * **Computer Graphics (The Forward Problem):** Well-posed and deterministic. Given complete 3D scene parameters, physics and projective geometry determine exactly where each photon lands on a virtual sensor.
  * **Computer Vision (The Inverse Problem):** Ill-posed and heavily underconstrained. When a 3D scene is projected onto a 2D sensor plane, the depth dimension ($Z$) is collapsed:
  
  $$\begin{pmatrix} x \\ y \\ 1 \end{pmatrix} \sim \mathbf{K} \begin{bmatrix} \mathbf{R} & \mathbf{t} \end{bmatrix} \begin{pmatrix} X \\ Y \\ Z \\ 1 \end{pmatrix}$$
  
  Infinitely many 3D configurations can project to the exact same 2D pixel pattern. Vision algorithms must introduce **prior assumptions, regularization constraints, and semantic heuristics** to resolve this fundamental ambiguity.
  
  ---
## 3. Deconstructing the "Messy World" (Slides 2–6)
  
  The introductory slides challenge us with an everyday, cluttered indoor scene (Slide 2) and an isometric apartment rendering (Slide 6). These illustrate the gap between raw pixel arrays and high-level scene understanding.
  
  ---
### A. Case Study: The Cluttered Kitchen Window (Slides 2–4)
  
  ```
       [ Outdoor Light: Sky / Overcast ]
                     │
                     ▼
          ┌─────────────────────┐
          │  Hanging Pots &     │ ◄── Rhododendron outside:
          │  Houseplants        │     Leaves curled downwards
          ├─────────────────────┤
          │  Sheer Curtains     │ ◄── Translucent; diffuse scattering
          ├─────────────────────┤
          │  Countertop Clutter │ ◄── Transparent glass, opaque ceramic,
          │  (Jars, Bowls, Pens)│     specular highlights, heavy occlusions
          └─────────────────────┘
  ```
  
  When humans inspect this scene, our visual cortex immediately extracts rich contextual layers:
#### 1. Low-Level Visual Features vs. High-Level Semantic Labels
  * **Raw Machine View:** A 2D array of discrete values $I(x, y) \in [0, 255]^3$. Edge detectors find thousands of high-frequency gradient discontinuities (leaves, jar rims, reflections).
  * **Human Perception:** We instantly group those discontinuities into coherent objects: *ceramic bowls, pens in holders, glass containers, hanging spider plants, a cardinal ornament, sheer curtains*.
#### 2. Spatial & Functional Inference (Where are we?)
  * Even without seeing an oven or stove, we infer **"Kitchen / Breakfast Nook"** through contextual co-occurrences: stacked cereal/soup bowls, spice/preserve jars, napkin dispensers, and high morning-window illumination.
#### 3. Temporal Inference (Time of Day)
  * The angle, color temperature, and intensity of the incoming light, combined with the lack of artificial room illumination, suggest **daytime (likely mid-morning)**.
#### 4. The Rhododendron Phenomenon: Cross-Domain Physical Inference
  Slide 4 poses a classic computer vision challenge: **"What is the temperature outside?"**
  * Looking closely through the window glass, rhododendron foliage is visible. 
  * **Biological Context (Thermonasty):** Rhododendron leaves display *thermonastic curling*—their leaves curl inward into tight cylinders and droop when ambient temperatures drop below freezing ($0^\circ\text{C} / 32^\circ\text{F}$), a protective mechanism against cellular desiccation.
  * **The Computer Vision Lesson:** True scene understanding often requires **cross-domain knowledge integration** (biology, thermodynamics, physics) rather than simple pattern recognition over isolated bounding boxes.
  
  ---
### B. What Does an Embodied Robot Need to Know? (Slide 6)
  
  Slide 6 shows a 3D isometric living room (couch, beanbag, TV unit, indoor tree, coffee table, ambient lighting). If an autonomous agent (e.g., a home mobile manipulator or vacuum) enters this space, standard 2D object detection is not enough. The robot requires four distinct layers of perceptual representation:
  
  ```
  ┌────────────────────────┬────────────────────────────────┬───────────────────────────┐
  │ Perceptual Layer       │ Question Answered              │ Mathematical / CV Tool    │
  ├────────────────────────┼────────────────────────────────┼───────────────────────────┤
  │ 1. Metric / Geometric  │ "Where is open, collision-free │ Depth estimation, SLAM,   │
  │    Representation      │ space to traverse?"            │ 3D Point Clouds, OctoMaps │
  ├────────────────────────┼────────────────────────────────┼───────────────────────────┤
  │ 2. Semantic Parsing    │ "Which object is the sofa, the │ Semantic Segmentation,    │
  │                        │ TV, or the doorway?"           │ 3D Bounding Boxes         │
  ├────────────────────────┼────────────────────────────────┼───────────────────────────┤
  │ 3. Affordance & Physics│ "Can I step on the beanbag?    │ Contact point estimation, │
  │                        │ Can the table support weight?" │ material classification   │
  ├────────────────────────┼────────────────────────────────┼───────────────────────────┤
  │ 4. Illumination / State│ "Is the room lit? Is the TV on │ Radiometry, Photometric   │
  │                        │ or off? Is the door open?"     │ Stereo, State Tracking    │
  └────────────────────────┴────────────────────────────────┴───────────────────────────┘
  ```
  
  1. **Geometric Layer (Metric Depth):** Pixel coordinates $(u, v)$ must be mapped to metric $(X, Y, Z)$ coordinates relative to the robot base so its motion planner does not collide with the coffee table.
  2. **Semantic Layer:** Differentiating navigable surfaces (hardwood floor vs. low rug) from obstacles.
  3. **Affordance & Material Properties:** A beanbag looks like a low obstacle, but unlike a solid wooden footstool, it deforms under load. A robot cannot set a beverage tray on a compliant beanbag.
  4. **Task-Relevant State Estimation:** Knowing whether the TV cabinet drawers are closed, whether pathways to the door are clear, and understanding light switches and lighting sources.
  
  ---
## 4. Context Over Resolution: The "Tiny Images" Insight (Slides 9–10)
  
  Slide 10 presents an extremely low-resolution, heavily pixelated image (from the seminal *80 Million Tiny Images* dataset by Torralba, Fergus, and Freeman, 2008) and asks:
  
  > **"How many people are in this image?"**
  
  ```
               [ Heavily Blurred 32x32 Patch ]
    ┌─────────────────────────────────────────────────────┐
    │  [Dark vertical blob]  [Dark vertical blob] [White] │
    │         │                     │                │    │
    │      Person 1              Person 2         Person 3│
    │         └── Background: Vertical stripes ──┘        │
    │             resembling an American Flag / Podium    │
    └─────────────────────────────────────────────────────┘
  ```
### Why This Matters:
  * **The Classical Sensor Trap:** An intuitive engineering reaction to computer vision challenges is: *"We just need higher-resolution sensors (4K, 8K, HDR)."*
  * **The Reality of Visual Cognition:** Humans can resolve the presence of **three to four people standing in front of an American flag (likely a press briefing or political stage)** even when individual faces consist of only 4 to 6 pixels, and eye/mouth features are absent.
  * **The Mechanism:** The human visual system leverages **global context, structural priors, and semantic expectations** to resolve low-level ambiguity:
  * Vertical symmetry implies an upright bipedal posture.
  * Dark lower halves indicate suits or formal trousers.
  * Contrast against a striped backdrop implies an indoor podium or press event.
  * **Machine Vision Takeaway:** Low-level features (edges, corners, color histograms) are brittle in isolation. Robust visual systems must combine local measurements with high-level structural models and scene context.
  
  ---
## Checkpoint & Discussion
  
  > **Concept Check:** If a camera takes a picture of a miniature toy car held 10 inches from the lens, and another picture of a real car 50 yards away, their 2D projections can look identical in scale and geometry. 
  > 
  > What visual cues and mathematical assumptions do both human vision and computer algorithms use to resolve this scale ambiguity?