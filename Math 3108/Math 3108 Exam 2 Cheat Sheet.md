- **Section 2.2: The Inverse of a Matrix**
  
  *   **Invertible Matrix:** An $n \times n$ matrix $A$ is invertible if there exists an $n \times n$ matrix $C$ ($=A^{-1}$) such that $CA = AC = I_n$. Only square matrices can be invertible in this sense.
  *   **Determinant (2x2):** For $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, $\det A = ad - bc$.
  *   **Inverse (2x2):** If $\det A = ad - bc \neq 0$, then $A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$. If $\det A = 0$, $A$ is not invertible (singular).
  *   **Solving Ax = b:** If $A$ is invertible, the unique solution is $\mathbf{x} = A^{-1}\mathbf{b}$. (Often used in exam Qs where $A^{-1}$ is given or calculated first).
  *   **Properties of Inverses:**
      *   $(A^{-1})^{-1} = A$
      *   $(AB)^{-1} = B^{-1}A^{-1}$ (Order reverses! Frequently tested).
      *   $(A^T)^{-1} = (A^{-1})^T$
  *   **Finding the inverse (General Method):** Row reduce the augmented matrix $\left[ \begin{array}{c|c} A & I \end{array} \right]$. If $A$ is row equivalent to $I$, then $\left[ \begin{array}{c|c} A & I \end{array} \right] \sim \left[ \begin{array}{c|c} I & A^{-1} \end{array} \right]$.
  
  ---
- **Section 2.3: Characterizations of Invertible Matrices**
  
  *   **The Invertible Matrix Theorem (IMT):** For a *square* $n \times n$ matrix $A$, the following are equivalent (all true or all false):
      *   $A$ is invertible.
      *   $A$ is row equivalent to $I_n$.
      *   $A$ has $n$ pivot positions.
      *   $A\mathbf{x} = \mathbf{0}$ has only the trivial solution ($\mathbf{x}=\mathbf{0}$).
      *   The columns of $A$ are linearly independent.
      *   The linear transformation $\mathbf{x} \mapsto A\mathbf{x}$ is one-to-one.
      *   $A\mathbf{x} = \mathbf{b}$ has a *unique* solution for each $\mathbf{b}$ in $\mathbb{R}^n$.
      *   The columns of $A$ span $\mathbb{R}^n$.
      *   The linear transformation $\mathbf{x} \mapsto A\mathbf{x}$ maps $\mathbb{R}^n$ onto $\mathbb{R}^n$.
      *   There exists $C$ such that $CA=I$.
      *   There exists $D$ such that $AD=I$.
      *   $A^T$ is invertible.
      *   $\det A \neq 0$ (from Chapter 3).
      *   *See also Section 4.5 additions.*
  *   **Key Use:** If you know one condition holds (or fails), you know *all* conditions hold (or fail) for a square matrix. (Many True/False or multiple-choice questions rely on this).
  
  ---
- **Section 2.4: Partitioned Matrices**
  
  *   **Block Multiplication:** Multiply partitioned matrices using row-column rule, treating blocks as elements, *provided the partitions are conformable* (column partition of first matrix matches row partition of second).
      *   Example: $\begin{bmatrix} A & B \\ C & D \end{bmatrix} \begin{bmatrix} E & F \\ G & H \end{bmatrix} = \begin{bmatrix} AE+BG & AF+BH \\ CE+DG & CF+DH \end{bmatrix}$ (if block sizes match).
  *   **Inverse of Block Diagonal:** $\left( \begin{bmatrix} A & 0 \\ 0 & D \end{bmatrix} \right)^{-1} = \begin{bmatrix} A^{-1} & 0 \\ 0 & D^{-1} \end{bmatrix}$ (if A, D invertible).
  
  ---
- **Section 2.5: Matrix Factorizations**
  
  *   **LU Factorization:** If $A$ can be row reduced to echelon form $U$ *without row interchanges*, then $A = LU$.
      *   $L$: Square lower triangular matrix with 1s on the diagonal. Same size as $A$ if $A$ is square.
      *   $U$: Echelon form of $A$. Same size as $A$.
  *   **Finding LU:**
      1.  Reduce $A$ to echelon form $U$ using only row replacements ($R_i \rightarrow R_i - k R_j$).
      2.  Construct $L$ by placing the multiplier $k$ used to eliminate the entry in row $i$ using row $j$ into the $(i, j)$ position of $L$. Place 1s on the diagonal and 0s above the diagonal.
  *   **Solving $A\mathbf{x} = \mathbf{b}$ using LU:**
      1.  Solve $L\mathbf{y} = \mathbf{b}$ for $\mathbf{y}$ (using forward substitution).
      2.  Solve $U\mathbf{x} = \mathbf{y}$ for $\mathbf{x}$ (using backward substitution).
  
  ---
- **Section 3.1: Introduction to Determinants**
  
  *   **Determinant:** A scalar value associated with a square matrix.
  *   **Determinant (2x2):** $\begin{vmatrix} a & b \\ c & d \end{vmatrix} = ad - bc$.
  *   **Cofactor:** $C_{ij} = (-1)^{i+j} \det A_{ij}$, where $A_{ij}$ is the submatrix obtained by deleting row $i$ and column $j$.
  *   **Determinant (nxn):** Cofactor expansion across row $i$ or down column $j$.
      *   Row $i$: $\det A = \sum_{j=1}^n a_{ij} C_{ij}$
      *   Column $j$: $\det A = \sum_{i=1}^n a_{ij} C_{ij}$
  *   **Determinant of Triangular Matrix:** Product of the diagonal entries.
  
  ---
- **Section 3.2: Properties of Determinants**
  
  *   **Row Operations:** Let $A \rightarrow B$.
      *   Row Replacement ($R_i \rightarrow R_i + k R_j$): $\det B = \det A$.
      *   Row Interchange ($R_i \leftrightarrow R_j$): $\det B = -\det A$.
      *   Row Scaling ($R_i \rightarrow k R_i$): $\det B = k \det A$.
  *   **Calculating Determinants:** Use row operations (mostly replacements and interchanges) to get to echelon form (triangular). Keep track of determinant changes. $\det A = (-1)^r \times (\text{product of pivots in } U)$, where $r$ is # interchanges. *If scaling is used, factor that out.*
  *   **Invertibility:** $A$ is invertible if and only if $\det A \neq 0$.
  *   **Properties:**
      *   $\det A^T = \det A$.
      *   $\det(AB) = (\det A)(\det B)$.
      *   $\det(A^k) = (\det A)^k$.
      *   If columns (or rows) are linearly dependent, $\det A = 0$.
  
  ---
- **Section 3.3: Cramer's Rule, Volume**
  
  *   **Cramer's Rule:** Solves $A\mathbf{x} = \mathbf{b}$ for *invertible* $A$.
      *   $x_i = \frac{\det A_i(\mathbf{b})}{\det A}$, where $A_i(\mathbf{b})$ is $A$ with column $i$ replaced by $\mathbf{b}$.
  *   **Inverse using Adjugate:** $A^{-1} = \frac{1}{\det A} \text{adj } A$, where $\text{adj } A = [C_{ji}]$ (matrix of cofactors, *transposed*).
  *   **Geometric Interpretation:**
      *   Area of parallelogram (in $\mathbb{R}^2$) determined by vectors $\mathbf{u}, \mathbf{v}$ is $|\det[\mathbf{u} \quad \mathbf{v}]|$. (Exam problems often give vertices, find vectors from origin).
      *   Volume of parallelepiped (in $\mathbb{R}^3$) determined by $\mathbf{u}, \mathbf{v}, \mathbf{w}$ is $|\det[\mathbf{u} \quad \mathbf{v} \quad \mathbf{w}]|$.
  
  ---
- **Section 4.1: Vector Spaces and Subspaces**
  
  *   **Vector Space:** A set with vector addition and scalar multiplication satisfying 10 axioms (closure, commutativity, associativity, zero vector, additive inverse, etc.). Key examples: $\mathbb{R}^n$, $\mathbb{P}_n$ (polynomials degree $\le n$).
  *   **Subspace:** A subset $H$ of a vector space $V$ that is itself a vector space.
  *   **Subspace Criteria (How to Prove):** Must satisfy ALL three:
      1.  $\mathbf{0} \in H$ (Zero vector of $V$ is in $H$).
      2.  Closed under addition: If $\mathbf{u}, \mathbf{v} \in H$, then $\mathbf{u} + \mathbf{v} \in H$.
      3.  Closed under scalar multiplication: If $\mathbf{u} \in H$ and $c$ is a scalar, then $c\mathbf{u} \in H$.
  *   **Span as Subspace:** Span{$\mathbf{v}_1, ..., \mathbf{v}_p$} is always a subspace.
  
  ---
- **Section 4.2: Null Spaces, Column Spaces, Row Spaces**
  
  *   **Nul A:** Set of all solutions to $A\mathbf{x} = \mathbf{0}$. Subspace of $\mathbb{R}^n$.
      *   *Basis for Nul A:* Solve $A\mathbf{x} = \mathbf{0}$, write solution in parametric vector form. The vectors multiplying the free variables form the basis.
  *   **Col A:** Set of all linear combinations of the columns of A (or Range of $T(\mathbf{x})=A\mathbf{x}$). Subspace of $\mathbb{R}^m$.
      *   *Basis for Col A:* The pivot columns of the *original* matrix $A$.
  *   **Row A:** Set of all linear combinations of the rows of A. Subspace of $\mathbb{R}^n$. (Same as Col $A^T$).
      *   *Basis for Row A:* The non-zero rows of the echelon form ($U$) of $A$.
  *   **Kernel of T:** Same as Nul A if $T(\mathbf{x})=A\mathbf{x}$.
  *   **Range of T:** Same as Col A if $T(\mathbf{x})=A\mathbf{x}$.
  
  ---
- **Section 4.3: Linearly Independent Sets and Bases**
  
  *   **Linearly Independent (LI):** The equation $c_1\mathbf{v}_1 + ... + c_p\mathbf{v}_p = \mathbf{0}$ has *only* the trivial solution ($c_1=...=c_p=0$).
  *   **Linearly Dependent (LD):** There exist weights $c_i$, *not all zero*, such that $c_1\mathbf{v}_1 + ... + c_p\mathbf{v}_p = \mathbf{0}$. (At least one vector is a linear combo of others).
  *   **Basis:** A set $\mathcal{B}$ that is **Linearly Independent** AND **Spans** the vector space/subspace.
  *   **Basis Views (Multiple Choice!):** A basis is a *minimal spanning set* and a *maximal linearly independent set*.
  *   **Spanning Set Theorem:** Can remove LD vectors from a spanning set without changing the span.
  *   **Finding Bases:** See methods for Nul A, Col A, Row A in Section 4.2.
  
  ---
- **Section 4.4: Coordinate Systems**
  
  *   **Coordinates:** If $\mathcal{B} = \{\mathbf{b}_1, ..., \mathbf{b}_n\}$ is a basis for $V$, and $\mathbf{x} = c_1\mathbf{b}_1 + ... + c_n\mathbf{b}_n$, then the $\mathcal{B}$-coordinate vector is $[\mathbf{x}]_\mathcal{B} = \begin{bmatrix} c_1 \\ \vdots \\ c_n \end{bmatrix}$. Representation is *unique*.
  *   **Change-of-Coordinates Matrix:** $P_\mathcal{B} = [\mathbf{b}_1 \dots \mathbf{b}_n]$. Then $\mathbf{x} = P_\mathcal{B}[\mathbf{x}]_\mathcal{B}$.
  *   **Finding [x]b:** Solve the system $P_\mathcal{B} \mathbf{c} = \mathbf{x}$ for the coordinate vector $\mathbf{c} = [\mathbf{x}]_\mathcal{B}$.
  *   **Coordinate Mapping:** $\mathbf{x} \mapsto [\mathbf{x}]_\mathcal{B}$ is an isomorphism (preserves structure). Useful for checking LI in spaces like $\mathbb{P}_n$ by converting to coordinate vectors in $\mathbb{R}^n$.
  
  ---
- **Section 4.5: The Dimension of a Vector Space**
  
  *   **Dimension (dim V):** The number of vectors in *any* basis for $V$.
  *   **Rank A:** dim Col A (= dim Row A) = number of pivot positions.
  *   **Nullity A:** dim Nul A = number of free variables.
  *   **Rank Theorem:** For an $m \times n$ matrix $A$: **rank A + nullity A = n** (number of columns). (Very important for finding dimensions).
  *   **Basis Theorem:** If dim V = n:
      *   Any LI set of $n$ vectors in V is a basis.
      *   Any set of $n$ vectors that spans V is a basis.
  *   **IMT Extended (for n x n matrix A):** Equivalent conditions include:
      *   Columns of A form a basis for $\mathbb{R}^n$.
      *   Col A = $\mathbb{R}^n$.
      *   dim Col A = n (rank A = n).
      *   Nul A = {**0**}.
      *   dim Nul A = 0 (nullity A = 0).