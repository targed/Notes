## 1. Clustering-Based Approaches (Slides 28–29)
  **Core Idea:** An object is an outlier if it does not strongly belong to any cluster.
  
  **How it works (depending on the algorithm):**
  *   **K-Means (Prototype):** Run K-Means to find centroids. An outlier is a point whose distance to its assigned centroid is exceptionally large.
  *   **DBSCAN (Density):** Density-based clustering groups tightly packed points. Points that fall in the low-density "empty space" between clusters are automatically labeled as noise/outliers.
  *   **Pros:** Fast, intuitive, and no labels required.
  *   **Cons:** "Garbage In, Garbage Out." A massive outlier can actually pull a K-Means centroid away from the true cluster, distorting the boundaries and hiding the anomaly.
  
  ---
## 2. Model-Based: One-Class SVM (Slide 31)
  Standard SVM (Lecture 9) draws a line to separate two classes (+1 and -1). But what if you only have one class (Normal Data)?
  
  **The One-Class SVM (SVDD Method):**
  *   **The Goal:** Find the smallest possible mathematical "Sphere" (or hypersphere) that completely encloses the normal training data.
  *   **The Rule:** If a new data point falls *inside* the sphere, it is Normal. If it falls *outside* the sphere, it is an Anomaly.
  *   *Note:* Just like standard SVM, this uses the **Kernel Trick** to warp the data into higher dimensions so that complex, non-circular clusters can be enclosed by a simple sphere in the new space.
  
  ---
## 3. Reconstruction-Based: Autoencoders & PCA (Slides 32–38)
  This is arguably the most powerful and popular method for modern, high-dimensional anomaly detection.
  
  **The Core Assumption:** 
  Normal data contains underlying patterns that can be mathematically compressed. Anomalies are chaotic and *cannot* be compressed using those same patterns.
  
  **The Pipeline (Autoencoder):**
  1.  **Encoder:** Takes a high-dimensional input (e.g., a $28 \times 28$ image) and compresses it into a tiny "Latent Space" bottleneck (e.g., a vector of size 10).
  2.  **Decoder:** Takes that size-10 vector and tries to reconstruct the original $28 \times 28$ image perfectly.
  3.  **Training:** We train this network on *only normal data*. It becomes an expert at compressing and rebuilding normal data.
  
  **The Anomaly Detection Trigger (Slide 33):**
  *   **Reconstruction Error:** $E = ||x - \hat{x}||$ (The difference between the original input and the decoded output).
  *   If we feed the network a **Normal** image, it knows how to rebuild it. The error is **Low**.
  *   If we feed the network an **Anomaly** (e.g., a picture of a dog when it was only trained on cats), the bottleneck forces the decoder to try and build a dog using cat features. The resulting image looks like a mess. The error is **Massive**.
  *   **Rule:** If Reconstruction Error $>$ Threshold, flag as an Anomaly.
  
  *(Note: PCA can be used exactly the same way. You project data onto the Principal Components, then un-project it back to the original space. The distance lost during the projection is the Reconstruction Error).*
  
  ***
# Problem Solving & Concept Check
  
  **1. One-Class SVM Logic:**
  You train a One-Class SVM on normal server log data. The algorithm draws a tight boundary around the data. You set the hyperparameter $\nu$ (nu) to control the strictness of the boundary. 
  *   If you make the boundary extremely tight (allowing a few normal training points to fall outside it), are you risking False Positives or False Negatives during testing?
  *   *Answer:* You are risking **False Positives**. Because the sphere is so tight, perfectly normal variations might fall just outside the boundary and trigger a false alarm. (This is similar to a High $C$ value in a standard Soft-Margin SVM).
  
  **2. Autoencoder Intuition:**
  You train an Autoencoder exclusively on pictures of **apples**. The network learns to compress and reconstruct apples with a 1% error rate.
  You then feed the network a picture of a **banana**. 
  *   What will the output of the decoder likely look like?
  *   *Answer:* It will likely look like a reddish, blurry, round blob (a failed attempt to reconstruct an apple).
  *   Will the Reconstruction Error be high or low?
  *   *Answer:* **Very High**. The raw pixels of a yellow, curved banana compared to the pixels of the reconstructed red blob will yield a massive mathematical difference. The system successfully flags the banana as an anomaly.
  
  **3. The PCA Reconstruction Trap:**
  You want to use PCA for anomaly detection. Your data has 100 features. You run PCA and keep all 100 Principal Components. You then reconstruct the data and measure the error.
  *   What will the reconstruction error be?
  *   *Answer:* Exactly **Zero**. If you keep all 100 dimensions, PCA simply rotates the data without losing any information. To use PCA for anomaly detection, you *must* drop the lower-variance components (e.g., keep only the top 10 PCs) to force a "bottleneck" that loses information.
  
  ***