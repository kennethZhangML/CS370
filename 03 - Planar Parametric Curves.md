A parametric curve in the $x, y$ plane is a directed curve described by a pair of continuous functions $x(t) \text{ and } y(t)$ of a common argument $t$, usually called the parameter, for $a \le t \le b$. The curve $C$ is the set of points $\vec{P}(t) = (x(t), y(t))$.

#### Examples
1. **A semi-circle in the upper half plane directed from $(1, 0)$ to $(-1, 0)$:**
   $$
   x(t) = \cos(\pi t), \; y(t) = \sin(\pi t), \; 0 \leq t \leq 1.
   $$
2. **Same as above but with direction reversed:**
   $$
   x(t) = \cos(\pi(1 - t)), \; y(t) = \sin(\pi(1 - t)), \; 0 \leq t \leq 1.
   $$
3. **Same as above but with an alternative parametrization:**
   $$
   x(t) = \cos(\pi t^2), \; y(t) = \sin(\pi t^2), \; 0 \leq t \leq 1.
   $$
4. **The curve shown in Figure 3.1 for a particular value of the numerical coefficient $K$:**
   $$
   x(t) = 1 + \frac{\cos(2\pi t)}{1 + Kt}, \; y(t) = 0.8\left(1 + \frac{\sin(2\pi t)}{1 + Kt}\right), \; 0 \leq t \leq 2.
   $$
Visually, it is clear that these example curves are smooth in some sense. Mathematically, this is reflected in the fact that all the derivatives of $x(t)$ and $y(t)$ exist, that is, the vector derivatives of $\vec{P}(t)$ exist for all $k$:
$$
   \frac{d^k \vec{P}(t)}{dt^k} = \left( \frac{d^k x(t)}{dt^k}, \frac{d^k y(t)}{dt^k} \right).
$$

### Tangent Line to a Parametric Curve
The tangent line to $C$ at $t = t_0$ has the direction of the first derivative, $\frac{d \vec{P}(t_0)}{dt}$. This tangent is a line which can also be expressed as a parametric curve, $T_{\text{tan}}(s) = (x_{\text{tan}}(s), y_{\text{tan}}(s))$, where we have used a new parameter $-\infty < s < \infty$. Thus:
$$
T_{\text{tan}}(s) = \vec{P}(t_0) + \frac{d \vec{P}(t_0)}{dt}(s - t_0).
$$

And consequently:
$$
\begin{aligned}
  x_{\text{tan}}(s) &= x(t_0) + \frac{dx(t_0)}{dt}(s - t_0), \\
  y_{\text{tan}}(s) &= y(t_0) + \frac{dy(t_0)}{dt}(s - t_0).
\end{aligned}
$$

### A Square as a Parametric Curve
A square can also be described as a parametric curve, using a piecewise definition:
$$
\begin{aligned}
  x(t) &= t, \; y(t) = 0, \; 0 \leq t \leq 1, \\
  x(t) &= 1, \; y(t) = t - 1, \; 1 \leq t \leq 2, \\
  x(t) &= 1 - (t - 2), \; y(t) = 1, \; 2 \leq t \leq 3, \\
  x(t) &= 0, \; y(t) = 1 - (t - 3), \; 3 \leq t \leq 4.
\end{aligned} \quad (3.0.1)
$$

Visually, this curve is not smooth. Mathematically, this is reflected in the fact that $x(t)$ and $y(t)$ do not have derivatives at $t = 1, 2, 3$.


## Interpolating Curve Data by a Parametric Curve

Suppose we had a sequence of points that lie on a curve in the plane, $(x_i, y_i), i = 1, \dots, n$. We can create a parametric curve that passes through these points if we can:

(a) identify suitable parameter values, $t_i, i = 1, \dots, n$, and

(b) construct interpolating functions, $x(t)$ for data $(t_i, x_i)$, and $y(t)$ for data $(t_i, y_i)$.

One simple approach to step (a) is to regard the row index $i$ as the sampled value of a real-valued parameter, $t$, that ranges from $1 \leq t \leq n$, that is, take $t_i = i$. For step (b), we could use piecewise linear spline interpolation. This would give us a function $x_{pl}(t)$ for $0 \leq t \leq n$. If we did the same with the $y$-coordinate data, then we would get a second function $y_{pl}(t)$ for $0 \leq t \leq n$, and together $(x_{pl}(t), y_{pl}(t))$ would make a parametric curve passing through the data points. If we were to plot the original data using the Python command `plot(x, y)`, then we would see $(x_{pl}(t), y_{pl}(t))$.

However, notice that such a curve would not necessarily be smooth. For a smoother interpolating curve, we could consider using cubic spline interpolation. That is, we interpolate $(t_i, x_i)$ by a cubic spline function, $x_{cs}(t)$, and do the same for the $y$-coordinate data. Then we would have a smooth interpolating parametric curve $(x_{cs}(t), y_{cs}(t))$.

How well would using a piecewise linear interpolating parametric curve work for representing the unit square curve (3.0.1)? How well would using a cubic spline interpolating parametric curve work for this same curve?

The simple idea for (a) of identifying the coordinate data with a parameter $t$ in the range $1 \leq t \leq n$ and sample values of $t = i$ can lead to a poor-looking curve in the case of cubic spline interpolation. A better idea is to let $t$ be (approximately) the arc length distance to points on the curve, that is:
$$
\begin{aligned}
  t_{i+1} = t_i + \sqrt{(x_{i+1} - x_i)^2 + (y_{i+1} - y_i)^2}. \quad (3.1.1)
\end{aligned}
$$
