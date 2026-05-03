### 1. What is a "Cut"? (Page 682)
Before we prove the theorem, we have to define a **Cut**. 
Imagine taking a pair of scissors and cutting the entire graph into two completely separate pieces.
*   **Set $S$:** The group of vertices that contains the Source ($s$).
*   **Set $T$:** The group of vertices that contains the Sink ($t$).

Any flow going from the source to the sink *must* cross over this cut. There are two metrics we care about for any cut:

1.  **Net Flow $f(S,T)$:** The total flow physically crossing the cut. We add the flow going forward ($S \to T$) and **subtract** the flow going backward ($T \to S$). 
  *   *Lemma 24.4* proves a beautiful property: The net flow across *any* valid cut is exactly equal to the total flow of the network, $|f|$.
2.  **Capacity $c(S,T)$:** The maximum possible space across the cut. We add the capacities of all edges going forward ($S \to T$). 
  *   *Crucial Detail:* We **ignore** edges going backward ($T \to S$) when calculating capacity!

**The Golden Rule (Corollary 24.5):** 
$$|f| \le c(S,T)$$
The total flow of your network can **never** exceed the capacity of a cut. Think of a cut as a physical bottleneck. If a set of pipes crossing a valley can only hold 50 gallons, you cannot push 60 gallons through the network, period.
- ### 2. The Theorem Statement (Theorem 24.6)
  The theorem states that if $f$ is a flow in a network, the following three conditions are completely mathematically equivalent. If one is true, they are all true:
  1.  **$f$ is a maximum flow in $G$.**
  2.  **The residual network $G_f$ contains no augmenting paths.**
  3.  **$|f| = c(S,T)$ for some cut $(S,T)$.**
- ### 3. The "Deep Dive" Proofs (Required for your presentation!)
  Your professor asked you to explain how these three conditions prove each other. We do this in a circle: $(1) \Rightarrow (2) \Rightarrow (3) \Rightarrow (1)$.
- #### **Proof: (1) implies (2)**
  *   *The Logic:* We prove this by contradiction. 
  *   Suppose condition 1 is true: $f$ is the absolute maximum flow. 
  *   But let's pretend condition 2 is false: the residual network $G_f$ *still* has an augmenting path $p$. 
  *   If an augmenting path exists, it has a bottleneck capacity $> 0$. We could push that much more flow through the network, increasing our total flow. 
  *   *The Contradiction:* We just increased the flow, but we assumed $f$ was already the maximum! That's impossible. Therefore, if $f$ is a max flow, there absolutely **cannot** be any augmenting paths left.
- #### **Proof: (2) implies (3)**
  *   *The Logic:* Suppose there are no augmenting paths left in $G_f$. The source $s$ is completely cut off from the sink $t$. 
  *   Let's create a specific cut. Define **Set $S$** as every single vertex you can still reach from the source in the residual network. Define **Set $T$** as everything else.
  *   Look at any edge crossing from $S$ to $T$. Its flow in the real graph *must* be perfectly equal to its capacity ($f(u,v) = c(u,v)$). Why? Because if the pipe wasn't totally full, there would be a forward edge in the residual network, and the vertex on the other side would have been included in Set $S$!
  *   Look at any edge going backward from $T$ to $S$. Its flow *must* be 0. Why? Because if there was flow on it, there would be a backward (undo) edge in the residual network allowing us to cross from $S$ to $T$, which would again pull that node into Set $S$!
  *   Since all forward pipes are 100% full, and all backward pipes are 100% empty, the net flow across this cut is exactly equal to its capacity. Thus, $|f| = c(S,T)$.
- #### **Proof: (3) implies (1)**
  *   *The Logic:* We know from our Golden Rule that the value of *any* flow is bounded by the capacity of *any* cut ($|f| \le c(S,T)$).
  *   If we find a specific flow whose value perfectly equals the capacity of a cut ($|f| = c(S,T)$), it is mathematically impossible for the flow to get any larger without breaking the laws of physics. 
  *   Thus, it must be the true global maximum flow. ■
  
  ---
- ### Part 3 Practice Questions (Concept Check)
  
  **Q1: Cut Capacity vs. Net Flow**
  You draw a cut $(S, T)$ through a graph. 
  *   Edge $A$ goes from $S \to T$ with capacity 15 and current flow 10.
  *   Edge $B$ goes from $S \to T$ with capacity 10 and current flow 10.
  *   Edge $C$ goes from $T \to S$ (backward) with capacity 5 and current flow 3.
  1. What is the Capacity $c(S,T)$ of this cut?
  2. What is the Net Flow $f(S,T)$ across this cut?
  
  **Q2: The Minimum Cut**
  What is a **Minimum Cut**? Based on the theorem we just proved, how does the capacity of the Minimum Cut relate to the Maximum Flow of the entire network?
  
  **Q3: The Contradiction Check**
  If a classmate claims they ran Ford-Fulkerson and found a maximum flow of 100, but you find a cut in their graph with a capacity of 90, how do you know immediately that they made a math error?
  
  ---
-