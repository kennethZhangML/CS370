Consider a $n \times n$ matrix $A$, with a solution vector $x$ and a right-hand side vector $b$.
**Idea**: We want to solve for $Ax = b$. 

Typically, we would use Gaussian elimination, but standard computer techniques referred to as Gaussian Elimination for solving $Ax = b$ are based on factoring $A$ into triangular factors. 
1. Compute Triangular Factors $L$ and $U$ from $A$
2. Solve $Lz = b$
3. Solve $Ux = z$

The goal is to factorize $A$ as $A = LU$, where $L$ is a lower triangular matrix with ones on the diagonal, and $U$ is an upper triangular matrix. 

The goal matrices are: 
$$ L = \begin{bmatrix} 1 & 0 & 0 \\ * & 1 & 0 \\ * & * & 1 \end{bmatrix}, \quad U = \begin{bmatrix} * & * & * \\ 0 & * & * \\ 0 & 0 & * \end{bmatrix}. $$
The original entries of $A$ have been overwritten by $L$ and $U$, that is, the strictly lower part of the modified $A$ array contains the strictly lower part of $L$ (the unit diagonal is understood). The upper triangular part of the $A$ array now contains $U$. 

**e.g., Example $LU$ Factorization on matrix $A$**
Given a matrix $A$: 
$$ A = \begin{bmatrix} 2 & 2 & 3 \\ 5 & 9 & 10 \\ 4 & 1 & 2 \end{bmatrix}. $$
**First Row Operations**
- Replace $R_2$ by $R_2 - k R_1$ where $k = \frac{5}{2}$. 
- Replace $R_3$ by $R_3 - k R_1$ where $k = 2$.
$$ U = \begin{bmatrix} 2 & 2 & 3 \\ 0 & 4 & \frac{5}{2} \\ 0 & -3 & -4 \end{bmatrix}. $$

**Second Row Operations**
- Replace $R_3$ by $R_3 + k R_2$ where $k = -\frac{3}{4}$.
This operation gives us the matrix $U$:
$$
U = \begin{bmatrix}
2 & 2 & 3 \\
0 & 4 & \frac{5}{2} \\
0 & 0 & -\frac{17}{8}
\end{bmatrix}.
$$
Record the $k$-values used in row operations into the $L$-matrix: 
$$ L = \begin{bmatrix} 1 & 0 & 0 \\ \frac{5}{2} & 1 & 0 \\ 2 & -\frac{3}{4} & 1 \end{bmatrix}. $$
The final LU factorization is: $$ A = LU = \begin{bmatrix} 1 & 0 & 0 \\ \frac{5}{2} & 1 & 0 \\ 2 & -\frac{3}{4} & 1 \end{bmatrix} \begin{bmatrix} 2 & 2 & 3 \\ 0 & 4 & \frac{5}{2} \\ 0 & 0 & -\frac{17}{8} \end{bmatrix}. $$
**Note**: No extra hard work or storage is required for factoring $A = LU$ compared to the standard elimination procedure. Now, to solve, the previous equation, we note that:
$$
Ax = LUx = b \implies Lz = b \quad\text{and}\quad Ux = x
$$


#### Procedure for $LU$ Factorization
The goal is to decompose a square matrix $A$ of size $N \times N$ into the product of a lower triangular matrix $L$ and an upper triangular matrix $U$, such that: $$ A = LU, $$ where: 
- $L$ is a lower triangular matrix with $1$'s on the diagonal. 
- $U$ is an upper triangular matrix.

1. **Initialization**: Start with the matrix $A$ and initialize: $$ L = \begin{bmatrix} 1 & 0 & 0 & \cdots & 0 \\ * & 1 & 0 & \cdots & 0 \\ * & * & 1 & \cdots & 0 \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ * & * & * & \cdots & 1 \end{bmatrix}, \quad U = \begin{bmatrix} * & * & * & \cdots & * \\ 0 & * & * & \cdots & * \\ 0 & 0 & * & \cdots & * \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & 0 & \cdots & * \end{bmatrix}. $$
2. 2. **Row Operations**: - For each pivot element $U_{ii}$ (where $i$ is the current row): a) Eliminate all elements below the pivot in column $i$ using row operations. b) Replace row $R_j$ (for $j > i$) with: $$ R_j \to R_j - k R_i, \quad \text{where } k = \frac{U_{ji}}{U_{ii}}. $$Record the elimination factor $k$ in the corresponding entry $L_{ji}$.

3. **Updating $U$ and $L$**: 
- After eliminating all elements below the pivot $U_{ii}$, the updated $U$ will have zeros below the diagonal in column $i$. 
- Record the $k$-values (elimination factors) into the $L$ matrix. 
- Repeat this process for all rows $i = 1, \dots, N-1$.

4. **Final Matrices**: 
- The resulting $L$ matrix contains the elimination factors in the sub-diagonal positions and $1$'s on the diagonal. 
- The resulting $U$ matrix is upper triangular.

For a matrix $A$ of size $N \times N$, the decomposition is: 
$$ 
A = LU \quad \text{where:}\quad L_{ji} = \frac{U_{ji}}{U_{ii}} \quad \text{(for $j > i$)}, \quad U_{ij} = \text{result of row operations}. 
$$


**e.g., Example for Forward Solve**
Suppose we wanted to solve the system $Ax = b$ for the matrix $A$ defined in the previous example, and the vector $b = (7, 4, 6)$. We have: $$ L = \begin{bmatrix} 1 & 0 & 0 \\ -0.3 & 1 & 0 \\ 0.5 & -25 & 1 \end{bmatrix}, \quad U = \begin{bmatrix} 10 & -7 & 0 \\ 0 & -0.1 & 6 \\ 0 & 0 & 155 \end{bmatrix}. $$**Forward solve $(Lz = b)$:** 
$$ \begin{bmatrix} 1 & 0 & 0 \\ -0.3 & 1 & 0 \\ 0.5 & -25 & 1 \end{bmatrix} \begin{bmatrix} z_0 \\ z_1 \\ z_2 \end{bmatrix} = \begin{bmatrix} 7 \\ 4 \\ 6 \end{bmatrix} \implies \begin{cases} z_0 = 7, \\ z_1 = 4 + 0.3(7) = 6.1, \\ z_2 = 6 - 0.5(7) + 25(6.1) = 155. \end{cases} $$


**e.g., Gaussian Elimination as $LU$ Factorization**
We solve the system: 
$$ \begin{bmatrix} 1 & 1 & 1 \\ 3 & 1 & -3 \\ 1 & -2 & -5 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} 1 \\ 5 \\ 10 \end{bmatrix}
$$
Starting with the augmented system: 
$$ \left[ \begin{array}{ccc|c} 1 & 1 & 1 & 1 \\ 3 & 1 & -3 & 5 \\ 1 & -2 & -5 & 10 \end{array} \right] 
$$
$$ R_2 \to R_2 - 3R_1, \quad R_3 \to R_3 - R_1. $$ Resulting in: $$ \left[ \begin{array}{ccc|c} 1 & 1 & 1 & 1 \\ 0 & -2 & -6 & 2 \\ 0 & -3 & -6 & 9 \end{array} \right]. $$$$ R_3 \to R_3 - \frac{3}{2} R_2. $$ Resulting in: $$ \left[ \begin{array}{ccc|c} 1 & 1 & 1 & 1 \\ 0 & -2 & -6 & 2 \\ 0 & 0 & 3 & 6 \end{array} \right]. $$We solve for $x_3$, $x_2$, and $x_1$: 
1. From the third row: $$ 3x_3 = 6 \implies x_3 = 2. $$
2. From the second row: $$ -2x_2 - 6x_3 = 2 \implies -2x_2 - 6(2) = 2 \implies -2x_2 = 14 \implies x_2 = -7. $$
3. From the first row: $$ x_1 + x_2 + x_3 = 1 \implies x_1 + (-7) + 2 = 1 \implies x_1 = 6. $$ Thus, the solution is: $$ x = \begin{bmatrix} 6 \\ -7 \\ 2 \end{bmatrix}. $$$$ 
U = \begin{bmatrix} 1 & 1 & 1 \\ 0 & -2 & -6 \\ 0 & 0 & 3 \end{bmatrix} \quad\text{and}\quad L = \begin{bmatrix} 1 & 0 & 0 \\ 3 & 1 & 0 \\ 1 & \frac{3}{2} & 1 \end{bmatrix}.
$$To verify $A = LU$, multiply $L$ and $U$: 
$$ A = LU = \begin{bmatrix} 1 & 0 & 0 \\ 3 & 1 & 0 \\ 1 & \frac{3}{2} & 1 \end{bmatrix} \begin{bmatrix} 1 & 1 & 1 \\ 0 & -2 & -6 \\ 0 & 0 & 3 \end{bmatrix}. $$


**e.g., Solving with the same matrix but different RHS**
We solve $Ax = b$ with: $$ b = \begin{bmatrix} 2 \\ -4 \\ -7 \end{bmatrix} $$ without refactoring $A$. We solve for $z$ using the lower triangular matrix $L$: $$ Lz = b \quad \text{where} \quad L = \begin{bmatrix} 1 & 0 & 0 \\ 3 & 1 & 0 \\ 1 & \frac{3}{2} & 1 \end{bmatrix}, \quad b = \begin{bmatrix} 2 \\ -4 \\ -7 \end{bmatrix}. $$ The system becomes:
$$
\begin{aligned} z_1 &= 2, \\ 3z_1 + z_2 &= -4 \implies 3(2) + z_2 = -4 \implies z_2 = -10, \\ z_1 + \frac{3}{2}z_2 + z_3 &= -7 \implies 2 + \frac{3}{2}(-10) + z_3 = -7 \implies z_3 = 6. \end{aligned}
$$ Thus, $z$ is: $$ z = \begin{bmatrix} 2 \\ -10 \\ 6 \end{bmatrix}. $$ We now solve $Ux = z$ using the upper triangular matrix $U$: $$ Ux = z \quad \text{where} \quad U = \begin{bmatrix} 1 & 1 & 1 \\ 0 & -2 & -6 \\ 0 & 0 & 3 \end{bmatrix}, \quad z = \begin{bmatrix} 2 \\ -10 \\ 6 \end{bmatrix}. $$$$
\begin{aligned} 3x_3 &= 6 \implies x_3 = 2, \\ -2x_2 - 6x_3 &= -10 \implies -2x_2 - 6(2) = -10 \implies x_2 = -1, \\ x_1 + x_2 + x_3 &= 2 \implies x_1 + (-1) + 2 = 2 \implies x_1 = 1. \end{aligned}$$Thus, the solution $x$ is: $$ x = \begin{bmatrix} 1 \\ -1 \\ 2 \end{bmatrix}. $$

#### Gaussian Elimination and Matrix Factorization
After reduction, the result is a new system with:
- subsystem of $n-1$ equations in unknowns $x_2, x_3, \dots, x_n$ 
- a single equation involve $x_1, x_2, \dots, x_n$. 

Thus the new solution of the new and old systems are the same. The next step of reduction, uses the first step recursively, which, after $k$ invocations of the recursion of the reduction process, we have:
- A subsystem of $n - k$ equations in unknowns $(x_{k + 1}, \dots, x_n)$ 
- A subsystem of $k$ equations where the first equation involves the unknowns $x_1, x_2, \dots, x_n$ where the $j = 2, ..., k$ equation does not involve unknowns $x_1, \dots, x_{j - 1}$

If we do row reduction via the reduction process describes above, we are given the terms of the matrix multiplications which can be expressed as matrix multiplication using an $n \times n$ matrix $M^{(1)}$ as follows:
$$
M^{(1)} A^{(1)} = A^{(2)}
$$
The $k$-th step of the row reduction process can be expressed as: $$ M^{(k)} A^{(k)} = A^{(k+1)} \quad (6.1.4) $$ where $M^{(k)}$ is an $n \times n$ matrix with the following pattern:
$$ \mathbf{M}^{(k)} = \begin{bmatrix} 1 & & & & \text{zeros} \\ 1 & 1 & & & \\ x & x & 1 & & \\ x & x & x & 1 & \\ \text{zeros} & x & x & x & 1 \end{bmatrix}. $$
The matrix $M^{(k)}$ has **ones** on the diagonal and eliminates entries below the pivot element in **column $k$**. The $k$-th reduction step can be broken into two parts: 

**Reduction of the Matrix**: 
$$ M^{(k)} A^{(k)} = A^{(k+1)}. $$
**Reduction of the Vector: $$ M^{(k)} b^{(k)} = b^{(k+1)}. \quad (6.1.5) $$$$ A^{(k)} = (M^{(k)})^{-1} A^{(k+1)}. \quad (6.1.6) $$$$ b^{(k)} = (M^{(k)})^{-1} b^{(k+1)}. \quad (6.1.7) $$
At step $k = n-1$, the recursive relationship becomes: $$ A^{(n-1)} = (M^{(n-1)})^{-1} A^{(n)}. $$ By applying multiple steps, we get: $$ A^{(n-k)} = (M^{(n-k)})^{-1} \cdots (M^{(n-1)})^{-1} A^{(n)}. \quad (6.1.8) $$
1. Each $(M^{(n-j)})^{-1}$ is **lower triangular** and **unit diagonal**. 
2. The product of such matrices is also **lower triangular** and **unit diagonal**.

Let: 
- $L$ be the product of the inverse matrices, 
- $A^{(n)} = U$ as the resulting upper triangular matrix.
Thus, the matrix $A$ can be factored as: 
$$ A^{(1)} = LU. \quad (6.1.9) $$

e.g., We are asked to factor:
$$
A = \begin{bmatrix}
1 & 1 & 1 \\ 
1 & -2 & 2\\
1 & 2 & -1
\end{bmatrix}
$$
$R_2 : R_2 - (1) R_1$
$$
(M^{(1)})^{-1} = \begin{bmatrix}
1 & 0 & 0 \\ 
1 & 1  & 0\\
0 & 0 & 1
\end{bmatrix}
\implies M^{(1)} \cdot A
\begin{bmatrix}
1 & 0 & 0 \\ 
-1 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 1 & 1\\
1 & -2 & 2\\
1&2&-1
\end{bmatrix}
=\begin{bmatrix}
1 & 1 & 1 \\ 
0 & -3 & 1\\
1 & 2 & -1
\end{bmatrix}
$$
$R_3 : R_3 - (1) R_1$
$$
(M^{(2)})^{-1} = \begin{bmatrix}
1 & 0 & 0\\
0 & 1 & 0\\
1 & 0 & 1
\end{bmatrix} 
\implies 
M^{(2)} \cdot (M^{(1)} \cdot A) = \begin{bmatrix}
1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & 0 & 1
\end{bmatrix} 
\begin{bmatrix}
1 & 1 & 1\\
0 & -3 & 1 \\ 
1 & 2 & -1
\end{bmatrix}
= \begin{bmatrix}
1 & 1 & 1 \\
0 & -3 & 1\\
0 & 1 & -2
\end{bmatrix}
$$
$R_3 = R_3 - (-1 / 3)R_2$
$$
(M^{(3)})^{-1} = \begin{bmatrix}
1 & 0 & 0\\
0 & 1 & 0\\
0 & -1/3 & 1
\end{bmatrix}
\implies 
M^{(3)} \cdot (M^{(2)} \cdot M^{(2)} \cdot M^{(1)} \cdot A) = 
\begin{bmatrix}
1 & 0 & 0\\
0 & 1 & 0\\
0 & 1/3 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 1 & 1 \\ 
0 & -3 & 1\\
0 & 1 & -2
\end{bmatrix}
$$
$$
= 
\begin{bmatrix}
1 & 1 & 1 \\
0 & -3 & 1\\
0 & 0 & -5/3
\end{bmatrix}
$$
Since $A = LU$ then we can write $L$ as:
$$
L = (M^{(1)})^{-1} (M^{(2)})^{-1} (M^{(3)})^{-1} = 
\begin{bmatrix}
1 & 0 & 0 \\
1 & 1 & 0 \\
1 & -1/3 & 1
\end{bmatrix}
$$


#### Cost of Matrix Factorization
The running time of the above algorithms will be proportional to the number of flops. Since most statements consist of linked tried operations, then we can usually take a multiplication operation that is paired with an addition or subtraction operation. So, often a measure of work is to simply count the number of multiply-adds. Usually, the total number of multiply-adds is about one-half the total number of additions + subtractions + multiplies + divides. 

The **flop count** will include the total number of adds + multiplies + divides + subtracts, where we have a simple calculation that gives the total number of flops for factoring an $n \times n$ matrix:
$$
\begin{align*} \sum_{k=1}^n \sum_{i=k+1}^n \left( 1 + \sum_{j=k+1}^n 2 \right) &= \sum_{k=1}^n \sum_{i=k+1}^n \left( 1 + 2 \left( n - (k+1) + 1 \right) \right) \\ &= \sum_{k=1}^n \sum_{i=k+1}^n \left( 2n - 2k + 1 \right) \\ &= \sum_{k=1}^n (n-k)(2n - 2k + 1) \\ &= \sum_{k=1}^n \left( 2k^2 + k(-4n-1) + (2n^2 + n) \right) \\ &= 2 \frac{n(n+1)(2n+1)}{6} + (-4n-1)\frac{n(n+1)}{2} + n(2n^2 + n). \end{align*}
$$
**Identities**:
$$
\boxed{
\sum_{k=1}^n k = \frac{n(n+1)}{2}, \quad \sum_{k=1}^n k^2 = \frac{n(n+1)(2n+1)}{6}.
}
$$
$$
\begin{align*} \text{Substitute into the equation:} \quad &= \frac{2n^3}{3} + n^2 + n - \frac{2n^3}{3} - \frac{5n^2}{2} - \frac{n}{2} + 2n^3 + n^2 \\ &= \frac{2n^3}{3} - \frac{n^2}{2} - \frac{n}{6}. \end{align*}
$$
$$
\text{Final FLOP Count:} \quad \frac{2n^3}{3} + \mathcal{O}(n^2) \quad \text{FLOPs}.
$$
Thus, the number of FLOPs (Floating Point Operations) needed are:
$$
\text{LU Factor Work} = \frac{2n^3}{3} + O(n^3) \text{ flops} = O(n^3) \text{ flops}
$$
Moreover the flop count for a forward or backward solve is given by:
$$
\begin{align*} \sum_{i=1}^n \left( 1 + \sum_{j=i+1}^n 2 \right) &= \sum_{i=1}^n 1 + 2 \sum_{i=1}^n \left( n - (i+1) + 1 \right) \\ &= \sum_{i=1}^n 1 + 2 \left[ \sum_{i=1}^n n - \sum_{i=1}^n i \right]. \end{align*}
$$
$$
\begin{align*} \sum_{i=1}^n \left( 1 + \sum_{j=i+1}^n 2 \right) &= n + 2 \left[ \sum_{i=1}^n n - \sum_{i=1}^n i \right] \\ &= n + 2n^2 - 2 \sum_{i=1}^n i \\ &= n + 2n^2 - 2 \frac{n(n+1)}{2}. \end{align*}
$$
$$
\begin{align*} \sum_{i=1}^n \left( 1 + \sum_{j=i+1}^n 2 \right) &= n + 2n^2 - n(n+1) \\ &= n^2 \quad \text{FLOPs}. \end{align*}
$$


#### Norms and Conditioning 
Norms are measurements of size/magnitude for vectors or matrices. That is, the norm of a vector if the measure of its size. Let $x$ be a vector with components:
$$
x = \begin{bmatrix}
x_1 \\ x_2 \\ \vdots \\ x_n
\end{bmatrix}
$$
And we define the following:
$$
||x||_1 = \sum_{i = 1}^n |x_i| \quad ||x||_2 = \left(\sum_{i = 1}^n x_i^2 \right)^{1/2} \quad ||x||_\infty = \text{max}_i |x_i|
$$
Collectively, we call the above **p-norms** for $p = 1, 2, \infty$ and can be written:
$$
||x||_p = \left(\sum_{i = 1}^n |x_i|^p \right)^{1/p}
$$

**Properties of Norms**
If the norm is zero, then the vector must be the zero vector:
$$
||x|| = 0 \implies x_i = 0 \forall i
$$
The norm of a scaled vector must satisfy:
$$
||\alpha x|| = |\alpha| \cdot ||x|| \quad\text{for scalar } \alpha
$$
The triangle inequality holds:
$$
||x + y|| \le ||x|| + ||y||
$$

**Matrix Norms**
Similar to vector norms, we can define a matrix norm for an $n \times n$ matrix $A$, where we say that the matrix norm is induced using p-norms of vectors:
$$
||A|| = \text{max}_{||x|| \neq 0} \frac{||Ax||}{||x||} = \text{max}_{||x|| = 1} ||Ax||
$$
for any $|| ||_p$ norm. Given that the matrix $p$-norm is often difficult to use directly, we can define simpler equivalent definitions for some cases:
1. Max Absolute Column Sum 
$$
||A||_1 = \text{max}_j \sum_{i = 1}^n |a_{ij}| = \text{max absolute column sum}
$$
2. Max Absolute Row Sum
$$
||A||_\infty = \text{max}_j \sum_{j = 0}^n |a_{ij}| = \text{max absolute row sum}
$$

**Matrix 2-norm (Spectral Norm)**
The matrix 2-norm is also called the spectral norm. It is not as straightforward to determine, since it requires computing eigenvalues. That is, for the $|| \text{ }||_2$ norm, we will have to consider the eigenvalues of $A$. Recall that an eigenvalue $\lambda$ is a scalar associated with a non-zero vector $x$ if:
$$
Ax = \lambda x
$$
where the vector $x$ is the eigenvector associated with the eigenvalue of $\lambda$. If $\lambda_i$ for $i = 1, \dots, n$ are the eigenvalues of $A^T A$, then we have that:
$$
||A||_2 = \text{max}_{||x|| \neq 0} \frac{||A x||_2}{||x||_2} = \text{max}_i |\lambda_i|^{1/2} = \text{max}_i \sqrt{|\lambda_i|}
$$
$$
||A||_2 = \sqrt{\text{max magnitude eigenvalue of } A^TA}
$$
**Properties of Matrix Norms**
$$
\begin{align*}
||A|| &= 0 \iff A_{ij} = 0 \, \forall i, j. \\
||\alpha A|| &= |\alpha| \cdot ||A|| \quad \text{for scalar } \alpha \\
||A + B|| &\leq ||A|| + ||B|| \\
||Ax|| &\leq ||A|| \cdot ||x|| \\
||AB|| &\leq ||A|| \cdot ||B|| \\
||I|| &= 1
\end{align*}
$$
**Theorem: Equivalence of Norms**
There exist constants $C_1, C_2$ such that:
$$
C_1 ||x||_a \le ||x||_b \le C_2||x||_a
$$
for all $x$, where $||\cdot||_a$ and $||\cdot||_b$ are two different norms.


#### Conditioning of Matrices
Let's say we want to perturb the right-hand side vector $b$ in $Ax = b$. What happens to $x$? Let's replace $b$ by $b + \Delta b$ which means that $x$ will change to $x + \Delta x$ which gives that:
$$
A(x + \Delta x) = b + \Delta b
$$
If $x$ is the exact solution, then $Ax = b$ so that the equation above becomes:
$$
\Delta x = A^{-1} \Delta b
$$
Note that $Ax = b$ gives that $||b|| \le ||A|| ||x||$ so that we get:
$$
||\Delta x|| \le ||A|| ||x||
$$
which now gives us:
$$
||x|| \ge \frac{||b||}{||A||} \quad\text{or}\quad \frac{||A||}{||b||}
 \ge \frac{1}{||x||}
$$
which gives us:
$$
\frac{||\Delta x||}{||x||} \le ||A|| ||A^{-1}|| \frac{||\Delta b||}{||b||}
$$

Thus from the equation above, we get the condition number of a matrix $A$ is denoted:
$$
\kappa(A) = ||A|| \cdot ||A^{-1}||
$$
- $\kappa \approx 1 \implies A$ is well-conditioned
- $\kappa >> 1 \implies A$ is ill-conditioned

which says that a relative change in $x$ is just the condition number times the relative change in $b$. Moreover, $\kappa$ depends on the norm, since different norms will give different $x$. 

For a system $Ax = b$, then $\kappa(A)$ provides upper bounds on relative change in $x$ due to relative change in $b$, then we have bounds:
$$
\frac{||\Delta x||}{||x||} \le \kappa(A) \frac{||\Delta b||}{||b||} \quad\text{and}\quad \frac{||\Delta x||}{||x + \Delta x||} \le \kappa(A)\frac{||\Delta A||}{||A||}
$$
Now, suppose we want to perturb the matrix elements in $A$:
$$
\begin{equation} (A + \Delta A)(x + \Delta x) = b, \tag{6.2.9} \end{equation}
$$
assuming that $Ax = b$ and that $x$ is the exact solution, then we get:
$$
\begin{align} A \Delta x &= -\Delta A (x + \Delta x) \notag \\ \Delta x &= -A^{-1} \left[ \Delta A (x + \Delta x) \right], \tag{6.2.10} \end{align}
$$
$$
\begin{equation} \|\Delta x\| \leq \|A^{-1}\| \|\Delta A\| \|x + \Delta x\|, \tag{6.2.11} \end{equation}
$$
$$
\begin{align} \frac{\|\Delta x\|}{\|x + \Delta x\|} &\leq \|A^{-1}\| \|A\| \frac{\|\Delta A\|}{\|A\|} \notag \\ &= \kappa(A) \frac{\|\Delta A\|}{\|A\|}. \tag{6.2.12} \end{align}
$$
which one again the condition number appears. So a measure of sensitivity to changes in $A$ or $b$ is the condition number, which gives us the properties:
- $\kappa(A) \geq 1$
- $\kappa(\alpha A) = \kappa(A); \, \alpha = \text{scalar}$

**Residual**
The accuracy of the computed solution is sometimes measured by the size of the residuals, i.e., where we let the computed solution to $Ax = b$ be the $x + \Delta x$ (it's not exact). We can let:
$$
r = b - A(x + \Delta x)
$$
If we let $r = 0$ then $\Delta x = 0$ and we have the exact solution. 
$$
A(x + \Delta x) = b - r
$$
We can apply our bound using $\Delta b = r$ to get:
$$
\frac{|| \Delta x||}{||x||} \le \kappa(A) \frac{||r||}{||b||}
$$
Thus a small residual does not necessarily imply a small error if $\kappa(A)$ is large. How can we interpret this error. Well, the solution's relative error $\frac{||\Delta x||}{||x||}$ is bounded by the condition number times the relative size of the residual $r$ with respect to the right hand side $b$.

If we roughly know $A$'s condition number, the computed residual indicates how large the error might be (at it's worst). If $\kappa \approx 1$, a small residual indicates a small relative error. But, if $\kappa$ is large, residual could still be small while error is quite large.
$$
\frac{||x - \hat{x}||}{||\hat{x}||} \le \kappa(A) \frac{(||A|| \epsilon_{\text{machine}})}{||A||} \le \kappa(A) \epsilon_{\text{machine}}
$$
$\kappa(A) = 10^{10}$ and $\epsilon_{\text{machine}} = 10^{-16}$ with relative error in $x$ is around $10^{10 + (-16)} = 10^{-6}$ or around 6 significant digits of accuracy. 

**Note:** Conditioning is a property of the problem itself - not a property of a particular algorithm, i.e., A system $Ax = b$ well- or ill-conditioned, independent, of how we choose to solve it. Even an "ideal" numerical algorithm can't necessarily guarantee a solution with small error if $k >> 1$ (very large).




### Exercises 
1. True or false: If $x$ is any $n$-vector, then $\|x\|_1 \geq \|x\|_\infty$.
	**Solution: True**, notice that for any $x = (x_1, x_2, \dots, x_n)$ the maximum absolute value $||x||_\infty$ corresponds to one component $|x_k|$ such that $|x_k| \le ||x||_\infty$ for all $k$. Since $||x||_1$ is the sum of all components' absolute values, then we can write:
	$$
	||x||_1 = |x_1| + |x_2| + \dots + |x_n|
	$$
	So for each $|x_i|$ it follows that $|x_i| \le ||x||_\infty$ by definition. Therefore summing over the components will give us:
	$$
	||x||_1 = \sum_{i = 1}^{n}|x_i| \le \sum_{i = 1}^n ||x||_\infty = n \cdot ||x||_\infty
	$$
2. True or false: If $\|A\| = 0$, then $A = 0$.
	**Solution:** **False**, for any matrix $||A||$ the property of positivity holds:
	- $||A|| \ge 0$ for all matrices $A$
	- $||A|| = 0$ if and only if $A = 0$ (the 0 matrix)
	Thus if $||A|| = 0$ it follows that all elements $a_{ij} = 0$ and $A$ is the zero matrix. 


3. Derive the algorithm to compute the $LU$ decomposition of a matrix $A$, namely,
$$
A = LU
$$
where $U$ and $L$ are the upper and lower triangular matrices, respectively. We assume that no zero pivots will occur in this case, so no pivoting strategy is needed.
**Solution**: Given a square matrix $A \in \mathbb{R}^{n \times n}$, we aim to find matrices $L$ and $U$ such that:
- $L$ is a lower triangular matrix with ones on the diagonal ($l_{ii} = 1$ for all $i$).
- $U$ is an upper triangular matrix.
$$
A = \begin{bmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{bmatrix}
$$Define 
$$
L = \begin{bmatrix} 1 & 0 & \cdots & 0 \\ l_{21} & 1 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ l_{n1} & l_{n2} & \cdots & 1 \end{bmatrix} \text{ and } 
U = \begin{bmatrix} u_{11} & u_{12} & \cdots & u_{1n} \\ 0 & u_{22} & \cdots & u_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & u_{nn} \end{bmatrix}
$$
**Algorithm**: For $k = 1, 2, \dots, n$:
- Compute the elements of $U$ for the $k$-th row
- Compute the elements of $L$ for the $k$-th column

**Boundary Conditions**:
- For $k = 1$, no summations are performed.
- The diagonal entries of $L$ are $l_{kk} = 1$ by definition.



4. Assume that you are given an $LU$ factorization of an $n \times n$ matrix $A$. Show how to use this to solve:
$$
A^2 \vec{x} = \vec{b}
$$
with computational complexity $O(n^2)$.
**Solution**: To solve the equation:
$$
A^2 \vec{x} = \vec{b},
$$
using the $LU$ factorization of $A$, where $A = LU$, we proceed as follows:
1. Using the given $LU$ factorization of $A$:$$
   A^2 = (LU)(LU).
   $$
   Substituting this into the equation, we have:$$
   (LU)(LU)\vec{x} = \vec{b}.
   $$
   2. Group the terms:$$
   LU(LU\vec{x}) = \vec{b}.
   $$
   Let $\vec{y} = LU\vec{x}$. Then the equation becomes:
   $$
   LU\vec{y} = \vec{b}.
   $$
   The equation $LU\vec{y} = \vec{b}$ can be solved in two steps:
1. Solve the lower triangular system $L\vec{z} = \vec{b}$ for $\vec{z}$ using forward substitution.
2. Solve the upper triangular system $U\vec{y} = \vec{z}$ for $\vec{y}$ using backward substitution.

	Once $\vec{y}$ is determined, solve the equation $LU\vec{x} = \vec{y}$ similarly:
1. Solve the lower triangular system $L\vec{w} = \vec{y}$ for $\vec{w}$ using forward substitution.
2. Solve the upper triangular system $U\vec{x} = \vec{w}$ for $\vec{x}$ using backward substitution.

1. Each forward or backward substitution step involves solving a triangular system, which has computational complexity $O(n^2)$.
2. There are four triangular systems to solve:
   - $L\vec{z} = \vec{b}$,
   - $U\vec{y} = \vec{z}$,
   - $L\vec{w} = \vec{y}$,
   - $U\vec{x} = \vec{w}$.

Thus, the total computational complexity is:
$$
O(n^2).
$$
The solution to $A^2\vec{x} = \vec{b}$ is obtained by:
1. Solving $L\vec{z} = \vec{b}$ for $\vec{z}$.
2. Solving $U\vec{y} = \vec{z}$ for $\vec{y}$.
3. Solving $L\vec{w} = \vec{y}$ for $\vec{w}$.
4. Solving $U\vec{x} = \vec{w}$ for $\vec{x}$.

3. Assume that you are given the decomposition $A = LU$ where $A$ is an $n \times n$ matrix. Describe how you can use this decomposition to solve the system $A^T \vec{x} = \vec{b}$ with computational complexity $O(n^2)$.
	**Solution**: To solve the system:
$$
A^T \vec{x} = \vec{b},
$$
	given the decomposition $A = LU$, we proceed as follows: Given $A = LU$, the transpose of $A$ can be written as:
$$
A^T = (LU)^T = U^T L^T.
$$
	Thus, the equation becomes:
$$
U^T L^T \vec{x} = \vec{b}.
$$
	Let $\vec{y} = L^T \vec{x}$. Then:
$$
U^T \vec{y} = \vec{b}.
$$
1. Solve the upper triangular system $U^T \vec{y} = \vec{b}$ for $\vec{y}$ using **backward substitution**.
2. Solve the lower triangular system $L^T \vec{x} = \vec{y}$ for $\vec{x}$ using **forward substitution**.
3. Solving $U^T \vec{y} = \vec{b}$ involves backward substitution, which has complexity $O(n^2)$.
4. Solving $L^T \vec{x} = \vec{y}$ involves forward substitution, which also has complexity $O(n^2)$.

Thus, the total computational complexity is:
$$
O(n^2).
$$


4. Classify each of the following matrices as well-conditioned or ill-conditioned, and provide a brief explanation for your answer in each case.$$
   \begin{bmatrix} 10^{10} & 10^{-10} \\ 10^{-10} & 10^{10} \end{bmatrix}, \quad \begin{bmatrix} 10^{10} & 10^{10} \\ 10^{10} & 10^{10} \end{bmatrix}, \quad \begin{bmatrix} 10^{-10} & 10^{-10} \\ 10^{-10} & 10^{-10} \end{bmatrix}
   $$
   **Solution:**$$
A_1 = \begin{bmatrix} 10^{10} & 10^{-10} \\ 10^{-10} & 10^{10} \end{bmatrix}
	$$The diagonal elements are large, and the off-diagonal elements are small. This matrix is nearly diagonal and has eigenvalues close to $10^{10}$, which are well-separated. The singular values are approximately equal, resulting in a condition number close to 1.  $A_1$ is **well-conditioned**.
$$
A_2 = \begin{bmatrix} 10^{10} & 10^{10} \\ 10^{10} & 10^{10} \end{bmatrix}
$$
	This matrix is rank-deficient, as its rows and columns are linearly dependent. All eigenvalues except one are zero. The condition number is very high (effectively infinite due to singularity).  $A_2$ is **ill-conditioned** (or singular).
$$
A_3 = \begin{bmatrix} 10^{-10} & 10^{-10} \\ 10^{-10} & 10^{-10} \end{bmatrix}
$$
	  This matrix is also rank-deficient because all rows and columns are linearly dependent. The condition number is very high (again, effectively infinite due to singularity). Additionally, the entries are extremely small, amplifying numerical issues.  $A_3$ is **ill-conditioned** (or singular).


   
   7. Show that the following two definitions of condition number are equivalent:
    $$
    \kappa(A) = \|A\|_2 \|A^{-1}\|_2; \quad \kappa(A) = \frac{\max_x \frac{\|Ax\|_2}{\|x\|_2}}{\min_x \frac{\|Ax\|_2}{\|x\|_2}}
    $$
    **Solution**: We aim to show the equivalence of the following two definitions of the condition number:
$$
\kappa(A) = \|A\|_2 \|A^{-1}\|_2; \quad \kappa(A) = \frac{\max_x \frac{\|Ax\|_2}{\|x\|_2}}{\min_x \frac{\|Ax\|_2}{\|x\|_2}}.
$$
The condition number is defined as:
$$
\kappa(A) = \|A\|_2 \|A^{-1}\|_2,
$$
where $\|A\|_2$ is the spectral norm (largest singular value of $A$), and $\|A^{-1}\|_2$ is the spectral norm of $A^{-1}$ (inverse of the smallest singular value of $A$).

The condition number can also be expressed as:
$$
\kappa(A) = \frac{\max_x \frac{\|Ax\|_2}{\|x\|_2}}{\min_x \frac{\|Ax\|_2}{\|x\|_2}}.
$$
- $\max_x \frac{\|Ax\|_2}{\|x\|_2}$ corresponds to the largest singular value of $A$.
- $\min_x \frac{\|Ax\|_2}{\|x\|_2}$ corresponds to the smallest singular value of $A$.

Thus:
$$
\kappa(A) = \frac{\sigma_{\max}(A)}{\sigma_{\min}(A)}.
$$
Since $\|A\|_2 = \sigma_{\max}(A)$ and $\|A^{-1}\|_2 = \frac{1}{\sigma_{\min}(A)}$, the two definitions are equivalent:
$$
\kappa(A) = \|A\|_2 \|A^{-1}\|_2 = \frac{\sigma_{\max}(A)}{\sigma_{\min}(A)}.
$$
    
   8. The $LU$ decomposition for the following famous $n \times n$ tridiagonal matrix:
$$
\begin{bmatrix} 2 & -1 & 0 & \cdots & 0 & 0 \\ -1 & 2 & -1 & 0 & 0 & 0 \\ 0 & -1 & 2 & -1 & 0 & 0 \\ \vdots & \vdots & \vdots & \ddots & \vdots & \vdots \\ 0 & 0 & 0 & -1 & 2 & -1 \\ 0 & 0 & 0 & 0 & -1 & 2 \end{bmatrix}_{n \times n}
$$
	has a very simple form $A = LU$ where:$$
	L = \begin{bmatrix} 1 & 0 & \cdots & 0 & 0 \\ l_2 & 1 & 0 & 0 & 0 \\ 0 & l_3 & 1 & 0 & 0 \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ 0 & 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & l_n & 1 \end{bmatrix}, \quad U = \begin{bmatrix} u_1 & -1 & \cdots & 0 & 0 \\ 0 & u_2 & -1 & 0 & 0 \\ 0 & 0 & u_3 & -1 & 0 \\ 0 & 0 & 0 & \ddots & \vdots \\ 0 & 0 & 0 & u_{n-1} & -1 \\ 0 & 0 & 0 & 0 & u_n \end{bmatrix}.
	$$
	Derive the recurrence relations for $l_i$ and $u_i$. If $n$ is large, show that $u_i$ monotonically decreases and approaches a limit. Provide a better estimate of this limit. What can you say about the sequence $l_i$?





