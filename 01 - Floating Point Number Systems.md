### Real Number System ($\mathbb{R}$)
The real number system is infinite in two senses:
1. **Infinite in extent**: there are numbers $x \in \mathbb{R}$ such that $|x|$ is arbitrarily large 
2. **Infinite in density**: any interval $I = \{x | a \le x \le b\}$ of $\mathbb{R}$ is an infinite set 

Computers can only represent a finite set of numbers, so all the actual implementations of algorithms must use approximations to $\mathbb{R}$ and inexact arithmetic.

**Normalized Digital Expansion relative to base number for the digits.** 

By **normalized**, we mean that we have re-written the number so that the **first non-zero digit** lies one place to the **right** of the decimal point; i.e. shifting the decimal point by multiplying by some power of the base number. 

Each positive number in $\mathbb{R}$ can be represented by an infinite base $\beta$ expansion in the normalized form given as:
$$
0.d_1 d_2 d_3 \cdots \times \beta^p
$$
where:
- $d_k$ are digits in base $\beta$ that is $0, 1, ..., \beta - 1$ 
- normalized mean that $d_1 \neq 0$ 
- the exponent $p$ is an integer (positive, negative or zero)

### Floating Point Number Format
Floating point number systems limit the infinite density of $\mathbb{R}$ by allowing only a finite number, $t$, of digits in the significand. They also limit the infinite extent of $\mathbb{R}$ by allowing only a finite set of integer values for the exponent $p$, that is $L \le p \le U$ for specified integers $L < 0$ and $U > 0$.

For floating point number system $F$, we characterize it by:
$$
\{\beta, t, L, U\}
$$
where:
$$
\pm 0.d_1 d_2 d_3 ... d_t \times \beta^p \text{ for } L \le p \le U \text{ and } d_1 \neq 0 \quad \text{or } 0
$$
- If exponent $p$ is too big ($> U$) or too small ($< L$), then our system cannot represent number 
- When arithmetic operations generate such a number, this is called overflow (too big) or underflow (too small) respectively.

**Underflow**: Answers simply round to 0 instead 
**Overflow**: Answers produce $\pm \infty$ or NaN (not a number)

The IEEE extends the range of available numbers near zero by allowing a so-called denormalized or subnormal number. Unlike Fixed Point number systems, floating point numbers are not evenly spaced. Floating point systems offer differ rounding modes when converting a real number into a representable FP number. 

1. **Round to Nearest**: Rounds to the closest available number in $F$
2. **Truncation/Chopping**: Rounds to the next number in $F$ towards zero

**Note**: we are essentially finding the best slow on the discrete floating point number line to put a continuous real number into. 

e.g., Express 253.9 info a floating point number system with $\beta = 10, t = 6, L = -5, U = 5$
1. Express 253.9 into base $\beta = 10 \implies 253.9$
2. Shift to get a leading 0 by setting the exponent $p \implies 0.2539 \times 10^3 \implies p = 3$
3. Pad, round or truncate to get to $t$ digits $\implies 0.253900 \times 10^3$

### Measuring Error: Absolute and Relative Errors
We must consider the error these numerical systems induce via the approximations they make, using the error that they induce.  

Given that $x_{\text{exact}}$ is the correct mathematical result, we can then measure either:
$$
\text{Error}_{\text{abs}} = |x_{\text{exact} - x}|
$$
or the relative error:
$$
\text{Error}_{\text{rel}} = \frac{|x_\text{exact} - x|}{|x_{\text{exact}}|}
$$
Note: When measuring relative error, occasionally the value used in the denominator in the computed value $|x|$ rather than $|x_{\text{exact}}|$ as stated above. We will also occasionally use the signed error and signed relative error, which are defined as above but without the absolute value signs. 

**Relative Error** is often most useful because there is a close relationship between relative error and the number of significant digits in the computed result. 
- Independent of the magnitudes of the numbers involved 
- Relates to the number of significant digits in the result

A result is correct to roughly $s$ digits if $E_{\text{rel}} \approx 10^{-8}$ or:
$$
0.5 \times 10^{-s} \le E_{\text{rel}} < 5 \times 10^{-s}
$$
e.g., If $E_{\text{rel}} \approx 0.8 \times 10^{-3}$ it describes a result is correct to about how many significant digits?
- $E_{\text{rel}} \approx 0.4 \times 10^{-3}$ 
- $0.4 \times 10^{-3} = 4 \times 10^{-4}$ 
- $0.5 \times 10^{-4} \le 4 \times 10^{-4} \le 5 \times 10^{-4} \implies s = 4$ so it's correct to around 4 digits 

The computed result of $x$ is said to be approximate $x_{\text{exact}}$ to about $s$ significant digits if the relative error is approximately $10^{-s}$ or to be more precise, if the relative error satisfies:
$$
0.5 \times 10^{-s} \le \frac{|x_{\text{exact}} - x|}{|x_{\text{exact}}|} < 5.0 \times 10^{-s}
$$
**Property**: Relative error gives a measure of the number of correct significant digits independent of the actual magnitudes of the numbers involved. 


### Properties of Floating Point Numbers 
We express for some real number $x \in \mathbb{R}$ which has an exponent in the range allowed by $F$ with it's corresponding floating point number approximation as $fl(x)$. Generally $fl(x) \neq x$ because most real numbers cannot be represented exactly by some floating point number representation. In order to compute $fl(x)$ we must modify $x$ to become a valid representable floating point number typically by eliminating some of its smaller digits. 

$x \in \mathbb{R}$ and $fl(x) \in \mathbb{F}$ are related in that the largest relative error that can occur in representing a real number $x$ by its floating point approximation $fl(x)$ is bounded for all $x$ whose exponents are in the valid range $[L, U]$. 

**Machine Epsilon**: maximum relative error measure of machine precision ($E$) also known as the united round-off error since $E$ can be defined as the smallest number s.t. $F(\beta, t, L, U)$
$$
fl(1 + E) > 1
$$
$E$ depends on exactly how we determine $fl(x)$ from the real number $x$ (which slot we assign the real number to). Either round-to-near where we take the closest slot to $x$ or truncation in which any digits after $t$ are simply discarded. 

#### Values of Machine Epsilon for Truncation and Rounding
Given base $\beta$ arithmetic with $t$ digits for $F(\beta, t, L, U)$
- **Round to Nearest**:
$$
E = \frac{1}{2} \beta^{1- t}
$$
- **Truncation**:
$$
E = \beta^{1-t}
$$
Note: we are usually only interested in the order of magnitude of $E$ as this gives a sufficient indication of the accuracy of our floating point system. From the bounds and definition of relative error, we can observe that the signed error between any non-zero real number $x$ and its floating point representation $fl(x)$ that can be written as a small multiple of $x$, the signed relative error $fl(x)$ for $x$ is
$$
\delta = \frac{fl(x) - x}{x} \implies fl(x) - x = \delta x \text{ where } |\delta| \le E 
$$
by design of $F$ does not exceed $E$
$$
fl(x) + x(1 + \delta)
$$
where $\delta$ is some value (positive, negative, or zero) which must satisfy:
$$
-E \le \delta \le E
$$
$E$ is positive which gives a bound on the magnitude of $\delta$ whereas $\delta$ is a signed number which gives the error for a specific conversion of $x$ to $fl(x)$. 

#### Machine Epsilon
The smallest value that $fl(1 + E)$ can take under the floating point system $F(\beta, t, L, U)$ is:
$$
fl(x) = x(1 + \delta) \text{ for some } |\delta| \le E
$$
Assuming that there is no overflow or underflow. It is the same as the floating point approximation of the exact real result. FP hardware typically uses extra guard digits (BTS) to ensure this. Take the floating point addition operator $\oplus$ for $F$ (likewise we use $\otimes, \ominus$). Translating the above, we get the following:
$$
w \oplus z = fl(w + z) = (w + z)(1 + \delta)
$$
Note: Results in Floating Point arithmetic are Order-Dependent:
$$
(a \oplus b) \oplus c = a \oplus (b \oplus c) = fl(a + b + c)
$$
For each floating point arithmetic conversion will incur some relative error that will depend on how far the resulting real is from its floating point counterpart. 
$$
\begin{align*}
	(a \oplus b) \oplus c &= fl(a + b) \oplus c = fl(fl(a + b) + c)\\
	&= fl((a + b)(1 + \delta_1) + c) = ((a + b)(1 + \delta_1) + c)(1 + \delta_2)\\
	&= (a + b + c + (a + b)\delta_1)(1 + \delta_2)
\end{align*}
$$
Thus we have a non-trivial expression for the unsigned error of the floating point result compared to the real result:
$$
(a + b + c) - (a \oplus b) \oplus c = -(a + b)\delta_1(1 + \delta_2) - (a + b + c)\delta_2
$$
If $a + b + c \neq 0$ then the relative error in the floating point sum is well-defined.  
$$
\begin{align*}
	\text{err}_{\text{rel}} &= \frac{a + b + c - (a \oplus b) \oplus}{(a + b + c)}\\
	&= \frac{-(a + b)\delta_1(1 + \delta_2) - (a + b + c)\delta_2}{a + b +c}\\
	&= -\left(\frac{a + b}{a + b + c}\right)\delta_1(1 + \delta_2) - \delta_2
\end{align*}
$$
- error due to the second $\oplus$ contributes $-\delta_2$ to $\text{error}_{\text{rel}}$  cannot be bigger than $E$ in magnitude 
- the error due to the first $\oplus$ contributes $-((a + b) / (a + b + c)) \delta_1 ( 1 + \delta_2)$ 
- if the factor $(a + b)/(a + b + c)$ is large, then this term can be much bigger than $\delta_1$ or $E$
- We can only say that this term is for sure not bigger than:
$$(|a + b| / |a + b + c|) E(1 + E)$$


### Conditioning and Stability 
Problems can be well-conditioned and some may be ill-conditioned. We would like to have a measure of how well-conditioned or ill-conditioned a given problem may be. The concept of conditioning may be defined as follows. 

Consider a problem $P$ with input values $I$ and output values $O$. 
- If a relative change of size $\Delta I$ in one or more input values causes a relative change in the mathematically correct output values which is guaranteed to be small the problem $P$ is said to be well-conditioned
- Otherwise, Problem $P$ is said to be ill-conditioned 

**Note:**
- Guarantee of independence of choice of algorithms 
- We can compare the relative conditioning of different problems, but there is NO absolute threshold for ill-conditioned versus well-conditioned problems 

We would like **small errors** in the input or floating point calculations of an algorithm to lead only to correspondingly small changes in the output of that algorithm. 
- Small error is steadily magnified by the algorithm, we consider the algorithm to be numerically unstable, since the error may grow to destroy the accuracy of our answer 
- If instead small errors shrink away towards zero during the algorithm's execution, then the algorithm is considered stable 

e.g., Take the computation below:
$$
I_n = \int_0^1 \frac{x^n}{x + \alpha} dx
$$
Perform a stability analysis on the recurrence:
$$
I_n = \frac{1}{n} - \alpha I_{n - 1}
$$
For simplicity assume that there are no other errors introduced at any stage of the computation after $I_0$. let $(I_n)_A$ be the approximate value of $I_n$; the computed value of $I_n$ including effects of $\epsilon_0$. Let $(I_n)_E$ be the exact value of $I_n$. 

Let $\epsilon_n = (I_n)_A - (I_n)_E$ (the error at step $n$ due to the initial error $\epsilon_0$). Thus, the exact $I_n$ satisfies:
$$
\begin{align*}
(I_n)_E &= \frac{1}{n} - \alpha(I_{n - 1})_E\\
(I_n)_A &= \frac{1}{n} - \alpha(I_{n - 1})_A\\
(I_n)_A - (I_n)_E &= \left(\frac{1}{n} - \frac{1}{n}\right) - \alpha((I_{n - 1})_A - (I_{n - 1})_E)\\
\epsilon_n &= (-\alpha) \epsilon_{n - 1}\\
&= (- \alpha)(-\alpha) \epsilon_{n - 2}\\
&...\\
&= (-\alpha)^n \epsilon_0
\end{align*}
$$
Thus, if $|\alpha| > 1$ then any initial error $\epsilon_0$ is magnified y an unbounded amount as $n \rightarrow \infty$.
Otherwise, if $|\alpha| < 1$ then any initial error is damped out i.e., approaches zero as $n \rightarrow \infty$. Thus, we can conclude that the algorithm is stable if and only if $|\alpha| < 1$ and the algorithm is unstable if and only if $|\alpha| < 1$.




## Exercises: Floating Point Number Systems

##### Question 1

**(a)** What is the largest value of $n$ so that $n!$ can be exactly represented in a FP system where:
$$
(\beta, t, L, U) = (2, 5, -10, 10)
$$
Base 2, with $t$ digits in the significand, with $L = -10, U = 10$.

Thus:
$$
\beta^{-10} = 2^{-20} = \frac{1}{1024} \text{ is the smallest positive number}
$$
$$
\beta^{10} = 2^{10} = 1024 \text{ is the largest representable number}
$$
We have that $n! \le 1024$ so we can compute:
$$
\begin{align*}
1! = 1\\
2! = 2\\
3! = 6\\
4! = 24\\
5! = 120\\
6! = 720\\
7! = 5040
\end{align*}
$$
So we have that $6! = 720 \le 1024$ is the largest such that $n!$ is representable in the FP system's range, so thus we have that:
$$
\boxed{n = 6}
$$
$$
720 \text{ in binary is represented as } 1011010000_2 \text{ which requires } 10 \text{ bits}
$$
The mantissa stores the first 5 bits, $10110_2$ with an exponent of $2^5$ allowing the exact representation. Thus $6!$ is both representable and exactly stored in this FP system. 

**(b)** On a base-2 machine, the distance between 7 and the next largest floating point number is $2^{-12}$. What is the distance between 70 and the next largest floating point number?
$$
\Delta_7 = 2^{-12} \implies \Delta_{70} = \Delta_7 \cdot \frac{70}{7} \implies 2^{-12} \cdot \frac{70}{7} = 2^{-12} \cdot 10 = 10 \cdot 2^{-12} = 2^{-8}
$$
Thus, the distance between 70 and the next largest FP number is:
$$
\boxed{2^{-8}}
$$
**(c)** Assume that x and y are normalized positive floating numbers in a base-2 computer with $t$-bit mantissa. How small can $y − x$ be if $x < 8 < y$?

In a base-2 FP system we have that the normalized number has the form:
$$
x = m \cdot 2^e \implies \text{ the spacing } \Delta = 2^e \cdot 2^{-t} = 2^{e - t}
$$
Given that $2^3 = 8$ so numbers around 8 have $c = 3$ as their exponent. The smallest spacing between consecutive FP numbers in the interval $[8, 16)$ is determined by $e = 3$:
$$
\Delta = 2^3 \cdot 2^{-t} = 2^{3 - t} \implies e = 3 \text{ has spacing } 2^{3 - t}
$$
$$
x = (2 - 2^{-t}) \cdot 2^3 = (2^3 - 2^{3 - t}) \implies y = 2^3 + 2^{3 - t}
$$
$$
y - x = (2^3 + 2^{3 - t})- (2^3 - 2^{3 - t}) = 2^{3 - t} + 2^{3 - t} = 2 \cdot 2^{3 - t} = 2^{4 - t}
$$
Thus the smallest possible value of $y - x$ is:
$$
\boxed{2^{4 - t}}
$$

##### Question 2
Consider a fictitious floating-point number system composed of the following numbers:

$$
S = \{ \pm b_1.b_2b_3 \times 2^{\pm y} : b_2, b_3, y = 0 \text{ or } 1, \text{ and } b_1 = 1 \text{ unless } b_1 = b_2 = b_3 = y = 0 \}
$$

Each number is normalized unless it is zero.
1. **(a)** Plot the elements of $S$ on the real axis.  
2. **(b)** Show how many elements are contained in $S$. What are the values of OFL (overflow limit), UFL (underflow limit), and the machine epsilon?


**(a)** **Mantissa values**: The mantissa has the form $b_1.b_2b_3$ where:
   - $b_1 = 1$ (normalized unless zero),
   - $b_2, b_3 = 0 \text{ or } 1$.

   Possible binary mantissa values are:
   $$
   1.00, 1.01, 1.10, 1.11
   $$

   Converting to decimal:
   $$
   1.00 = 1.0, \quad 1.01 = 1.25, \quad 1.10 = 1.5, \quad 1.11 = 1.75.
   $$

2. **Exponent values**: The exponent $y$ can take values $0$ or $1$:
   - For $y = 0$: Multiply mantissa by $2^0 = 1$,
   - For $y = 1$: Multiply mantissa by $2^1 = 2$.

3. **Sign values**: The sign can be $+$ or $-$.

4. **Combining all possibilities**:
   - For $y = 0$ (no scaling): $\pm 1.0, \pm 1.25, \pm 1.5, \pm 1.75$,
   - For $y = 1$ (scaled by 2): $\pm 2.0, \pm 2.5, \pm 3.0, \pm 3.5$.

5. **Include zero**: By the problem statement, zero is included.

**Plot on the real axis:**  
The numbers are:
$$
\pm 1.0, \pm 1.25, \pm 1.5, \pm 1.75, \pm 2.0, \pm 2.5, \pm 3.0, \pm 3.5, \text{ and } 0.
$$

**(b)** **Count of elements**:
   - There are $4$ mantissa values: $1.0, 1.25, 1.5, 1.75$,
   - There are $2$ exponent values: $y = 0 \text{ and } y = 1$,
   - There are $2$ signs: $+$ and $-$.

   Total non-zero elements:
   $$
   4 \times 2 \times 2 = 16.
   $$
   Including zero, the total number of elements is $17$.

2. **Overflow Limit (OFL):**
   - The largest positive number is $1.75 \times 2^1 = 3.5$.
   - **OFL = 3.5**.

3. **Underflow Limit (UFL):**
   - The smallest positive number is $1.0 \times 2^0 = 1.0$.
   - **UFL = 1.0**.

4. **Machine Epsilon:**
   - Machine epsilon is the difference between $1.0$ and the next largest representable number.
   - Next number after $1.0$ is $1.25$, so:
   $$
   \epsilon = 1.25 - 1.0 = 0.25.
   $$
   - **Machine epsilon = 0.25**.


##### Question 3
Using the floating-point number format $(\beta, t, U, L) = (2, 20, −200, 200)$, store the distance between Earth and Sun ($1.5 \times 10^{8}$ kilometers) and the distance between Toronto and Waterloo (75 kilometers). What length does the last bit of the mantissa represent in each case?
$$
x = m \cdot 2^e \implies \Delta = 2^{e - t}
$$
Thus we have that the distance $1.5 \times 10^{8}$ is approximately:
$$
1.5 \times 10^8 \approx 1.5 \cdot 2^{27} \implies \Delta_{\text{Earth-Sun}} = 2^{e - t} = 2^{27 - 20} = 2^7 = 128
$$
Since the distance 75 kilometres can be represented as:
$$
75 \approx 1.171875 \cdot 2^6
$$
$$
\Delta_{\text{Toronto-Waterloo}} = 2^{e - t} = 2^{6 - 20} = 2^{-14} = \frac{1}{16384} \approx 0.000061035 \text{km}
$$
