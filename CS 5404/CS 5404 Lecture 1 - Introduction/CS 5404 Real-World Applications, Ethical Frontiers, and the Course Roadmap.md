## 1. Everyday Consumer Vision & Biometrics (Slides 26–28)
  
  Computer vision has shifted from high-performance server clusters to sub-watt embedded mobile silicon. Slides 26–28 highlight three ubiquitous consumer applications:
  
  ```
                    [ Everyday Mobile Vision ]
                                │
       ┌────────────────────────┼────────────────────────┐
       ▼                        ▼                        ▼
  Face Detection             Face Unlock            Augmented Reality
  (Slide 26)                 (Slide 27)               (Slide 28)
  • 2D Bounding Boxes        • TrueDepth IR Sensor    • 3D Morphable Models
  • Viola-Jones / CNNs       • Metric Depth / Anti-   • Real-Time Facial
  • Auto-exposure/focus        spoofing Verification   Landmark Tracking
  ```
  
  ---
### A. Real-Time Face Detection on Smartphones (Slide 26)
  * **The Classical Foundation (Viola-Jones, 2001):**
  Historically, real-time face detection on low-power devices was enabled by the **Viola-Jones algorithm**:
  1. **Haar-like Features:** Rectangular differential operators calculating differences in pixel sums (e.g., eye regions are darker than cheekbones).
  2. **The Integral Image:** A spatial data structure enabling sum evaluations across arbitrary rectangular regions in $\mathcal{O}(1)$ constant time:
  
  $$I_{\text{integral}}(x, y) = \sum_{x' \le x, \, y' \le y} I(x', y')$$
  
  3. **Adaboost & Attentional Cascades:** Cascaded classifiers that discard 95% of non-face background windows in the first few stages, spending compute only on face-like candidates.
  * **Modern Mobile Systems:** Modern smartphones use lightweight single-shot deep neural networks (e.g., MobileNet backbones with SSD/RetinaFace heads) accelerated by dedicated Neural Processing Units (NPUs) to compute bounding boxes, automatic focal adjustments, and auto-exposure metering.
  
  ---
### B. Device Authentication: 2D RGB vs. 3D Structured Light (Slide 27)
  Slide 27 compares fingerprint scanning with facial unlock systems (Apple's Face ID on iPhone X and Sensible Vision).
  
  ```
   Passive 2D RGB Verification                  Active 3D Structured Light (TrueDepth)
   ───────────────────────────                  ──────────────────────────────────────
   • Uses standard camera sensor.               • Infrared (IR) VCSEL Projector emits
   • Highly vulnerable to spoofing                30,000+ structured dot patterns.
     (photographs, digital screens).            • IR camera reads pattern deformations.
   • Fails in zero-ambient light.               • Triangulates dense 3D facial depth map.
                                                • Invariant to ambient lighting & spoofing.
  ```
#### Why 2D Face Recognition Fails as a Security Primitive:
  A standard 2D camera records only ambient RGB reflections. An adversary can bypass a naive 2D face authentication model using a high-resolution printed photograph or a video played on a tablet (presentation attack).
#### The 3D Structured Light Solution:
  Apple's Face ID projects an array of over 30,000 invisible infrared dots via a Vertical-Cavity Surface-Emitting Laser (VCSEL).
  1. An infrared camera captures the reflected dot matrix.
  2. The system computes **geometric parallax displacements** of the dots relative to a calibrated reference plane:
  
  $$Z = \frac{f \cdot b}{d}$$
  
  3. This derives an immediate **sub-millimeter accurate 3D topological mesh** of the human face.
  4. Because the sensor operates strictly in the infrared spectrum ($940\text{ nm}$), it works in total darkness and is immune to 2D photograph spoofing.
  
  ---
### C. Real-Time AR & Snapchat Filters (Slide 28)
  Slide 28 displays Snapchat filters mapping interactive 3D graphical assets (e.g., rainbow vomit, eye enlargements) onto a human face in real time.
#### Mathematical Pipeline:
  1. **Facial Landmark Localization:** Regression networks predict canonical fiducial points (typically 68 to 468 landmarks across eyes, nose bridge, jawline, and lips).
  2. **3D Morphable Face Models (3DMM):** A low-dimensional parametric model decomposes facial shape into identity ($\boldsymbol{\alpha}$) and expression ($\boldsymbol{\beta}$) coefficients:
  
  $$\mathbf{S} = \mathbf{\bar{S}} + \sum_{i} \alpha_i \mathbf{S}_i^{\text{id}} + \sum_{j} \beta_j \mathbf{S}_j^{\text{exp}}$$
  
  3. **Perspective-n-Point (PnP) Pose Estimation:** Determines the 6-DOF transformation (Rotation $\mathbf{R}$, Translation $\mathbf{t}$) between the user's facial coordinate frame and the camera plane.
  4. **Augmented Graphics Overlay:** Graphical meshes are deformed and rendered onto the estimated facial rig, tracking expressions frame-by-frame at $60\text{ FPS}$.
  
  ---
## 2. Sports Analytics & Broadcast Augmentation (Slide 29)
  
  Slide 29 illustrates augmented reality overlays in live sports broadcasting:
  * The **Sportvision 1st & Ten System** (the virtual yellow first-down line in American football).
  * Olympic swimming broadcast overlays (virtual lane graphics and world-record split lines).
  
  ```
   Stadium Pan-Tilt-Zoom (PTZ) Camera ──► Homography Matrix H ──► Reproject Graphics onto Field
                    │                                                        │
                    ▼                                                        ▼
         [ Camera Calibration ]                                 [ Color Keying / Masking ]
         Calibrated to static field                              Only draws line ON GRASS;
         yard markers & hash lines                              masks OUT occluding players!
  ```
### The Technical Challenge:
  The yellow first-down line must look as if it is painted directly on the grass. It must:
  1. Remain locked to the physical turf during camera pans, tilts, and zooms.
  2. **Render underneath players** so athletes appear to walk over the graphic without distortion.
### The Mathematical Mechanism:
  * **Planar Homography:** Because the playing field is approximately planar, points on the ground plane $\mathbf{X}_{\text{field}} = (X, Y, 1)^T$ are mapped to camera pixel coordinates $\mathbf{x}_{\text{image}} = (u, v, 1)^T$ via a $3 \times 3$ projective matrix $\mathbf{H}$:
  
  $$\begin{pmatrix} u \\ v \\ 1 \end{pmatrix} \sim \mathbf{H} \begin{pmatrix} X \\ Y \\ 1 \end{pmatrix} = \begin{bmatrix} h_{11} & h_{12} & h_{13} \\ h_{21} & h_{22} & h_{23} \\ h_{31} & h_{32} & h_{33} \end{bmatrix} \begin{pmatrix} X \\ Y \\ 1 \end{pmatrix}$$
  
  * **Sensor-Instrumented Cameras:** Specialized pan-tilt-zoom (PTZ) encoders update $\mathbf{H}$ dynamically in real time as the camera moves.
  * **Chroma Keying & Luma Matting:** A fast per-pixel classifier evaluates the color profile of the turf. Pixels matching the green turf spectrum receive the yellow color transformation; pixels belonging to player uniforms, cleats, or footballs are masked out, preserving correct occlusion ordering.
  
  ---
## 3. Intelligent Vehicles & Autonomous Systems (Slides 30–34)
  
  ---
### A. Advanced Driver Assistance Systems (ADAS): Mobileye (Slide 30)
  Slide 30 shows the architecture of early commercial smart vehicles powered by **Mobileye’s EyeQ** system-on-chip:
  * **Monocular Vision Efficiency:** Relies on dedicated, low-cost forward-facing CMOS cameras rather than expensive sensor suites.
  * **Target Capabilities:** Time-to-Collision (TTC) estimation via optical expansion rates, Lane Departure Warning (LDW) via line fitting, and Forward Collision Warning (FCW) via monocular pedestrian and vehicle classification.
  
  ---
### B. Around View Monitors & Bird's-Eye View Synthesis (Slide 31)
  Slide 31 shows Nissan's Around View Monitor, which generates an artificial, top-down orthographic view of a car and its surroundings.
  
  ```
   [Front Camera]   [Left Mirror]   [Right Mirror]   [Rear Camera]
    (Fisheye Lens)   (Fisheye Lens)  (Fisheye Lens)   (Fisheye Lens)
           │               │               │               │
           └───────────────┼───────────────┴───────────────┘
                           ▼
              [ Radial Distortion Removal ]
              Undistorts wide-angle curvature
                           ▼
                 [ Homography Warping ]
              Projects each camera onto common
              virtual ground plane: x_ground = H_i · x_cam_i
                           ▼
             [ Photometric Seam Blending ]
              Smooths exposure transitions
                           ▼
          [ Synthesized Top-Down Orthographic View ]
  ```
  
  1. **Distortion Correction:** Fisheye lenses introduce significant barrel distortion. Pixels are remapped using polynomial radial lens models:
  
  $$r_u = r_d (1 + k_1 r_d^2 + k_2 r_d^4 + \dots)$$
  
  2. **Homography Warping:** Each undistorted camera image is projected onto a common ground plane reference frame using pre-calibrated homographies $\mathbf{H}_i$.
  3. **Multi-Band Alpha Blending:** Where the four camera fields of view overlap (at the vehicle's corners), smooth blending functions prevent visible seams caused by varying illumination conditions.
  
  ---
### C. Fully Autonomous Vehicles: Waymo (Slides 32–34)
  Slides 32–34 outline Alphabet’s **Waymo** autonomous driving system.
  
  ```
                  ┌─────────────────────────────────────┐
                  │    Waymo Multimodal Sensor Suite    │
                  └──────────────────┬──────────────────┘
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
   360° LiDAR Systems         Vision System Array             Radar Arrays
  • Millimeter metric depth  • Long-range telephoto     • Direct Doppler velocity
  • Direct 3D point cloud      for traffic signals        measurement
  • Dense spatial bounds     • Perimeter vision for     • Robust in fog, rain,
                              pedestrians & gestures     and heavy snow
  ```
#### Sensor Fusion Pipeline (Slide 34):
  Slide 34 illustrates Waymo’s internal scene representation across four driving edge-cases:
  1. **Jaywalker with median:** Tracks pedestrian trajectory vectors across occlusions.
  2. **Jaywalker without median:** Dynamic trajectory forecasting and yield planning.
  3. **Construction worker in a manhole:** Detects non-standard human poses (torso emerging from road surface) that do not match standard pedestrian upright models.
  4. **Parallel parking maneuver:** Resolves fine-grained spatial clearances between vehicles and curbs.
  
  * **Representation:** Point clouds from LiDAR provide precise 3D bounding proposals:
  
  $$\mathbf{b} = [x, y, z, l, w, h, \theta_{\text{yaw}}]$$
  
  Camera vision provides semantic confirmation (e.g., discerning whether an amber light is flashing or solid, reading construction signs, or identifying turn signals).
  
  ---
## 4. 3D Spatial Reconstruction & Robotics (Slides 35–37)
  
  ---
### A. Structure from Motion (SfM): "Building Rome in a Day" (Slide 35)
  Slide 35 references the landmark computer vision paper *Building Rome in a Day* (Agarwal et al., 2009).
  
  ```
   Thousands of Unordered Internet Photos (Flickr)
                         │
                         ▼
        [ Feature Extraction: SIFT Keypoints ]
                         │
                         ▼
     [ Pairwise Matching via Approximate Nearest Neighbors ]
                         │
                         ▼
          [ Robust Epipolar Geometry (RANSAC) ]
       Estimates Essential Matrix E between camera pairs
                         │
                         ▼
           [ Global Bundle Adjustment (BA) ]
     Jointly optimizes camera poses and 3D point clouds:
     min_{P_i, X_j}  Σ || x_{ij} - Proj(P_i, X_j) ||^2
                         │
                         ▼
           Dense 3D Point Cloud of Dubrovnik / Rome
  ```
  
  The system takes millions of random, uncalibrated tourist photos captured with different lenses, light conditions, and angles, and automatically reconstructs the 3D geometry of entire historic cities without any prior metric data.
  
  ---
### B. Spatial Computing: AR & VR Inside-Out Tracking (Slide 36)
  Slide 36 shows Facebook's acquisition of Oculus VR for $2 billion (2014), highlighting the shift toward **Inside-Out Tracking**.
  * **Outside-In Tracking (Early VR):** Required external stationary base stations (e.g., HTC Vive lighthouses) tracking markers on the headset.
  * **Inside-Out Tracking (Modern VR/AR):** Uses monochrome fisheye cameras embedded directly on the headset. Running real-time **Visual-Inertial Odometry (VIO)**, the headset computes its own 6-DOF pose relative to natural environmental features at millisecond latencies, rendering stable virtual scenes.
  
  ---
### C. Autonomous Field Robotics (Slide 37)
  Slide 37 highlights three applications of computer vision in dynamic environments:
  1. **NASA's Mars Curiosity Rover:** Navigates extraterrestrial terrain using stereo-vision cameras to build local elevation maps and estimate wheel slippage via visual odometry.
  2. **Amazon Picking Challenge:** Industrial robot arms segment, plan grasps, and manipulate irregular goods from warehouse bins using RGB-D perception.
  3. **Amazon Prime Air:** Autonomous aerial delivery drones relying on real-time stereo disparity and optical flow for hazard avoidance (power lines, pets, branches) during delivery descents.
  
  ---
## 5. Failures, Surveillance, and Ethical Frontiers (Slides 24–25, 38–40)
  
  Computer vision systems are susceptible to specific failure modes when operational environments deviate from training distributions.
  
  ```
                    ┌─────────────────────────────────────┐
                    │      Computer Vision Challenges     │
                    └──────────────────┬──────────────────┘
                                       │
            ┌──────────────────────────┴──────────────────────────┐
            ▼                                                     ▼
     Societal & Privacy Risks                               Model Failure Modes
     • Automated photo profiling (Slide 25)                • Missing 3D context (Slide 38)
     • Facial attribute surveillance (Slide 39)            • Semantic false positives (Slide 40)
  ```
  
  ---
### A. Accessibility vs. Mass Surveillance (Slides 24–25)
  * **Accessibility Benefit:** Meta/Facebook uses automated object recognition and dense image captioning to generate synthetic alt-text for visually impaired users.
  * **Privacy Implication:** The same pipeline indexes private personal photographs across millions of users, cataloging locations, interpersonal relationships, lifestyle choices, and consumer brands without explicit user consent.
  
  ---
### B. Facial Attribute Inference (Face++, Slide 39)
  Slide 39 highlights Megvii’s **Face++** platform predicting age, gender, race, emotion/mood, and facial landmarks.
  * **Technical Fragility:** Facial expression does not equal emotional intent; predicting internal psychological states (*"mood: angry 69%"*) from surface geometry is scientifically fraught and susceptible to demographic bias.
  * **Fairness & Representation:** Classifiers trained on unbalanced datasets exhibit higher false-negative rates on underrepresented demographics, posing severe risks in commercial hiring tools or automated policing.
  
  ---
### C. The Dong Mingzhu Case Study: Context Failure (Slide 40)
  Slide 40 highlights a high-profile computer vision failure in Ningbo, China:
  
  ```
                Real Scene: A bus drives past a traffic intersection.
                     Printed on the exterior side of the bus:
                   An advertisement showing corporate executive
                               Dong Mingzhu's face.
                                       │
                                       ▼
                     [ City Surveillance AI Traffic Camera ]
                 1. Detects face: Dong Mingzhu (High confidence).
                 2. Detects location: Pedestrian Crosswalk.
                 3. Camera logic checks crosswalk state: RED LIGHT.
                                       │
                                       ▼
              System Triggers: "JAYWALKING VIOLATION RECORDED"
         Dong Mingzhu's photo and national ID displayed on public billboard!
  ```
#### Why the System Failed:
  1. **Lack of 3D Metric Depth:** The camera operated on a 2D projection. It could not differentiate between a living human walking on the asphalt and a flat 2D portrait printed on the side of a moving vehicle.
  2. **Missing Temporal Tracking & Physical Priors:** A real pedestrian does not move across a crosswalk at 30 miles per hour attached to the flank of a municipal transit bus. 
  3. **The Core Lesson:** High classification accuracy on isolated components (face recognition, crosswalk detection) does not equal true scene understanding. Without common sense, physics constraints, and 3D geometric reasoning, automated visual inference remains brittle.
  
  ---
## 6. Structure of CS 5404: The Course Roadmap (Slides 51–59)
  
  The semester is structured to build the computer vision stack systematically from raw pixels to autonomous systems:
  
  ```
  Weeks 1–5: Unit 1                Weeks 6–10: Unit 2               Weeks 10–14: Unit 3
   [ 2D IMAGES ]                    [ 3D STRUCTURE ]               [ MODERN APPLICATIONS ]
  ──────────────────────           ────────────────────           ─────────────────────────
  • Point Operations               • Multi-View Geometry          • Deep Neural Nets (CNNs)
  • Spatial Filtering              • Epipolar Geometry            • Image Classification
  • Resampling & Warping           • Structure from Motion        • Semantic Segmentation
  • Feature Detectors (SIFT)       • Photometric Stereo           • Visual SLAM (VINS-Mono)
  • Image Transformations          • 3D Simulation (Blender)      • Place Recognition
  ```
  
  ---
### Unit 1: Images — Mathematical Tools for 2D Signals (Weeks 1–5, Slides 53–54)
  * **Point Processing:** Brightness, contrast adjustment, histogram equalization, gamma correction.
  * **Filtering:** Linear shift-invariant systems, spatial convolution, Gaussian smoothing, derivative filters (Sobel, Laplacian).
  * **Upsampling & Interpolation:** Nearest-neighbor, bilinear, and bicubic interpolation; anti-aliasing pyramids.
  * **Feature Detection & Matching:** Harris corners, SIFT (Scale-Invariant Feature Transform), orientation histograms.
  * **Geometric Transformations:** Affine transformations, projective homographies, RANSAC for robust model estimation.
  
  ---
### Unit 2: Structure — Geometry, Light, and 3D Space (Weeks 6–10, Slides 55–57)
  * **Camera Models:** The pinhole perspective camera, intrinsic matrix $\mathbf{K}$, extrinsic pose $[\mathbf{R} \mid \mathbf{t}]$.
  * **Stereo Vision & Epipolar Geometry:** Epipolar lines, the Essential matrix $\mathbf{E}$, the Fundamental matrix $\mathbf{F}$, disparity estimation.
  * **Photometric Stereo:** Reconstructing surface normals and albedo by varying lighting directions while holding the camera static.
  * **Structure from Motion (SfM):** Triangulation, resectioning, and non-linear Bundle Adjustment.
  * **Synthetic Generation (Blender):** Using 3D rendering engines to generate physically accurate synthetic imagery with perfect ground-truth depth and surface normal maps.
  
  ---
### Unit 3: Modern Applications — Learning-Based Vision (Weeks 10–14, Slides 58–59)
  * **Deep Neural Networks:** Backpropagation, convolutional layers, pooling, receptive fields, residual connections (ResNet).
  * **Recognition Pipelines:** Object classification (AlexNet to modern backbones), Bag of Visual Words (BoVW), and modern visual embeddings.
  * **Visual SLAM (Simultaneous Localization and Mapping):** Combining visual odometry with loop closure detection (e.g., Fab-Map, DBoW) and inertial sensor fusion (VINS-Mono).
  
  ---
## Complete Introductory Module Summary
  
  1. **Computer Vision is an underconstrained inverse problem:** Projecting 3D light and geometry onto a 2D sensor discards depth, making single-view inversion ill-posed.
  2. **Invariance vs. Discriminability:** Vision systems must remain invariant to camera viewpoints, illumination swings, motion blur, and intra-class variation, while resolving fine semantic distinctions.
  3. **Priors Resolve Ambiguity:** Context, vanishing lines, ground-plane constraints, and natural scene statistics allow algorithms to resolve ambiguous visual inputs.
  4. **The Modern Frontier is Multimodal & Embodied:** Vision has evolved from isolated pixel processing to foundation models (SAM, VLMs) and embodied physical agents (SayCan, autonomous vehicles) that combine visual perception with world knowledge and physical actions.