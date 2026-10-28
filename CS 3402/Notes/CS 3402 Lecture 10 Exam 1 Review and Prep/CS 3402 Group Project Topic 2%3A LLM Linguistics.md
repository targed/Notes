### **1. The Core Research Question (Slides 15–17)**
The project title is: **Evaluating Cross-Linguistic Performance Discrepancies in Large Language Models.**

*   **The Background Problem:** Models like ChatGPT and Gemini are trained on "the whole internet." However, the internet is predominantly English. Because of this **Data Representation Imbalance**, LLMs are highly intelligent in English but can become inaccurate, culturally biased, or less logical when prompted in low-resource languages (e.g., Urdu, Swahili, or even Spanish compared to English).
*   **The Project Goal:** You are acting as an "AI Auditor." Your goal is to prove, using statistics, whether a model gives a worse or fundamentally different answer just because the prompt was written in a different language.

---
- ### **2. Requirement 1: Data Collection & Preprocessing (Slide 18)**
  For this project, "Data" means the **Prompts** you will feed the AI.
  *   **The Requirement:** You must test at least **two languages** (e.g., English vs. Chinese).
  *   **The Task Types:** You need a structured dataset of questions.
    *   *Factual QA:* "What is the capital of France?"
    *   *Reasoning:* Math word problems or logic puzzles.
    *   *Sentiment:* Asking the AI to write a story and checking if one language is inherently more negative/toxic than the other.
  *   **The Trap (Confounding Factors):** You must ensure **Semantic Equivalence**. If your English prompt asks about "American Football" and your translated Spanish prompt asks about "Soccer," the model will give different answers based on the *culture*, not the *language*. You must carefully translate the prompts so the logic is identical.
  
  ---
- ### **3. Requirement 2: Model Selection & Design (Slides 19–21)**
  This is the experimental phase. You must test and compare three different types of models:
- #### **A. Closed-Source Models (Slide 19)**
  *   *Examples:* ChatGPT (OpenAI), Claude (Anthropic), Gemini (Google).
  *   *Access:* You interact with these via API or web interface. You don't know exactly how they were trained.
- #### **B. Open-Source Pretrained Models (Slide 20)**
  *   *Examples:* LLaMA (Meta), Mistral.
  *   *Access:* You can download the actual weights of these models (often from Hugging Face).
  
  **The Testing Modes (For A & B):**
  *   **Zero-shot:** Asking the question directly. (e.g., "Translate this: Hello.")
  *   **Few-shot:** Giving the AI examples first. (e.g., "English: Apple -> Spanish: Manzana. English: Dog -> Spanish: Perro. English: Hello -> Spanish: ?")
- #### **C. Fine-Tuning a Small LLM (Slide 21)**
  *This is the most technically impressive part of the project.*
  *   *The Task:* Take a small, older open-source model (like GPT-2 or LLaMA-1B). It will likely perform terribly on non-English tasks at first.
  *   *The Action:* You will **Train** it using the Gradient Descent loops we learned in Lecture 7/9. You feed it your specific prompt dataset to force it to learn the task.
  *   *The Goal:* Compare your custom-trained "Small" model against the massive, general-purpose "Pretrained" models.
  
  ---
- ### **4. Requirement 3: Evaluation Metrics (Slide 22)**
  How do you mathematically prove one text response is better than another?
  *   **Accuracy:** Used for Factual QA (Did it get the math problem right?).
  *   **BLEU / ROUGE Scores:** *Fill-in info:* These are standard NLP metrics. They measure how many words in the AI's generated response perfectly overlap with a human-written "reference" answer.
  *   **Statistical Comparison:** You cannot just say "English scored 90 and Spanish scored 85, so English is better." You must use **Paired Statistical Tests** (like the Z-test from Lecture 5) to prove the 5-point difference is statistically significant and not just random variance.
  
  ---
- ### **5. Requirement 4: Deliverables (Slides 23–24)**
  *   **The Report:** Standard scientific format. Must include tables and figures (Matplotlib!).
  *   **The Presentation:** You must emphasize **what was learned**, not just which model won. If LLaMA failed in Spanish, *why* did it fail? Was it hallucinating, or did it refuse to answer?
  *   **Peer Evaluation:** Just like Topic 1, your grade depends heavily on how well you communicate your findings to the rest of the class.
  
  ---
- ### **Summary: Which Project Should You Choose?**
  
  Now that you have seen both options, you need to decide with your group:
  
  *   **Choose Topic 1 (Medical AI) IF:** You like working with **Images/Vision** or highly structured tabular data. You are interested in Computer Vision (CNNs) and high-stakes classification (Cancer vs. No Cancer).
  *   **Choose Topic 2 (LLM Linguistics) IF:** You are fascinated by **ChatGPT and Text Generation**. You are interested in AI ethics, prompt engineering, and the cultural impacts of algorithms.
  
  ---