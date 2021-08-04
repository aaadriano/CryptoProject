---
title: Grover's Algorithm
date: '2021-01-26'
---
<center>
</br>
<img src="https://spectrum.ieee.org/image/MzEwMTk5OA.jpeg">
</center>

The **Grover Algorithm** is a search function which returns "True" for one of its possible inputs, and "False" for all the others; its purpose is to find any object in the database that would return "True". Grover's algorithm has a lot of applications, and can be used for estimating the mean and median of a set of numbers, and for solving the collision problem. In addition, it can be used to perform exhaustive searches over the set of possible solution. It is the fastest possible quantum algorithm for searching an unsorted database.

**Fair warning**, the following bits launch into a really mathematically heavy description of how the algorithm is constructed. If this does not interest you, you probably already know enough to understand what we have done in this project. You can skip [here](https://wucrypto-project.netlify.app/) to the proposed model.

Grover's algorithm was designed to speed up an unstructured database search quadratically. It is comprised of Hadamard gates, an oracle, controlled-Z gates, and a reflection. We will discuss each of these pieces in turn. First, here is a visual representation of the Grover circuit:

<center>
</br>
<img src="https://qiskit.org/textbook/ch-algorithms/images/grover_circuit_3qubits.png">
</center>

*How does the search work?*
</br>
Suppose you are given a large list of $N$ items. Among these items there is one item with a unique property that we wish to locate called $w$. In general, <i>n</i> qubits are represented by a superposition state vector in $2^{n}$ dimensional Hilbert space, so our database contains $N=2^n$ items, which is dependent on the number of qubits.

In classical computation, the search (on average) would require checking $N/2$ items, and at worst all $N$ items. On a quantum computer, however, we can find the marked item in roughly $\sqrt{N}$ steps with Grover's algorithm, which is a significant improvement to computational efficiency. Since the algorithm does not depend on the structure of the database itself, it can be used more generically.

*The Hadamard Gate*
</br>
The <b>Hadamard gate</b> is a single-qubit operation that creates an uniform superposition of the two basis states (a measurement of an output state will return 1 or 0 with equal probabilities).

In coordinates, the Hadamard gate applies the <i>Hadamard Matrix</i> to a coordinatized qubit:
$$H=\frac{1}{\sqrt{2}}\begin{bmatrix}1 & 1\\1 & -1\end{bmatrix}$$

For example, maps the observational basis state |0⟩ to the superimposed state $\frac{|0\rangle+|1\rangle}{\sqrt{2}}$ and |1⟩ to $\frac{|0\rangle-|1\rangle}{\sqrt{2}}$ 

The Hadamard gate can also be considered geometrically as a 90º rotation of the Bloch Sphere about the Y-axis, followed by a 180º rotation about the X-axis.

<i>Advanced Note</i>: The Hadamard gate is the lowest dimensional case (one-qubit) of the quantum Fourier transform.
</br>
*The Oracle*
</br>
For this example, consider the database to be all the possible computational basis states our qubits can be in. 
If we have 3 qubits, the states would run from  $|000\rangle, |001\rangle, \dots |111\rangle$.

Grover’s algorithm involves oracles that add a negative phase to the solution states. I.e. for any state $|x\rangle$ in the computational basis:

\begin{equation}
    f: \{0,1\}^n \mapsto \{0,1\} \mbox{ such that } f(w)=1 \mbox{ and } f(u)=0 \mbox{ for } u \not = w 
\end{equation}

We assume that we can implement this oracle as a quantum oracle.

Specifically, consider the unitary linear transformation of dimension $(n,n)$: 

\begin{equation}
  U_w = I - 2|w><w|  
\end{equation}

where $|w>$ is the ket vector corresponding to the object $w$. Then, it will act on any ket thusly:

$$
U_\omega|x\rangle = \bigg\{
\begin{aligned}
\phantom{-}|x\rangle \quad \text{if} \; x \neq \omega \\
-|x\rangle \quad \text{if} \; x = \omega \\
\end{aligned}
$$


This oracle will be a diagonal matrix, where the entry that correspond to the marked item will have a negative phase. For example, if we have three qubits and $\omega = |101\rangle$, our oracle will have the matrix:

$$
U_\omega = 
\begin{bmatrix}
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & -1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{aligned}
\\
\\
\\
\\
\\
\\
\leftarrow \omega = \text{101}\\
\\
\\
\\
\end{aligned}
$$

There are many computational problems in which it’s difficult to _find_ a solution, but relatively easy to _verify_ a solution. For these problems, we can create a function $f$ that takes a proposed solution $x$, and returns $f(x) = 0$ if $x$ is not a solution ($x \neq \omega$) and $f(x) = 1$ for a valid solution ($x = \omega$). Our oracle can then be described as:

$$
U_\omega|x\rangle = (-1)^{f(x)}|x\rangle
$$

and the oracle's matrix will be a diagonal matrix of the form:

$$
U_\omega = 
\begin{bmatrix}
(-1)^{f(0)} &   0         & \cdots &   0         \\
0           & (-1)^{f(1)} & \cdots &   0         \\
\vdots      &   0         & \ddots & \vdots      \\
0           &   0         & \cdots & (-1)^{f(2^n-1)} \\
\end{bmatrix}
$$

Thus, for an arbitrary ket $|y>$, the component of $|y>$ in the direction $|w>$ is reversed, while the components of $|y>$ in all the directions orthogonal to $w$ are unchanged. Effectively, this corresponds to a reflection of $|y>$ around the vector $|y-w>$.  

*Defintion: Amplitude*
</br>
When we start running the algorithm, we do not know which item in $N$ is $w$, and any random guess would have the same probability of success. We call this a uniform superposition:  $|s \rangle = \frac{1}{\sqrt{N}} \sum_{x = 0}^{N -1} | x
\rangle.$

Note that for any $|y>$, one of the $n$ vectors of the basis of $\mathbb{C}^n$ 
\begin{equation}
    <y|s>=\frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}<y|x>=\frac{1}{\sqrt{N}}
\end{equation}
Thus, if we make a measurement of $s$, all the possible outcome $y$ are equally likely, with probability $|<y|s>|^2=\frac{1}{N}$. 

If at this point we were to measure in the standard basis $\{ | x \rangle \}$, this superposition would return to any one of the basis states with the same probability of $\frac{1}{N} = \frac{1}{2^n}$. Our chances of guessing the right value $w$ is therefore $1$ in $2^n$, as could be expected. Hence, on average we would need to try about $N/2 = 2^{n-1}$ times to guess the correct item.

Through amplitude amplification, a quantum computer significantly enhances this probability. This procedure stretches out (amplifies) the amplitude of the marked item, which shrinks the other items' amplitude, so that measuring the final state will return the right item with near-certainty. 

We can think of amplification in a geometric sense- two reflections generate a rotation in a two-dimensional plane. The only two special states we need to consider are $| w \rangle$ and the uniform superposition $| s \rangle$. These two vectors span a two-dimensional plane in the vector space $\mathbb{C}^N.$ They are not perpendicular because $| w \rangle$ occurs in the superposition with amplitude $N^{-1/2}$ as well.
We can, however, introduce an additional state $|s'\rangle$ that is in the span of these two vectors, which is perpendicular to $| w \rangle$ and is obtained from $|s \rangle$ by removing $| w \rangle$ and
rescaling. 

As mentioned above, the probability of finding $| w \rangle$ is only $\frac{1}{N}$. Then, the probability of finding $| s' \rangle$ is an equal probability of any value from N-1. P($| s' \rangle$)= $\frac{N-1}{N}$.

Let's put it together.

<u>*Step By Step Grover*</u>
Start with $|x_0>=|s>$, and at each step $t$, compute

\begin{equation}
    |x_t>= U_s U_w |x_{t-1}>
\end{equation}

The key is to compute the angle $\alpha_t$ between $|x_t>$ and $|w>$. For convenience, notate $\theta_t=\frac{\pi}{2}-\alpha_t$, the angle between $|x_t>$ and $w^{\perp}$.  First, notice that 

\begin{equation}
|<w|x_0>| = |<w|s>|=\frac{1}{\sqrt{N}}=\cos \alpha_0=\sin \theta_0    
\end{equation}

since $N$ is large, we approximate 

\begin{equation}
\frac{1}{\sqrt{N}}=\sin(\theta_0) \approx \theta_0   
\end{equation}

Next, let us proceed geometrically. First we compute $U_w |x_0>$ which rotate $x_0$ 
around $|x_0-w>$. Next, we rotate this vector around $s$ by the action of $U_s$. Overall, 

\begin{equation}
    \theta_1=\theta_0+2\theta_0
\end{equation}

and after $T$ steps, 

\begin{equation}
    \theta_t = \theta_0 + 2T \theta_0 \approx \frac{1}{\sqrt{N}}(1 + 2T)
\end{equation}

<img src="https://miro.medium.com/max/1020/1*GArCJX_Jx8b1U3F68GYibg.png" />

A visual representation of the amplitude change between the inversion and final step:
<img src="https://www.researchgate.net/publication/274056986/figure/fig2/AS:393378433912833@1470800135225/Single-call-of-the-Grover-iteration-on-the-equal-superposition-state-Grover.png" />

The equation $\theta_T=\frac{\pi}{2}$ provides a method for choosing the number of steps,  
\begin{equation}
T=\frac{\pi}{4}\sqrt{N} - \frac{1}{2} \approx \sqrt{N}    
\end{equation}

The probability of guessing correctly the oracle after $t$ steps is 

\begin{equation}
    |<x_t,w>|^2=\sin^2(\theta_t)=\sin^2(\frac{1}{\sqrt{N}}(1+2t))
\end{equation}

It is cautioned not to run too many iterations, otherwise the algorithms ability to pick out the desired values begins to diminish again.

*A note on recovering $\theta$ the simplest way possible:*

tan($\theta$) = $\frac{1}{\sqrt{N}}$ $\div$  $\frac{\sqrt{N-1}}{\sqrt{N}}$
= 1 $\div \sqrt{N-1}$ $\Rightarrow$ $tan^{-1}$($\frac{1}{\sqrt{N-1}})$ = $\theta$ 

</br>

$\theta$ $\approx$ $\frac{1}{\sqrt{N-1}}$

****
Whew. Now that we can build an oracle and run a Grover Algorithm, we can move on to the next portion of this project [here](https://wucrypto-project.netlify.app/).

*Image from Spectrum.ieee*
