### **Mock Quiz: Part I - Data Science Foundations (Easy/Conceptual)**

**Question 1 (Multiple Choice)**
Which of the following best describes the structural relationship between Artificial Intelligence (AI), Machine Learning (ML), and Deep Learning (DL)?
A) AI, ML, and DL are three completely separate fields that do not overlap.
B) Deep Learning is the broadest category, containing Machine Learning, which contains AI.
C) AI is the broadest category, containing Machine Learning, which contains Deep Learning.
D) Machine Learning and Deep Learning are the same thing, but AI is different.
- **Question 2 (Multiple Choice)**
  According to the Data Science Venn Diagram discussed in class, what happens when a person possesses "Hacking Skills" and "Substantive/Domain Expertise" but lacks "Math & Statistics" knowledge?
  A) They are a perfect Data Scientist.
  B) They are doing Traditional Research.
  C) They are doing Machine Learning.
  D) They are in the "Danger Zone."
- **Question 3 (Short Answer)**
  Briefly explain the fundamental difference between **Traditional Computer Science (Software 1.0)** and **Machine Learning (Software 2.0)** in terms of their Inputs and Outputs.
- **Question 4 (Scenario / Multiple Choice)**
  You are hired by a bank. They hand you a dataset of 100,000 credit card transactions. Each transaction has a tag attached to it saying either `"Fraud"` or `"Safe"`. They want you to build an algorithm to predict if future transactions are fraudulent. What type of Machine Learning task is this?
  A) Unsupervised Learning (Clustering)
  B) Supervised Learning (Classification)
  C) Supervised Learning (Regression)
  D) Reinforcement Learning
- **Question 5 (True/False)**
  Unsupervised learning algorithms require a training set of labeled data (an "answer key") to learn how to make predictions.
- **Question 6 (Short Answer)**
  What is the primary purpose of an IDE (Integrated Development Environment), and how does it relate to the Software Development Life Cycle (SDLC)?
  
  ***
  ***
- ### **Answers and Detailed Explanations**
  
  **Question 1 Answer: C**
  *   **Explanation:** Think of the concentric circles from Lecture 7, Slide 2. **AI** is the largest circle (any machine mimicking human intelligence). **Machine Learning** is inside AI (machines learning from data). **Deep Learning** is inside ML (learning from data specifically using Artificial Neural Networks).
- **Question 2 Answer: D**
  *   **Explanation:** This tests Drew Conway's famous Venn diagram. If you can code (Hacking) and you know the field (Domain Expertise), you know enough to run a Python script and get a result. However, without **Math & Statistics**, you don't know if that result is statistically valid, meaning you might confidently present a completely false conclusion to a CEO. Hence, the "Danger Zone." *(Note: Math + Hacking = Machine Learning. Math + Expertise = Traditional Research).*
- **Question 3 Answer:**
  *   **Explanation:** 
    *   In **Traditional CS**, the inputs are **Data and Rules (Code)**, and the computer outputs the **Answers**. 
    *   In **Machine Learning**, the inputs are **Data and Answers (Labels)**, and the computer outputs the **Rules (The Model)**.
- **Question 4 Answer: B**
  *   **Explanation:** Let's break this down. First, the data has tags (`"Fraud"` or `"Safe"`). Because you have the answer key, it must be **Supervised Learning** (eliminating A and D). Second, the output is a discrete category (Fraud vs Safe), not a continuous number (like $150.25). Predicting categories is called **Classification**. (Predicting numbers is Regression).
- **Question 5 Answer: False**
  *   **Explanation:** This is the exact opposite of the truth. **Supervised** learning requires labeled data (an answer key). **Unsupervised** learning works on *unlabeled* data to find hidden structures or groups on its own (like clustering).
- **Question 6 Answer:**
  *   **Explanation:** The primary purpose of an IDE is to enable programmers to create software **more efficiently**. It combines multiple stages of the SDLC (specifically Development, Testing, and Deployment) into a single "one-stop-shop" window. Instead of using a separate text editor to write code, a separate terminal to run it, and a separate tool to debug it, an IDE does it all, minimizing context-switching.
  
  ***
- ---
- ---
- ### **Mock Quiz: Part II - Python & Tools (Easy-Medium)**
  
  **Question 1 (Multiple Choice)**
  You are starting a new Data Science project. You need to load a messy `.csv` file containing 10,000 rows of housing data, remove any rows with missing values, and filter the columns. Which Python library is specifically designed for this type of structured, tabular data manipulation?
  A) NumPy
  B) Matplotlib
  C) Pandas
  D) Scikit-learn
- **Question 2 (Multiple Choice)**
  PyTorch Tensors are fundamentally very similar to NumPy arrays. However, PyTorch is the industry standard for Deep Learning because its Tensors possess two major superpowers that NumPy arrays do not. What are they?
  A) They can handle strings, and they automatically generate HTML dashboards.
  B) They can be processed on GPUs (CUDA), and they track gradients automatically (Autograd).
  C) They take up zero memory on your hard drive, and they never crash.
  D) They are the only data type that can be plotted using Matplotlib.
- **Question 3 (Code Interpretation)**
  Read the following Python code snippet carefully.
  ```python
  import numpy as np
  
  data = np.array([2, 4, 6, 8])
  result = np.mean(data)
  
  print(result)
  ```
  **(a)** What will be the exact output printed to the terminal?
  **(b)** If the data array was changed to `np.array([10, 10, 10, 10])`, what would the variance of this new array be? *(Hint: Think conceptually, no complex math needed).*
- **Question 4 (True/False)**
  When starting a new Machine Learning project, it is considered "Best Practice" to install all your libraries (like PyTorch and Pandas) directly into your computer's global Python system rather than using a Virtual Environment (like Conda or `.venv`), because it saves hard drive space.
- **Question 5 (Short Answer)**
  During the IDE setup lecture, the professor showed a trick where you type `# %%` into a standard Python script (`.py` file) in VS Code. What does this specific syntax do?
- **Question 6 (Matching)**
  Match the Python library to its primary use case in this course:
  1. **Scikit-learn**
  2. **Matplotlib**
  3. **NumPy**
  
  A) Creating static, interactive, and animated visualizations (like scatter plots and histograms).
  B) Performing traditional machine learning tasks (like Logistic Regression) and splitting data into train/test sets.
  C) Performing fast, numeric, and scientific computations on matrices and arrays.
  
  ***
  ***
- ### **Answers and Detailed Explanations**
  
  **Question 1 Answer: C**
  *   **Explanation:** **Pandas** is the "Excel of Python." It is built explicitly for structured data analysis, loading CSVs, and cleaning data using DataFrames. *NumPy* is for raw numerical arrays, *Matplotlib* is for drawing graphs, and *Scikit-learn* is for training the actual machine learning models.
- **Question 2 Answer: B**
  *   **Explanation:** This is a guaranteed test concept. Deep learning requires massive parallel computation and calculus. PyTorch Tensors solve this because they are **Device Aware** (they can travel from the CPU to the GPU to utilize thousands of CUDA cores) and they support **Autograd** (they silently record every math operation you do to them so they can automatically calculate the derivative/gradient during Backpropagation).
- **Question 3 Answer:**
  *   **(a) Output: `5.0`**
    *   *Explanation:* `np.mean()` calculates the average. $(2 + 4 + 6 + 8) = 20$. $20 \div 4 = 5.0$.
  *   **(b) Variance: `0`**
    *   *Explanation:* Variance measures how "spread out" the data is from the mean. If every number in the array is exactly $10$, there is zero spread. Therefore, the variance is 0.
- **Question 4 Answer: False**
  *   **Explanation:** Installing libraries globally leads to **Dependency Hell**. If Project A requires an old version of a library and Project B requires a new version, installing them globally will cause one of the projects to break. You should *always* use a Virtual Environment (`conda create` or `python -m venv`) to create an isolated "sandbox" for every new project.
- **Question 5 Answer:**
  *   **Explanation:** Typing `# %%` creates an **Interactive Python Window** (or "Cell") within a standard script. It allows you to run chunks of code and display their outputs (like graphs or dataframes) side-by-side, giving you the visual benefits of a Jupyter Notebook (`.ipynb`) while keeping your actual file a clean, version-controllable standard Python script.
- **Question 6 Answer:**
  *   **1 -> B** (Scikit-learn is the standard library for traditional ML and data splitting).
  *   **2 -> A** (Matplotlib is the visualization/graphing library).
  *   **3 -> C** (NumPy is the foundational library for fast numeric array computation).
  
  ***
- ---
- ---
  ***
- ### **Mock Quiz: Part III - Descriptive Statistics (Medium)**
  
  **Question 1 (Calculation)**
  Given the following dataset representing the number of hours students spent studying: `[1, 3, 3, 5, 8]`
  **(a)** Compute the Mean.
  **(b)** Compute the Median.
  **(c)** Compute the Population Variance (using the $\frac{1}{n}$ formula from class).
- **Question 2 (Multiple Choice)**
  If a dataset has a very large Variance, what does this mathematically tell you about the data?
  A) The average value of the dataset is very high.
  B) The data points are tightly clustered around the median.
  C) The data points are widely spread out from the mean.
  D) The data contains no outliers.
- **Question 3 (Calculation - PMF)**
  A software company tracks the number of server crashes per week. The probabilities are proportional to the number of crashes, defined by the Probability Mass Function (PMF): 
  $P(X = x) = cx$, for $x = 2, 3, 5$.
  **(a)** Find the value of the constant $c$.
  **(b)** Compute the Expected Value (Mean) of X.
  **(c)** Compute the Variance of X.
- **Question 4 (True/False)**
  A Probability Density Function (PDF) is used to describe continuous random variables, and the total area under its curve must exactly equal 1.
- **Question 5 (Scenario / Short Answer)**
  You are analyzing house prices in Rolla. 99 houses cost exactly \$100,000. One house (a massive mansion) costs \$10,000,000. 
  **(a)** Will the distribution be Positively Skewed (Right Skewed) or Negatively Skewed (Left Skewed)?
  **(b)** Which metric is a better representation of the "typical" house price: the Mean or the Median?
- **Question 6 (Calculation - Z-Score)**
  In a machine learning image dataset, the average pixel brightness (Mean) is 120, and the Standard Deviation is 20. A specific pixel has a brightness value of 170. 
  Compute the **Z-score** for this pixel.
- **Question 7 (Multiple Choice)**
  What is the primary mathematical difference between Covariance and Pearson’s Correlation?
  A) Covariance measures spread, while Correlation measures central tendency.
  B) Covariance can only be positive, while Correlation can be negative.
  C) Correlation is normalized by standard deviations, restricting its value to between -1 and 1, whereas Covariance is not scaled.
  D) There is no difference; they are two words for the exact same formula.
- **Question 8 (Scenario)**
  An AI researcher wants to estimate the average typing speed of all 8,000 undergraduate students at Missouri S&T. They randomly select 150 students and record their speeds. 
  **(a)** What is the population?
  **(b)** What is the sample size ($n$)?
- **Question 9 (Short Answer)**
  You are looking at a Cumulative Distribution Function (CDF) graph for the ages of patients in a medical dataset. You look at the X-axis for Age = 40, follow it up to the curve, and the Y-axis reads `0.75`. What does this specific point tell you about the patients?
- **Question 10 (Multiple Choice)**
  When training a Neural Network, why is converting your raw input data into Z-scores (Standardization) a popular and highly recommended preprocessing step?
  A) It permanently removes outliers from the dataset.
  B) It scales data down to small numbers (centered around 0), which helps gradient descent converge faster and prevents large weights from dominating.
  C) It converts continuous variables into discrete variables.
  D) It changes the fundamental shape of the distribution to a perfect bell curve.
  
  ***
  ***
- ### **Answers and Detailed Explanations**
  
  **Question 1 Answer:**
  *   **(a) Mean:** $20 \div 5 = \mathbf{4.0}$ 
    *   *(Math: $1+3+3+5+8 = 20$)*
  *   **(b) Median:** $\mathbf{3}$ 
    *   *(The sorted list is 1, 3, **3**, 5, 8. The middle number is 3).*
  *   **(c) Variance:** $\mathbf{5.6}$
    *   *Step 1 (Subtract mean from each):* $(1-4)=-3, (3-4)=-1, (3-4)=-1, (5-4)=1, (8-4)=4$.
    *   *Step 2 (Square them):* $9, 1, 1, 1, 16$.
    *   *Step 3 (Average the squares):* $(9 + 1 + 1 + 1 + 16) / 5 = 28 / 5 = \mathbf{5.6}$.
- **Question 2 Answer: C**
  *   **Explanation:** Variance specifically measures "Dispersion." It has nothing to do with how high the average is. If every person in a room is exactly 6 feet tall, the mean is high (72 inches), but the variance is **0** because there is no spread.
- **Question 3 Answer (The PMF Algebra):**
  *   **(a) $c = 0.1$**
    *   *Rule:* All probabilities in a PMF must sum to 1.
    *   $c(2) + c(3) + c(5) = 1 \rightarrow 10c = 1 \rightarrow c = 0.1$.
    *   *Probabilities:* $P(2)=0.2$, $P(3)=0.3$, $P(5)=0.5$.
  *   **(b) Mean (Expected Value, $E[X]$): $3.8$**
    *   *Formula:* Multiply each outcome by its probability and sum them.
    *   $(2 \times 0.2) + (3 \times 0.3) + (5 \times 0.5) = 0.4 + 0.9 + 2.5 = \mathbf{3.8}$.
  *   **(c) Variance: $1.56$**
    *   *Formula:* $E[X^2] - (E[X])^2$.
    *   *Find $E[X^2]$:* $(2^2 \times 0.2) + (3^2 \times 0.3) + (5^2 \times 0.5) = (4 \times 0.2) + (9 \times 0.3) + (25 \times 0.5) = 0.8 + 2.7 + 12.5 = \mathbf{16.0}$.
    *   *Subtract Mean Squared:* $16.0 - (3.8)^2 = 16.0 - 14.44 = \mathbf{1.56}$.
- **Question 4 Answer: True**
  *   **Explanation:** A PMF is used for discrete variables (where probabilities *sum* to 1). A PDF is used for continuous variables, and because you can't sum infinite numbers, you take the integral. The integral (total area under the curve) must equal 1.
- **Question 5 Answer:**
  *   **(a) Positively Skewed (Right Skewed)**. The "tail" of the distribution gets pulled far to the right by the \$10,000,000 outlier.
  *   **(b) The Median**. The mean would be artificially dragged up to nearly \$200,000, making it look like the "average" house is twice as expensive as 99% of the actual houses. The median stays robust at \$100,000.
- **Question 6 Answer: Z = 2.5**
  *   **Explanation:** The formula is $Z = \frac{x - \mu}{\sigma}$.
    *   $Z = \frac{170 - 120}{20} = \frac{50}{20} = \mathbf{2.5}$.
    *   *(This means the pixel is 2.5 standard deviations brighter than the average pixel).*
- **Question 7 Answer: C**
  *   **Explanation:** Covariance tells you direction (positive/negative relationship), but its actual number is meaningless because it depends on your units (e.g., measuring in inches vs miles changes the covariance). Pearson's Correlation fixes this by dividing by the standard deviations, shrinking the metric to a clean $-1$ to $1$ scale.
- **Question 8 Answer:**
  *   **(a) Population:** All 8,000 undergraduate students at Missouri S&T.
  *   **(b) Sample Size ($n$):** 150.
- **Question 9 Answer:**
  *   **Explanation:** The CDF measures the probability that a variable is *less than or equal to* a value. Therefore, this point means that **75% of the patients in the dataset are 40 years old or younger.**
- **Question 10 Answer: B**
  *   **Explanation:** Neural networks use Gradient Descent. If you feed the network massive, unscaled numbers, the gradients become unstable (they explode or vanish). Z-scores center your data around 0 with a standard deviation of 1, creating a smooth, "bowl-shaped" loss landscape that the optimizer can easily slide down. 
  
  ***
- ---
- ---
  ***
- ### **Mock Quiz: Part IV - Derivatives in Deep Learning (Medium)**
  
  **Question 1 (Basic Derivative Calculation)**
  Given the function $f(x) = 3x^2 + 4x - 5$, compute the derivative $\frac{df}{dx}$.
- **Question 2 (Gradient Chain Rule Calculation)**
  In Machine Learning, we often use the Squared Error loss function. Assume $a$ and $b$ are constants. 
  If $f(x) = (bx - a)^2$, compute the derivative $\frac{df}{dx}$ using the Chain Rule.
- **Question 3 (Short Answer)**
  In one or two sentences, explain *why* calculating derivatives is absolutely essential for training Artificial Neural Networks. What exactly do these derivatives tell the model to do?
- **Question 4 (Multiple Choice)**
  During the backpropagation phase of training, PyTorch calculates the gradient (derivative) of the Loss function with respect to a specific weight ($w_1$). The calculated slope is **positive**. 
  According to the Gradient Descent update rule, what will happen to $w_1$ during the next step?
  A) The weight $w_1$ will increase, because a positive slope means we are moving in the right direction.
  B) The weight $w_1$ will decrease, because we must move in the opposite direction of a positive slope to reach the bottom of the loss valley.
  C) The weight $w_1$ will remain exactly the same.
  D) The learning rate will be forced to become a negative number.
- **Question 5 (Formula Identification)**
  Which of the following is the correct mathematical formula for the **Gradient Descent Update Rule**? 
  *(Note: $w$ is the weight, $\eta$ is the learning rate, and $L$ is the loss function).*
  A) $w_{new} = w_{old} + \eta \frac{\partial L}{\partial w}$
  B) $w_{new} = w_{old} \times \eta \frac{\partial L}{\partial w}$
  C) $w_{new} = w_{old} - \eta \frac{\partial L}{\partial w}$
  D) $w_{new} = \eta - w_{old} \frac{\partial L}{\partial w}$
- **Question 6 (True/False)**
  In a neural network flow diagram (Computational Graph), you must complete the Forward Pass (calculating the hidden layers and the final output) *before* you can compute the loss and perform Backpropagation.
  
  ***
  ***
- ### **Answers and Detailed Explanations**
  
  **Question 1 Answer: $6x + 4$**
  *   **Explanation:** You use the **Power Rule** from basic calculus. 
    *   For $3x^2$, multiply the coefficient (3) by the exponent (2) and decrease the exponent by 1 $\rightarrow 6x^1 = 6x$.
    *   For $4x$, the derivative of $x$ is 1 $\rightarrow 4(1) = 4$.
    *   For the constant $-5$, the derivative of any constant is $0$.
    *   Result: $6x + 4$.
- Question 2 Answer: $2b(bx - a)$
  *   **Explanation:** You must use the **Chain Rule** because you have a function nested inside an exponent. The Chain Rule is "Derivative of the Outside $\times$ Derivative of the Inside".
    *   *Step 1 (Outside):* Treat $(bx - a)$ as a single chunk. The derivative of $(\text{chunk})^2$ is $2(\text{chunk})^1$. So we have $2(bx - a)$.
    *   *Step 2 (Inside):* Now take the derivative of the inside: $(bx - a)$ with respect to $x$. Since $a$ is a constant, it becomes 0. The derivative of $bx$ is just $b$.
    *   *Step 3 (Multiply):* $2(bx - a) \times b = \mathbf{2b(bx - a)}$. *(Note: This exact formula is on your Quiz 1 Review Slide 8!)*
- **Question 3 Answer:**
  *   **Explanation:** Derivatives (gradients) are essential because they measure the **slope of the loss function**. They tell the network the *direction* and *magnitude* to adjust the model's weights in order to minimize the error (loss) and improve predictions. (Without derivatives, the model would just be guessing blindly).
- **Question 4 Answer: B**
  *   **Explanation:** Think of the loss function as a U-shaped valley. A **positive slope** means you are on the right side of the valley, sloping upwards. If you increase the weight, you climb higher up the mountain (increasing your error). Therefore, the algorithm must subtract the gradient to move *left*, back down toward the bottom of the valley (minimizing the loss).
- **Question 5 Answer: C**
  *   **Explanation:** $w_{new} = w_{old} - \eta \frac{\partial L}{\partial w}$. 
    *   The **minus sign** is the most important part of this equation. It is the "Descent" in Gradient Descent. It forces the update to step in the *opposite* direction of the gradient to reach the minimum loss. $\eta$ (Eta) is the learning rate, which controls how big of a step you take.
- **Question 6 Answer: True**
  *   **Explanation:** This is a core rule of computational graphs (as seen in Lecture 5/7). You cannot calculate the error (Loss) until you have a prediction ($\hat{y}$). You cannot get a prediction without running the data forward through the network. Therefore, Forward Pass strictly must happen before Backpropagation.
  
  ***
- ---
- ---
  ***
- ### **Mock Quiz: Part V - Supervised Learning (Medium)**
  
  **Question 1 (Short Answer)**
  In the context of training an Artificial Neural Network, what is a **loss function**? Give one standard example of a loss function discussed in class.
- **Question 2 (Scenario / Conceptual)**
  You finish training a Deep Learning model to classify spam emails. When you evaluate it on your **Training Data**, it achieves **99% accuracy**. However, when you evaluate it on your **Testing Data**, it only achieves **55% accuracy**. 
  **(a)** Is this a good model? 
  **(b)** What specific machine learning phenomenon is happening here? 
  **(c)** Why did this happen?
- **Question 3 (Multiple Choice)**
  Which of the following is the defining characteristic of **Supervised Learning**?
  A) The algorithm groups data points based on hidden similarities without any human guidance.
  B) The algorithm learns from a training dataset that contains "labeled" examples (the correct answers are provided).
  C) The algorithm is supervised by a human who manually writes `if/else` statements for every scenario.
  D) The algorithm uses Reinforcement Learning to maximize a reward score.
- **Question 4 (The Gradient Descent Calculation)**
  *This simulates the exact calculation from Lecture 10, Slides 12-14, but with new numbers so you can test your actual understanding of the formula!*
  
  You are updating a simple neural network model with two inputs: $\hat{y} = w_1x_1 + w_2x_2$.
  You are using the Squared Error loss function: $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$.
  
  *   **Initial Weights:** $w_1 = 1.0, w_2 = 1.0$
  *   **Learning Rate ($\eta$):** $0.1$
  *   **Training Sample:** $x_1 = 3, x_2 = 2$
  *   **Target Answer ($y$):** $10$
  
  Please calculate the following for this single iteration:
  **(a)** Compute the prediction ($\hat{y}$).
  **(b)** Compute the Loss ($\mathcal{L}$).
  **(c)** Compute the gradients for both weights ($\frac{\partial \mathcal{L}}{\partial w_1}$ and $\frac{\partial \mathcal{L}}{\partial w_2}$).
  **(d)** Compute the **new updated weights** ($w_1^{new}$ and $w_2^{new}$).
  
  ***
  ***
- ### **Answers and Detailed Explanations**
  
  **Question 1 Answer:**
  *   **Explanation:** A loss function is a mathematical formula that measures the "error" or difference between the model's predicted output ($\hat{y}$) and the actual, true target ($y$). It tells the model how wrong it is. 
  *   **Example:** Mean Squared Error (MSE) / Squared Error.
- **Question 2 Answer:**
  *   **(a)** No, this is a very bad model.
  *   **(b)** **Overfitting**.
  *   **(c)** The model failed to *generalize*. Instead of learning the underlying patterns of spam emails, it simply memorized the specific emails in the Training dataset. When faced with completely new emails in the Testing dataset, it had no idea what to do, performing barely better than a random coin flip (55%).
- **Question 3 Answer: B**
  *   **Explanation:** Supervised learning requires **Labels** (the "Answer Key"). Unsupervised learning (like Clustering) works with unlabeled data to find hidden groupings. 
  
  **Question 4 Answer (The Big Calculation):**
  
  **Step (a): Compute the Prediction**
  *   *Formula:* $\hat{y} = (w_1 \times x_1) + (w_2 \times x_2)$
  *   *Math:* $\hat{y} = (1.0 \times 3) + (1.0 \times 2) = 3 + 2 = \mathbf{5}$
  
  **Step (b): Compute the Loss**
  *   *Formula:* $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$
  *   *Math:* $\mathcal{L} = \frac{1}{2}(5 - 10)^2 = \frac{1}{2}(-5)^2 = \frac{1}{2}(25) = \mathbf{12.5}$
  *   *(Note: The model guessed 5, but the answer was 10. The loss is 12.5).*
  
  **Step (c): Compute the Gradients**
  *   *The Shortcut Formula (from the Chain Rule):* The gradient for any weight $w_i$ is just the **Error** multiplied by the **Input** $x_i$.  Gradient = $(\hat{y} - y) \times x_i$.
  *   *The Error:* $(5 - 10) = \mathbf{-5}$
  *   *Gradient for $w_1$:* $-5 \times x_1 = -5 \times 3 = \mathbf{-15}$
  *   *Gradient for $w_2$:* $-5 \times x_2 = -5 \times 2 = \mathbf{-10}$
  *   *(Interpretation: The slopes are highly negative, meaning we need to heavily increase our weights to reach the target of 10).*
  
  **Step (d): Compute the New Weights**
  *   *Formula:* $w^{new} = w^{old} - (\eta \times \text{Gradient})$
  *   *Update $w_1$:* $w_1^{new} = 1.0 - (0.1 \times -15) \rightarrow 1.0 - (-1.5) \rightarrow 1.0 + 1.5 = \mathbf{2.5}$
  *   *Update $w_2$:* $w_2^{new} = 1.0 - (0.1 \times -10) \rightarrow 1.0 - (-1.0) \rightarrow 1.0 + 1.0 = \mathbf{2.0}$
  *   **Final Answer:** The new weights are **$(w_1 = 2.5, w_2 = 2.0)$**.
  
  *(Self-Check: If we run the forward pass again with these new weights: $(2.5 \times 3) + (2.0 \times 2) = 7.5 + 4.0 = 11.5$. The prediction jumped from 5 to 11.5, heavily overshooting the target of 10. This shows us that our Learning Rate of 0.1 was actually a bit too high for this specific data point!)*
  
  ***
- ---
- ---
-
- #### **Practice Problem 1: The Negative Trap**
  This problem tests if you can keep track of your negative signs during the update step.
  *   **Model:** $\hat{y} = w_1x_1 + w_2x_2$
  *   **Loss Function:** $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$
  *   **Learning Rate ($\eta$):** $0.1$
  *   **Initial Weights:** $w_1 = 0.5$, $\; w_2 = -1.0$
  *   **Training Sample:** $x_1 = 2$, $\; x_2 = 4$
  *   **Target Answer ($y$):** $7$
  
  **Task:** Calculate the new, updated weights ($w_1^{new}$ and $w_2^{new}$) after **one** iteration.
  
  ---
- #### **Practice Problem 2: The 3-Neuron Setup**
  This mimics the exact format from the Quiz 1 Review slides.
  *   **Model:** $\hat{y} = w_1x_1 + w_2x_2 + w_3x_3$
  *   **Loss Function:** $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$
  *   **Learning Rate ($\eta$):** $0.2$
  *   **Initial Weights:** $w_1 = 1$, $\; w_2 = 1$, $\; w_3 = 1$
  *   **Training Sample:** $x_1 = 1$, $\; x_2 = 0$, $\; x_3 = 2$
  *   **Target Answer ($y$):** $9$
  
  **Task:** Calculate the new, updated weights after **one** iteration.
  
  ---
- #### **Practice Problem 3: The Two-Step (Multi-Iteration)**
  In class, he showed what happens when you train on one sample, and then use those *new* weights to train on a second sample. Let's do a 2-iteration loop.
  *   **Model:** $\hat{y} = w_1x_1 + w_2x_2$
  *   **Loss Function:** $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$
  *   **Learning Rate ($\eta$):** $0.5$
  *   **Initial Weights:** $w_1 = 2$, $\; w_2 = 2$
  
  *   **Iteration 1 Sample:** $x_1 = 1$, $\; x_2 = 1$, $\;$ Target ($y$) = $6$
  *   **Iteration 2 Sample:** $x_1 = 2$, $\; x_2 = 0$, $\;$ Target ($y$) = $2$
  
  **Task:** Calculate the final weights after **both** iterations are complete.
  
  ***
- ### **The Worked-Out Solutions**
- #### **Solution 1: The Negative Trap**
  *   **Step 1 (Prediction):** $\hat{y} = (0.5 \times 2) + (-1.0 \times 4) \rightarrow 1 - 4 = \mathbf{-3}$
  *   **Step 2 (Loss):** Error is $(-3 - 7) = \mathbf{-10}$. Loss is $\frac{1}{2}(-10)^2 = \frac{1}{2}(100) = \mathbf{50}$
  *   **Step 3 (Gradients):** 
    *   $dw_1 = \text{Error} \times x_1 \rightarrow -10 \times 2 = \mathbf{-20}$
    *   $dw_2 = \text{Error} \times x_2 \rightarrow -10 \times 4 = \mathbf{-40}$
  *   **Step 4 (Update):** 
    *   $w_1 = 0.5 - (0.1 \times -20) \rightarrow 0.5 - (-2) \rightarrow 0.5 + 2 = \mathbf{2.5}$
    *   $w_2 = -1.0 - (0.1 \times -40) \rightarrow -1.0 - (-4) \rightarrow -1.0 + 4 = \mathbf{3.0}$
  *   **Final Answer:** $w_1 = 2.5, \; w_2 = 3.0$
  
  ---
- #### **Solution 2: The 3-Neuron Setup**
  *   **Step 1 (Prediction):** $\hat{y} = (1 \times 1) + (1 \times 0) + (1 \times 2) \rightarrow 1 + 0 + 2 = \mathbf{3}$
  *   **Step 2 (Loss):** Error is $(3 - 9) = \mathbf{-6}$. Loss is $\frac{1}{2}(-6)^2 = \frac{1}{2}(36) = \mathbf{18}$
  *   **Step 3 (Gradients):** 
    *   $dw_1 = -6 \times 1 = \mathbf{-6}$
    *   $dw_2 = -6 \times 0 = \mathbf{0}$
    *   $dw_3 = -6 \times 2 = \mathbf{-12}$
  *   **Step 4 (Update):** 
    *   $w_1 = 1 - (0.2 \times -6) \rightarrow 1 - (-1.2) = \mathbf{2.2}$
    *   $w_2 = 1 - (0.2 \times 0) = \mathbf{1.0}$ *(Since $x_2$ was 0, it didn't contribute, so its weight doesn't change!)*
    *   $w_3 = 1 - (0.2 \times -12) \rightarrow 1 - (-2.4) = \mathbf{3.4}$
  *   **Final Answer:** $w_1 = 2.2, \; w_2 = 1.0, \; w_3 = 3.4$
  
  ---
- #### **Solution 3: The Two-Step**
  **--- Iteration 1 ---**
  *   **Prediction:** $\hat{y} = (2 \times 1) + (2 \times 1) = \mathbf{4}$
  *   **Error:** $4 - 6 = \mathbf{-2}$
  *   **Gradients:**
    *   $dw_1 = -2 \times 1 = \mathbf{-2}$
    *   $dw_2 = -2 \times 1 = \mathbf{-2}$
  *   **Update:**
    *   $w_1 = 2 - (0.5 \times -2) \rightarrow 2 - (-1) = \mathbf{3}$
    *   $w_2 = 2 - (0.5 \times -2) \rightarrow 2 - (-1) = \mathbf{3}$
  *   *Weights are now $w_1=3, w_2=3$. We carry these into Iteration 2!*
  
  **--- Iteration 2 ---**
  *   **Prediction:** $\hat{y} = (3 \times 2) + (3 \times 0) = \mathbf{6}$
  *   **Error:** $6 - 2 = \mathbf{4}$ *(Notice the error is positive this time! We overshot).*
  *   **Gradients:**
    *   $dw_1 = 4 \times 2 = \mathbf{8}$
    *   $dw_2 = 4 \times 0 = \mathbf{0}$
  *   **Update:**
    *   $w_1 = 3 - (0.5 \times 8) \rightarrow 3 - 4 = \mathbf{-1}$
    *   $w_2 = 3 - (0.5 \times 0) \rightarrow 3 - 0 = \mathbf{3}$
  *   **Final Answer:** $w_1 = -1, \; w_2 = 3$