- Here is a slimmed-down, highly conceptual version of the crib sheet designed specifically to be written by hand. 
  
  I have removed the heavy calculus and long formulas (since you will put those in your TI-84 Python program) and focused strictly on **definitions, relationships, pros/cons, and "when to use what"**—which is exactly what you need for Multiple Choice, True/False, and Short Answer questions. 
  
  I have used lines and boxes to show you exactly how to partition your physical paper using a ruler.
  
  ***
### **--- PAGE 1: Foundations, Evaluation, & Trees ---**
  
  ```text
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 1. DATA TYPES & PREPROCESSING                                               │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Nominal: Categories, no order (Color, ZIP). Use One-Hot Encoding.         │
  │ • Ordinal: Meaningful rank, uneven spacing (Grades). Use Label Encoding.    │
  │ • Interval: Diff is meaningful (+/-), NO true zero (Temp C). No ratios.     │
  │ • Ratio: True zero exists (*, /). (Age, Salary, Kelvin).                    │
  │ • Outliers (IQR): IQR = Q3 - Q1. Bounds = Q1-(1.5*IQR) to Q3+(1.5*IQR).     │
  │ • Data Leakage: ALWAYS Split into Train/Test BEFORE scaling/imputing!       │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 2. DISTANCE & SIMILARITY                                                    │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Euclidean (L2): Straight line. VERY sensitive to scale and outliers.      │
  │ • Manhattan (L1): Grid/City-block. More robust to outliers than L2.         │
  │ • Cosine: Measures ANGLE, ignores magnitude. Best for Text/Documents.       │
  │ • Jaccard: Intersection over Union. Best for Sets/Binary (Market baskets).  │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 3. MODEL EVALUATION & VALIDATION                                            │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • K-Fold CV: Train on K-1 folds, test on 1. Average results.                │
  │ • Bias vs. Var: High Bias = Underfitting. High Var = Overfitting.           │
  │ • Metrics:                                                                  │
  │    - Accuracy: (TP+TN)/Total. TERRIBLE for imbalanced data.                 │
  │    - Precision: TP/(TP+FP). Use when False Alarms are costly (Spam).        │
  │    - Recall: TP/(TP+FN). Use when Misses are fatal (Cancer/Weapons).        │
  │    - F1-Score: Harmonic mean of Prec & Rec. Good for imbalanced data.       │
  │ • ROC Curve: Plots True Positive Rate vs. False Positive Rate.              │
  │    - AUC = 1.0 (Perfect), AUC = 0.5 (Random Guess). Robust to imbalance.    │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 4. PROBABILITY & NAIVE BAYES                                                │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Bayes Theorem: Posterior = (Likelihood * Prior) / Evidence.               │
  │ • Entropy: Measures uncertainty. Max entropy = 50/50 split. 0 = Pure.       │
  │ • Mutual Info: Amount of uncertainty removed by knowing another variable.   │
  │ • Naive Bayes Assumption: Assumes ALL features are conditionally            │
  │   independent given the class. (Solves parameter explosion).                │
  │ • Log Trick: Add logs of probabilities to avoid arithmetic underflow.       │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 5. DECISION TREES                                                           │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Info Gain: Reduction in entropy. FLAW: Biased to features with many       │
  │   unique values (like IDs). Fix = Gain Ratio (penalizes fragmentation).     │
  │ • Continuous Data: Sort values, test midpoints, pick highest Info Gain.     │
  │ • Pre-pruning: Early stop (Max depth, min samples). Risk: Horizon effect.   │
  │ • Post-pruning: Grow full tree, cut weak branches using Validation Set.     │
  │ • CART Algorithm: Uses Gini, strictly BINARY splits (prevents fragmenting). │
  │ • Pros/Cons: White-box/interpretable, but high variance (overfits easily).  │
  └─────────────────────────────────────────────────────────────────────────────┘
  ```
  
  \pagebreak
### **--- PAGE 2: Advanced Models, Ensembles & Unsupervised ---**
  
  ```text
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 6. LINEAR MODELS: PERCEPTRON VS. LOGISTIC REGRESSION                        │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Perceptron: Hard boundary (Sign func). Updates weights ONLY on mistakes.  │
  │    - Will loop FOREVER if data is not linearly separable (XOR problem).     │
  │    - Does NOT maximize margin; stops at the first valid line.               │
  │ • Logistic Reg: Soft boundary (Sigmoid func). Outputs Probabilities (0-1).  │
  │    - Uses Log-Loss (Cross-Entropy). Updates weights on EVERY sample.        │
  │ • Gradient Descent: Steps opposite the gradient. Step size = Learning Rate. │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 7. SUPPORT VECTOR MACHINES (SVM)                                            │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Goal: Maximize the Margin (distance to closest points).                   │
  │ • Support Vectors: The ONLY points that dictate the boundary line.          │
  │ • Dual Problem: Optimization depends ONLY on dot products of data points.   │
  │ • Soft-Margin (C Parameter): Allows slack (errors) for noisy/messy data.    │
  │    - HIGH C: Strict! Narrow margin, avoids errors -> Risk of Overfitting.   │
  │    - LOW C: Lazy! Wide margin, ignores errors -> Risk of Underfitting.      │
  │ • Kernel Trick: Maps data to high-D space to solve non-linear problems      │
  │   using simple math (no actual 3D coordinates calculated).                  │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 8. ENSEMBLE LEARNING: BAGGING VS. BOOSTING                                  │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Bagging (Random Forest):                                                  │
  │    - Goal: Reduce VARIANCE (fixes overfitting).                             │
  │    - Method: Parallel training, Bootstrapping (random sample w/ replace).   │
  │    - Base Learner: Deep, complex trees.                                     │
  │    - RF Trick: Uses random subset of features at each split to decorrelate. │
  │ • Boosting (AdaBoost):                                                      │
  │    - Goal: Reduce BIAS (fixes underfitting).                                │
  │    - Method: Sequential training. Increases weight of misclassified points. │
  │    - Base Learner: Shallow trees/Stumps. (Deep trees will overfit here!)    │
  │    - Alpha (Amount of say): High accuracy model = High voting power.        │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 9. CLUSTERING: K-MEANS VS. GMM                                              │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • K-Means (Hard Clustering):                                                │
  │    - Assumes: Spherical clusters, equal variance.                           │
  │    - Flaws: Sensitive to init (Local Optima -> run multiple times!) and     │
  │      sensitive to outliers. Fails on non-convex shapes (donuts).            │
  │    - Elbow Method: Plot K vs. Cost to find optimal K.                       │
  │ • GMM (Soft Clustering):                                                    │
  │    - Assigns probabilities of membership. Generative model.                 │
  │    - Uses EM Algorithm (E-step: Calc prob. M-step: Update Mean/Covariance). │
  │    - Can form Elliptical shapes (because it calculates covariance matrix).  │
  └─────────────────────────────────────────────────────────────────────────────┘
  
  ┌─────────────────────────────────────────────────────────────────────────────┐
  │ 10. DIMENSIONALITY REDUCTION (PCA vs. t-SNE)                                │
  ├─────────────────────────────────────────────────────────────────────────────┤
  │ • Curse of Dimensionality: Distance loses meaning; data becomes sparse.     │
  │ • PCA (Linear): Maximizes Global Variance. Flattens data (destroys Swiss    │
  │   Roll). Fast. Deterministic. Use for noise reduction/preprocessing.        │
  │ • t-SNE (Non-Linear): Preserves Local Neighborhoods.                        │
  │    - Uses probability (KL Divergence). Best for 2D/3D Visualization.        │
  │    - Crowding Problem: Uses t-distribution (fat tails) to push apart points.│
  │    - Perplexity: Balances local vs global attention. (Too low = shards).    │
  └─────────────────────────────────────────────────────────────────────────────┘
  ```
### Next Steps for your TI-84
  When you are ready to build your Python program for the calculator, let me know! We can write a script containing a menu system that holds:
  1. The exact formula for Information Gain and Entropy.
  2. The manual Perceptron update formulas.
  3. The AdaBoost Alpha ($\alpha$) and weight update formulas.
  4. The F1-score/Accuracy/Precision/Recall formulas.