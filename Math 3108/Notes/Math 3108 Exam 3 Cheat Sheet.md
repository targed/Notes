### **Chapter 5: Eigenvalues and Eigenvectors**
#### **Section 5.1: Eigenvalues and Eigenvectors**
  
  *   **Definition:**
    *   An **eigenvector** of an $n \times n$ matrix $A$ is a **nonzero** vector $\mathbf{x}$ such that $A\mathbf{x} = \lambda \mathbf{x}$ for some scalar $\lambda$.
    *   A scalar $\lambda$ is called an **eigenvalue** of $A$ if $A\mathbf{x} = \lambda \mathbf{x}$ has a non-trivial solution $\mathbf{x}$.
    *   Such an $\mathbf{x}$ is an eigenvector corresponding to eigenvalue $\lambda$.
  *   **Checking:**
    *   To check if $\mathbf{x}$ is an eigenvector of $A$: compute $A\mathbf{x}$ and see if it's a scalar multiple ($\lambda \mathbf{x}$) of $\mathbf{x}$. $\mathbf{x}$ must be non-zero.
    *   To check if $\lambda$ is an eigenvalue of $A$: determine if the equation $(A - \lambda I)\mathbf{x} = \mathbf{0}$ has a non-trivial solution (i.e., if $A - \lambda I$ is not invertible, or $\det(A-\lambda I) = 0$).
  *   **Eigenspace:**
    *   The **eigenspace** corresponding to an eigenvalue $\lambda$ is the set of all solutions to $(A - \lambda I)\mathbf{x} = \mathbf{0}$.
    *   It's the null space of the matrix $(A - \lambda I)$.
    *   The eigenspace is a subspace of $\mathbb{R}^n$.
    *   It always contains the zero vector, but eigenvectors are non-zero by definition.
  *   **Triangular Matrices:**
    *   The eigenvalues of a triangular matrix are the entries on its main diagonal.
  *   **Eigenvalues of $A^{-1}$:**
    *   If $A$ is invertible and has eigenvalue $\lambda$ (so $\lambda \neq 0$), then $A^{-1}$ has eigenvalue $1/\lambda$ (or $\lambda^{-1}$) with the same corresponding eigenvector $\mathbf{x}$.
    *   Proof: $A\mathbf{x} = \lambda \mathbf{x} \implies A^{-1}A\mathbf{x} = A^{-1}(\lambda \mathbf{x}) \implies \mathbf{x} = \lambda A^{-1}\mathbf{x} \implies \frac{1}{\lambda}\mathbf{x} = A^{-1}\mathbf{x}$.
  *   **Invertibility:**
    *   $A$ is invertible if and only if $0$ is **not** an eigenvalue of $A$.
  *   **Linear Independence:**
    *   Eigenvectors corresponding to **distinct** eigenvalues are linearly independent.
#### **Section 5.2: The Characteristic Equation**
  
  *   **Finding Eigenvalues:**
    *   $\lambda$ is an eigenvalue of $A$ iff $(A - \lambda I)\mathbf{x} = \mathbf{0}$ has a non-trivial solution.
    *   This happens iff $A - \lambda I$ is not invertible.
    *   This happens iff $\det(A - \lambda I) = 0$.
  *   **Characteristic Equation/Polynomial:**
    *   The **characteristic equation** of $A$ is $\det(A - \lambda I) = 0$.
    *   $p(\lambda) = \det(A - \lambda I)$ is the **characteristic polynomial** of $A$. It's a polynomial in $\lambda$ of degree $n$.
    *   The eigenvalues are the roots of the characteristic polynomial.
  *   **Multiplicity:**
    *   The **(algebraic) multiplicity** of an eigenvalue $\lambda$ is its multiplicity as a root of the characteristic equation.
    *   The **geometric multiplicity** is the dimension of the eigenspace for $\lambda$.
    *   Geometric Multiplicity $\leq$ Algebraic Multiplicity.
#### **Section 5.3: Diagonalization**
  
  *   **Similarity:**
    *   $A$ is **similar** to $B$ if $A = PBP^{-1}$ for some invertible matrix $P$.
    *   Similar matrices have the same characteristic polynomial (and thus the same eigenvalues with the same multiplicities).
    *   Similarity is NOT the same as row equivalence.
  *   **Diagonalization:**
    *   $A$ is **diagonalizable** if $A$ is similar to a diagonal matrix $D$.
    *   $A = PDP^{-1}$ for some invertible $P$ and diagonal $D$.
  *   **The Diagonalization Theorem:**
    *   An $n \times n$ matrix $A$ is diagonalizable if and only if $A$ has $n$ linearly independent eigenvectors.
    *   If $A = PDP^{-1}$:
        *   The columns of $P$ are $n$ linearly independent eigenvectors of $A$.
        *   The diagonal entries of $D$ are the eigenvalues of $A$ corresponding to the eigenvectors in $P$ *in the same order*.
  *   **Diagonalizability Conditions:**
    *   An $n \times n$ matrix $A$ is diagonalizable iff the sum of the dimensions of its distinct eigenspaces equals $n$.
    *   This happens iff the characteristic polynomial factors completely into linear factors (over the field, e.g., $\mathbb{R}$) AND the geometric multiplicity equals the algebraic multiplicity for every eigenvalue.
    *   An $n \times n$ matrix with $n$ distinct eigenvalues is **always** diagonalizable. (Sufficient, not necessary).
  *   **Powers of Diagonalizable Matrices:**
    *   If $A = PDP^{-1}$, then $A^k = PD^kP^{-1}$.
    *   $D^k$ is found by raising the diagonal entries of $D$ to the power $k$.
#### **Section 5.5: Complex Eigenvalues**
  
  *   **Complex Numbers:**
    *   Conjugate: $\overline{a+bi} = a-bi$.
    *   Real part: $\text{Re}(a+bi) = a$. Imaginary part: $\text{Im}(a+bi) = b$.
    *   Vector Conjugate/Real/Imaginary parts are component-wise.
  *   **Eigenvalues of Real Matrices:**
    *   If $A$ is a real matrix and $\lambda$ is a complex eigenvalue with eigenvector $\mathbf{v}$, then its conjugate $\overline{\lambda}$ is also an eigenvalue with conjugate eigenvector $\overline{\mathbf{v}}$.
    *   $A\mathbf{v} = \lambda \mathbf{v} \implies \overline{A\mathbf{v}} = \overline{\lambda \mathbf{v}} \implies A\overline{\mathbf{v}} = \overline{\lambda} \overline{\mathbf{v}}$ (since $A$ is real, $\overline{A}=A$).
  *   **Rotation-Scaling Matrices ($2 \times 2$ Real):**
    *   The matrix $C = \begin{bmatrix} a & -b \\ b & a \end{bmatrix}$ (where $a, b \in \mathbb{R}$, not both zero) has eigenvalues $\lambda = a \pm bi$.
    *   If $\lambda = a+ib$, an eigenvector is $\begin{bmatrix} 1 \\ -i \end{bmatrix}$. If $\lambda = a-ib$, an eigenvector is $\begin{bmatrix} 1 \\ i \end{bmatrix}$.
    *   This transformation represents a rotation by angle $\phi$ and scaling by $r = |\lambda| = \sqrt{a^2+b^2}$, where $a = r\cos\phi, b = r\sin\phi$.
    *   $C = r \begin{bmatrix} \cos\phi & -\sin\phi \\ \sin\phi & \cos\phi \end{bmatrix}$.
  *   **Real Factorization ($2 \times 2$):**
    *   Let $A$ be a real $2 \times 2$ matrix with a complex eigenvalue $\lambda = a - bi$ ($b \neq 0$) and eigenvector $\mathbf{v}$.
    *   Then $A = PCP^{-1}$, where:
        *   $P = \begin{bmatrix} \text{Re}(\mathbf{v}) & \text{Im}(\mathbf{v}) \end{bmatrix}$ (P is a real matrix)
        *   $C = \begin{bmatrix} a & -b \\ b & a \end{bmatrix}$ (C is a real matrix representing rotation+scaling)
  
  ---
### **Chapter 6: Orthogonality and Least Squares**
#### **Section 6.1: Inner Product, Length, and Orthogonality**
  
  *   **Inner (Dot) Product:**
    *   $\mathbf{u} \cdot \mathbf{v} = \mathbf{u}^T \mathbf{v} = u_1v_1 + u_2v_2 + ... + u_nv_n$.
    *   Properties: Commutative, Distributive, Scalar Associativity, $\mathbf{u} \cdot \mathbf{u} \geq 0$ (and $=0$ iff $\mathbf{u}=\mathbf{0}$).
  *   **Length (Norm):**
    *   $\lVert \mathbf{v} \rVert = \sqrt{\mathbf{v} \cdot \mathbf{v}} = \sqrt{v_1^2 + ... + v_n^2}$.
    *   $\lVert c\mathbf{v} \rVert = |c| \lVert \mathbf{v} \rVert$.
  *   **Unit Vector:**
    *   A vector with length 1.
    *   **Normalizing:** To get a unit vector $\mathbf{u}$ in the same direction as $\mathbf{v}$ ($\mathbf{v} \neq \mathbf{0}$): $\mathbf{u} = \frac{1}{\lVert \mathbf{v} \rVert} \mathbf{v}$.
  *   **Distance:**
    *   The distance between $\mathbf{u}$ and $\mathbf{v}$ is $\text{dist}(\mathbf{u}, \mathbf{v}) = \lVert \mathbf{u} - \mathbf{v} \rVert$.
  *   **Orthogonality:**
    *   Two vectors $\mathbf{u}, \mathbf{v}$ are **orthogonal** if $\mathbf{u} \cdot \mathbf{v} = 0$. Notation: $\mathbf{u} \perp \mathbf{v}$.
    *   The zero vector $\mathbf{0}$ is orthogonal to every vector.
    *   **Pythagorean Theorem:** $\mathbf{u} \perp \mathbf{v} \iff \lVert \mathbf{u} + \mathbf{v} \rVert^2 = \lVert \mathbf{u} \rVert^2 + \lVert \mathbf{v} \rVert^2$.
  *   **Orthogonal Complement:**
    *   A vector $\mathbf{z}$ is orthogonal to a subspace $W$ if $\mathbf{z} \cdot \mathbf{w} = 0$ for all $\mathbf{w} \in W$.
    *   The **orthogonal complement** $W^\perp$ is the set of all vectors orthogonal to $W$.
    *   $W^\perp$ is a subspace of $\mathbb{R}^n$.
    *   $\mathbf{z} \in W^\perp \iff \mathbf{z}$ is orthogonal to every vector in a spanning set (or basis) for $W$.
  *   **Row, Column, and Null Spaces:**
    *   $(\text{Row } A)^\perp = \text{Nul } A$
    *   $(\text{Col } A)^\perp = \text{Nul } A^T$
#### **Section 6.2: Orthogonal Sets**
  
  *   **Orthogonal Set:**
    *   A set of vectors $\{\mathbf{u}_1, ..., \mathbf{u}_p\}$ is **orthogonal** if $\mathbf{u}_i \cdot \mathbf{u}_j = 0$ for all $i \neq j$.
  *   **Linear Independence:**
    *   An orthogonal set of **nonzero** vectors is linearly independent.
  *   **Orthogonal Basis:**
    *   A basis for a subspace $W$ that is also an orthogonal set.
  *   **Coordinates in an Orthogonal Basis:**
    *   If $\{\mathbf{u}_1, ..., \mathbf{u}_p\}$ is an orthogonal basis for $W$, then any $\mathbf{y} \in W$ can be written as $\mathbf{y} = c_1\mathbf{u}_1 + ... + c_p\mathbf{u}_p$, where the weights are:
        $c_j = \frac{\mathbf{y} \cdot \mathbf{u}_j}{\mathbf{u}_j \cdot \mathbf{u}_j} = \frac{\mathbf{y} \cdot \mathbf{u}_j}{\lVert \mathbf{u}_j \rVert^2}$.
  *   **Orthogonal Projection onto a Vector:**
    *   Projection of $\mathbf{y}$ onto $\mathbf{u}$: $\hat{\mathbf{y}} = \text{proj}_{\mathbf{u}} \mathbf{y} = \left( \frac{\mathbf{y} \cdot \mathbf{u}}{\mathbf{u} \cdot \mathbf{u}} \right) \mathbf{u}$.
    *   Component of $\mathbf{y}$ orthogonal to $\mathbf{u}$: $\mathbf{z} = \mathbf{y} - \hat{\mathbf{y}}$. (Note: $\mathbf{z} \cdot \mathbf{u} = 0$).
    *   Decomposition: $\mathbf{y} = \hat{\mathbf{y}} + \mathbf{z}$.
  *   **Orthonormal Set:**
    *   An orthogonal set where each vector is a unit vector ($\lVert \mathbf{u}_i \rVert = 1$ for all $i$).
    *   An **orthonormal basis** is a basis that is an orthonormal set.
  *   **Matrices with Orthonormal Columns:**
    *   An $m \times n$ matrix $U$ has orthonormal columns if and only if $U^T U = I_n$.
    *   If $U$ has orthonormal columns:
        *   Preserves lengths: $\lVert U\mathbf{x} \rVert = \lVert \mathbf{x} \rVert$.
        *   Preserves dot products: $(U\mathbf{x}) \cdot (U\mathbf{y}) = \mathbf{x} \cdot \mathbf{y}$.
        *   Preserves orthogonality.
  *   **Orthogonal Matrix:**
    *   A **square** invertible matrix $U$ such that $U^{-1} = U^T$.
    *   This is equivalent to having orthonormal columns (or rows) for a square matrix.
#### **Section 6.3: Orthogonal Projections onto Subspaces**
  
  *   **Orthogonal Decomposition Theorem:**
    *   Let $W$ be a subspace of $\mathbb{R}^n$. Any $\mathbf{y} \in \mathbb{R}^n$ can be written uniquely as $\mathbf{y} = \hat{\mathbf{y}} + \mathbf{z}$, where $\hat{\mathbf{y}} \in W$ and $\mathbf{z} \in W^\perp$.
    *   $\hat{\mathbf{y}}$ is the **orthogonal projection** of $\mathbf{y}$ onto $W$, denoted $\text{proj}_W \mathbf{y}$.
    *   $\mathbf{z} = \mathbf{y} - \hat{\mathbf{y}}$ is the component of $\mathbf{y}$ orthogonal to $W$.
  *   **Calculating the Projection:**
    *   If $\{\mathbf{u}_1, ..., \mathbf{u}_p\}$ is an **orthogonal basis** for $W$:
        $\hat{\mathbf{y}} = \text{proj}_W \mathbf{y} = \left( \frac{\mathbf{y} \cdot \mathbf{u}_1}{\mathbf{u}_1 \cdot \mathbf{u}_1} \right) \mathbf{u}_1 + ... + \left( \frac{\mathbf{y} \cdot \mathbf{u}_p}{\mathbf{u}_p \cdot \mathbf{u}_p} \right) \mathbf{u}_p$.
    *   If $\{\mathbf{u}_1, ..., \mathbf{u}_p\}$ is an **orthonormal basis** for $W$:
        $\hat{\mathbf{y}} = \text{proj}_W \mathbf{y} = (\mathbf{y} \cdot \mathbf{u}_1)\mathbf{u}_1 + ... + (\mathbf{y} \cdot \mathbf{u}_p)\mathbf{u}_p$.
  *   **Projection Matrix (Orthonormal Basis):**
    *   If the columns of $U = [\mathbf{u}_1 \ ... \ \mathbf{u}_p]$ form an orthonormal basis for $W$, then the projection matrix onto $W$ is $UU^T$.
    *   $\text{proj}_W \mathbf{y} = (UU^T) \mathbf{y}$.
    *   Note: $U$ is $n \times p$, $U^T$ is $p \times n$, $UU^T$ is $n \times n$. $U^TU = I_p$.
  *   **Best Approximation Theorem:**
    *   $\hat{\mathbf{y}} = \text{proj}_W \mathbf{y}$ is the vector in $W$ closest to $\mathbf{y}$.
    *   $\lVert \mathbf{y} - \hat{\mathbf{y}} \rVert < \lVert \mathbf{y} - \mathbf{w} \rVert$ for all $\mathbf{w} \in W, \mathbf{w} \neq \hat{\mathbf{y}}$.
    *   The distance from $\mathbf{y}$ to $W$ is $\lVert \mathbf{y} - \hat{\mathbf{y}} \rVert = \lVert \mathbf{z} \rVert$.
#### **Section 6.4: The Gram-Schmidt Process**
  
  *   **Goal:** To construct an orthogonal (or orthonormal) basis for any non-zero subspace $W$ of $\mathbb{R}^n$, starting from any basis $\{\mathbf{x}_1, ..., \mathbf{x}_p\}$ for $W$.
  *   **Process (Orthogonal Basis $\{\mathbf{v}_1, ..., \mathbf{v}_p\}$):**
    *   $\mathbf{v}_1 = \mathbf{x}_1$
    *   $\mathbf{v}_2 = \mathbf{x}_2 - \text{proj}_{\mathbf{v}_1} \mathbf{x}_2 = \mathbf{x}_2 - \frac{\mathbf{x}_2 \cdot \mathbf{v}_1}{\mathbf{v}_1 \cdot \mathbf{v}_1} \mathbf{v}_1$
    *   $\mathbf{v}_3 = \mathbf{x}_3 - \text{proj}_{\mathbf{v}_1} \mathbf{x}_3 - \text{proj}_{\mathbf{v}_2} \mathbf{x}_3 = \mathbf{x}_3 - \frac{\mathbf{x}_3 \cdot \mathbf{v}_1}{\mathbf{v}_1 \cdot \mathbf{v}_1} \mathbf{v}_1 - \frac{\mathbf{x}_3 \cdot \mathbf{v}_2}{\mathbf{v}_2 \cdot \mathbf{v}_2} \mathbf{v}_2$
    *   ...
    *   $\mathbf{v}_k = \mathbf{x}_k - \sum_{j=1}^{k-1} \text{proj}_{\mathbf{v}_j} \mathbf{x}_k = \mathbf{x}_k - \sum_{j=1}^{k-1} \frac{\mathbf{x}_k \cdot \mathbf{v}_j}{\mathbf{v}_j \cdot \mathbf{v}_j} \mathbf{v}_j$
  *   **Property:** $\text{Span}\{\mathbf{v}_1, ..., \mathbf{v}_k\} = \text{Span}\{\mathbf{x}_1, ..., \mathbf{x}_k\}$ for each $k$.
  *   **Orthonormal Basis:** Normalize the resulting orthogonal vectors: $\mathbf{u}_k = \frac{1}{\lVert \mathbf{v}_k \rVert} \mathbf{v}_k$.
  *   **QR Factorization:**
    *   If $A$ is an $m \times n$ matrix with linearly independent columns, then $A = QR$.
    *   $Q$: $m \times n$ matrix whose columns form an orthonormal basis for $\text{Col } A$ (obtained by applying Gram-Schmidt to columns of $A$ and normalizing).
    *   $R$: $n \times n$ upper triangular invertible matrix with positive diagonal entries.
    *   Can compute $R$ as $R = Q^T A$.
  
  ---
### **Chapter 7: Symmetric Matrices and Quadratic Forms**
#### **Section 7.1: Diagonalization of Symmetric Matrices**
  
  *   **Symmetric Matrix:**
    *   $A$ is symmetric if $A^T = A$.
  *   **Properties of Symmetric Matrices:**
    *   Eigenvectors corresponding to distinct eigenvalues are orthogonal.
    *   A matrix is **orthogonally diagonalizable** if there exists an orthogonal matrix $P$ ($P^{-1}=P^T$) and diagonal $D$ such that $A = PDP^T$.
    *   An $n \times n$ matrix is orthogonally diagonalizable if and only if it is symmetric.
  *   **The Spectral Theorem for Symmetric Matrices:** An $n \times n$ symmetric matrix $A$ has:
    *   $n$ real eigenvalues (counting multiplicities).
    *   Eigenspaces corresponding to distinct eigenvalues are orthogonal.
    *   The dimension of each eigenspace equals the algebraic multiplicity of the eigenvalue.
    *   $A$ is orthogonally diagonalizable.
  *   **Orthogonal Diagonalization Process:**
    1.  Find eigenvalues (they will be real).
    2.  Find a basis for each eigenspace.
    3.  Use Gram-Schmidt *within each eigenspace* to get an orthogonal basis for that eigenspace.
    4.  The collection of all these orthogonal bases forms an orthogonal basis of eigenvectors for $\mathbb{R}^n$.
    5.  Normalize all these orthogonal eigenvectors to get an orthonormal basis $\{\mathbf{u}_1, ..., \mathbf{u}_n\}$.
    6.  Form $P = [\mathbf{u}_1 \ ... \ \mathbf{u}_n]$ (orthogonal matrix).
    7.  Form $D$ with corresponding eigenvalues on the diagonal.
    8.  Then $A = PDP^T$.
  *   **Spectral Decomposition:**
    *   If $A = PDP^T$ with $P=[\mathbf{u}_1 \ ... \ \mathbf{u}_n]$ and $D=\text{diag}(\lambda_1, ..., \lambda_n)$.
    *   $A = \lambda_1 \mathbf{u}_1 \mathbf{u}_1^T + \lambda_2 \mathbf{u}_2 \mathbf{u}_2^T + ... + \lambda_n \mathbf{u}_n \mathbf{u}_n^T$.
    *   Each term $\lambda_k \mathbf{u}_k \mathbf{u}_k^T$ is a rank-1 matrix.
    *   Each $\mathbf{u}_k \mathbf{u}_k^T$ is the projection matrix onto the line spanned by $\mathbf{u}_k$.
#### **Section 7.2: Quadratic Forms**
  
  *   **Definition:**
    *   A **quadratic form** is a function $Q: \mathbb{R}^n \to \mathbb{R}$ given by $Q(\mathbf{x}) = \mathbf{x}^T A \mathbf{x}$, where $A$ is an $n \times n$ **symmetric** matrix.
    *   $A$ is the **matrix of the quadratic form**.
  *   **Constructing A from Q(x):**
    *   Diagonal entries $a_{ii}$ are the coefficients of $x_i^2$.
    *   Off-diagonal entries $a_{ij} = a_{ji}$ are **half** the coefficient of the cross-product term $x_i x_j$.
  *   **Change of Variables:**
    *   Let $\mathbf{x} = P\mathbf{y}$ be a change of variable, where $P$ is invertible.
    *   $Q(\mathbf{x}) = \mathbf{x}^T A \mathbf{x} = (P\mathbf{y})^T A (P\mathbf{y}) = \mathbf{y}^T (P^T A P) \mathbf{y}$.
    *   The new quadratic form in $\mathbf{y}$ has matrix $A' = P^T A P$.
  *   **Principal Axes Theorem:**
    *   Let $A$ be an $n \times n$ symmetric matrix. There exists an **orthogonal** change of variable $\mathbf{x} = P\mathbf{y}$ (where $P$ orthogonally diagonalizes $A$, so $P^T A P = D$) that transforms $Q(\mathbf{x}) = \mathbf{x}^T A \mathbf{x}$ into a quadratic form $\mathbf{y}^T D \mathbf{y}$ with **no cross-product term**.
    *   $Q(\mathbf{x}) = \mathbf{y}^T D \mathbf{y} = \lambda_1 y_1^2 + \lambda_2 y_2^2 + ... + \lambda_n y_n^2$.
    *   The columns of $P$ (orthonormal eigenvectors of $A$) are called the **principal axes** of the quadratic form. They define the directions of the axes in the new coordinate system $(\mathbf{y})$ where the form is diagonal.
  
  ---