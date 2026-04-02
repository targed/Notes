#### 1. The Core Research Question (Slides 31–33)
In 2026, the AI world is split into two camps. Your project is to determine which camp wins in the high-stakes field of medicine.

*   **Camp A: Foundation Models (Generalists):** These are models like GPT-4, Claude, or Vision Transformers (ViT) that have been trained on the "entire internet." They are great at **Generalization**.
*   **Camp B: Task-Specific Models (Specialists):** These are smaller models built from scratch specifically for one task (e.g., detecting pneumonia in X-rays). They use **Domain Knowledge** and **Inductive Biases** (mathematical assumptions tailored to the data).
*   **The Medical Conflict:** Medical data is usually **small** and **expensive** to label. Generalists might be smarter, but specialists are built specifically for the "noise" of clinical data.

---
- #### 2. Requirement 1: Data Mastery (Slide 34)
  You cannot just download a file and hit "run." You must act as a Data Engineer.
  *   **Selection:** You must use **at least two** medical datasets. (e.g., one image-based dataset of skin cancer and one tabular dataset of heart disease).
  *   **Inspection:** You must find the missing values or corrupted samples.
  *   **Preprocessing:** You must **normalize/standardize** the data (Z-scores, as discussed in Lecture 5).
  *   **Splitting:** You must clearly define your **Training, Validation, and Test sets**.
  
  ---
- #### 3. Requirement 2: The Experimental Design (Slides 35–37)
  This is where you perform the "Battle of the Models."
  
  **A. Large Language Models (LLMs) (Slide 35):**
  If your medical data is text or tabular, you use models like ChatGPT or Gemini. You must test two modes:
  *   **Zero-shot:** Asking the AI to solve the problem with no examples.
  *   **Few-shot:** Providing the AI with 3–5 examples of "Correct" diagnoses in the prompt.
  
  **B. Pretrained Vision Models (Slide 36):**
  If you have images, you use **Transfer Learning**. You take a model like **ResNet** (trained on cats/dogs) and see if it can be "fine-tuned" to see tumors.
  
  **C. Task-Specific Models (Slide 37):**
  You must build a model **from scratch** (using the `nn.Module` blueprint from Lecture 5).
  *   *Heuristic:* Use an **MLP** (Multi-Layer Perceptron) for table data or a **CNN** for images.
  
  ---
- #### 4. Requirement 3: Evaluation & "Beyond Accuracy" (Slide 38)
  In medicine, **Accuracy is often a lie.** 
  *   *The Example:* If 99% of patients are healthy, a model that predicts "Healthy" for everyone is 99% accurate but will let the 1% of sick people die.
  *   **Required Metrics:** You must report **F1-score** and **ROC-AUC** (which measures the tradeoff between False Positives and False Negatives).
  *   **Interpretability:** You must show *why* the model made a choice. For images, this often means using **Attention Maps** (highlighting the pixels the AI looked at).
  
  ---
- #### 5. Requirement 4: The Deliverables (Slides 39–40)
  *   **The Report:** A formal scientific paper (Introduction $\to$ Methods $\to$ Results).
  *   **The Presentation:** 20 minutes. 
  *   **The Peer Review:** Your grade isn't just from the professor. **Other student teams will rate your presentation** on quality, clarity, and organization. 
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  
  1.  **Transfer Learning:** This is the "secret sauce" of modern AI. It’s the idea that a model that learned to see the "edges" of a cat can reuse that knowledge to see the "edges" of a lung on an X-ray.
  2.  **Inductive Bias:** This is a fancy term for "Design Choices." By choosing a CNN for images, you are "biasing" the model to look for spatial patterns. By choosing a Transformer, you are "biasing" it to look for relationships between words.
  3.  **Reproducibility:** On Slide 34, "All choices should be clearly documented." If you don't record your `random_state` (from the Wine example), no one can verify your results.
  
  ---
- ### **Action Items for the Group Project:**
  *   **Group Formation:** DDL (Deadline) is **02/15/2026**. You need 2–4 people. 
  *   **Dataset Search:** Start looking on **Kaggle** or the **UCI Machine Learning Repository** for "Medical" datasets. Look for ones that are "Clean" so you don't spend the whole semester just fixing typos in CSV files.
  *   **Tool Setup:** Ensure everyone in your group has the **GitHub Student Developer Pack** (Slide 33 of the previous deck) so you can use Copilot to help write the training loops.