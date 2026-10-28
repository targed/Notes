## 1. The Problem: "Parameter Explosion" (Slide 5)
  Why can't we just use the standard Fully Connected Neural Networks (MLPs) from Lecture 15 for images?
  * **The Concept:** In a standard MLP, every pixel in the input connects to every neuron in the first hidden layer.
  * **The Math Example:** 
  * For a tiny MNIST digit ($28 \times 28$ pixels), the input is $784$. 
  * If the first hidden layer has $500$ neurons, that is $784 \times 500 = \mathbf{392,000}$ weights to learn. 
  * What if the image is a standard smartphone photo ($700 \times 700$ pixels)? The input is $490,000$. If we connect that to 500 neurons, we have **$245,000,000$ parameters** in just the first layer! 
  * **The Consequence:** The model will be too massive to fit in memory, incredibly slow to train, and will instantly **overfit**. We need a way to process images locally.
## 2. The Solution: Convolution (Slides 6, 9-16)
  Instead of connecting everything to everything, we slide a small window (called a **Filter** or **Kernel**) across the image. 
  
  **The Hand-Calculation Example (Slides 10 & 16):**
  The slide shows a classic edge-detection filter applied to an image.
  * **The Filter ($3 \times 3$):** 
  $$ \begin{bmatrix} +1 & +2 & +1 \\ 0 & 0 & 0 \\ -1 & -2 & -1 \end{bmatrix} $$
  * **The Operation:** Element-wise multiplication followed by addition.
  
  **Case A (Slide 10): A flat region of color.**
  * The filter sits over a patch of the image where every pixel is `255` (pure white).
  * Calculation: $255(+1) + 255(+2) + 255(+1) + 255(0) \dots + 255(-1) + 255(-2) + 255(-1)$.
  * The positives and negatives cancel out perfectly. **Result = 0**. 
  * *Intuition:* The filter outputs 0 when there is no change in the image (a flat, featureless area).
  
  **Case B (Slide 16): An edge (Color changing from 0 to 255).**
  * The filter sits over a patch where the top row is `0` (black) and the bottom row is `255` (white).
  * Calculation: 
  * Top row: $0(+1) + 0(+2) + 0(+1) = 0$
  * Middle row: $0(0) + 0(0) + 0(0) = 0$
  * Bottom row: $255(-1) + 255(-2) + 255(-1) = -255 - 510 - 255 = \mathbf{-1020}$.
  * *Intuition:* The filter outputs a massive negative number! It successfully "detected" the horizontal edge where the color transitioned.
## 3. Controlling the Output Size: Stride and Padding (Slides 21-29)
  When you slide a $3 \times 3$ filter over an image, the output is smaller than the input because the filter can't hang off the edges. We control the output shape using two hyperparameters:
  
  * **Stride (Slides 21-24):** How many pixels the filter "jumps" each time it moves. 
  * A stride of 1 moves pixel by pixel.
  * A stride of 3 jumps 3 pixels at a time, creating a much smaller output image.
  * **Padding (Slides 25-29):** Adding a border of fake `0` pixels around the outside of the original image.
  * *Why?* To allow the filter to center itself on the very edge pixels of the original image. 
  * If `padding=1`, we add a 1-pixel border. If `padding=2`, we add a 2-pixel border.
  
  **The Output Size Formula (Critical for the Exam!):**
  Though not explicitly spelled out in these early slides, you must know this formula for the After-Class Questions:
  $$ \text{Output Dimension} = \frac{\text{Input} - \text{Filter Size} + (2 \times \text{Padding})}{\text{Stride}} + 1 $$
  
  ***
# Problem Solving & Concept Check
  
  **1. Convolution Math Check:**
  You have a $3 \times 3$ image patch and a $3 \times 3$ filter. 
  * Image Patch: `[[1, 1, 1], [1, 1, 1],[1, 1, 1]]`
  * Filter: `[[0, 1, 0],[1, -4, 1], [0, 1, 0]]`
  What is the resulting output value for this single step of convolution?
  * *Calculation:* $(1\times0) + (1\times1) + (1\times0) + (1\times1) + (1\times-4) + (1\times1) + (1\times0) + (1\times1) + (1\times0)$.
  * *Sum:* $0 + 1 + 0 + 1 - 4 + 1 + 0 + 1 + 0 = \mathbf{0}$.
  
  **2. Output Size Calculation:**
  You have a $32 \times 32$ input image. You apply a $5 \times 5$ filter with `stride = 1` and `padding = 0` (Valid padding). 
  What is the size of the output feature map?
  * *Calculation:* $(32 - 5 + 0) / 1 + 1 = 27 / 1 + 1 = \mathbf{28 \times 28}$.
  
  **3. The Purpose of Padding:**
  If you apply a $3 \times 3$ filter (stride 1) repeatedly through a deep network of 50 layers, and you never use padding, what will happen to your image?
  * *Answer:* It will shrink by 2 pixels on every layer. By layer 16, a $32 \times 32$ image will completely vanish into a $0 \times 0$ box. Padding allows us to build incredibly deep networks without the image shrinking out of existence.
  
  ***