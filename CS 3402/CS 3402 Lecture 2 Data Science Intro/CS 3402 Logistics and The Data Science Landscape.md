#### **1. Computing Resources: The "Mill" Cluster (Slide 3)**
The slides mention "GPU Nodes" and "The Mill cluster." This is critical because deep learning requires massive parallel processing power that your laptop likely doesn't have.

*   **What is it?** The "Mill" is Missouri S&T's High-Performance Computing (HPC) cluster.
*   **How to access:** It uses an interface likely based on **Open OnDemand**. This allows you to launch a Jupyter Notebook in your web browser that is actually running on a supercomputer.
*   **The "Enable GPU" Checkbox:**
  *   *Context:* By default, Jupyter creates a "CPU-only" instance. This is fine for data cleaning (Pandas).
  *   *The Warning:* When you start doing Deep Learning (PyTorch), you **must** check "Enable GPU." If you don't, your code might take 10 hours instead of 10 minutes.
  *   *Under the hood:* This provisions an NVIDIA GPU (likely A100 or H100) for your session.
- #### **2. The Course Roadmap (Slide 5)**
  The curriculum follows a specific hierarchy of complexity:
  1.  **Tools:** Python, Numpy, Pandas (The foundation).
  2.  **Supervised Learning:** Teaching a computer with "Answer Keys" (e.g., this image is a cat).
  3.  **Unsupervised Learning:** Finding patterns without answer keys (e.g., grouping similar customers).
  4.  **NLP (Natural Language Processing):** Text analysis (ChatGPT logic).
  5.  **Deep Learning:** Neural Networks (The state-of-the-art).
- #### **3. The Software Ecosystem (Slides 6–7)**
  Slide 6 shows Python dominating the ratings (22.61%).
  *   **Why Python?** It is not the fastest language (C++ is faster). However, it is the best "Glue Language." Libraries like PyTorch are written in C++ for speed but have a Python "wrapper" for ease of use.
  *   **The "Logo Soup" (Slide 7) - Decoded:**
    *   **The Frameworks:** *PyTorch* (Academic/Research standard) vs. *TensorFlow* (Older, industry legacy).
    *   **The LLM Tools:** *LangChain* & *LlamaIndex* (Tools to connect LLMs to your own data), *OpenAI* (The model provider).
    *   **The Hub:** *Hugging Face* (The "GitHub" of Machine Learning—where you download pre-trained models).
- #### **4. What is Data Science? (Slides 8–10)**
  **The Pipeline (OSEMN Model):**
  1.  **Problem:** Define the business/scientific question.
  2.  **Collect & Understand:** Scraping, SQL, API calls.
  3.  **Clean & Format:** *Crucial Context:* The slides show this as one box, but this is 80% of the work. Real data is dirty (missing values, wrong formats).
  4.  **Model:** Applying the algorithm.
  5.  **Solution:** Deploying the result.
  
  **The Venn Diagram (Drew Conway):**
  *   **Hacking Skills + Math:** Machine Learning (You can build the model and understand it).
  *   **Math + Expertise:** Traditional Research (You understand the theory and the field, but can't code it at scale).
  *   **Hacking + Expertise:** **The Danger Zone**. This is where you know enough to run the code but don't understand the math, leading to false conclusions (e.g., overfitting or p-hacking).
  
  **Exploration Strategies (Slide 10):**
  *   **Passive Methods:** Analyzing historical data. The data is "dead"—it won't change based on your analysis. (Standard ML).
  *   **Active Methods:** "Taking decisions about subsequent actions." This refers to **Reinforcement Learning** (where an agent learns by trial and error) or **A/B Testing** (where you change the website to see if user behavior changes).
  
  ---