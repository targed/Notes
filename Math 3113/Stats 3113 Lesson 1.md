### **Stat 3113: Lesson 1 - Foundational Concepts**
  
  This lesson lays the groundwork for the entire course by introducing the two main branches of statistics and defining the fundamental concepts of data collection and classification.
#### **1. The Two Branches of Statistics**
  
  Statistics can be broadly divided into two main areas:
  
  1.  **Descriptive Statistics:** This branch focuses on the methods for collecting, organizing, summarizing, and presenting data. The goal is to describe the main features of a dataset clearly and concisely. Before we can make inferences, we must first understand the data we have.
  2.  **Inferential Statistics (Statistical Inference):** This is the ultimate goal of most statistical analysis. It involves using data from a small group (a sample) to draw conclusions, make predictions, or make decisions about a much larger group (the population). This is the primary focus of the Stat 3113 course.
#### **2. Core Definitions**
  
  *   **Population:** The *entire* collection of all individuals, objects, or outcomes of interest in a study.
    *   The size of the population is denoted by the uppercase letter **N**.
  *   **Sample:** A *subset* of the population that is selected for actual observation and data collection.
    *   The size of the sample is denoted by the lowercase letter **n**.
  *   **Observation:** A single recorded value of a particular characteristic for a member of the sample or population.
  
  **Example from the Slides:**
  *   **Population:** All 7 million college students in the United States. (N = 7,000,000)
  *   **Sample:** The 150 selected college students whose GPA is computed. (n = 150)
  *   **Observation:** The specific GPA of a single student in the sample (e.g., 3.2, 2.8, 4.0).
#### **3. The Rationale for Sampling**
  
  It is often impractical or impossible to study an entire population. Therefore, we select a sample for several key reasons:
  
  1.  **Resource Savings:** Studying a full population is expensive and time-consuming. Sampling saves time, money, and labor.
  2.  **Infinite or Unidentifiable Populations:** Sometimes, the population is effectively infinite or its members cannot all be identified. For example, the population of all stars in the sky or all grains of sand on a beach.
  3.  **Destructive Testing:** In many engineering applications, the process of measuring a characteristic destroys the item being tested. For example, to find the breaking strength of a steel beam or the lifespan of a light bulb, the item must be tested until it fails. If you tested the entire population, you would have no products left to sell.
  
  The core idea is to move from a **Sample** to the **Population** through the process of **Inference**.
#### **4. Sampling Methods and Potential Pitfalls**
  
  The quality of statistical inference depends entirely on the quality of the sample.
  
  *   **Representative Sample:** A sample that accurately reflects the characteristics of the population from which it was drawn. This is the goal of good sampling.
  
  *   **Sampling Bias:** A systematic error in the sampling process that results in a sample that is not representative of the population. This occurs when some members of the population are more likely to be selected than others.
    *   **Consequence of Bias:** Incorrect and misleading conclusions about the population.
  
  *   **Simple Random Sample (SRS):** This is the ideal sampling method and the foundation for most statistical theory. An SRS is a sample chosen in such a way that **every possible sample of size *n* has an equal chance of being selected**. This is like drawing names from a hat or using a random number generator.
    *   **Example 1 (SRS):** The utility company that used a random number generator to select 200 customers from a list of all 10,000 customers. This is a valid SRS.
  
  *   **Sample of Convenience:** A sample that is not drawn by a well-defined random method, but is chosen for ease of collection. Convenience samples are highly prone to bias.
    *   **Example 2 (Not SRS):** The quality engineer who only tests the 20 most recently produced circuits each hour. This method is convenient but biased because circuits produced at other times have no chance of being selected. This is a sample of convenience.
  
  *   **Sampling Variation (or Sampling Error):** The natural, random, and expected differences that exist between different samples drawn from the same population. No two samples will be exactly alike. This is not an error in the sense of a mistake; it is an inherent property of sampling that inferential statistics is designed to handle.
#### **5. Variables and Data**
  
  *   **Variable:** A characteristic that can change or vary among different individuals or objects. Examples include temperature, material type, defect count, or time-to-failure.
  
  *   **Data:** The observed values of variables. We classify data based on the number of variables measured:
    *   **Univariate data:** One variable is measured (e.g., recording only the height of each person).
    *   **Bivariate data:** Two variables are measured (e.g., recording both the height and weight of each person).
    *   **Multivariate data:** More than two variables are measured.
#### **6. Types of Variables**
  
  Variables can be classified into two main types, which determines the kind of statistical analysis that can be performed.
  
  **A. Qualitative (or Categorical) Variables**
  These variables describe a quality or category and place individuals into groups.
  
  1.  **Nominal Variable:** The values are names or labels with no natural order or ranking.
    *   *Examples:* Material Type ("Steel," "Aluminum," "Plastic"), Make of Car, Engineering Discipline ("Mechanical," "Electrical"). Jersey numbers are also nominal because number 32 is not "better" or "greater" in value than number 19; it's just a unique identifier.
  
  2.  **Ordinal Variable:** The values have a natural rank or order, but the differences or distances between the ranks are not meaningful or are unequal.
    *   *Examples:* Educational Attainment ("High School," "Bachelors," "Masters"), Component Wear ("Low," "Medium," "High"), a satisfaction survey ("Unsatisfied," "Neutral," "Satisfied").
  
  **B. Quantitative (or Numerical) Variables**
  These variables represent a measurable, numerical quantity. Arithmetic operations (like addition or averaging) make sense.
  
  1.  **Discrete Variable:** The variable can only take on a finite or "countable" number of values. There are gaps between the possible values.
    *   *Examples:* The number of defects in a batch of products (you can have 2 or 3 defects, but not 2.5), the number of students in a class.
  
  2.  **Continuous Variable:** The variable can take on any value within a given range. There are infinitely many possible values between any two points.
    *   *Examples:* The time-to-failure of a component, the diameter of a steel ball, temperature, pressure, height, and weight.
  
  This classification can be summarized as follows:
  ![A chart showing the different types of variables. The main variable splits into Qualitative/Categorical and Quantitative/Numerical. Qualitative splits into Nominal and Ordinal. Quantitative splits into Discrete and Continuous.](https://miro.medium.com/v2/resize:fit:1400/0*LZSToHT9pmPcWv8W){:height 90, :width 780}