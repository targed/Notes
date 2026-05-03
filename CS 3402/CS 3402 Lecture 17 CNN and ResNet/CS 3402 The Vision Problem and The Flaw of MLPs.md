### **Part 1: The Vision Problem & The Flaw of MLPs**
**(Covering Slides 4–15)**

Before 2012, computers were notoriously terrible at understanding images. To understand why Convolutional Neural Networks (CNNs) were such a massive breakthrough, we first have to understand how computers process visual data and why the Multi-Layer Perceptrons (MLPs) we learned about in Lecture 12 completely fail at this task.
- #### **1. How Computers "See" (Slide 7)**
  Humans see a photograph of Abraham Lincoln. Computers see a spreadsheet of numbers. 
  *   **Pixel Intensity:** Every pixel in a grayscale image is represented by a single number, typically ranging from 0 (black) to 255 (white).
  *   **Color Images (RGB):** A color image is fundamentally a 3D Tensor. It has Height, Width, and 3 "Channels" (one grid for Red, one for Green, one for Blue).
- #### **2. The Biological Inspiration (Slides 8–10)**
  Just like standard neural networks were inspired by brain neurons, CNNs were inspired by the **visual cortex**. 
  *   In the 1950s/60s, researchers Torsten Wiesel and David Hubel mapped the visual cortex of cats. They discovered that neurons don't just look at the *whole* picture. 
  *   Instead, individual neurons only fire when they see a very specific pattern (like a vertical line or a diagonal edge) in a very specific, tiny area of their vision (a **local neighborhood**). 
  *   CNNs are designed to replicate this exact biological mechanism.
- #### **3. The Fatal Flaw of MLPs for Vision (Slides 13–15)**
  In Lecture 12, we learned that in a standard MLP (Fully-Connected Network), every single input node connects to every single neuron in the next layer. For images, this is a mathematical disaster.
  
  **The Math Problem (Slide 14):**
  Let's say you have a very basic, low-resolution color image: $640 \times 480$ pixels.
  *   Total pixels = $640 \times 480 = 307,200$ pixels.
  *   Because it's color (RGB), we multiply by 3 channels = **$921,600$ input values**.
  *   If we want just *one* hidden layer with 100 neurons, every single one of those 921,600 inputs must connect to all 100 neurons.
  *   $921,600 \times 100 =$ **$92,160,000$ weights!** 
  
  And that is just for a tiny, low-res image and a tiny 100-neuron layer! If you used a modern 4K smartphone photo, you would need *billions* of weights for a single layer.
  
  **The Consequences (Slide 15):**
  Why is having 92 million weights bad?
  1.  **Massive Overfitting:** With that many parameters, the model will just memorize the training images pixel-by-pixel and fail completely on new images.
  2.  **Training Time:** It requires supercomputers and weeks of time to calculate gradients for that many parameters.
  3.  **Destruction of Spatial Data:** To feed an image into an MLP, you have to "flatten" the 2D grid into a 1D line of numbers. When you do this, the network loses all understanding of "up," "down," "left," and "right." It doesn't know that the pixels forming a cat's ear are right next to each other.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Translation Invariance:** MLPs struggle with this. If an MLP learns to recognize a cat in the *center* of a photo, and you show it a photo where the cat is in the *bottom left corner*, the MLP will fail because the pixels activated completely different input nodes. CNNs (which we will cover next) solve this!
  
  ---
- ### **Action Items for Section 1:**
  *   **Concept Check:** If you take a standard 1080p color computer monitor ($1920 \times 1080$) and feed it into a fully-connected layer with just 10 hidden neurons, how many weights (parameters) must the network learn?
    *   *Answer:* $(1920 \times 1080 \times 3 \text{ color channels}) \times 10 = \mathbf{62,208,000 \text{ weights}}$.