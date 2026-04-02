#### **1. The Group Project (Slides 24–25)**
*   **Team Dynamics:** You need a team of **2–4 people**.
*   **The Spreadsheet:** Registration is via a shared Google Sheet. You must use your `@mst.edu` email to access it. 
*   **The "Topic" Rule:** Slide 24 says "Leave project topic blank." This is common in Data Science courses—the instructor usually presents a list of "Vetted Datasets" later to ensure you don't pick a project that is impossible to solve.
*   **Deadlines:** Team registration is due **02/15/2026**. If you don't pick a team, you will be randomly assigned. 
  *   *Pro-Tip:* Try to find teammates with diverse skills (e.g., one person who is great at the "Hacking" circle and one who is great at "Expertise/Writing").
- #### **2. Homework 1: The "Rolla Housing" Study (Slides 26–30)**
  This assignment tests your ability to spot discrepancies between "Official" and "Real-world" data.
  
  **The Problem Definition:** Compare the **Median Listing Price per Square Foot** in Rolla, MO.
  *   **Source A (Official):** [FRED (Federal Reserve Economic Data)](https://fred.stlouisfed.org/series/MEDLISPRIPERSQUFEE40620).
  *   **Source B (Scraped):** A site of your choice (Zillow, Redfin, or Realtor.com).
  
  **Why do these differ? (The "Fill-in" info):**
  *   **Methodology:** FRED uses aggregated data that might be 1–2 months old. Real-estate sites show houses currently on the market *today*.
  *   **Coverage:** FRED might include the entire MSA (Phelps County), while your search might only include the "City of Rolla."
- #### **3. Step-by-Step HW Execution (Slides 28–30)**
  1.  **Collect Data:** Find at least 20 active listings. Record the **Price** and **Square Footage**.
  2.  **Compute PPSF:** For *each* house, calculate:
    $$PPSF = \frac{\text{Listing Price}}{\text{Square Feet}}$$
  3.  **Find the Median:** Using Python (`numpy.median()` or `statistics.median()`), find the middle value of your PPSF list.
  4.  **Visualize:** Create a scatter plot in **Matplotlib**. 
    *   *Note:* The sample ID is just for your records; the plot should show Price vs. SqFt.
  5.  **Calculate the "Diff":** Compare your median to the FRED median.
    *   *Metrics to use (Slide 30):* 
        *   **Absolute Difference:** $|Official - Scraped|$
        *   **Relative Difference:** $\frac{|Official - Scraped|}{Official} \times 100$
- #### **4. The "AI Disclosure" Mandate (Slide 30)**
  The instructor is very strict about this:
  *   You **can** use AI (ChatGPT, Copilot) to help write the code.
  *   You **must** disclose it. 
    *   *Requirement:* State which tool you used and *exactly* what it did (e.g., "I used ChatGPT to generate the Matplotlib scatter plot logic").
  
  ---
- ### **Student Checklist for Homework 1**
  
  To ensure you get a 100%, check these boxes before submitting on **02/22/2026**:
  *   [ ] **Comment your Code:** Don't just submit code; explain what each line is doing.
  *   [ ] **Handle Outliers:** Look at your data. If you found a house listed for \$5,000,000 in Rolla, it might be a outlier. Decide if you should keep it or remove it based on Slide 22.
  *   [ ] **Visual Clarity:** Make sure your Matplotlib plot has a Title, X-axis label ("Square Feet"), and Y-axis label ("Price").
  *   [ ] **Metric Choice:** Don't just give the raw difference; explain the **Ratio** or **Relative Difference**. (e.g., "My scraped data was 10% lower than the FRED data").
  
  ---