#### **1. The Foundation: Python & VS Code (Slides 25–26)**
*   **Python Installation:** 
  *   *Critical Warning:* On Windows, you **must** check the box "Add Python to PATH." If you miss this, the terminal won't recognize the `python` command, and you'll have to reinstall.
  *   *Fill-in info:* For this course, Python 3.10, 3.11, or 3.12 are ideal. Avoid "bleeding edge" versions (like 3.13) immediately upon release, as PyTorch often takes a few months to support them.
*   **The Extensions (Slide 26):** 
  *   VS Code doesn't know how to handle Python out of the box. You must install the **Python** and **Jupyter** extensions by Microsoft. This enables the "Play" buttons and the data visualization tools.
- #### **2. The "Sandbox": Virtual Environments (Slide 27)**
  This is the most important workflow habit in the deck.
  *   **What is a Virtual Environment (`.venv`)?** It is a private folder inside your project that contains a clean copy of Python.
  *   **Why use it?** If you install PyTorch globally and then another class requires an older version, your computer will have a "conflict" and neither will work. By using a `.venv`, your "Intro to DS" settings stay isolated.
  *   **The Command:** `python -m venv .venv` creates the room. `source .venv/bin/activate` (Mac) or `.venv\Scripts\activate` (Windows) walks you into that room.
  *   **VS Code Integration:** When you see the pop-up "We noticed a new environment...", click **Yes**. This tells VS Code to use your private Python instead of the computer's default one.
- #### **3. Installing PyTorch & Jupyter Integration (Slides 28–30)**
  *   **The Selector (Slide 28):** Because PyTorch is hardware-heavy, you can't just `pip install`. You use the generator at `pytorch.org`.
    *   *Pro-Tip:* If you don't have an NVIDIA GPU, select "CPU" to save gigabytes of download space.
  *   **The Missing Link (`ipykernel`):** If you want to use Jupyter Notebooks, you **must** run `pip install ipykernel`. Without this, your notebook won't be able to "talk" to your Python environment.
  *   **Selecting the Kernel (Slide 29):** In the top right of your `test.ipynb` file, you must click **Select Kernel** and pick your `.venv`. 
    *   *Note:* If you see "Global" or "Base" selected, your code will likely crash because PyTorch isn't installed there.
- #### **4. Professional Debugging Tools (Slides 31–32)**
  *   **The Variable Explorer (Slide 31):** This is the "Data Scientist's Secret Weapon."
    *   Instead of writing `print(my_data)` and scrolling through thousands of lines of text, you click the **Variables** button.
    *   It opens a spreadsheet view. You can **filter** to find specific values or **sort** to find outliers (e.g., "Why is this house price $0?").
  *   **The `# %%` Magic (Slide 32):** This is the "Interactive Window."
    *   Standard `.ipynb` files are hard to track in Git.
    *   By typing `# %%` in a standard `.py` script, you turn that block into a "Cell." You get the power of a Notebook (instant graphs) but the cleanliness of a professional Python script.
- #### **5. The "Student Developer Pack": Unlocking the Pro Tools (Slides 33–35)**
  The instructor is giving you a shortcut to over $1,000 worth of free software.
  *   **GitHub Student Pack (Slide 33):**
    *   **Unlocks:** GitHub Copilot (The AI Agent from Slide 9), Canva Pro, cloud credits, and more.
    *   **Requirement:** You must use your `@mst.edu` email and often upload a photo of your Student ID.
  *   **JetBrains & PyCharm (Slides 34–35):**
    *   While the course uses VS Code, **PyCharm Professional** is the other industry giant. It is much heavier than VS Code but has the best "Code Analysis" in the world.
    *   *Strategy:* Use the GitHub Pack to get the JetBrains "All Products Pack" license. This gives you **PyCharm Pro**, **DataGrip** (for SQL), and **CLion** (for C++) for free as long as you are a student.
  
  ---
- ### **Summary of the "Digital Workstation" Setup**
  1.  **Isolate:** Always use a `.venv`.
  2.  **Verify:** Check `torch.cuda.is_available()` to make sure your hardware is helping you.
  3.  **Visualize:** Use the Variable Explorer instead of `print()` statements.
  4.  **Leverage:** Get the Student Pack immediately; Copilot will be your primary "pair programmer" for the group project.
  
  ---
- ### **Final Action Items for the Course Intro:**
  *   **Complete the Homework:** Follow the steps on Slides 27–30 to get a working notebook.
  *   **Apply for the Pack:** Go to `education.github.com/pack`. Don't wait, as manual verification can take 3-5 days.
  *   **Explore the "Mill":** Compare the speed of your laptop vs. the Cluster (Slide 3) by running the "Simple Neural Network" (Slide 24) on both.