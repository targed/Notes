## 1. Time-Series Anomaly Detection (Slides 40–43)
  Standard anomaly detection looks at isolated points. But in time-series data (like stock prices, ECG heart monitors, or video frames), the current value depends heavily on the past.
### A. The Moving Average (Slide 40)
  The simplest way to detect a temporal anomaly is to see if the current data point deviates from its recent history.
  *   **The Prediction ($\hat{x}_t$):** Calculate the average of the last $k$ time steps (the "Window").
    $$ \hat{x}_t = \frac{1}{k} \sum_{i=t-k+1}^{t} x_i $$
  *   **The Residual/Error ($e_t$):** $e_t = x_t - \hat{x}_t$
  *   *Rule:* If the Residual is suddenly massive, an anomaly has occurred.
### B. Predictability Modeling (Slides 41–43)
  **Core Assumption:** Normal instances are temporally *predictable*. Anomalies are *unpredictable*.
  *   **Time-Series:** We train a Deep Learning model (like an LSTM) on normal sine waves. When a chaotic anomaly occurs, the LSTM's prediction for the next step will be completely wrong, resulting in a huge error spike (Slide 41).
  *   **Video Frames:** We train a CNN/LSTM to predict the next frame of a video. If the video is just people walking on a sidewalk, the model easily predicts the next frame. If a bicyclist suddenly speeds across the sidewalk (Slide 43), the model will fail to predict it, and the pixel-wise reconstruction error will spike, flagging the cyclist as an anomaly!
  
  ---
## 2. The Danger of Reconstruction Methods (Slide 44)
  While Autoencoders and PCA are brilliant, your professor highlights a critical weakness you must know:
  *   **Risk of Over-Generalization:** If you make your Autoencoder or Deep Neural Network *too powerful* (e.g., too many hidden layers or neurons), it becomes too good at its job. It will actually be able to reconstruct the anomalies perfectly! If it reconstructs the anomaly perfectly, the Error is 0, and the anomaly slips right past your detector. 
  *   *Fix:* You must tightly restrict the bottleneck (latent space) to force the model to *only* learn the most basic normal patterns.
  
  ---
## 3. The Evaluation Nightmare (Slides 45–50)
  How do you know if your unsupervised anomaly detector is actually working? 
  
  **The Challenges:**
  *   **Lack of Labels:** You usually don't have a list of true anomalies to check against.
  *   **Extreme Imbalance:** 99.99% of data is normal.
  *   **Ambiguity:** What counts as an anomaly is highly subjective and depends on business context.
  
  **Evaluation Strategies:**
  1.  **Fully / Semi-Supervised:** If you have some labels, NEVER use Accuracy. Use **Precision, Recall, F1-Score**, or **PR-AUC** (Precision-Recall Area Under Curve). *Note: PR-AUC is explicitly preferred over ROC-AUC for extreme class imbalance (Slide 50).*
  2.  **Unsupervised (No Labels):**
    *   *Visual Inspection:* Project the data into 2D using **t-SNE or PCA** and visually check if the points with high anomaly scores are sitting isolated from the main clusters.
    *   *Synthetic Evaluation (The Best Hack):* Take your clean data and **manually inject** fake anomalies (e.g., randomly multiply a few values by 100). Run your detector and see if it catches your injected points.
  
  ***
# Problem Solving & Concept Check
  
  **1. Moving Average Math:**
  You are tracking a server's CPU temperature. You use a window size of $k = 3$. 
  The temperatures at $t_1, t_2,$ and $t_3$ were $40^\circ, 42^\circ,$ and $44^\circ$. 
  At $t_4$, the temperature spikes to $60^\circ$. 
  *   What was the predicted value ($\hat{x}_4$) based on the moving average?
    *   *Answer:* $(40 + 42 + 44) / 3 = 126 / 3 = \mathbf{42^\circ}$.
  *   What is the Residual Error ($e_4$)?
    *   *Answer:* $x_4 - \hat{x}_4 = 60 - 42 = \mathbf{18^\circ}$. (This large residual triggers the alarm).
  
  **2. Video Anomaly Logic:**
  You train a video prediction model on thousands of hours of highway traffic footage. It learns to predict the movement of cars perfectly. One day, a small airplane makes an emergency landing on the highway. 
  *   Will the prediction error be high or low for the frames containing the airplane, and why?
  *   *Answer:* **Extremely High.** The model has never seen an airplane. Its internal weights only know how to move "car pixels" forward. When it tries to predict the next frame of the airplane, it will draw a blurry, car-like mess. The difference between its prediction and the true next frame will cause a massive error spike.
  
  **3. The "Over-Generalization" Trap:**
  You train an Autoencoder on normal banking transactions. You make the bottleneck layer 500 neurons wide (almost the same size as the input). During testing, a fraudster executes a highly abnormal transaction. The Autoencoder perfectly reconstructs the fraudulent transaction with zero error. 
  *   Why did this happen?
  *   *Answer:* **Over-generalization.** Because the bottleneck was too wide, the network didn't have to learn the strict "rules" of normal transactions; it just memorized how to pass inputs directly to outputs. You must shrink the bottleneck to force the network to drop abnormal data.
  
  **4. The Ultimate Metric:**
  You have an anomaly dataset with a 99.9% to 0.1% split. Which metric does Slide 50 explicitly state is preferred for evaluating models on this data?
  *   *Answer:* **PR-AUC (Precision-Recall Area Under Curve).** While ROC-AUC is good, it includes False Positives divided by True Negatives. When True Negatives are in the millions (99.9%), the FPR stays artificially close to 0, making the ROC curve look overly optimistic. PR-AUC focuses strictly on the minority class.
  
  ***