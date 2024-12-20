
##### Fourier Series
Consider a series of continuous data, where we are given a function $f(t)$ and a period $T$. We assume that our function is periodic with period $T$, that is, it has the property that:
$$
f(t \pm T) = f(t)
$$
Let:
$$
g(t) = \cos\left(\frac{2\pi k t}{T}\right) \quad\text{and}\quad h(t) = \sin\left(\frac{2 \pi kt}{T}\right)
$$
have such a property for every since integer such that $f(t \pm T) = f(t)$. 
$$
h(t + T) = \sin \left(\frac{2 \pi k(t + T)}{T}\right) = \sin \left(\frac{2 \pi kt}{T} + 2 \pi\right) = h(t)
$$
We want to represent $f(t)$ in terms of a sum of trig function:
$$
f(t) = a_0 + \sum_{k = 1}^\infty a_k \cos \left(\frac{2 \pi kt}{T}\right) + \sum_{k = 1}^\infty b_k \sin \left(\frac{2 \pi kt}{T}\right)
$$
where:
- $a_k, b_k$ represent the amount of information in a harmonic period $T/k$ or frequency $k/T$
- we assume that we have a function defined on $t \in [0, 2\pi]$ with $T = 2\pi$ 

##### Orthogonality Identities
$$
\begin{align*} \int_0^{2\pi} \cos(kt) \sin(k't) \, dt &= 0 \quad ; \text{ for all } k, k' \\ \int_0^{2\pi} \cos(kt) \cos(k't) \, dt &= 0 \quad ; \ k \ne k' \\ \int_0^{2\pi} \sin(kt) \sin(k't) \, dt &= 0 \quad ; \ k \ne k' \\ \int_0^{2\pi} \sin(kt) \, dt &= 0 \\ \int_0^{2\pi} \cos(kt) \, dt &= 0. \end{align*}
$$


##### Finding Formulas for $a_k, b_k$ in terms of Input
Since we have functions $1, \cos kt, \sin kt$ that have the properties listed previously,, these functions are orthogonal on $[0, 2\pi]$, thus we can obtain:
$$
\begin{align*} a_0 &= \frac{\int_0^{2\pi} f(t) \, dt}{2\pi}, \\ a_k &= \frac{\int_0^{2\pi} f(t) \cos(kt) \, dt}{\int_0^{2\pi} \cos^2(kt) \, dt}, \\ b_k &= \frac{\int_0^{2\pi} f(t) \sin(kt) \, dt}{\int_0^{2\pi} \sin^2(kt) \, dt}. \end{align*}
$$
However, we can write our Fourier Series more compactly:
$$
f(t) = a_0 + \sum_{k = 1}^\infty a_k \cos \left(\frac{2 \pi kt}{T}\right) + \sum_{k = 1}^\infty \sin \left(\frac{2 \pi kt}{T}\right)
$$
which can be represented concisely via Euler's formula:
$$
e^{i \cdot \theta} = \cos(\theta) + i\sin(\theta)
$$
where $i = \sqrt{-1}$. Using the identity above, we can get that:
$$
\cos(\theta) = \frac{e^{i \cdot \theta} + e^{i \cdot \theta}}{2} \quad\text{and}\quad \sin(\theta) = \frac{e^{i \cdot \theta} - e^{i \cdot \theta}}{2i}
$$
Thus, we can write,
$$
f(t) = \sum_{k = -\infty}^\infty c_k e^{ikt}
$$
where $c_k$ are complex numbers, with the correspondence between $c_k$ and $a_k, b_k$ representation given by:
$$
c_0 = a_0, \quad c_k = \frac{a_k}{2} - i \cdot \frac{b_k}{2} \quad\text{and}\quad c_{-k} = \frac{a_k}{2} + i \cdot \frac{b_k}{2} \quad\text{for } k > 0
$$
$$
|c_0| = |a_0|, |c_k| = |c_{-k}| = \frac{1}{2}\sqrt{a_k^2 + b_k^2} \quad\text{for } k > 0
$$
Thus, we can get a formula for $c_l$:
$$
\begin{align*}
\int_0^{2 \pi} e^{ikt} e^{-ilt} dt &= 0; \quad k \neq l\\
&= 2 \pi; \quad k = l\\
\\
c_l &= \frac{1}{2\pi} \int_0^{2\pi} e^{-ilt} f(t) dt\\
f(t) &\approx \sum_{k = -M}^M c_k e^{i kt}
\end{align*}
$$
Thus, for typical functions $f(t)$, there is a small amount of information in the high frequency harmonics, therefore, we can approximate any input signal $f(t)$ by the final equation above for $M$ is not too large. 


#### Discrete Fourier Transform 
We usually do NOT have an analytical expression for $f(t)$. We usually have an input signal sampled with $N$ samples over a period $T$ with samples being $\Delta t = T / N$ apart.

Denote the input signal by $f(n \Delta t) = f_n$ and so the input sampled data is given by the set $f_0, f_1, \dots, f_{N - 1}$. Note that $f(0) = f(T)$ so that the given $f(0), f(T)$ is redundant. 

Let the discrete sample time be:
$$
t_n = n \Delta t = \frac{nT}{N} \quad\text{for } n = 0, 1, \dots, N - 1
$$
Given a finite set of points $(t_0, f_0), \dots, (t_{N - 1}, f_{N - 1})$, we wish to find some constants $a_0, a_k, b_k$ such that:
$$
a_0 + \sum_{k = 0}^M a_k \cos \left(\frac{2\pi}{T} kt\right) + \sum_{k = 0}^M b_k \sin \left(\frac{2 \pi}{T} kt\right)
$$
for some $M$ interpolates these values. Since we have $N$ sample points, we can fit this data exactly is we use $N$ degrees of freedom. 

Therefore we approximate the input $f(t)$ by where $N$ is assumed to be even:
$$
f(t) \approx \sum_{-k = N / 2 + 1}^{N / 2} c_k e^{\frac{i 2\pi k t}{T}}
$$
For the above, we need formulas for $c_k$ in terms of $f(n \Delta t) = f_n$ where $t_n = n \Delta t = \frac{nT}{N}$
$$
\begin{align*}
f(n\Delta t) &= f_n = \sum_{k = -N / 2 + 1}^{N / 2} c_k e^{i \frac{2 \pi k t}{T}}\\
f_n &= \sum_{k = -N/2 + 1}^{N / 2} c_k e^{i \frac{2\pi n k}{N}}\\
&= \sum_{k = 0}^{N / 2} c_k e^{i \frac{2 \pi nk}{N}} + \sum_{k = -N/2 + 1}^{-1} c_k e^{i \frac{2\pi nk}{N}}\\
\sum_{k = -N/2 + 1}^{-1} c_k e^{i \frac{2 \pi n k}{N}} &= \sum_{j = N/2 + 1}^{N - 1} c_{j - N} e^{i \frac{2\pi n (j - N)}{N}} \quad\text{ where } j = N + k\\
&= \sum_{j = N/2 + 1}^{N - 1} c_{j - N} e^{i \frac{2 \pi n j}{N}}
\end{align*}
$$
$$
e^{i \frac{2\pi n (-N)}{N}} = e^{-i 2\pi n} = 1 \implies c = c_{j - N} \text{ for } j = N/2 + 1, \dots, N - 1
$$
Thus, we let:
$$
F_k = c_l \quad\text{ and }\quad W = e^{\frac{i 2\pi}{N}}
$$
After some algebra, and re-arranging the above, we get our **DFT Pair**:
$$
\boxed{
\begin{align*}
f_n &= \sum_{j = 0}^{N - 1} F_j W^{nj}\\
F_k &= \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk}
\end{align*}
}
$$

**e.g.,** Given $f_n = \cos\left(\frac{4n\pi}{N}\right) + i\sin\left(\frac{4n\pi}{N}\right)$, we want to find a simple formula for $F_k$ given $f_n$ and while assuming that $N \ge 3$. 

**Solution: Determine $\theta$, then using $e^{i \theta}$, solve for F_k via sums and Geometric Series**
$$
\begin{align*} f_n &= \cos\left(\frac{4n\pi}{N}\right) + i\sin\left(\frac{4n\pi}{N}\right) \implies \theta = \frac{4n\pi}{N} \\ e^{i\theta} &= \cos\theta + i\sin\theta \\ 
f_n &= \cos\left(\frac{4n\pi}{N}\right) + i\sin\left(\frac{4n\pi}{N}\right) = e^{i\left(\frac{4n\pi}{N}\right)} \\ 
F_k &= \sum_{n=0}^{N-1} S_n \cdot e^{-i\left(\frac{2\pi kn}{N}\right)}, \quad k = 0, 1, \dots, N-1 \\ 
F_k &= \sum_{n=0}^{N-1} e^{i\left(\frac{4n\pi}{N}\right)} e^{-i\left(\frac{2\pi kn}{N}\right)} \\ 
&= \sum_{n=0}^{N-1} e^{i\left(\frac{n}{N}(4\pi - 2\pi k)\right)} \\ 
&= \sum_{n=0}^{N-1} e^{i\left(\frac{2\pi n}{N}(2 - k)\right)} \\ 
&= \sum_{n=0}^{N-1} \left(e^{i\frac{2\pi}{N}(2-k)}\right)^n \\ \sum_{n=0}^{N-1} r^n &= \frac{1 - r^N}{1 - r} \implies F_k = \frac{1 - \left[e^{i\frac{2\pi}{N}(2-k)}\right]^N}{1 - e^{i\frac{2\pi}{N}(2-k)}} \\ 
e^{i\frac{2\pi}{N}(2-k)N} &= e^{2\pi i(2-k)} = e^{i2\pi(2-k)} = 1 \\ 
F_k &= \begin{cases} N & \text{if } k = 2, \\ 0 & \text{otherwise.} \end{cases} \end{align*}
$$

e.g., Show that for $\{f_0, \dots, f_{N - 1}\} = \{0, ..., 0, 1, ..., 1\}$, $F_{2k} = 0$ for $k = 1, 2, \dots, \frac{N}{2} - 1$.
$$ 
\begin{align*} 
f_0, \dots, f_{N-1} &= \{ 0, \dots, 0, 1, \dots, 1 \} \\ 
f_n &= \begin{cases} 0 & \text{for } n = 0, \dots, \frac{N}{2} - 1, \\ 
1 & \text{for } n = \frac{N}{2}, \dots, N-1. 
\end{cases} 
\end{align*} 
$$$$ \begin{align*} F_k &= \sum_{n=0}^{N-1} f_n e^{-i \frac{2\pi kn}{N}} \quad \text{where} \, f_n = 0 \, \text{for} \, n = 0, 1, \dots, \frac{N}{2} - 1, \\ F_k &= \sum_{n = \frac{N}{2}}^{N-1} e^{-i \frac{2\pi kn}{N}}. \end{align*} $$$$ \begin{align*} \text{Let } n &= \frac{N}{2} + m \text{ for } m = 0, 1, \dots, \frac{N}{2} - 1, \\ F_k &= \sum_{m=0}^{\frac{N}{2} - 1} e^{-i \frac{2\pi k \left( \frac{N}{2} + m \right)}{N}} \\ &= e^{-i\pi k} \sum_{m=0}^{\frac{N}{2} - 1} e^{-i \frac{2\pi k m}{N}}. \end{align*} $$$$ \begin{align*} \sum_{m=0}^{M} r^m &= \frac{1 - r^{M+1}}{1 - r}, \quad r = e^{-i \frac{2\pi k}{N}}, \\ F_k &= e^{-i \pi k} \left( \frac{1 - (-1)^k}{1 - e^{-i \frac{2\pi k}{N}}} \right), \\ F_k &= (-1)^k \left( \frac{1 - (-1)^k}{1 - e^{-i \frac{2\pi k}{N}}} \right). \end{align*} $$


#### Discrete Fourier Transform (DFT) II
Previously, we had referred to Fourier series of continuous functions. Generally, the DFT is a reversible transformation of sequence of $N$ complex numbers. Specifically, it transforms the sequence $\{f_n \in \mathbb{C} | n = 0, \dots, N - 1\}$ which we will refer to as a data sample, into another sequence of $N$ complex numbers $\{F_k \in \mathbb{C} | k = 0, \dots, N - 1\}$ which we refer to as the Fourier coefficients of the sample. 

**Reversible**: If we are given the Fourier coefficients $\{F_k\}$, there is a simple inverse transformation by which we can reproduce the data sample $\{f_n\}$. 

Let $W = e^{2\pi i / N}$ be the $N$th root of unity. Then the Fourier coefficient $F_k$ is defined as:
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk}
$$
This is meant to apply for $0 \le k \le N - 1$. However, it applies for all integer values of $k$. Mathematically, we can think of $\{F_k\}$ as a doubly-infinite sequence, with only $N$ of the terms being independent, and the rest being correlated. 

##### Properties of $\{F_k\}$ 
**Double Infinite and Periodic**: Assume that $\{F_k | k = 0, ..., N - 1\}$. We have $N$ element in our data sample. Given that a set of integers, any integer $k$ can be expressed uniquely as $k = mN + p$ for some pair of integers $m, p$ with $0 \le p \le N - 1$. This means:
$$
k \equiv p (\text{ mod } N)
$$
Each $W^{-k}$:
$$
W^{-k} = e^{-2k \pi i / N} = e^{-2m N \pi i / N} e^{-2 p \pi i/ N} = W^{-p}
$$
This gives us:
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk} = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-np} = F_p
$$
Thus, $F_k$ is doubly infinite and a periodic sequence:
1. $-\infty < k < \infty$ 
2. $0 \le k \le N - 1$

**$F_k = \overline{F_{N - k}}$ for $f_n$ is real**
For $1 \le k \le N -1$ note that $N - k$ also ties between $1$ and $N - 1$, thus:
$$
\begin{align*}
W^{N - k} &= e^{2\pi i(N - k)/N}\\
&= e^{2 \pi i} e^{-2 \pi i k / N}\\
&= W^{-k}\\
\\
\implies F_k &= \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-kn} = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{(N - k)n} = \frac{1}{N} \sum_{n = 0}^{N - 1} \overline{f_n} \overline{W^{-(N - k)n}} = \overline{F}_{N - k}
\end{align*}
$$


#### Inverse Discrete Fourier Transform (IDFT)
Previously, we had mentioned that there is a simple transformation from the $F_k$ that recovers the original data sample values. If $F_n$ are defined:
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk}
$$
Then we can define the transformation that obtains the original data sample values back from $F_k$ (our generated Fourier coefficients) as:
$$
f_n = \sum_{n = 0}^{N - 1} F_k W^{nk}
$$
Let $f$ be the column vector of data samples, and let $F$ be the corresponding column vector of Fourier coefficients, then we can write:
$$
F = M \cdot f \quad\text{ where } M \text{ is the } N \times N \text{ matrix with } j\text{-th column} = \frac{1}{N}\overline{W(j)}
$$
Let $I$ be the $N \times N$ identity matrix, then we get:
$$
M^{-1} = N \overline{M}^t \implies f = N \overline{M}^t F
$$
The lack of standardization for the formula used to define the Fourier Transform leads it to not having a standardized methods of inverse transformation. The differences do not affect the basic role of the transform as an alternative representation of a sequence of real numbers, but do result in different numerical values for the coefficients of the transform. 

Let $U$ be the two possible basic $n$th roots of unity $e^{\pm i 2\pi / N}$ that can be used for the transform
$$
F_n = sc \sum_{k = 0}^{N - 1} f_k (\overline{U}^n)^k \implies f_n = \frac{1}{N sc} \sum_{k = 0}^{N - 1} F_k(U^n)^k
$$
Using $U = E^{2 \pi i / N}$ and $sc = \frac{1}{N}$. 



#### Fast Fourier Transform 
Again, we would like to find:
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk}
$$
for $k = 0, \dots, N - 1$ where $W = e^{2\pi i/N}$ is a complex $N$th root of unity. We do this in the standard way with time complexity $O(N^2)$ complex floating point operations. 

Now, we show a method on how to find these $N$ quantities $F_0, \dots, F_{N - 1}$ in $O(N \text{log}_2 N)$ time using a divide and conquer approach. 

Assume that $N = 2^m$ for some $m$. If this is not the case, we pad the input signal with zeros (which does not effect the sums).
Then, we **split** the sums into two parts:
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N/2 - 1} f_n W^{-nk} + \frac{1}{N} \sum_{n = N / 2}^{N - 1} f_n W^{-nk}
$$
Set $m = n - \frac{N}{2}$, which gives:
$$
\begin{align*} 
F_k &= \frac{1}{N} \sum_{n=0}^{N/2-1} f_n W^{-nk} + \frac{1}{N} \sum_{m=0}^{N/2-1} f_{m+N/2} W^{-k(m+N/2)} \\ 
&= \frac{1}{N} \sum_{n=0}^{N/2-1} f_n W^{-nk} + \frac{1}{N} \sum_{n=0}^{N/2-1} f_{n+N/2} W^{-nk} W^{-kN/2} \\ 
&= \frac{1}{N} \sum_{n=0}^{N/2-1} \left( f_n + W^{-kN/2} f_{n+N/2} \right) W^{-nk}
\end{align*}
$$
Note: $W^{-k N / 2} = e^{ik \pi} = (-1)^k$, thus our **even** coefficients:
$$
F_{2k} = \frac{1}{N} \sum_{n = 0}^{N/2 - 1} (f_n + f_{n + N / 2})W^{-2nk}
$$
and for **odd** coefficients become:
$$
\begin{align*}
F_{2k + 1} &= \frac{1}{N} \sum_{n = 0}^{N / 2 - 1} (f_n - f_{n + N/2})W^{-n(2k + 1)}\\
&= \frac{1}{N} \sum_{n = 0}^{N/2 - 1} [(f_n - f_{n + N/2})W^{-n}] W^{-2nk}
\end{align*}
$$
where $k = 0, \dots, N / 2 - 1$

Thus, when we recombine our sample into two half-length sequences $\{g_n\}$ and $\{h_n\}$ using:
$$
\boxed{
\begin{align*}\\
\text{ }g_n &= \frac{1}{2}(f_n + f_{n + N/2})\\
\text{ }h_n &= \frac{1}{2}(f_n - f_{n + N/2})W^{-n}\\
\\
\end{align*}
\quad\text{ where } n = 0, \dots, N / 2 - 1\text{ }
}
$$
Then we have:
$$
\begin{align*}
F_{2k} &= \frac{2}{N} \sum_{n = 0}^{N/2 - 1} g_n W^{-2nk}\\
F_{2k + 1} &= \frac{2}{N} \sum_{n = 0}^{N / 2 - 1} h_n W^{-2nk}
\end{align*}
$$
for $k = 0, \dots, N / 2-1$, with:
$$
W^{-2nk} = e^{-\frac{2n\pi}{N} 2nk} = \left(e^{- \frac{-2 \pi i}{N/2}}\right)^{nk}
$$


#### A Note on Roots of Unity
$$
\begin{align*}
\text{The roots of unity } e^{-i\frac{2\pi k}{N}} \text{ for } k = 0, 1, \dots, N-1 \text{ are:} \\
\text{For } N \text{ points: } e^{-i\frac{2\pi k}{N}} &= 
\begin{cases}
1 & \text{if } k = 0, \\
e^{-i\frac{\pi}{2}} = -i & \text{if } k = \frac{N}{4}, \\
e^{-i\pi} = -1 & \text{if } k = \frac{N}{2}, \\
e^{-i\frac{3\pi}{2}} = i & \text{if } k = \frac{3N}{4}.
\end{cases} \\
\text{General form: } e^{-i\frac{2\pi k}{N}} &= \cos\left(\frac{2\pi k}{N}\right) - i\sin\left(\frac{2\pi k}{N}\right).
\end{align*}
$$


#### FFT (Butterfly) Algorithm
$$
F_k = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n W^{-nk} \quad\text{and}\quad f_n = \sum_{k = 0}^{N - 1} F_k W^{kn}
$$
for the input of complex data $f_0, f_1, \dots, f_{N - 1}$, with $N = 2^m$ computes:
$$
f_k' = \frac{1}{N} \sum_{n = 0}^{N - 1} f_n U^{nk}
$$
We overwrite the original array $f$ with $f'$, which results in an output array which is bit-reversed in order, so it must later be unscrambled. 

**For Forward FFT**:
$$
U = W^{-1} = e^{-i \frac{2\pi}{N}}
$$
**For Reverse FFT**:
$$
U = W = e^{i \frac{2\pi}{N}}
$$
and the $f_k'$ is multiplied by $N$. 


#### **FFT Algorithm (Butterfly) Walk-through
We are given an input sequence:

$$ \mathbf{f} = (1, 2, 3, 4, 1, 2, 3, 4) \quad (N = 8). $$

The Fast Fourier Transform (FFT) algorithm recursively applies **butterfly operations** to compute the Fourier Transform efficiently.

The steps involve:
1. Splitting the sequence into smaller parts.
2. Performing recursive operations to combine the results.
3. Unscrambling the output to produce the final result.

##### **Splitting into Half-Length Sequences**
The first step divides the input vector $\mathbf{f}$ into two **half-length sequences**:

1. **Even indices:** Values at positions $0, 2, 4, 6$.
2. **Odd indices:** Values at positions $1, 3, 5, 7$.

The sequences are:
$$
\mathbf{g} = (1, 2, 3, 4), \quad \mathbf{h} = (0, 0, 0, 0).
$$
- $\mathbf{g}$: Represents the sum of the even indices.
- $\mathbf{h}$: Represents the differences between the even and odd indices, initially 0 because the odd indices equal the even indices in this example.

At this stage, the input $\mathbf{f}$ is:
$$
\mathbf{f} = \begin{bmatrix} 1 \\
2 \\
3 \\
4 \\
1 \\
2 \\
3 \\
4 \end{bmatrix} \implies \begin{bmatrix} 1 \\
2 \\
3 \\
4 \\
0 \\
0 \\
0 \\
0 \end{bmatrix}
$$

##### **Step 2: Recursive Butterfly Operations**
The FFT uses **butterfly operations** to combine and split the input recursively:
The first butterfly operation combines adjacent pairs using the following formulas:
1. **Sum** of the elements:
   $$
   g_n = f_n + f_{n+N/2}.
   $$
2. **Difference** of the elements with a phase factor:
   $$
   h_n = (f_n - f_{n+N/2}) W^{-n}, \quad W = e^{-i \frac{2\pi}{N}}.
   $$
###### **Given Input:**
$$
\mathbf{f} = \begin{bmatrix} 1 \\
2 \\
3 \\
4 \\
1 \\
2 \\
3 \\
4 \end{bmatrix}, \quad N = 8.
$$

1. **Pair 1:** $f_0$ and $f_4$
   - Sum: $g_0 = f_0 + f_4 = 1 + 1 = 2$
   - Difference: $h_0 = (f_0 - f_4) W^0 = (1 - 1) W^0 = 0$

2. **Pair 2:** $f_1$ and $f_5$
   - Sum: $g_1 = f_1 + f_5 = 2 + 2 = 3$
   - Difference: $h_1 = (f_1 - f_5) W^{-1} = (2 - 2) W^{-1} = 0$

3. **Pair 3:** $f_2$ and $f_6$
   - Sum: $g_2 = f_2 + f_6 = 3 + 3 = 6$
   - Difference: $h_2 = (f_2 - f_6) W^{-2} = (3 - 3) W^{-2} = 0$

4. **Pair 4:** $f_3$ and $f_7$
   - Sum: $g_3 = f_3 + f_7 = 4 + 4 = 8$
   - Difference: $h_3 = (f_3 - f_7) W^{-3} = (4 - 4) W^{-3} = 0$

The sums and differences overwrite the original array. After the first butterfly operation, we have:
$$
\mathbf{f} = \begin{bmatrix} 2 \\
3 \\
6 \\
8 \\
0 \\
0 \\
0 \\
0 \end{bmatrix}.
$$
- **2, 3, 6, and 8** are the sums of the pairs.
- **0s** are the differences, as the odd indices matched the even indices.

In the next recursion step, the sums (2, 3, 6, 8) are further combined:
1. **Pair 1:** $g_0 = 2$ and $g_2 = 6$
   - Sum: $g'_0 = \frac{2 + 6}{2} = 4$
   - Difference: $h'_0 = \frac{2 - 6}{2} = -2$

2. **Pair 2:** $g_1 = 3$ and $g_3 = 8$
   - Sum: $g'_1 = \frac{3 + 8}{2} = 5.5$
   - Difference: $h'_1 = \frac{3 - 8}{2} = -2.5$

- **$-0.5 + 0.5i$ and $-0.5 - 0.5i$:** Represent contributions from higher frequency components.
- **0:** Indicates no contributions from other frequencies.

Once all butterfly operations are complete, the sequence $\mathbf{f}$ is in a bit-reversed order. The output must be unscrambled to produce the correct Fourier coefficients.

The final output is:
$$
\mathbf{F} = \begin{bmatrix} 2.5 \\
0 \\
-0.5 + 0.5i \\
0 \\
-0.5 \\
0 \\
-0.5 - 0.5i \\
0 \end{bmatrix}.
$$



#### Aliasing 
Let's take the periodic signal representation of our Fourier series:
$$
f(t) = \sum_{k = -\infty}^{\infty} c_k e^{i \frac{2\pi kt}{T}}
$$
where $T$ is the period and the equation is considered exact. Suppose that we have $t_n = \frac{Tn}{N}$, which then turns the equation above into:
$$
f(t_n) = f_n = \sum_{-\infty}^{\infty} c_k e^{i \frac{2\pi kn}{N}}
$$
Notice that the index $k$ in the equation above runs from $[-N/2 + 1, ..., +N/2]$ so that it corresponds to the previous equation. However, how closely does $F_k$ correspond to $c_k$ for $k \in [-N/2 + 1, +N/2]$, and how can we determine the precise relationship between $F_k$ and $c_k$  using the orthogonality principals?
$$
\begin{align*}
\sum_{p = 0}^{N - 1} e^{i \frac{2 \pi p(1 - k)}{N}} &= N \delta_{k, l}\\
\sum_{p = -N/2 + 1}^{N / 2} e^{i \frac{2\pi p(l - k)}{N}} &= N \delta_{k, l}\\
\implies F_l &= \frac{1}{N} \sum_{n = -N/2 + 1}^{N / 2} f_n e^{-i \frac{2\pi n l}{N}}\\
&= \frac{1}{N} \sum_{n = -N/2 + 1}^{N/2} e^{-i \frac{2\pi nl}{N}} \sum_{k = -\infty}^{\infty} c_k e^{i \frac{2\pi kn}{N}}\\
&= \sum_{k = -\infty}^{\infty} c_k \frac{1}{N} \sum_{n = -N/2 + 1}^{N/2} e^{i \frac{2\pi n(k - l)}{N}}
\end{align*}
$$


#### Correlation Function 
Consider we have two periodic input arrays $y_i = z_i$ for $i = 0, \dots, N - 1$. 

The correlation function $\phi_n$ is defined as the following:
$$
\phi_n = \frac{1}{N} \sum_{i = 0}^{i = N-1} y_{i + n} \cdot z_i
$$
Suppose the maximum of $\phi$ occurs at $n = p$. Then, we know that if we shift the array $z$ by $p$ positions to the right, then this gives us the best match (the maximum positive correlation) between arrays $y$ and $z$. 

$z$ can be regarded as a reference signal and we can imagine sliding $z$ to the right by $p$ units in order to get the best match with $y$. A naive evaluation of the equation above would require $N^2$ flops. However, we can do this using Fast Fourier Transforms in $O(N \log N)$ operations:
$$
\begin{align*}
y_{l + n} &= \sum_{m = 0}^{N - 1} Y_m W^{m(l + n)}\\
z_l &= \sum_{r = 0}^{N - 1} Z_r W^{rl} \quad\text{ where } Y = FFT(y),\quad Z = FFT(z)\\
\\
\text{Let } \Phi = FFT(\phi)\\
\Phi_k &= \frac{1}{N} \sum_{n = 0}^{N - 1} \phi_n W^{-nk}
\end{align*}
$$
Then when we substitute the above for the preceding equation:
$$
\begin{align*} 
\Phi_k &= \frac{1}{N^2} \sum_{n=0}^{N-1} \sum_{m=0}^{N-1} \sum_{r=0}^{N-1} \sum_{l=0}^{N-1} Y_m Z_r W^{-n(k-m)} W^{l(r+m)} \\ &= \frac{1}{N^2} \sum_{n=0}^{N-1} \sum_{m=0}^{N-1} \sum_{r=0}^{N-1} Y_m Z_r W^{-n(k-m)} \sum_{l=0}^{N-1} W^{l(r+m)} \\ &= \frac{1}{N^2} \sum_{n=0}^{N-1} \sum_{m=0}^{N-1} \sum_{r=0}^{N-1} Y_m Z_r W^{-n(k-m)} \delta_{r, N-m} N \\ &= \frac{1}{N} \sum_{n=0}^{N-1} \sum_{m=0}^{N-1} Y_m Z_{N-m} W^{-n(k-m)} \\ &= \frac{1}{N} \sum_{m=0}^{N-1} Y_m Z_{N-m} \sum_{n=0}^{N-1} W^{n(m-k)} \\ &= \frac{1}{N} \sum_{m=0}^{N-1} Y_m Z_{N-m} \delta_{m,k} N \\ &= Y_k Z_{N-k} \\ &= Y_k Z_k \, . \end{align*}
$$


#### 2-D Fast Fourier Transform 
A grey scale image is represented by a 2D array of grey scale values $f_{n, j}, n = 0, \dots, N - 1$ and $j = 0, \dots, M - 1$. In this case, the 2D DFT uses two roots of unity, then, by defining:
$$
W_N = e^{2 \pi i/N} \quad\text{ and }\quad W_M = e^{2 \pi i / N}
$$
Then, we have that,
$$
F_{k, l} = \frac{1}{NM} \sum_{n = 0}^{N - 1} \sum_{j = 0}^{M - 1} f_{n, j} W_N^{-nk} W_M^{-jl}
$$
We can rewrite the equation above as:
$$
\begin{align*}
F_{k, l} &= \frac{1}{NM} \sum_{n = 0}^{N - 1} W_N^{-nk} \sum_{j = 0}^{M - 1} f_{n, j} W_M^{-jl}\\
&= \frac{1}{N} \sum_{n = 0}^{N - 1} W_N^{-nk} H_{n, l}
\end{align*}
$$
where we define $H_{n, l} = \frac{1}{M} \sum_{j = 0}^{M - 1} f_{n, j} W_M^{-jl}$. 




### Exercises

**Question 1**
Let $\{f_n\}$, $n = 0, 1, \dots, N-1$, be $N$ samples of a real signal, and $N$ be even
(a) Show that $W^{-nk} + W^{-(N-n)k} = 2\cos\left(\frac{2\pi nk}{N}\right)$
$$
\begin{align*}
W &= e^{-i\frac{2\pi}{N}}, \\
W^{-nk} &= e^{-i\frac{2\pi nk}{N}}, \\
W^{-(N-n)k} &= e^{-i\frac{2\pi (N-n)k}{N}} = e^{-i\frac{2\pi Nk}{N}} \cdot e^{i\frac{2\pi nk}{N}}, \\
e^{-i\frac{2\pi Nk}{N}} &= 1, \\
W^{-(N-n)k} &= e^{i\frac{2\pi nk}{N}}, \\
W^{-nk} + W^{-(N-n)k} &= e^{-i\frac{2\pi nk}{N}} + e^{i\frac{2\pi nk}{N}}, \\
e^{ix} + e^{-ix} &= 2\cos(x), \\
W^{-nk} + W^{-(N-n)k} &= 2\cos\left(\frac{2\pi nk}{N}\right).
\end{align*}
$$
(b) Suppose the signal $\{f_n\}$ is an even function; i.e., $f_n = f_{N-n}$, $n = 1, 2, \dots, N/2-1$. 
Show that $F_k$ is real.
$$
\begin{align*}
F_k &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi nk}{N}}, \\
f_n &= f_{N-n}, \quad n = 1, 2, \dots, \frac{N}{2}-1, \\
F_k &= \sum_{n=0}^{N/2} f_n e^{-i\frac{2\pi nk}{N}} + \sum_{m=1}^{N/2-1} f_m e^{i\frac{2\pi mk}{N}}, \\
e^{-i\frac{2\pi nk}{N}} + e^{i\frac{2\pi nk}{N}} &= 2\cos\left(\frac{2\pi nk}{N}\right), \\
F_k &= 2 \sum_{n=0}^{N/2} f_n \cos\left(\frac{2\pi nk}{N}\right).
\end{align*}
$$

##### Question
Suppose the signal $\{f_n\}$ is a square signal, defined as:
$$
f_n = 
\begin{cases}
0 & \text{if } n = 0, 1, \dots, \frac{N}{4} - 1 \text{ or } n = \frac{3N}{4}, \frac{3N}{4} + 1, \dots, N - 1, \\
1 & \text{if } n = \frac{N}{4}, \frac{N}{4} + 1, \dots, \frac{3N}{4} - 1.
\end{cases}
$$
Show that $F_{2k} = 0$, $k = 1, 2, \dots, \frac{N}{2} - 1$.
$$
\begin{align*}
F_k &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi nk}{N}}, \\
F_{2k} &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi n (2k)}{N}}, \\
F_{2k} &= \sum_{n \in A} 0 \cdot e^{-i\frac{4\pi nk}{N}} + \sum_{n \in B} 1 \cdot e^{-i\frac{4\pi nk}{N}} + \sum_{n \in C} 0 \cdot e^{-i\frac{4\pi nk}{N}}, \\
&= \sum_{n=\frac{N}{4}}^{\frac{3N}{4}-1} e^{-i\frac{4\pi nk}{N}}, \\
e^{-i\frac{4\pi nk}{N}} &= e^{-i\frac{4\pi (n + \frac{N}{2})k}{N}} = -e^{-i\frac{4\pi nk}{N}}, \\
\text{Thus: } F_{2k} &= \sum_{n=\frac{N}{4}}^{\frac{3N}{4}-1} e^{-i\frac{4\pi nk}{N}} + \sum_{n=\frac{N}{4}}^{\frac{3N}{4}-1} -e^{-i\frac{4\pi nk}{N}} = 0.
\end{align*}
$$

##### Question
For a given input sequence $f_i$ why is the first component of its DFT, $F_0$ always equal to the average of the components of $f_i$?
$$
\begin{align*}
F_k &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi nk}{N}}, \\
F_0 &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi n \cdot 0}{N}}, \\
F_0 &= \sum_{n=0}^{N-1} f_n \cdot 1, \\
F_0 &= \sum_{n=0}^{N-1} f_n, \\
\text{Average of } f_i &= \frac{1}{N} \sum_{n=0}^{N-1} f_n, \\
F_0 &= N \cdot \text{(Average of } f_i).
\end{align*}
$$


##### Question
For a constant $c$ determine the DFT of a signal $f_0 + c, \dots, f_{N - 1} + c$ in terms of the DFT of the signal $f_0, \dots, f_{N- 1}$. 
$$
\begin{align*}
\text{Let } g_n &= f_n + c, \quad n = 0, 1, \dots, N-1, \\
G_k &= \sum_{n=0}^{N-1} g_n e^{-i\frac{2\pi nk}{N}}, \\
G_k &= \sum_{n=0}^{N-1} (f_n + c) e^{-i\frac{2\pi nk}{N}}, \\
G_k &= \sum_{n=0}^{N-1} f_n e^{-i\frac{2\pi nk}{N}} + \sum_{n=0}^{N-1} c e^{-i\frac{2\pi nk}{N}}, \\
G_k &= F_k + c \sum_{n=0}^{N-1} e^{-i\frac{2\pi nk}{N}}, \\
\text{For } k = 0, &\quad \sum_{n=0}^{N-1} e^{-i\frac{2\pi n \cdot 0}{N}} = N, \\
\text{For } k \neq 0, &\quad \sum_{n=0}^{N-1} e^{-i\frac{2\pi nk}{N}} = 0, \\
G_0 &= F_0 + cN, \\
G_k &= F_k \quad \text{for } k = 1, 2, \dots, N-1.
\end{align*}
$$


##### Question
Calculate the Discrete Fourier Transform of the following periodic time sequences by hand, both using the direct DFT formula.
1. $f[n] = (1, 2, 2, 1) \quad (n = 0, \dots, 3; \ N = 4)$

2. $f[n] = (1, 2, 3, 2) \quad (n = 0, \dots, 3; \ N = 4)$

Why is the resulting transform real in this case?
$$
\begin{align*}
\text{The DFT formula is: } F_k &= \sum_{n=0}^{N-1} f[n] e^{-i\frac{2\pi nk}{N}}. \\
\textbf{(i) For } f[n] = (1, 2, 2, 1): \\
F_0 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 0}{4}} = 1 + 2 + 2 + 1 = 6, \\
F_1 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 1}{4}} = 1 + 2e^{-i\frac{\pi}{2}} + 2e^{-i\pi} + 1e^{-i\frac{3\pi}{2}}, \\
&= 1 + 2(-i) + 2(-1) + 1(i) = 0, \\
F_2 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 2}{4}} = 1 + 2e^{-i\pi} + 2e^{-i2\pi} + 1e^{-i3\pi}, \\
&= 1 + 2(-1) + 2(1) + 1(-1) = 0, \\
F_3 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 3}{4}} = 1 + 2e^{-i\frac{3\pi}{2}} + 2e^{-i3\pi} + 1e^{-i\frac{9\pi}{2}}, \\
&= 1 + 2(i) + 2(-1) + 1(-i) = 0. \\
\textbf{Result: } F_k &= (6, 0, 0, 0).
\end{align*}
$$
$$
\begin{align*}
\textbf{(ii) For } f[n] = (1, 2, 3, 2): \\
F_0 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 0}{4}} = 1 + 2 + 3 + 2 = 8, \\
F_1 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 1}{4}} = 1 + 2e^{-i\frac{\pi}{2}} + 3e^{-i\pi} + 2e^{-i\frac{3\pi}{2}}, \\
&= 1 + 2(-i) + 3(-1) + 2(i) = 0, \\
F_2 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 2}{4}} = 1 + 2e^{-i\pi} + 3e^{-i2\pi} + 2e^{-i3\pi}, \\
&= 1 + 2(-1) + 3(1) + 2(-1) = 0, \\
F_3 &= \sum_{n=0}^3 f[n] e^{-i\frac{2\pi n \cdot 3}{4}} = 1 + 2e^{-i\frac{3\pi}{2}} + 3e^{-i3\pi} + 2e^{-i\frac{9\pi}{2}}, \\
&= 1 + 2(i) + 3(-1) + 2(-i) = 0. \\
\textbf{Result: } F_k &= (8, 0, 0, 0).
\end{align*}
$$
$$
\begin{align*}
\text{Why is the transform real?} \\
\text{The sequences are symmetric (even symmetry), and for real signals:} \\
f[n] = f[N-n] \implies F_k \text{ is real.}
\end{align*}
$$


##### Question: From Assignment 4
Using the DFT formula:
$$
F_k = \sum_{n=0}^{N-1} x_n e^{-i\frac{2\pi kn}{N}},
$$
we compute the DFT of $f_{\text{even}} = [1, 1, -1, -1]$ and $f_{\text{odd}} = [4, -4, 4, -4]$.
$$
\begin{align*}
G_0 &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\frac{2\pi \cdot 0 \cdot n}{4}} \\
    &= \sum_{n=0}^3 f_{\text{even}, n} = 1 + 1 + (-1) + (-1) \\
    &= 0.
\end{align*}
$$
$$
\begin{align*}
G_1 &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\frac{\pi n}{2}} \\
    &= (1)(1) + (1)(-i) + (-1)(-1) + (-1)(i) \\
    &= 1 - i + 1 - i \\
    &= 2 - 2i.
\end{align*}
$$
$$
\begin{align*}
G_2 &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\frac{2\pi \cdot 2 \cdot n}{4}} \\
    &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\pi n} \\
    &= (1)(1) + (1)(-1) + (-1)(1) + (-1)(-1) \\
    &= 1 - 1 - 1 + 1 \\
    &= 0.
\end{align*}
$$
$$
\begin{align*}
G_3 &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\frac{2\pi \cdot 3 \cdot n}{4}} \\
    &= \sum_{n=0}^3 f_{\text{even}, n} \cdot e^{-i\frac{3\pi n}{2}} \\
    &= (1)(1) + (1)(i) + (-1)(-1) + (-1)(-i) \\
    &= 1 + i + 1 + i \\
    &= 2 + 2i.
\end{align*}
$$
$$
\begin{align*}
G_k &= [G_0, G_1, G_2, G_3] = [0, 2 - 2i, 0, 2 + 2i].
\end{align*}
$$
Using the DFT formula: $$ 
F_k = \sum_{n=0}^{N-1} x_n e^{-i\frac{2\pi kn}{N}}
$$we compute the DFT for $f_{\text{odd}} = [4, -4, 4, -4]$
$$
\begin{align*} H_0 &= \sum_{n=0}^3 f_{\text{odd}, n} \cdot e^{-i\frac{2\pi \cdot 0 \cdot n}{4}} \\ &= \sum_{n=0}^3 f_{\text{odd}, n} \\ &= 4 + (-4) + 4 + (-4) \\ &= 0. \end{align*}
$$
$$
\begin{align*}
H_1 &= \sum_{n=0}^3 f_{\text{odd}, n} \cdot e^{-i\frac{\pi n}{2}} \\
    &= (4)(1) + (-4)(-i) + (4)(-1) + (-4)(i) \\
    &= 4 + 4i - 4 - 4i \\
    &= 0.
\end{align*}
$$
$$
\begin{align*}
e^{-i\frac{\pi \cdot 0}{2}} &= 1, \\
e^{-i\frac{\pi \cdot 1}{2}} &= -i, \\
e^{-i\frac{\pi \cdot 2}{2}} &= -1, \\
e^{-i\frac{\pi \cdot 3}{2}} &= i.
\end{align*}
$$
$$
\begin{align*}
H_1 &= \sum_{n=0}^3 f_{\text{odd}, n} \cdot e^{-i\frac{\pi n}{2}} \\
    &= (4)(1) + (-4)(-i) + (4)(-1) + (-4)(i) \\
    &= 4 + 4i - 4 - 4i \\
    &= 0.
\end{align*}
$$
$$
\begin{align*}
e^{-i\pi \cdot 0} &= 1, \\
e^{-i\pi \cdot 1} &= -1, \\
e^{-i\pi \cdot 2} &= 1, \\
e^{-i\pi \cdot 3} &= -1.
\end{align*}
$$
$$
\begin{align*}
H_2 &= \sum_{n=0}^3 f_{\text{odd}, n} \cdot e^{-i\pi n} \\
    &= (4)(1) + (-4)(-1) + (4)(1) + (-4)(-1) \\
    &= 4 + 4 + 4 + 4 \\
    &= 16.
\end{align*}
$$
$$
\begin{align*}
e^{-i\frac{\pi \cdot 0}{2} \cdot 3} &= 1, \\
e^{-i\frac{\pi \cdot 1}{2} \cdot 3} &= i, \\
e^{-i\frac{\pi \cdot 2}{2} \cdot 3} &= -1, \\
e^{-i\frac{\pi \cdot 3}{2} \cdot 3} &= -i.
\end{align*}
$$
$$
\begin{align*}
H_3 &= \sum_{n=0}^3 f_{\text{odd}, n} \cdot e^{-i\frac{3\pi n}{2}} \\
    &= (4)(1) + (-4)(i) + (4)(-1) + (-4)(-i) \\
    &= 4 - 4i - 4 + 4i \\
    &= 0.
\end{align*}
$$
