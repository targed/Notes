### **1. The Solution to MLPs: Parameter Sharing (Slides 17–19)**
In Part 1, we saw that a fully connected network needs 92 million weights for a small image because every pixel connects to every neuron. 

**Convolutional Neural Networks (CNNs)** solve this with two genius ideas:
1.  **Sparse Connectivity (Slide 17):** Instead of looking at the whole image at once, a neuron only looks at a tiny, $3 \times 3$ or $5 \times 5$ square of pixels (a "local neighborhood").
2.  **Parameter Sharing (Slide 19):** If we learn a specific pattern (like a vertical line) in the top-left corner of an image, that exact same pattern is probably useful in the bottom-right corner. Therefore, we use the *exact same weights* across the entire image.
  *   *Result:* Instead of learning 92 million weights, a CNN layer might only need to learn **25 weights** (a $5 \times 5$ filter), regardless of how big the input image is!
- ### **2. The Core Mechanic: The Filter / Kernel (Slides 20–22, 29–30)**
  A **Filter** (also called a **Kernel**) is a tiny grid of numbers (weights) that slides across the image. 
  
  *   **What does it do? (Slide 20):** It performs a mathematical operation called a **Convolution**. It acts like a magnifying glass hunting for a specific feature.
  *   **Different Filters, Different Jobs (Slide 29):** Depending on the numbers inside the kernel, it will transform the image in different ways. 
    *   *Identity:* Does nothing.
    *   *Edge Detection:* Turns the image black but highlights sharp outlines in white.
    *   *Sharpen / Blur:* Adjusts the contrast between neighboring pixels. 
    *   *Note:* In old Photoshop software, programmers had to manually write the numbers for these grids. In a CNN, the neural network **learns** these numbers automatically via Gradient Descent to find the best filters for the task!
- ### **3. The Math: The Dot Product (Slide 22)**
  How does the kernel actually process the pixels? It uses a **Dot Product**.
  
  **Let's do the exact math from the "Toy Example" on Slide 22.**
  You have a $3 \times 3$ section of an image (the blue box on the left), and a $3 \times 3$ Filter (in the middle).
  1.  You overlay the Filter exactly on top of the Image patch.
  2.  You multiply the overlapping numbers.
    *   Top row: $(1 \times 1) + (1 \times 0) + (1 \times 1) = \mathbf{2}$
    *   Middle row: $(0 \times 0) + (1 \times 1) + (1 \times 0) = \mathbf{1}$
    *   Bottom row: $(0 \times 1) + (0 \times 0) + (1 \times 1) = \mathbf{1}$
  3.  You sum all those results together.
    *   $2 + 1 + 1 = \mathbf{4}$
  4.  That final number (4) is placed into the new **Feature Map** grid on the right.
- ### **4. Moving the Filter: Stride (Slides 23–24)**
  After you calculate the first box, you have to move the filter.
  
  *   **Stride:** This hyperparameter dictates how many pixels the filter shifts over before doing the next calculation.
  *   *Slide 23:* A Stride of $1$ means the filter moves exactly one pixel to the right. 
  *   *Slide 24:* Notice how the blue box on the left shifted one column over. You would repeat the dot product math for this new $3 \times 3$ section to get the next number in the Feature Map (which the slide shows is **3**).
- ### **5. Feature Maps & Activation (Slides 31–35)**
  Once the filter has slid across the entire image, it has created a brand new grid of numbers called an **Activation Map** (or Feature Map).
  
  *   **The Activation Function (Slide 31):** Just like an MLP, we must apply **ReLU** to the output. If the dot product calculation results in a negative number, ReLU turns it into a 0. (This creates the black backgrounds you see in the Edge Detection filters on Slide 29).
  *   **Depth / Channels (Slides 34–35):** One filter only detects one thing (e.g., horizontal lines). A real CNN uses a **Bank of Filters** (e.g., 6 different filters) on the exact same image.
    *   This outputs 6 separate Activation Maps.
    *   We stack them like pancakes. If our original image was `[32x32x3]` (RGB), our new representation is `[28x28x6]`. It got slightly smaller in width/height, but much "deeper" in features.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **The Waldo Analogy (Slides 25–28):** Think of the Filter as a transparent sheet with Waldo's face drawn on it. As you slide it across the page, the dot product will be very low (close to 0) when it is over the sand or the water. But when it slides perfectly over the real Waldo, the math "lights up" with a massive number (a "Large Response" like 6600). 
  *   **Why did the image get smaller? (Slide 32):** Notice the image went from $32 \times 32$ to $28 \times 28$. Because a $5 \times 5$ filter can't hang off the edge of the image, it stops 2 pixels before the border. This naturally shrinks the Feature Map. (We fix this in PyTorch using "Padding").
  
  ---
- ### **Action Items for Section 2:**
  *   **Math Check:** On Slide 22, look at the $3 \times 3$ Image patch in the very bottom left corner (outlined in orange). The values are:
    `[0, 0, 1]`
    `[0, 0, 1]`
    `[0, 1, 1]`
    Multiply that by the yellow Filter in the middle. What is the dot product?
    *   *Answer:* $(0 \times 1) + (0 \times 0) + (1 \times 1) + (0 \times 0) + (0 \times 1) + (1 \times 0) + (0 \times 1) + (1 \times 0) + (1 \times 1) = 1 + 0 + 1 = \mathbf{2}$.