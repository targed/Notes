### **Stat 3113: Lesson 4 - Distributions of Quantitative Variables**
  
  While graphs provide a visual summary of data, **numerical measures** give us precise, objective values to describe a distribution's key features. It's crucial to distinguish between measures for a population and a sample.
  
  *   **Parameter:** A numerical value describing a characteristic of a **population** (e.g., the true average height of all students at a university). Parameters are typically unknown and are often represented by Greek letters (e.g., \(\mu, \sigma\)).
  *   **Statistic:** A numerical value calculated from **sample** data (e.g., the average height of 100 sampled students). We use statistics to estimate unknown population parameters. Statistics are usually represented by Roman letters (e.g., \(\bar{x}, s\)).
  
  ---
### **1. Measures of Center (Central Tendency)**
  
  Measures of center describe the "typical" or central value of a dataset.
#### **A. The Mean (Average)**
  The mean is the sum of all measurements divided by the number of measurements.
  
  *   **Sample Mean (\(\bar{x}\)):**
    \(\bar{x} = \frac{\sum x_i}{n} = \frac{x_1 + x_2 + ... + x_n}{n}\)
    where \(n\) is the sample size.
  *   **Population Mean (\(\mu\)):**
    \(\mu = \frac{\sum x_i}{N}\)
    where \(N\) is the population size.
#### **B. The Median (M)**
  The median is the middle value in a dataset when the data is arranged in order from smallest to largest.
  
  *   It is the **50th percentile**, meaning 50% of the data falls below it and 50% falls above it.
  *   If the dataset has an odd number of values, the median is the single middle value.
  *   If the dataset has an even number of values, the median is the average of the two middle values.
#### **C. Mean vs. Median: The Effect of Skewness and Outliers**
  
  The shape of the distribution affects the relationship between the mean and the median.
  
  *   **Symmetric Distribution:** The mean and median are approximately equal (`Mean ≈ Median`).
  *   **Skewed Right Distribution:** The mean is pulled higher than the median by the high-value tail (`Mean > Median`).
  *   **Skewed Left Distribution:** The mean is pulled lower than the median by the low-value tail (`Mean < Median`).
  
  **Key Point:** The mean is sensitive to extreme values (outliers), while the **median is resistant (or robust)**. Therefore, for skewed data or data with significant outliers, the **median is often the preferred measure of center**.
  
  ---
### **2. Measures of Variability (Spread)**
  
  Measures of variability describe how spread out or dispersed the data is from the center.
#### **A. The Variance**
  The variance measures the average squared deviation of the data points from their mean.
  
  *   **Population Variance (\(\sigma^2\)):**
    \(\sigma^2 = \frac{\sum (x_i - \mu)^2}{N}\)
  *   **Sample Variance (\\(s^2\\)):**
    \(s^2 = \frac{\sum (x_i - \bar{x})^2}{n-1}\)
  
  **Note on \(n-1\):** We divide the sample variance by \(n-1\) instead of \(n\) because it provides a better, unbiased estimate of the true population variance \(\sigma^2\).
#### **B. The Standard Deviation**
  The standard deviation is the positive square root of the variance. It is the most common measure of spread.
  
  *   **Advantage:** Its units are the same as the original data, making it more interpretable than the variance.
  *   **Population Standard Deviation (\(\sigma\)):** \(\sigma = \sqrt{\sigma^2}\)
  *   **Sample Standard Deviation (\(s\\)):** \\(s = \sqrt{s^2}\)
  
  A larger value for \(s\) indicates greater variability in the data. Like the mean, the standard deviation is **not resistant to outliers**.
#### **C. Quartiles and the Interquartile Range (IQR)**
  *   **Quartiles** are percentiles that divide the ordered data into four equal parts.
    *   **First Quartile (Q1):** The 25th percentile (25% of data is below it).
    *   **Second Quartile (Q2):** The Median or 50th percentile.
    *   **Third Quartile (Q3):** The 75th percentile (75% of data is below it).
  *   **Interquartile Range (IQR):** The distance between the first and third quartiles.
    \(\text{IQR} = Q_3 - Q_1\)
    The IQR represents the spread of the **middle 50%** of the data and is **resistant to outliers**, making it a good measure of spread for skewed data.
  
  ---
### **3. The Five-Number Summary and Boxplots**
  
  A **boxplot** is a graphical representation of the **five-number summary**, which consists of:
  1.  **Minimum**
  2.  **First Quartile (Q1)**
  3.  **Median (M or Q2)**
  4.  **Third Quartile (Q3)**
  5.  **Maximum**
  
  Boxplots provide a concise visual summary of center (Median), spread (IQR and overall range), and symmetry.
#### **Identifying Outliers with the 1.5 × IQR Rule**
  A common rule of thumb is to identify an observation as a **suspected outlier** if it falls:
  *   Below: \(Q_1 - 1.5 \times \text{IQR}\)
  *   Above: \(Q_3 + 1.5 \times \text{IQR}\)
  
  In a **modified boxplot**, the "whiskers" extend only to the smallest and largest non-outlier values, and the outliers are plotted as individual points.
  
  ---
### **4. Five-Step Strategy for Describing a Distribution**
  
  When analyzing a quantitative variable, always address these five aspects:
  
  1.  **Shape:** Describe the modality (unimodal, bimodal, etc.) and symmetry (symmetric, skewed left, skewed right).
  2.  **Outliers:** List any suspected outliers identified using the 1.5 × IQR rule or visual inspection. If none, state that.
  3.  **Center:**
    *   If the data is symmetric with no outliers, use the **mean**.
    *   If the data is skewed or has outliers, use the **median**. (It is often good practice to report both but state which is more appropriate).
  4.  **Spread:**
    *   If using the mean, report the **standard deviation**.
    *   If using the median, report the **IQR** (or Q1 and Q3).
  5.  **Groups:** Note if the data appears to fall into distinct groups or clusters.
  
  **Side-by-side boxplots** are an excellent tool for comparing the distributions (center, spread, and shape) of a quantitative variable across multiple groups.