# Topics as listed on slides
- Data and Data Preprocessing
- Model Evaluation
- Classifiers
  • Bayes Classifier
  • Decision Tree
  • Perceptron
  • Logistic Regression
  • SVM
- Ensemble Method
  • Bagging
  • Boosting
- Clustering
  • K-Means
  • GMM
- Dimensionality Reduction
  • t-SNE
- Data
  • types of attributes
  • Common Data Types
  • Data Quality Issues
- Data Preprocessing
  • Common data preprocessing steps
  • Pipeline
- Common metrics for model evaluation
  • Precision, recall, accuracy, F1-score, confusion matrix
  • AUC, ROC
  • Given a scenario, which metric should we use?
- Model evaluation strategies
  • Cross-validation
  • LOOCV
  • Overfitting vs. Underfitting
- Bayes Classifier
  • Use it to solve a toy example.
- Decision Tree
  • Uses metrics like Gini Impurity or Entropy (Information Gain) to choose the best feature to split on.
  • How to avoid overfitting
- Perceptron
  • Loss function
  • Gradient Descent Algorithm
  • Use GD to solve a toy example.
  • Limitations
- Logistic Regression
  • From perceptron to logistic regression (Motivation?)
  • Compare Logistic regression to Perceptron.
  • Hand-calculating a Logistic regression (with GD) on a toy dataset.
- SVM
  • Motivation
  • Hard-SVM and Soft-SVM (concepts, loss function)
  • Kernel-tricks
  • Hand-calculating a Hard-SVM on a toy dataset.
- Ensemble Method:
  • Core ideas of Bagging and Boosting
  • Famous algorithms:
  • Random Forest,
  • Adaboost
  • Bias-Variance Perspective
  • Bagging vs. Boosting (difference, and given a scenario, which method
  is more appropriate?)
- K-means
  • Algorithms
  • Limitations (random initialization, sensitive to outliers, K values)
  • How to address the limitations
  GMM
  • Comparison between K-means and GMM
  • Learned parameters
  • Generative model
  • How to use it in real applications
- Curse of Dimensionality
- PCA and t-SNE
-
- # My list
- ## Lecture-4&5_Evaluation
- distance metrics
- Manhattan distance
- Euclidean distance
- Cosine similarity
- Jacard similarity
- Model Evaluation/validation
- Cross validation
- Overfitting
- Underfitting
- Performance metrics (Regression)
- Performance metrics (Classification)
- Confusion Matrix
- Accuracy
- Precision
- Recall
- F1-Score
- Confusion matrix with multiple classes
- ROC Curve
- Area under the ROC Curve
- ## Lecture-6_Review and Bayes Classifier
- Experiment
- Sample Space
- Event
- Probability addition and multiplication rules
- Conditional probability
- Independent Events
- Dependent Events
- Rule of total probability
- Bayes' Theorem:
- Random Variables
- PDF, PMF and CDF
- Joint Probability Distribution
- Probability Chain Rule
- Mean and Variance
- Covariance
- Information
- Entropy
- Joint Entropy
- Conditional Entropy
- Mutual Information
- Relative Entropy:
- Bayes Classifier
- Estimating the Prior Probability:
- Estimating the Likelihood:
- Naïve Bayes
- ## Lecture-7_DT
- Rule-Based Classifier
- decision tree
- Gini impurity
- Feature Selection
- Information Gain
- Information Gain Ratio
- Prevent Overfitting in Decision Trees
- Pruning (Pre and post)
- ## Lecture-8_Perceptron
- Perceptron
- Margin
- Linearly Separable
- Loss Function
- Gradient Descent
- Perceptron Algorithm with SGD
- Perceptron Convergence Theorem
- Perceptron Limitation
- ## Lecture-9&10_Logistic&SVM
- Logistic Regression
- Sigmoid Function
- Gradient of Logistic Loss
- SVM: Hyperplane
- SVM: Optimization
- Lagrange and Dual Problem
- Soft-Margin SVM
- Slack Variables
- kernel function
- ## Lecture-11_Boosting
- Ensemble Learning:
- Bagging:
- Random Forests
- Bias-Variance Tradeoff
- Boosting:
- AdaBoost
- ## Lecture-12_Clustering
- Supervised Learning
- Unsupervised Learning
- Clustering
- Hard Clustering vs. Soft Clustering:
- K-means Clustering
- K-means
- picking K
- Mixtures of Gaussians/Gaussian Mixture Model
- Gaussian Distribution/Mixture Models
- ## Lecture-14_t-SNE
- The Curse of Dimensionality
- Feature Selection
- Feature Extraction
- Dimensionality Reduction: PCA
- Nonlinear Dimensionality Reduction/t-SNE
- Data Visualization Gap
- Stochastic Neighbor Embedding (SNE)
- KL Divergence
- Hyperparameter: Perplexity