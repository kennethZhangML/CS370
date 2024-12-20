We often have a discrete finite set of data $(x_1, y_1), ..., (x_n, y_n)$ which describes the behaviour of some (unknown) function $y = p(x)$ and one wishes to determine $p(x)$ or at least some approximation to $p(x)$. 

Our approximate function $p(x)$ has the property that it **interpolates** the data points:
$$
p(x_1) = y_1, ..., p(x_n) = y_n
$$
We will use the example $(0, 1), (1, 2), (2, 0), \text{ and } (3, 3)$ for interpolation throughout these notes.

**Uniqueness**: In general, an interpolation function $p(x)$ for a given dataset is not unique. There are **infinitely** many functions that will pass through any given dataset. Therefore, we restrict ourselves to a few types of interpolation strategies. 

#### Polynomial Interpolation 
Given $n$ data pairs $(x_i, y_i)$ where $i = 1, ..., n$ distinct $x_i$, there is a unique polynomial $p(x)$ of degree not exceeding $n - 1$ that interpolates the data (prove this using unisolvence). 

##### Linear Equations: Vandermonde Systems 
Polynomials of degree $n - 1$ or less are commonly represented int he form:
$$
p(x) = c_1 + c_2 x + \cdots + c_n x^{n - 1}
$$
which uses $n$ coefficients $c_1, ..., c_n$. 

e.g., using our data pairs $(0, 1), (1, 2), (2, 0), \text{ and } (3, 3)$, we can set up a linear system and solve the linear systems of equations:
$$
\begin{align*}
p(0) &= 1 \implies c_1 &= 1\\
p(1) &= 2 \implies c_1 + c_2 + c_3 + c_4 &= 2\\
p(2) &= 0 \implies c_1 + 2c_3 + 4c_3 + 8c_4 &= 0\\
p(3) &= 3 \implies c_1 + 3c_2 + 9c_3 + 27c_4 &= 3
\end{align*}
$$
In general, if we had a set of data $(x_1, y_2), ..., (x_n, y_n)$ and want a polynomial of the form, $p(x) = c_1 + c_2 x + \cdots + c_n x^{n - 1}$ then we can set up $V \cdot \vec{c} = \vec{y}$ where:

1. The first column of $V$ is all ones, followed by $(x_1 ... x_1^{n - 1}) ... (x_n ... x_n^{n - 1})$ for the rows to the right of rows in the leftmost one's column. 
2. $\vec{c} = \begin{bmatrix} c_1 \\ . \\ . \\ . \\ c_n \end{bmatrix}$   and $\vec{y} = \begin{bmatrix} y_1 \\ . \\ . \\ . \\ y_n \end{bmatrix}$

Matrices of the form $V$ are called Vandermonde matrices with all of the data required for creating one is contained in its second column. 


##### Lagrange Form 
Given a set of data $(x_i, y_i)$ $i = 1, ..., n$ consider a set of $n$ Lagrange basis functions $L_k(x)$ $k = 1, ..., n$ defined as:
$$
L_k(x) = \frac{(x - x_1) \cdots (x - x_{k - 1})(x - x_{k + 1}) \cdots (x - x_n)}{(x_k - x_1) \cdots (x_k - x_{k -1})(x_k - x_{k + 1}) \cdots (x_k - x_n)}
$$
These polynomials have the property that $L_i(x_j) = 1$ if $i = j$ and $L_i(x_j) = 0$ for all other $j$. 

Let:
$$
p(x) = y_1 L_1(x) + \cdots + y_i L_i(x) + \cdots + y_n L_n(x)
$$
Then $p(x)$ is a polynomial of degree $n - 1$ which interpolates the $n$ points since for each $i$ we have the following:
$$
\begin{align*}
p(x_i) &= y_1 L_1(x) + \cdots + y_i L_i(x_i) + \cdots + y_n L_n(x_i)\\
&= y_1 \cdot 0 + \cdots + y_i \cdot 1 + \cdots + y_n \cdot 0\\
&= y_i
\end{align*}
$$
$(0, 1), (1, 2), (2, 0), \text{ and } (3, 3)$ then we can get:
$$
\begin{align*}
	(x_0, y_0) &= (0, 1)\\
	(x_1, y_1) &= (1, 2)\\
	(x_2, y_2) &= (2, 0)\\
	(x_3, y_3) &= (3, 3)
\end{align*}
$$
$$
\begin{align*}
L_0(x) &= \frac{(x - x_1)(x - x_2)(x - x_3)}{(x_0 - x_1)(x_0 - x_2)(x_0 - x_3)} = \frac{(x - 1)(x - 2)(x - 3)}{(0 - 1)(0 - 2)(0 - 3)} = \frac{(x-1)(x-2)(x-3)}{-6}\\
L_1(x) &= \frac{(x - x_0)(x - x_2)(x - x_3)}{(x_1 - x_0)(x_1 - x_2)(x_1 - x_3)} = \frac{(x - 0)(x - 2)(x - 3)}{(1 - 0)(1 - 2)(1 - 3)} = \frac{x(x - 2)(x - 3)}{2}\\
L_2(x) &= \frac{(x - x_0)(x - x_1)(x - x_3)}{(x_2 - x_0)(x_2 - x_1)(x_2 - x_3)} = \frac{(x - 0)(x - 1)(x - 3)}{(2 - 0)(2 - 1)(2 - 3)} = \frac{x(x - 1)(x - 3)}{-2}\\
L_3(x) &= \frac{(x - x_0)(x - x_1)(x - x_2)}{(x_3 - x_0)(x_3 - x_1)(x_3 - x_2)} = \frac{(x - 0)(x - 1)(x - 2)}{(3 - 0)(3 - 1)(3 - 2)} = \frac{x(x-1)(x-2)}{6}
\end{align*}
$$
Thus we have that:
$$
p(x) = L_1(x) + 2L_2(x) + 0 \cdot L_3(x) + 3 \cdot L_4(x)
$$


##### Piecewise Polynomial Interpolation
Assume that we have $n$ points $(x_1, y_1) ... (x_n, y_n)$ with $x_1 < x_2 < \cdots < x_n$. Then a piecewise interpolating polynomial for these $n$ points is a function $p(x)$ satisfying:
- A function of $x$ for $x_1 \le x \le x_n$ 
- An interpolant $p(x_i) = y_i$
- A polynomial for each subinterval $x_i \le x_{i + 1}$ usually different on each subinterval 


##### Piecewise Hermite Interpolation 
Suppose that the domain of interest is divided into $N - 1$ subintervals $[x_i, x_{i + 1}]$ where $i = 1, ..., n - 1$. Thus, we can write a cubic polynomial into the $i$-th interval as:
$$
p_i(x) = a_i + b_i(x - x_i) + c_i(x - x_i)^2 + d_i(x - x_i)^3 \text{ where } x \in [x_i, x_{i + 1}]
$$
Let $p_i(x), i = 1, ..., n - 1$ denote a cubic polynomial on the $i$-th subinterval given in the form:
$$
p_i(x) = a_i + b_i(x - x_i) + c_i(x - x_i)^2 + d_i(x - x_i)^3
$$
Suppose we want to specify both the function values and the derivatives at $x = x_i$:
$$
p(x_i) = y_i, \quad p'(x_i) = s_i, \quad i = 1, ..., n
$$
We require the following 4 conditions on each subinterval for $i = 1, ..., n - 1$
$$
\begin{align*}
p_i(x_i) &= y_i\\
p_i(x_{i + 1}) &= y_{i + 1}\\
\\
p_i'(x_i) &= s_i\\
p_i'(x_{i + 1}) &= s_{i + 1}
\end{align*}
$$
The solution to the set of equations above for $a_i, b_i, c_i, d_i$ is:
$$
\boxed{
\begin{align*}
a_i &= y_i\\
b_i &= s_i\\
c_i &= \frac{3y_i' - 2s_i - s_{i + 1}}{\Delta x_i}\\
d_i &= \frac{s_{i + 1} + s_i - 2y_i'}{(\Delta x_i)^2}
\end{align*}
}
$$
where:
$$
\boxed{
y_i' = \frac{y_{i + 1} - y_i}{\Delta x_i} \text{ and } \Delta x_i = x_{i + 1} - x_i
}
$$


e.g., Fit a cubic $(x_1, y_1) = (0, 0) \text{ and } (x_2, y_2) = (2, 3)$ with $s_1 = 1, s_2 = 0$
$$
\begin{align*}
\Delta_1 &= x_2 - x_1 = 1 - 0 = 1\\
y_1' &= \frac{y_2 - y_1}{\Delta x_1} = \frac{3 - 0}{1} = 3\\
\\
\text{Then,}&\\
a_1 &= y_1 = 0\\
b_1 &= s_1 = 1\\
c_1 &= \frac{3y_1' - 2s_1 - s_2}{\Delta x_1} = \frac{3(3) - 2(1) - 0}{1} = 7\\
d_1 &= \frac{s_2 + s_1 - 2y_1'}{x_1^2} = \frac{0 + 1 - 2(3)}{1^2} = -5\\
p(x) &= a_1 + b_1(x - x_1) + c_1(x - x_1)^2 + d(x - x_1)^2\\
&= 0 + 1(x - 0) + 7(x - 0) + (-5)(x - 0)^3\\
&= x + 7x^2 - 5x^3
\end{align*}
$$


##### Spline Interpolants
$$
\begin{align*}
p_i(x_i) &= y_i\\
p_i(x_{i + 1}) &= y_{i + 1}\\
\\
p_i'(x_i) &= s_i\\
p_i'(x_{i + 1}) &= s_{i + 1}
\end{align*}
$$
We have a set of equations above that at each of the $n - 2$ interior points, we have the following relationships for $i = 1, ..., n - 2$
$$
\begin{align*}
p_i(x_{i + 1}) &= p_{i + 1}(x_{i + 1})\\
p_i'(x_{i + 1}) &= p_{i + 1}'(x_{i + 1})
\end{align*}
$$
The interpolant and its first derivatives are continuous at the knots (breakpoints) $x_i$. 
- **Knots** refer to points where the interpolant is permitted to change its definition from one polynomial to another 
- **Nodes** refer to point where data is specified
Note: in this case, the two equations above make knots and nodes the same points 

A **Cubic Spline** is a piecewise cubic polynomial such that $S(x), S'(x), S''(x)$ are all continuous at the knots. $S(x)$ is a cubic polynomial on each particular subinterval $[x_i, x_{i + 1}]$ denotes by $S_i(x)$ for $i = 1, ..., n - 1$. The following interpolating conditions must hold for $i = 1, ..., n - 1$
$$
\begin{align*}
S_i(x_i) &= y_i\\
S_i(x_{i + 1}) &= y_{i + 1}
\end{align*}
$$
In addition, the following derivative continuity conditions must hold at the interior knots for $i = 1, ..., n - 2$ where:
$$
S_i'(x_{i + 1}) = S_{i + 1}'(x_{i + 1}) \text{ and } S_i''(x_{i + 1}) = S_{i + 1}''(x_{i + 1})
$$
There are $n - 1$ subintervals, and 4 unknowns per subinterval, thus we have $4n - 4$ unknowns. We have specified above $2(n-1)$ interpolating conditions and $2(n - 2)$ derivative conditions, for a total of $4n - 6$ equations. meaning that in order to determine a unique cubic spline interpolant, we must specify two additional equations.

**Boundary Conditions** at $x = x_1$ and $x = x_n$ 
1. **Free Boundary**: $S''(x_1) = 0 \text{ or } S''(x_n) = 0$
2. **Clamped Boundary**: $S''(x_1) = \text{specified} \text{ or } S''(x_n) = \text{specified}$
3. **Natural Cubic Spline** has free boundaries at both ends
4. **Complete Cubic Spline or Clamped Spline** has both ends clamped
5. **Periodic Spline**:
$$
S(x_1) = S(x_n) \text{ and } S'(x_1) = S'(x_n) \text{ and } S''(x_1) = S''(x_n)
$$

#### Spline Representation
The cubic spline $S_i(x)$ on each interval $[x_i, x_{i+1}]$ is expressed in the form:
$$
S_i(x) = a_i + b_i(x - x_i) + c_i(x - x_i)^2 + d_i(x - x_i)^3, \quad (2.5.1)
$$
where the coefficients are related to the given data $y_i$ and derivatives:
- $a_i = y_i$,
- $b_i = s_i$ (slope values, unknown),
- $c_i = \frac{3y_i' - 2s_i - s_{i+1}}{\Delta x_i}$,
- $d_i = \frac{s_{i+1} + s_i - 2y_i'}{(\Delta x_i)^2}$.

Here, $s_i$ represents the slopes at each point $x_i$, which are unknowns to be determined.

#### Spline Continuity and Conditions
To enforce smoothness, the following conditions must hold:
1. **Second derivative continuity**: $S''_i(x_{i+1}) = S''_{i+1}(x_{i+1})$ for $i = 1, \dots, n-2$.
2. Boundary conditions are specified as either *clamped* (derivatives specified) or *natural* (second derivative zero at boundaries).

From these conditions, the system of equations is derived:
$$
S_i''(x) = 2\left( \frac{3y_i' - 2s_i - s_{i+1}}{\Delta x_i} \right) + 6\left( \frac{s_i + s_{i+1} - 2y_i'}{(\Delta x_i)^2} \right)(x - x_i). \quad (2.5.2)
$$

Using the same expression for $S''_{i+1}(x)$ and matching values, we derive the following tridiagonal system:
$$
\Delta x_{i-1}s_{i-1} + 2(\Delta x_{i-1} + \Delta x_i)s_i + \Delta x_i s_{i+1} = 3(\Delta x_i y_i' + \Delta x_{i-1}y_{i-1}'). \quad (2.5.4)
$$

#### Boundary Conditions
The system needs two additional equations to close the problem:
1. **Clamped boundary**: Specified slopes $s_1$ and $s_n$:
   $$
   s_1 = s_1^*, \quad s_n = s_n^*. \quad (2.5.6, 2.5.7)
   $$
2. **Natural boundary**: The second derivatives vanish at the endpoints:
   $$
   S''(x_1) = 0, \quad S''(x_n) = 0.
   $$
   Substituting into the second derivative expressions, we get:
   $$
   s_1 + \frac{s_2}{2} = \frac{3}{2}y_1', \quad s_{n-1} + \frac{s_n}{2} = \frac{3}{2}y_{n-1}'. \quad (2.5.8, 2.5.9)
   $$

#### Solving the System
The resulting system of equations is tridiagonal with the general form:
$$
T\vec{s} = \vec{r}, \quad (2.5.10)
$$
where:
- $T$ is a tridiagonal matrix derived from $(2.5.4)$ and boundary conditions,
- $\vec{s}$ is the vector of unknown slopes $s_i$,
- $\vec{r}$ is the right-hand side vector, computed using:
   $$
   r_i = 3(\Delta x_i y_i' + \Delta x_{i-1}y_{i-1}'), \quad i = 2, \dots, n-1. \quad (2.5.11)
   $$




## Exercises: Interpolation

##### Question 1
**(a)** Show that there is a unique cubic polynomial $p_3(x)$ for which
$$
\begin{align*}
	p_3(x_0) = f(x_0),\quad p_3(x_2) = f(x_2)\\
	p_3'(x_1) = f'(x_1),\quad p_3''(x_1) = f''(x_1)
\end{align*}
$$
where $f(x)$ is a given function and $x_0 \neq x_2$.

$$
p_3(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3.
$$
- $p_3(x_0) = f(x_0)$
- $p_3(x_2) = f(x_2)$
- $p_3'(x_1) = f'(x_1)$
- $p_3''(x_1) = f''(x_1)$

The first step is to compute the derivatives of $p_3(x)$:
1. The first derivative is:
$$
p_3'(x) = a_1 + 2a_2 x + 3a_3 x^2
$$
2. The second derivative is:
$$
p_3''(x) = 2a_2 + 6a_3 x
$$
$$
\begin{align*}
    p_3(x_0) & = a_0 + a_1 x_0 + a_2 x_0^2 + a_3 x_0^3 = f(x_0), \\
    p_3(x_2) & = a_0 + a_1 x_2 + a_2 x_2^2 + a_3 x_2^3 = f(x_2), \\
    p_3'(x_1) & = a_1 + 2a_2 x_1 + 3a_3 x_1^2 = f'(x_1), \\
    p_3''(x_1) & = 2a_2 + 6a_3 x_1 = f''(x_1).
\end{align*}
$$
These four equations uniquely determine the four coefficients $a_0, a_1, a_2, a_3$ because the matrix of this linear system is invertible (provided $x_0 \neq x_2$ ensures distinct interpolation points). Therefore, there exists a unique cubic polynomial $p_3(x)$ that satisfies the given conditions.

**(b)** Derive base functions analogous to Lagrange basis for $p_3(x)$
$$
   \phi_0(x) = \frac{(x - x_1)^2 (x - x_2)}{(x_0 - x_1)^2 (x_0 - x_2)}.
$$$$
   \phi_1(x) = \frac{(x - x_0) (x - x_1)^2}{(x_2 - x_0) (x_2 - x_1)^2}.
$$$$
   \phi_2(x) = \frac{(x - x_0)(x - x_2)^2}{(x_1 - x_0)(x_1 - x_2)^2}.
$$$$
   \phi_3(x) = \frac{(x - x_0)(x - x_2)}{(x_1 - x_0)(x_1 - x_2)}.
   $$
$$
p_3(x) = f(x_0) \phi_0(x) + f(x_2) \phi_1(x) + f'(x_1) \phi_2(x) + f''(x_1) \phi_3(x).
$$

##### Question 2
For Lagrange polynomial interpolation, $n$ data points $(x_i, y_i)$ and $i = 1, 2, ..., n$ are given.

**(a)** What is the degree of each polynomial function $L_j(x)$ in the Lagrange basis?

The Lagrange basis function satisfies the property where $L_j(x) = 1$ if and only if $i = j$ and equals 0 otherwise. Thus, the product involves $n - 1$ terms (one for each $x_i$) and each term is a degree one polynomial. Multiplying these $n - 1$ degree-one terms gives a polynomial of degree $n - 1$. Thus, the degree of each polynomial function $L_j(x)$ in the Lagrange basis is $n - 1$

**(b)** What function if we sum the $n$ functions in the global Lagrange basis, that is:
$$
g(x) = \sum_{j = 1}^n L_j(x)
$$
Explain.

By definition $L_j(x)$ satisfies $L_j(x_i) = \delta_{ij}$ meaning it equals 1 when $x = x_j$ and 0 when $x = x_i$ for $i \neq j$. Therefore, for any $x = x_k$ (one of the given data points), exactly one term $L_k(x_k) = 1$, while all other terms $L_j(x_k)$ for $j \neq k$ are 0. Thus we have:
$$
g(x_i) = \sum_{j = 1}^n L_j(x_i) = 1 \text{ for all }i 
$$
The Lagrange basis polynomials $L_j(x)$ are constructed such that the sum $g(x)$ satisfies:
$$
g(x) = \sum_{j = 1}^n L_j(x) = 1 \text{ for all }x
$$
Each $L_j(x)$ is a polynomial of degree $n - 1$ so that sum $g(x)$ is also at most a polynomial of degree $n - 1$. However, the polynomial that $g(x_i) = 1$ at all $n$ interpolation points ensures that $g(x) \equiv 1$ as this is the unique polynomial of degree $n - 1$ or lower, that is constant and equals 1 at $n$ distinct points.

The sum of $n$ Lagrange Basis polynomials is the constant function:
$$
g(x) = 1 \text{ for all } x
$$


##### Question 
Let $S(x)$ be the natural spline interpolant of $x^3$ at $x = -3, -1, 1$ and $x = 3$. What is $S(0)$?

$S(x)$ is piecewise cubic over each interval and $S(x), S'(x), S''(x)$ are continuous at the interpolation points. The natural spline condition $S''(x) = 0$ at the endpoints $x = -3, x = 3$. The function $x^3$ is symmetric and smooth meaning it is already a cubic polynomial. When interpolating a cubic function using a cubic spline, the spline will exactly reproduce the function over the entire domain. The spline's values match the function values at all interpolation points. Therefore $S(x) = x^3$ is itself the natural cubic spline. 

Thus $S(0) = 0^3 = 0$ so $S(0) = 0$.


##### Question 
We are given the natural spline $S(x)$ defined as:
$$
S(x) =
\begin{cases}
  a_1 + 25x + 9x^2 + x^3 & x \in [-3, -1] \\
  26 + a_2x + a_3x^2 - x^3 & x \in [-1, 0] \\
  26 + 19x + a_4x^2 + a_5x^3 & x \in [0, 3] \\
  -163 + 208x - 60x^2 + a_6x^3 & x \in [3, 4]
\end{cases}
$$
The conditions for a natural cubic spline require that:
1. $S(x)$ is continuous at the breakpoints $x = -1, 0, 3$,
2. The first derivative $S'(x)$ is continuous at $x = -1, 0, 3$,
3. The second derivative $S''(x)$ is continuous at $x = -1, 0, 3$,
4. $S''(x) = 0$ at the endpoints $x = -3$ and $x = 4$ (natural spline boundary condition).

At $x = -1$, $S(x)$ from the first and second piece must be equal:
$$
a_1 + 25(-1) + 9(-1)^2 + (-1)^3 = 26 + a_2(-1) + a_3(-1)^2 - (-1)^3.
$$
$$
a_1 - 25 + 9 - 1 = 26 - a_2 + a_3 + 1.
$$
$$
a_1 - 18 = 27 - a_2 + a_3 \quad \text{(Equation 1)}.
$$
At $x = 0$, $S(x)$ from the second and third piece must be equal:
$$
26 + a_2(0) + a_3(0)^2 - (0)^3 = 26 + 19(0) + a_4(0)^2 + a_5(0)^3.
$$
$$
26 = 26 \quad \text{(automatically satisfied)}.
$$

At $x = 3$, $S(x)$ from the third and fourth piece must be equal:
$$
26 + 19(3) + a_4(3)^2 + a_5(3)^3 = -163 + 208(3) - 60(3)^2 + a_6(3)^3.
$$
$$
26 + 57 + 9a_4 + 27a_5 = -163 + 624 - 540 + 27a_6.
$$
$$
83 + 9a_4 + 27a_5 = -79 + 27a_6 \quad \text{(Equation 2)}.
$$
Compute derivatives $S'(x)$ for each piece:
- First piece: $S_1'(x) = 25 + 18x + 3x^2$,
- Second piece: $S_2'(x) = a_2 + 2a_3x - 3x^2$,
- Third piece: $S_3'(x) = 19 + 2a_4x + 3a_5x^2$,
- Fourth piece: $S_4'(x) = 208 - 120x + 3a_6x^2$.

At $x = -1$, match $S_1'(x)$ and $S_2'(x)$:
$$
25 + 18(-1) + 3(-1)^2 = a_2 + 2a_3(-1) - 3(-1)^2.
$$
$$
25 - 18 + 3 = a_2 - 2a_3 - 3.
$$
$$
10 = a_2 - 2a_3 \quad \text{(Equation 3)}.
$$

At $x = 3$, match $S_3'(x)$ and $S_4'(x)$:
$$
19 + 2a_4(3) + 3a_5(3)^2 = 208 - 120(3) + 3a_6(3)^2.
$$
$$
19 + 6a_4 + 27a_5 = 208 - 360 + 27a_6.
$$
$$
19 + 6a_4 + 27a_5 = -152 + 27a_6 \quad \text{(Equation 4)}.
$$

Compute second derivatives $S''(x)$:
- First piece: $S_1''(x) = 18 + 6x$,
- Second piece: $S_2''(x) = 2a_3 - 6x$,
- Third piece: $S_3''(x) = 2a_4 + 6a_5x$,
- Fourth piece: $S_4''(x) = -120 + 6a_6x$.

At $x = -1$, match $S_1''(x)$ and $S_2''(x)$:
$$
18 + 6(-1) = 2a_3 - 6(-1).
$$
$$
18 - 6 = 2a_3 + 6.
$$
$$
6 = 2a_3 \implies a_3 = 3 \quad \text{(Equation 5)}.
$$

At $x = 3$, match $S_3''(x)$ and $S_4''(x)$:
$$
2a_4 + 6a_5(3) = -120 + 6a_6(3).
$$
$$
2a_4 + 18a_5 = -120 + 18a_6 \quad \text{(Equation 6)}.
$$
For a natural spline, $S''(x) = 0$ at $x = -3$ and $x = 4$.

At $x = 4$, $S_4''(4) = 0$:
$$
-120 + 6a_6(4) = 0.
$$
$$
-120 + 24a_6 = 0 \implies a_6 = 5 \quad \text{(Equation 7)}.
$$
From the equations:
- $a_3 = 3$,
- $a_6 = 5$,
- Solve Equations (1), (2), (3), (4), (6) for $a_1, a_2, a_4, a_5$.

To compute $S(x)$ at $x = -3, -1, 0, 3$, substitute the known coefficients into the spline. Then, construct the Lagrange polynomial using:
$$
p(x) = \sum_{i=0}^3 y_i L_i(x),
$$
where $L_i(x)$ are the Lagrange basis functions.



##### Question 
We are tasked with constructing a quadratic spline $S_i(x)$ of the form:
$$
S_i(x) = a_i(x_{i+1} - x) + b_i(x - x_i)(x - x_{i+1}) + c_i(x - x_i)
$$
for $x \in [x_i, x_{i+1}]$ where $i = 1, \dots, n-1$.

The spline conditions are:
1. $S_i(x_i) = f_i$ for $i = 1, \dots, n$,
2. $S_i(x_{i+1}) = f_{i+1}$ for $i = 1, \dots, n-1$,
3. $S_{i+1}'(x_{i+1}) = S_i'(x_{i+1})$ for $i = 1, \dots, n-2$ (continuity of the first derivative).
4. Boundary condition: $S'(x_0) = \text{specified}$ at the left endpoint.

We aim to determine the equations for the spline parameters $a_i$, $b_i$, and $c_i$.

Condition 1: $S_i(x_i) = f_i$
At $x = x_i$, the spline $S_i(x)$ simplifies to:
$$
S_i(x_i) = a_i(x_{i+1} - x_i) + b_i(x_i - x_i)(x_i - x_{i+1}) + c_i(x_i - x_i).
$$
$$
S_i(x_i) = a_i(x_{i+1} - x_i) = f_i.
$$
$$
a_i = \frac{f_i}{x_{i+1} - x_i}. \quad (1)
$$
Condition 2: $S_i(x_{i+1}) = f_{i+1}$
At $x = x_{i+1}$, the spline $S_i(x)$ simplifies to:
$$
S_i(x_{i+1}) = a_i(x_{i+1} - x_{i+1}) + b_i(x_{i+1} - x_i)(x_{i+1} - x_{i+1}) + c_i(x_{i+1} - x_i).
$$
$$
S_i(x_{i+1}) = c_i(x_{i+1} - x_i) = f_{i+1}.
$$
$$
c_i = \frac{f_{i+1}}{x_{i+1} - x_i}. \quad (2)
$$

Condition 3: Continuity of $S'(x)$ at $x_{i+1}$
To ensure continuity of the first derivative $S'(x)$ at $x_{i+1}$, we compute the derivative of $S_i(x)$:
$$
S_i'(x) = \frac{d}{dx} \Big( a_i(x_{i+1} - x) + b_i(x - x_i)(x - x_{i+1}) + c_i(x - x_i) \Big).
$$
$$
S_i'(x) = -a_i + b_i(2x - x_i - x_{i+1}) + c_i.
$$
At $x = x_{i+1}$:
$$
S_i'(x_{i+1}) = -a_i + b_i(x_{i+1} - x_i) + c_i.
$$

At $x = x_{i+1}$ for the next spline $S_{i+1}(x)$:
$$
S_{i+1}'(x_{i+1}) = -a_{i+1} + b_{i+1}(x_{i+1} - x_{i+1}) + c_{i+1}.
$$
$$
S_{i+1}'(x_{i+1}) = -a_{i+1} + c_{i+1}.
$$
$$
-a_i + b_i(x_{i+1} - x_i) + c_i = -a_{i+1} + c_{i+1}. \quad (3)
$$
We are given the boundary condition at the left endpoint $x_0$:
$$
S_1'(x_0) = \text{specified} = \alpha.
$$
From $S_1'(x)$:
$$
S_1'(x_0) = -a_1 + b_1(x_0 - x_0) + c_1 = -a_1 + c_1.
$$
$$
-a_1 + c_1 = \alpha. \quad (4)
$$
Using Equations (1), (2), (3), and (4):
1. $a_i = \frac{f_i}{x_{i+1} - x_i}$,
2. $c_i = \frac{f_{i+1}}{x_{i+1} - x_i}$,
3. $-a_i + b_i(x_{i+1} - x_i) + c_i = -a_{i+1} + c_{i+1}$ for $i = 1, \dots, n-2$,
4. $-a_1 + c_1 = \alpha$ (boundary condition).

We solve this tridiagonal system for $b_i$ using the known values of $a_i$ and $c_i$. The spline parameters $a_i$, $b_i$, and $c_i$ are determined as follows:
- $a_i = \frac{f_i}{x_{i+1} - x_i}$,
- $c_i = \frac{f_{i+1}}{x_{i+1} - x_i}$,
- Solve for $b_i$ using:
$$
  -a_i + b_i(x_{i+1} - x_i) + c_i = -a_{i+1} + c_{i+1}.
  $$
The boundary condition $-a_1 + c_1 = \alpha$ ensures the spline satisfies the specified derivative at the left endpoint.



##### Question 
We are given the function $S(x)$ defined in three pieces as follows:
$$
S(x) = \begin{cases}
  x + 1, & -2 \leq x \leq -1, \\
  ax^3 + bx^2 + cx + d, & -1 \leq x \leq 1, \\
  x - 1, & 1 \leq x \leq 2.
\end{cases}
$$
We are tasked to determine if there exists a choice of coefficients $a$, $b$, $c$, and $d$ such that $S(x)$ is a natural cubic spline. A natural cubic spline requires:
1. **Continuity of the function** $S(x)$ at the knots $x = -1$ and $x = 1$.
2. **Continuity of the first derivative** $S'(x)$ at the knots $x = -1$ and $x = 1$.
3. **Continuity of the second derivative** $S''(x)$ at the knots $x = -1$ and $x = 1$.
4. **Natural boundary conditions**: $S''(x)$ at the endpoints $x = -2$ and $x = 2$ must be equal to zero.

Continuity of $S(x)$ at $x = -1$ and $x = 1$
At $x = -1$:
From the first piece $S(x) = x + 1$:
$$
S(-1) = -1 + 1 = 0.
$$
From the second piece $S(x) = ax^3 + bx^2 + cx + d$:
$$
S(-1) = a(-1)^3 + b(-1)^2 + c(-1) + d = -a + b - c + d.
$$
For continuity at $x = -1$:
$$
-a + b - c + d = 0. \quad (1)
$$
At $x = 1$:
From the second piece $S(x) = ax^3 + bx^2 + cx + d$:
$$
S(1) = a(1)^3 + b(1)^2 + c(1) + d = a + b + c + d.
$$
From the third piece $S(x) = x - 1$:
$$
S(1) = 1 - 1 = 0.
$$
For continuity at $x = 1$:
$$
a + b + c + d = 0. \quad (2)
$$

Continuity of $S'(x)$ at $x = -1$ and $x = 1$
The first derivative of $S(x)$ is:
- For $x + 1$: $S'(x) = 1$,
- For $ax^3 + bx^2 + cx + d$: $S'(x) = 3ax^2 + 2bx + c$,
- For $x - 1$: $S'(x) = 1$.

At $x = -1$:
From the first piece:
$$
S'(-1) = 1.
$$
From the second piece:
$$
S'(-1) = 3a(-1)^2 + 2b(-1) + c = 3a - 2b + c.
$$
For continuity of the first derivative at $x = -1$:
$$
3a - 2b + c = 1. \quad (3)
$$
At $x = 1$:
From the second piece:
$$
S'(1) = 3a(1)^2 + 2b(1) + c = 3a + 2b + c.
$$
From the third piece:
$$
S'(1) = 1.
$$
For continuity of the first derivative at $x = 1$:
$$
3a + 2b + c = 1. \quad (4)
$$

Continuity of $S''(x)$ at $x = -1$ and $x = 1$
The second derivative of $S(x)$ is:
- For $x + 1$: $S''(x) = 0$,
- For $ax^3 + bx^2 + cx + d$: $S''(x) = 6ax + 2b$,
- For $x - 1$: $S''(x) = 0$.

At $x = -1$:
From the first piece:
$$
S''(-1) = 0.
$$
From the second piece:
$$
S''(-1) = 6a(-1) + 2b = -6a + 2b.
$$
For continuity of the second derivative at $x = -1$:
$$
-6a + 2b = 0. \quad (5)
$$
At $x = 1$:
From the second piece:
$$
S''(1) = 6a(1) + 2b = 6a + 2b.
$$
From the third piece:
$$
S''(1) = 0.
$$
For continuity of the second derivative at $x = 1$:
$$
6a + 2b = 0. \quad (6)
$$

Natural Boundary Conditions
For a natural cubic spline, $S''(x) = 0$ at the endpoints $x = -2$ and $x = 2$.
- From the first piece $S''(x) = 0$ for all $x$ in $[-2, -1]$ (satisfied).
- From the third piece $S''(x) = 0$ for all $x$ in $[1, 2]$ (satisfied).
Thus, the natural boundary conditions are already satisfied.

We now solve the system of equations:
1. $-a + b - c + d = 0$,
2. $a + b + c + d = 0$,
3. $3a - 2b + c = 1$,
4. $3a + 2b + c = 1$,
5. $-6a + 2b = 0$,
6. $6a + 2b = 0$.

From equations (5) and (6):
$$
-6a + 2b = 0 \implies b = 3a, \quad 6a + 2b = 0 \implies b = -3a.
$$
This implies $3a = -3a$, which means $a = 0$ and $b = 0$. 
Substitute $a = 0$ and $b = 0$ into equations (3) and (4):
$$
c = 1, \quad c = 1.
$$
Substitute into equations (1) and (2):
$$
d = 0.
$$
The coefficients are:
$$
a = 0, \; b = 0, \; c = 1, \; d = 0.
$$
Thus, $S(x)$ is a natural cubic spline. 

If $S(x)$ is a clamped spline instead of a natural spline, the conditions $S''(x) = 0$ at the endpoints no longer apply. Instead, the first derivative at the endpoints $x = -2$ and $x = 2$ is specified.

Given the linear pieces at the endpoints ($S(x) = x + 1$ and $S(x) = x - 1$), the derivatives are constant:
$$
S'(-2) = 1 \quad \text{and} \quad S'(2) = 1.
$$
These derivatives match the derivative of the central cubic piece at the endpoints, so the clamped spline conditions are also satisfied without changing the coefficients. There is no additional choice of coefficients required. The same coefficients $a = 0$, $b = 0$, $c = 1$, and $d = 0$ satisfy the clamped spline conditions as well.


