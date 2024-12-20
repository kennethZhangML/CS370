In general we are interested in the problem of numerically solving an equation of the form:
$$
y'(t) = f(t, y(t)) \quad \text{ with} y(t_0) = y_0
$$
where, the equation above describes a model of a particular quantity $y(t)$ depending on a given parameter $t$. The model states that:
1. There is an initial **known** starting value 
2. and this Initial Value is a description of how the quantity changes 
3. For a fixed $t$, the changes depend only on the parameter and the quantity $y(t)$ at this parameter

**IVP: Initial Value Problem**
For $y(t)$, its data are:
- the function of two variables $f(t, z)$ called the dynamics function 
- the values $t_0, y_0$ usually called initial conditions 

**Dynamics Function**: $f(t, y(t)) = \alpha \cdot y(t)$ 
**Initial Condition**: $y(t_0) = 100$ 

##### Systems of Differential Equations 
Consider a model with multiple variables that interact: $x, y$ are coordinates of a moving object for example. This gives us a system of differential equations, such as:
$$
\begin{align*}
x'(t) &= f_z(t, x(t), y(t)) \quad\text{ with } x(t_0) = x_0\\
y'(t) &= f_y(t, x(t), y(t)) \quad\text{ with } y(t_0) = y_0\\
\\
\begin{bmatrix}
x(t) \\ y(t)
\end{bmatrix}
&= 
\begin{bmatrix}
f_x(t, x(t), y(t))\\
f_y(t, x(t), y(t))
\end{bmatrix}
\quad\text{ with }
\begin{bmatrix}
x(t_0) \\ y(t_0)
\end{bmatrix}
= \begin{bmatrix} x_0 \\ y_0 \end{bmatrix}
\end{align*}
$$
Thus we can get:
$$
\begin{align*}
y'(t) &= f(t, y(t)) \\ y^{(n)}(t) &= f(t, y(t), y'(t), y''(t), y'''(t), \cdots, y^{(n-1)}(t))
\end{align*}
$$
Thus, we have, for a **time-dependent** function $y(t)$, we are given only the time **rate-of-change** (derivative) $y'(t)$. so the **Standard Form of 1st Order ODE** is given by:
$$
y'(t) = f(t, y(t)) \quad\text{ with } y(t_0) = y_0
$$


### Approximating Methods 
Assume that no-closed form solutions exist for such an equation (a 1st order ODE with Initial Condition) and hence we need to obtain a numerical solution. 

We need to choose a set of times $t_0 < t_1 \cdots < t_N$ at which we estimate the value of the solution $y_0, y_1, \cdots, y_N$. Our goal is that $y_n$ will be close to the **true value** of $y(t_n)$ for each $n$. Such a numerical solution will allows us to produce a plot of our function $y(t)$ in a particular interval and to approximate any value of the function through a process such as the piecewise polynomial interpolation covered in chapter 2. 

#### Time-Stepping Methods 
A time-step is the interval $h_n = t_{n + 1} - t_n$ which is determined by the method. Time-stepping methods carry a candidate size $h^{\text{cand}}$ for the next time-step which may be revised during each time-step. 
$$
\begin{align*}
&\text{initialize } y_0, t_0, h^{\text{cand}}, n = 0\\
&\text{repeat }\\
\\
&\text{i)} \text{ compute } y_{n + 1} \text{ and } h_n \text{ using data } t_n, y_n, h^{\text{cand}} \text{ and } f(t, z)\\
&\text{ii) } t_{n + 1} \leftarrow t_n + h_n\\
&\text{iii)} \text{ recompute } h^{\text{cand}}\\
&\text{iv) } n \leftarrow n + 1
\end{align*}
$$
Methods for advancing the solution can be classified into groups several ways, one such way:
- Single-step methods (where $y_{n + 1}$ is determined from $(t_n, y_n)$ and the dynamics function $f$)
- Multi-step methods (where $y_{n + 1}$ is determined from $(t_{n - i}, y_{n - i})$ and $f$ for $i = 0, \cdots, N$ with $N > 0$ (number of steps))

**Explicit vs. Implicit Methods**
- **Explicit**: those with $y_{n + 1}$ given in terms of $(t_{n - i}, y_{n - i})$ and the equation for the derivative for $i = 0, 1, \cdots$
- **Implicit**: those where $y_{n + 1}$ is computed by solving an algebraic equation involving $f$



#### Forward-Euler Method 
Given a set of $t$ points $t_0 < t_1 < \cdots < t_N$ with our problem being to find $y_i$. We use the slope as an approximation to the derivative and then develops a recursive scheme for determine $y_n$
$$
\frac{y(t_{n + 1}) - y(t_n)}{t_{n + 1} - t_n} \approx y'(t_n) = f(t_n, y(t_n))
$$
$$
y_0 = y(t_0) \text{ and } y(t_{n + 1}) \approx y(t_n) + f(t_n, y(t_n))(t_{n + 1} - t_n) \text{ for } n = 0, \cdots, N - 1
$$
Thus, we get our method for advancing the numerical solution:
$$
y_0 = y(t_0) \text{ and } y_{n + 1} = y_n + f(t_n, y_n)(t_{n + 1} - t_n) \text{ for } n = 0, \cdots, N - 1
$$

**e.g., Consider the following FE method for the IVP:**
$$
y'(t) = y(t)\left(2.5 t - t^2 \sqrt{y(t)}\right) \text{ with } y(0) = 1
$$
We can obtain approximations to $y(t)$ for $t = 0.4, 0.8, 1.2, 1.6, \text{ and } 2.0$ by using:
$$
y(0) = 1 \text{ and } y_{n + 1} = y_n + h \cdot y_n \cdot (2.5 t_n - t_n^2 \sqrt{y_n})
$$
$t_1 = 0.4 \text{ with } y_0 = 1, t_0 = 0, h = 0.4$
$$
y_1 = 1 + 0.4 \cdot 1 \cdot (2.5 \cdot 0 - 0^2 \cdot \sqrt{1}) = 1
$$
$t_2 = 0.8 \text{ with } y_1 = 1, t_1 = 0.4, h = 0.4$
$$
y_2 = 1 + 0.4 \cdot 1 \cdot (2.5 \cdot 0.4 - 0.4^2 \cdot \sqrt{1}) = 1.336
$$
$t_3 = 1.2, y_2 = 1.336, t_2 = 0.8, h = 0.4$
$$
y_3 = 1.336 + 0.4 \cdot 1.336 \cdot (2.5 \cdot 0.8 - 0.8^2 \cdot \sqrt{1.336}) = 2.009
$$
$t_4 = 1.6, y_3 = 2.009, t_3 = 1.2, h = 0.4$
$$
y_4 = 2.009 + 0.4 \cdot 2.009 \cdot (2.5 \cdot 1.2 - 1.2^2 \cdot \sqrt
2.009) = 2.780
$$


#### Discrete Approximation
Recall that for any function $y(t)$ we can do a Taylor expression about the point $t = a$ where $h = t - a$. We can be more precise about what happens when we truncate after a finite number of terms. If $a = t_n$ and $t = t_{n + 1}$ and $p = 1$ then the equation:
$$
y(t_{n + 1}) = y(t_n) + y'(t_n)\cdot h + O(h^2)
$$
which can also be re-written as:
$$
y'(t_n) = \frac{y(t_{n + 1}) - y(t_n)}{h} + O(h)
$$
This is called the **forward difference approximation** to $y'(t_n)$ since we are using information at $t = t_n$ and the forward point $t = t_{n + 1}$. Since the error term is $O(h)$ we say that the approximation is a first order approximation. Intuitively this says that the error is reduced proportionally to $h$ as $h \rightarrow 0$. Since $y'(t) = f(t, y(t))$ we can write:
$$
y(t_{n + 1}) = y(t_n) + f(t_n, y(t_n)) \cdot h + O(h^2)
$$
The $O(h^2)$ is called the Local Truncation Error for our formula for the Forward Euler method. 
$$
\text{LTE}_{\text{FE}} = y(t_{n + 1}) - (y(t_n) + hf(t_n, y(t_n))) = O(h^2)
$$


#### Trapezoidal and Modified Euler Methods 
In the following $y(t_n)$ denotes the exact solution of our differential equation at $t = t_n$ while $y_n$ will denote the approximation solution at $t = t_n$. Let's set $h = t_{n + 1} - t_n$, the our expansions give:
$$
\begin{align*}
y(t_{n + 1}) &= y(t_n) + y'(t_n)\cdot h + \frac{y''(t_n)}{2}h^2 + \frac{y'''(n)}{6}h^3\\
y''(t_n) &= \frac{y'(t_{n + 1}) - y'(t_n)}{h} + O(h)\\
\implies y(t_{n + 1}) &= y(t_n) + h \left(\frac{y'(t_{n + 1}) + y'(t_n)}{2}\right) + O(h^3)
\end{align*}
$$
$y(t)$ satisfies the differential equation $y'(t) = f(t, y(t))$ so that:
$$
\begin{align*}
y'(t_n) &= f(t_n, y(t_n))\\
y'(t_{n + 1}) &= f(t_{n + 1}, y(t_{n+1}))
\end{align*}
$$
Thus we have:
$$
y(t_{n + 1}) = y(t_n) + \frac{h}{2} \left[f(t_n, y(t_n)) + f(t_{n + 1}, y(t_{n + 1}))\right] + O(h^3)
$$
$$
y_{n + 1} = y_n + \frac{h}{2}(f(t_n, y_n) + f(t_{n + 1}, y_{n + 1}))
$$
Where, the previous equation is approximating the derivative $y'(t)$ at a point half way between $t_n, t_{n + 1}$ by using the average of the slopes at the two endpoints. 
$$
\text{LTE}_{\text{trapezoidal}} = \frac{O(h^3)}{\left(1 - \frac{hL}{2}\right)} = O(h^3)
$$

**Implicit vs. Explicit**
1. **Implicit**: $y_{n + 1}$ term appears on both sides of the equation 
2. **Explicit**: $y_{n + 1}$ term appears only on the LHS of the equation 


#### Modified Euler Method (Improved Euler)
One technique for handling the implicit equation for the trapezoidal method is to combine it with the Forward Euler recursion procedure. 
$$
y(t_{n + 1}) = y(t_n) + hf(t_n, y(t_n)) + O(h^2)
$$
Then we can set:
$$
y^{*}_{n + 1} = y(t_n) + hf(t_n, y(t_n)) \implies y(t_{n + 1}) - y^{*}_{n + 1} = O(h^2)
$$
Thus we have our **Explicit Method with Higher Order LTE**
$$
\begin{align*}
y^{*}_{n + 1} &= y_n + hf(t_n, y_n)\\
y_{n + 1} &= y_n + \frac{h}{2}(f(t_n, y_n) + f(t_{n + 1}, y^{*}_{n + 1}))
\end{align*}
$$
is an explicit method with higher order Truncation Error than FE.


#### Runge-Kutta Methods 
$$
\begin{align*}
k_1 &= hf(t_n, y_n)\\
k_2 &= hf(t_n + h, y_n + k_1)\\
y_{n + 1} &= y_n + \frac{k_1}{2} + \frac{k_2}{2}
\end{align*}
$$
Runge-Kutta Methods have LTE $O(h^3)$ due to truncation. 

##### Midpoint Runge-Kutta Method 
$$
\begin{align*}
k_1 &= hf(t_n, y_n)\\
k_2 &= hf(t_n + \frac{h}{2}, y_n + \frac{k_1}{2})\\
y_{n + 1} &= y_n + k_2
\end{align*}
$$
which also has LTE $O(h^3)$. We can continue to obtain higher order methods which have local truncation error $O(h^{\alpha})$ for $\alpha = 4, 5, 6, \dots$
$$
\begin{align*}
k_1 &= hf(t_n, y_n)\\
k_2 &= hf(t_n + \frac{h}{2}, y_n + \frac{k_1}{2})\\
k_3 &= hf(t_n + \frac{h}{2}, y_n + \frac{k_2}{2})\\
k_4 &= hf(t_n + h, y_n + k_2)\\
y_{n + 1} &= y_n + \frac{k_1}{6} + \frac{k_2}{3} + \frac{k_3}{3} + \frac{k_4}{6}
\end{align*}
$$
The Runge-Kutta method is nested in the pair of methods of order 4 and 5 that permit both advancing the solution from $y_n$ to $y_{n + 1}$ and choosing the step size $h_n = t_{n + 1} - t_n$ as described in conceptual time stepping scheme. The 5th order method is used to advance the solution. A comparison of the result of the 4th and 5th order methods provides a local error estimate that is used to select $h_n$. 



#### Global vs. Local Error 
Local Truncation Error for Forward and Euler methods are the local error in the sense that it is the error when making a single step. We ask, how does this affect the global error when we make a larger number of steps such that $y(t_n) - y_n$ for some finite $t_n > t_o$. 

These errors, such as those observed in the Modified Euler method ($O(h^3)$) at each step accumulate from step to step so that the global error in the computed solution at the last step will be larger than the local error. 
$$
\begin{align*}
\text{Number of Steps} &= \frac{t_n - t_0}{h} = O \left(\frac{1}{h}\right)\\
\\
\text{Global Error} &\le \text{Local Error} \cdot \text{Number of Steps}\\
&= O(h^3) \cdot O(1/h)\\
&= O(h^2)
\end{align*}
$$
**Worst Case**: All Local Errors accumulate. 
For some method, with LTE (local error) $O(h^{p + 1})$ then the global error is $O(h^p)$
We refer to the order of a method whilst we refer to the global error. We call Modified Euler a 2nd order method while FE is a 1st order method. 


#### Time-Step Control Principles 
Let $y_{n + 1}$ be the high order estimate and $y^{*}_{n + 1}$ be the low order estimate corresponding to two different methods of order $p$ and $p - 1$ respectively. 
$$
\begin{align*}
y^{*}_{n + 1} &= y(t_{n + 1}) + O(h^p)\\
&= y(t_{n + 1}) + Ah^p + ( \text{smaller terms} )\\
y_{n + 1} &= y(t_{n + 1}) + O(h^{p + 1}) \quad \text{where } h = t_{n + 1} - t_n \text{ and } A \text{ is constant.}\\
|y^{*}_{n + 1} - y(t_{n + 1})| &= \text{Local Error} \approx |Ah^p|\\
|y^{*}_{n + 1} - y_{n + 1}| &= Ah^p + (\text{smaller terms})\\
&\approx Ah^p\\
\implies \text{Local Error} &= |y^*_{n + 1} - y(t_{n + 1})| \approx |y^*_{n + 1} - y_{n + 1}| = Ah^p
\end{align*}
$$
Suppose that a user specifies an error tolerance of *tol* for each recursive step. Then we can estimate the actual error in our computation of $y^*_{n + 1}$ for this step:

Let:
1. $h_{\text{old}} = t_{n + 1} - t_n$
2. $h_{\text{new}} = t_{n + 2} - t_{n + 1}$
$$
\begin{align*}
y^*_{n+ 1} &= y(t_{n + 2}) + Ah_{\text{new}}^p + (\text{Smaller Terms})\\
\text{Local Error} &= \left(\frac{h_\text{new}}{h_\text{old}}\right)^p |y_{n + 1}^* - y_{n + 1}|
\end{align*}
$$
which gives us:
$$
h_\text{new} = h_\text{old} \left(\frac{\text{tol}}{|y^*_{n + 1} - y_{n + 1}|}\right)^{1/p}
$$


#### Stability 
We wish to test the stability of an algorithm by introducing an error $\epsilon_0$ and seeing if it becomes amplified exponentially as the number of $t$ steps becomes large. For example we can test a method by seeing how it behaves on a simple test equation,
$$
y'(t) = -\lambda y(t) \text{ with } y(0) = y_0
$$
where $\lambda$ is a positive constant. This of course is a very simple equation with a corresponding exact equation. Note, that the exact solution tends to 0 and $t \rightarrow \infty$ and that the exact solution is positive if $y_0$ is positive. 

For example, the Forward Euler method with constant increments behaves with the initial value problem. If $h$ is the constant interval size then the recursion is given by:
$$
\begin{align*}
y_{n + 1} &= y_n - \lambda h y_n\\
&= (1 - \lambda h) y_n\\
&= (1 - \lambda h)^2 y_{n - 1}\\
&= \dots\\
&= (1 - \lambda h)^{n + 1} y_0
\end{align*}
$$
The exact solution decays to 0 as $t \rightarrow \infty$ and $n \rightarrow \infty$. We obtain the same behaviour only if:
$$
|1 - \lambda h| \le 1 \text{ that is } h < \frac{2}{\lambda}
$$
Similarly, we can obtain $y^{(p)}(0) = y_0 + \epsilon_0$ 
$$
\begin{align*}
y_{n + 1}^{(p)} &= y_{n}^{(p)} - \lambda h y_n^{(p)}\\
&= (1 - \lambda h)y_n^{(p)}\\
&= (1 - \lambda h)^2 y_{n - 1}^{(p)}\\
&= \dots\\
&= (1 - \lambda h)^{n + 1} (y_0 + \epsilon_0)
\end{align*}
$$
If we let $e_{n + 1} = y_{n + 1}^{(p)} - y_{n + 1}$, then we have:
$$
e_{n + 1} = (1 - \lambda h) e_n
$$


### Exercises: ODEs

##### Question:
With an unconditionally stable method, one can take arbitrarily large time-steps in numerically solving a stable ODE within a given accuracy. 

**Solution:** True, we can take arbitrarily large time-steps because we are not restricted by stability when we perturb with respect to a given magnitude. 

##### Question 
Verify for the following system of equations:
$$
\begin{align*}
Y_1' &= Y_2\\
Y_2' &= -x^2 Y_1 - xY_2
\end{align*}
$$
that $Y_1(x) = y(x)$ where $y(x)$ satisfies the 2nd order equation:
$$
y''(x) + x y'(x) + x^2 y(x) = 0
$$

**Solution**: Suppose that $Y_1(x) = y(x)$ and that $Y_2(x) = y'(x)$

Then $Y_1' = Y_2$ which gives that $Y_1(x) = y(x)$ and $Y_2(x) = y'(x)$
$$
Y_1' = y'(x) = Y_2
$$
Thus, we have that $Y_2' = -x^2 Y_1 - xY_2$ giving that $Y_1(x) = y(x)$ and $Y_2(x) = y'(x)$
$$
Y_2' = y''(x), \quad-x^2Y_1 = -x^2 y(x), \quad-xY_2 = -xy'(x) \implies y''(x) = -x^2 y(x) - xy'(x)
$$
Thus, re-writing, we get that:
$$
y''(x) = -xy'(x) - x^2 y(x)
$$


##### Question:
Write the following third order differential equation as a system of three first-order equations:
$$
y'''(t) + sin(t) y''(t) - g(t) y'(t) + g(t)y(t) = f(t)
$$
We let:
- $y_1(t) = y(t)$
- $y_2(t) = y'(t)$
- $y_3(t) = y''(t)$
$$
y_1'(t) = y_2(t), \quad y_2'(t) = y_3(t)
$$
Substituting we get:
$$
y_3'(t) + sin(t)y_3(t) - g(t)y_2(t) + g(t)y_1(t) = f(t)
$$
$$
\begin{align*}
y_1'(t) &= y_2(t),\\
y_2'(t) &= y_3(t),\\
y_3'(t) &= -sin(t)y_3(t) + g(t)y_2(t) - g(t)y_1(t) + f(t)
\end{align*}
$$


##### Question
State the following problem in first order form. Differentiation is respect to $t$.
$$
\begin{align*}
u''(t) + 3v'(t) + 4u(t) + v(t) &= t\\
v''(t) - v'(t) + u(t) + v(t) &= cos(t)
\end{align*}
$$
Let:
$$
u_1(t) = u(t), u_2(t) = u'(t), v_1(t) = v(t), v_2(t) = v'(t)
$$
we get:
$$
u_1'(t) = u_2(t), u_2'(t) = u''(t), v_1'(t) = v_2(t), v_2'(t) = v''(t)
$$
Thus we get:
- $u''(t) + 3v'(t) + 4u(t) + v(t) = t$
- $v''(t) - v'(t) + u(t) + v(t) = cos(t)$
$$
\begin{align*}
u''(t) = u_2'(t), v'(t) = v_2(t), u(t) = u_1(t), v(t) = v_1(t)\\
u_2'(t) + 3v_2(t) + 4u_1(t) + v_1(t) = t\\
v''(t) = v_2'(t), v'(t) = v_2(t), u(t) = u_1(t), v(t) = v_1(t)\\
v_2'(t) - v_2(t) + u_1(t) + v_1(t) = cos(t)
\end{align*}
$$
Thus our final system in first-order equation is:
$$
\begin{align*}
u_1'(t) &= u_2(t)\\
u_2'(t) &= -3v_2(t) - 4u_1(t) - v_1(t) + t\\
v_1'(t) &= v_2(t)\\
v_2'(t) &= v_2(t) - u_1(t) - v_1(t) + cos(t)
\end{align*}
$$


##### Question
Apply both the Forward Euler and Modified Euler methods to the IVP:
$$
\begin{cases}
y'(t) = -5y(t)\\
y(0) = 5
\end{cases}
$$
Show the computation schemes for both methods and estimate the step-size for each case that ensures stable computations.

**Forward Euler Method**
$$
y_{n + 1} = y_n + hf(t_n, y_n)
$$
where $h$ is our step-size and $f(t_n, y_n) = -5y_n$ and $y_n$ is our current approximation.
Substitue $f(t_n, y_n) = -5y_n$:
$$
y_{n + 1} = y_n + h(-5 y_n) \implies y_{n + 1} = y_n(1 - 5h)
$$
Then, for stable computations, the amplification factor $|1 - 5h|$ must satisfy
$$
|1 - 5h| \le 1
$$
So using $-1 \le 1 - 5h \le 1$ and $-2 \le -5h \le 0$ so we get:
$$
0 \le h \le \frac{2}{5} \implies h \le 0.4
$$

##### Modified Euler Method 
$$
\begin{align*}
y_{n + 1}^* = y_n + hf(t_n, y_n)\\
y_{n + 1} = y_n + \frac{h}{2} [f(t_n, y_n) + f(t_{n + 1}, y_{n+1}^*)]
\end{align*}
$$
Here, we have that $f(t, y) = -5y$.
$$
\begin{align*}
y_{n+1}^* &= y_n + h(-5y_n)\\
y_{n + 1}^* &= y_n(1 - 5h)\\
\\
f(t_{n + 1}, y^*_{n + 1}) &= -5y_{n + 1}^* = -5y_n(1 - 5h)\\
y_{n + 1} &= y_n + \frac{h}{2}[-5y_n + (-5y_n(1 - 5h))]\\
y_{n + 1} &= y_n + \frac{h}{2}[-5y_n - 5y_n(1 - 5h)]\\
y_{n + 1} &= y_n + \frac{h}{2}[-5y_n - 5y_n + 25hy_n]\\
y_{n + 1} &= y_n + \frac{h}{2}[-10y_n + 25hy_n]\\
y_{n + 1} &= y_n \left(1 - 5h + \frac{25h^2}{2}\right)
\end{align*}
$$
Thus, we have our stability criterion by;
$$
\left|1 - 5h + \frac{25h^2}{2}\right| \le 1
$$

