---
title: Shor’s Algorithm
author: ''
date: '2021-01-25'
---

<center>
</br>
<img src="https://inteng-storage.s3.amazonaws.com/img/iea/qrwBlp7vG5/sizes/encryption_resize_md.jpg">
</center>

For a long time, factoring numbers was one of the most computationally complex things you could do on a computer. Any integer can be broken down into a product of prime numbers, but finding those numbers can be very difficult. In the financial context, we see factoring problems as part of the security and encryption of data. An integer with a thousand digits (or more) is nearly impossible to factor, and it was thought to be out of reach for technology prior to 1995. 

Shor's Algorithm was designed to factor integers in polynomial time, allowing for very large speed-ups. Shor took these near-impossible thousand-digit integers and proved that the factoring problem can be solved (and efficiently) by using a quantum computer. You can find his original paper [here](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.123.5183&rep=rep1&type=pdf).

Before we discuss how Shor works, we can talk about why it is important. While it may not have been the original intent of the algorithm, it can now break encryption on connections that were designed to keep financial transactions secure. Specifically, the publication of Shor's work brought codes dependent on keys, like RSA encryption, to a highly vulnerable point. This algorithm itself helped spur the development of a quantum computer.

While it is a complex function, Shor's Algorithm can be broken down into a few simpler steps. Let us consider and integer *n* that we wish to factor. These first steps can be done on a classic computer:

1) Figure out if *n* is a prime number, an even number, or an integer power of a prime number. If any of those are true, then the problem isn't complex at all and we wouldn't need to use Shor's Algorithm.

2) Next, choose a number *q*. *q* <u>must</u> be a power of 2, where $n^2$≤*q*<$2n^2$.

3) Then, we need another number *x*, just as long as the greatest common divisor it shares with *n* is 1.

Now, we need our quantum computer, however, any calculation for the appropriate number of qubits needed can be done on a classic computer.

4) Generate a quantum register with two parts: register 1 and register 2. Register one must have enough qubits to represent integers up to *q*-1, and Register 2 must be large enough to represent *n*-1. We will use the notation $|reg1, reg2\rangle$ to represent the state of the quantum computer.

5) Now, let register 1 contain an even superposition of all integers from 0 to *q*-1. Let register 2 be loaded completely with 0s. Them the total state of the memory register would be: $$\frac{1}{\sqrt{q}}\sum_{a=0}^{q-1}|a,0\rangle$$

6) Next, we apply a transformation $x^{a}$ mod *n* to each digit in register 1 and save the output to register 2. Since all states in a quantum computer can run in parallel due to the super position, this take a single step. Now, the memory is in state 
$$\frac{1}{\sqrt{q}}\sum_{a=0}^{q-1}|a,x^a\text{ mod }n\rangle$$

7) We then measure register 2 and observe some value, *k*. As always, the act of observation collapses the resister into a single superposition between 0 and *q*-1 such that $x^{a}$ mod *n* = *k*. Now, out state is $$\frac{1}{\sqrt{||A||}}\sum_{a'=a'\in A}|a',k\rangle$$ where $\|A\|$ is the number of elements in the set of all *a*'s where $x^{a}$ mod *n* = *k*.

8) We now apply a Fourier transform in a single step to state $|a\rangle$, defined as $$|a\rangle=\frac{1}{\sqrt{q}}\sum_{c=0}^{q-1}|c\rangle*e^{2\pi iac/q}$$ to get 
$$\frac{1}{\sqrt{||A||}}\sum_{a'\in A}\frac{1}{\sqrt{q}}\sum_{c=0}^{q-1}|c,k\rangle*e^{2\pi ia'c/q}$$

9) Finally, we can measure register 1. Let this measurement be called *m*. We note that *m* is a multiple of *q/r* with very high probability, where *r* is the desired period.

At this stage, we can move back to a classical computer.
10) Using what we know about *m* and *q*, we calculate *r*.

11) Lastly, once we have the value of *r*, a factor of *n* is found by taking the greatest common divisor  of $x^{r/2}$ and *n* and the greatest common divisor  of $x^{r/2}-1$ and *n*. If this process yields a factor of *n*, we are finished. If it does not, we repeat the above process starting at step 4.

Shor's algorithm does not often fail, but it can if the Fourier transform measures to be 0, for example, or if it returns the factors of 1 and *n*.

While the quantum computers in use today are not strong enough to perform these computations,they will be soon, so post-quantum cryptographic systems is a growing field. These systems must be redesigned to survive an attack by Shor’s algorithm.

If we are content with this explanation of the Shor's algorithm, click [here](https://amoderninvestor.netlify.app/) to return to the main page.

Just as Shor was able to break RSA encryption (which would otherwise take thousands of years) with a quantum computer, we talk about how quantum computing can change Bitcoin mining [here](https://amoderninvestor.netlify.app/22/2021/01/).

*Image from amazonaws.com*