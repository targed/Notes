- We have established that Convolutional Neural Networks (CNNs) are incredible at vision tasks because they build hierarchies of features (Edges $\rightarrow$ Shapes $\rightarrow$ Objects).
  
  Naturally, researchers in the 2010s asked: *"If a 10-layer network is good, wouldn't a 100-layer network be 10x better?"*
  
  They tried it, and the results were a disaster. **ResNet (Residual Networks)** was invented in 2015 to solve this exact problem, and it fundamentally changed how we build deep AI models.
  
  ---
- ### **1. The "Degradation Problem" (Slide 44)**
  When researchers started stacking dozens of convolutional layers on top of each other, they expected the model to get smarter. Instead, something weird happened.
  
  Look at the graph on **Slide 44**:
  *   The **blue line** is a 18-layer "Plain Net" (a standard CNN).
  *   The **red line** is a 34-layer "Plain Net".
  *   *The Result:* The 34-layer network performed **worse** (higher error rate) than the shallower 18-layer network on *both* the training data and the testing data.
  
  **Why did this happen?**
  It wasn't Overfitting (because the training error was also terrible). It was the **Vanishing Gradient Problem**.
  *   During Backpropagation (which we learned about in Lecture 9/10), the error signal has to travel backward through every single layer, getting multiplied by decimals over and over again.
  *   By the time the signal travels backward through 34 layers, the math becomes so microscopically small that the first few layers of the network receive a gradient of basically `0.000000000001`.
  *   Because the gradient is zero, the early layers **stop learning completely**, and the entire network breaks down.
- ### **2. The Genius Solution: The Residual Block (Slides 41–42)**
  How do we get the gradient signal to travel backward through 100 layers without shrinking to zero? We build a highway.
  
  **The "Skip Connection" (Slide 41):**
  *   Look at the diagram on the right. Notice the arrow that loops *around* the weight layers and the activation function? This is called a **Skip Connection** (or a Shortcut).
  *   **The Math:** Instead of forcing the data ($x$) to go *through* the complex math block ($F(x)$), we give it a bypass lane. We take the original input ($x$) and **add** it to the output of the math block.
  *   *Formula (Slide 42):* $$H(x) = F(x) + x$$
  
  **Why this fixes everything:**
  1.  **Forward Pass:** If the math block ($F(x)$) turns out to be useless or destructive, the network can easily learn to push all the weights in $F(x)$ to zero. If $F(x) = 0$, then $H(x) = 0 + x$. The data just passes straight through unharmed! This means a 34-layer network can *never* perform worse than an 18-layer network, because it can just turn off the extra 16 layers if it needs to.
  2.  **Backward Pass (Gradients):** During Backpropagation, the error signal can hop onto the Skip Connection highway and travel backward at full strength, completely bypassing the messy math blocks that would normally shrink it.
- ### **3. Building ResNet (Slide 43)**
  With the Vanishing Gradient problem solved, researchers could finally build networks as deep as they wanted.
  
  *   **The Architecture:** Slide 43 shows the architecture of **VGG-19** (a famous 19-layer Plain Net) next to a **34-layer ResNet**.
  *   Notice all the little looping arrows on the right side of the ResNet diagram? Every single one of those is a Skip Connection (the $F(x) + x$ block).
  *   **The Design Rules:** The creators kept it incredibly simple.
    *   Almost every filter is exactly $3 \times 3$ pixels.
    *   Whenever the image is shrunk by half (Downsampling with a stride of 2), they simply **double** the number of filters to compensate for the lost spatial data.
- ### **4. The Results (Slide 45)**
  Did the Skip Connections work?
  
  Look at the graph on **Slide 45**:
  *   The **red line** (34-layer ResNet) is now completely below the **blue line** (18-layer ResNet).
  *   *The Conclusion:* By simply adding that one looping $+ x$ arrow, they solved the Degradation Problem. Deeper networks finally performed better than shallower networks!
  *   This specific invention (Residual Learning) is why modern AIs like ChatGPT can have 96+ layers. *(Remember the "Add & Norm" block from the Transformer in Lecture 14? That "Add" is literally a ResNet Skip Connection!)*
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Identity Mapping:** The mathematical term for $F(x) + x$ when $F(x) = 0$ is an *Identity Mapping* (the output is identical to the input).
  *   **Batch Normalization:** Slide 43 mentions this. It is a technique used inside the ResNet blocks (similar to the "Norm" step in Transformers) to keep the numbers stable and centered around 0 during training.
  
  ---
- ### **Action Items for Section 4:**
  *   **Concept Check:** If a 50-layer Plain CNN gets 80% accuracy, but a 100-layer Plain CNN gets 60% accuracy, what is this phenomenon called?
    *   *Answer:* The **Degradation Problem** (caused by vanishing gradients).
  *   **Formula Check:** Be able to write the formula for a Residual Block from memory: **$H(x) = F(x) + x$**.