When we use Google to search web pages for keywords, a set of pages is returned, each ranked in order of its importance or relevance. We model the structure of the Web by a directed graph. The nodes in the graph represent web pages. A directed arc is drawn from node $j$ to node $i$ if there is link from page $j$ to page $i$. Let $deg(j)$ be the out-degree of node $j$, that is, the number of arcs leaving node $j$. 

**Idea**:
A link from webpage $j$ to webpage $i$ can be viewed as a vote on the importance of page $i$ by page $j$. We assume that all out-links are equally important, so that the importance conferred on page $i$ by page $j$ is simply $1/deg(j)$ assuming that there is a link from $j \rightarrow i$. 

**Random Surfer**
A surfer selects a page in the Web in turn. From this initial page, the surfer then selects at random an outlink from this initial page, and visits this page. Another outlink is selected at random from this page, and so on. The surfer keeps track of the number of times each page is visited. After $K$ visits, the surfer then begins the process again, by selecting another initial page.  Notice, for $K$ that is sufficiently large, then we would get a good estimate of the importance of each page on the Web. This would be very expensive because the number of webpages $R$ is very large. Moreover, $K$ needs to be large as well to ensure that we are getting a good sample of all the paths in the Web.

#### Markov Chain Matrix: Random Surfer
Let $P$ be a matrix of size $R \times R$ where $R$ is the number of pages in the Web. $P$ is a large matrix, where each entry $P_{ij}$ is the probability that the random surfer visits page $i$ given he is at page $j$:
$$
P_{ij} = \begin{cases}
\frac{1}{\text{deg}(j)} &\text{if there is a link from } j \rightarrow i\\
0 &\text{otherwise.}
\end{cases}
$$
When page $i$ has NO outlinks, we suppose that our random surfer teleports to another page at random, if he encounters a dead end page. We suppose that this teleportation moves the surfer to any other page in the Web with equal probability. 

Let $d$ be an $R$-dimensional column vector such that:
$$
[d]_i = \begin{cases}
1 &\text{if } deg(i) = 0\\
0 &\text{otherwise}
\end{cases}
$$
and let $e = [1, \dots, 1]^T$ be the $R$-dimensional column vector of ones. Then we define a new matrix $P'$ of transition probabilities that include the teleportation property:
$$
P' = P + \frac{1}{R}e \cdot d^T
$$


#### Escaping Cycles 
Suppose that page $j$ contains only a link to page $i$, and page $i$ contains only a link to page $j$. This will trap our random surfer in an endless cycle. We will again use the teleportation idea to make sure that the random surfer can escape from the boredom of visiting the same pages in cyclic fashion. Let $0 < \alpha < 1$ and define a new matrix of probabilities:
$$
M = \alpha P' + (1 - \alpha) \cdot \frac{1}{R} e\cdot e^T
$$
where: 
- $\alpha P'$ makes surf randomly (out teleporting out of dead ends)
- the last term teleports randomly from any page 

This is our **Google Matrix** where $M$ expresses one step of random surfing. Essentially, we are saying that the surfer moves to other pages based on the usual idea of selecting an outlink at random and, in addition, the surfer can also teleport to any other page on the web with equal probability. We weight the usual outlink probabilities by $\alpha$, and the teleportation probability by $1 - \alpha$ so that the total probability of visiting the next page on the Web is one. 

$M_{ij}$ is the probability that the surfer will move from page $j$ to page $i$
$$
\sum_i M_{ij} = 1 \quad\text{for } 0 < M_{ij} \le 1
$$
each column sums to 1, so the probability that the surfer will end up on some new page is 1. 

**e.g., Page Rank Example**
Let's say we have the following node relationships:
$$
\begin{align*}
1 &\rightarrow 2, 3\\
2 &\rightarrow N/A\\
3 &\rightarrow 1, 2, 5\\
4 &\rightarrow 5, 6\\
5 &\rightarrow 4, 6\\
6 &\rightarrow 4
\end{align*}
$$
$P = R \times R = 6 \times 6$ matrix, since $R = 6$ and:
$$
d = [0, 1, 0, 0, 0, 0]^T \quad\text{and}\quad e = [1, 1, 1, 1, 1, 1]^T
$$
since page 2 is a dead end. 
$$
P = \begin{bmatrix} 
0 & 0 & \frac{1}{3} & 0 & 0  & 0 \\ 
\frac{1}{2} & 0 & \frac{1}{3} & 0 & 0  & 0 \\ 
\frac{1}{2} & 0 & 0 & 0 & 0 & 0 \\ 
0 & 0 & 0 & 0 & \frac{1}{2} & 1 \\ 
0 & 0 & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\
0 & 0 & 0 & \frac{1}{2} & \frac{1}{2} & 0 \\ 
\end{bmatrix}
$$
$$
P' = P + \frac{1}{R}e \cdot d^T
$$
$$
\begin{aligned} P' &= \begin{bmatrix} 0 & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & \frac{1}{2} \\ 0 & 0 & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & 0 & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix} + \frac{1}{6} \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \\ 1 \\ 1 \end{bmatrix} \begin{bmatrix} 0 & 1 & 0 & 0 & 0 & 0 \end{bmatrix}. \end{aligned}
$$
$$
\begin{aligned} P' &= \begin{bmatrix} 0 & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & \frac{1}{2} \\ 0 & 0 & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & 0 & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix} + \frac{1}{6} \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \\ 1 \\ 1 \end{bmatrix} \begin{bmatrix} 0 & 1 & 0 & 0 & 0 & 0 \end{bmatrix} \\ &= \begin{bmatrix} 0 & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & \frac{1}{2} \\ 0 & 0 & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & 0 & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix} + \begin{bmatrix} 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & 0 \end{bmatrix} \\ &= \begin{bmatrix} 0 & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & \frac{1}{2} \\ 0 & \frac{1}{6} & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & \frac{1}{6} & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix}. \end{aligned}
$$
Let $\alpha = 0.85$, and using $P'$ and $R = 6$ and 
$$
\begin{align*} P' &= \begin{bmatrix} 0 & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & \frac{1}{2} \\ 0 & \frac{1}{6} & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & \frac{1}{6} & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix}, \\[1em] e \cdot e^T &= \begin{bmatrix} 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \end{bmatrix}, \\[1em] \frac{1}{R} e \cdot e^T &= \frac{1}{6} \begin{bmatrix} 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 & 1 \end{bmatrix} = \begin{bmatrix} \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \end{bmatrix}, \\[1em] M &= \alpha P' + (1 - \alpha) \cdot \frac{1}{R} e \cdot e^T \\[1em] &= 0.85 \begin{bmatrix} 0 & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & \frac{1}{3} & 0 & 0 & 0 \\ \frac{1}{2} & \frac{1}{6} & 0 & 0 & 0 & 0 \\ 0 & \frac{1}{6} & 0 & 0 & 0 & \frac{1}{2} \\ 0 & \frac{1}{6} & \frac{1}{3} & \frac{1}{2} & 0 & 0 \\ 0 & \frac{1}{6} & 0 & \frac{1}{2} & \frac{1}{2} & 0 \end{bmatrix} + 0.15 \begin{bmatrix} \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \\ \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} & \frac{1}{6} \end{bmatrix}. \end{align*}
$$
Notes: 
- $e \cdot e^T$ is the $R \times R$ ones matrix
- thus, $\frac{1}{R} e \cdot e^T$ is simply the $R \times R$ matrix with $\frac{1}{R}$ entries 


#### Markov Transition Matrices 
A matrix $Q$ is a markov matrix if:
$$
0 \le Q_{ij} \le 1 \quad\text{and}\quad \sum_i Q_{ij} = 1
$$
Obviously matrix $M$ in equation is a Markov matrix. Let $[p]_i$ represent the probability that the random surfer is at page $i$ so that $p$ is an $R$ dimensional column vector.
$$
[p]_i = \frac{1}{R} \forall i
$$
that is, the random surfer visits each page initially with equal probability. 

A vector $q$ is a probability vector if:
$$
0 \le [q]_i \le 1 \quad\text{and}\quad \sum_i [q]_i = 1
$$
Given that, at loop $n$, the random surfer is in the state represented by the probability vector $p^n$ that is the probability that the surfer is at page $i$ $[p^n]_i$ then the probability vector at state $n + 1$ is:
$$
p^{n + 1} = Mp^n
$$
Verify that $p^{n+1}$ is a probability vector. Since $M$ is a Markov matrix, and $p^n$ is a probability vector, then we have that:
$$
[p^{n + 1}]_i \ge 0 \forall i
$$
$$
\begin{align*}
\sum_i [p^{n + 1}]_i &= \sum_i \sum_j M_{ij} [p^n]_j\\
&= \sum_j [p^n]_j \sum_i M_{ij}\\
&= \sum_j [p^n]_j\\
&= 1
\end{align*}
$$
Thus, $p^n$ is a probability vector implies that $p^{n + 1}$ is also a probability vector for each $n$. 
$$
p^0 = \frac{1}{R} e \implies p^{\infty} = \lim_{k \rightarrow \infty} (M)^k p^0
$$

