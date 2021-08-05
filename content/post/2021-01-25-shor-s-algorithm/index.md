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

Shor's Algorithm was designed to factor integers in polynomial time, allowing for very large speed-ups. Shor took these near-impossible thousand-digit integers and proved that the factoring problem can be solved (and efficiently) by using a quantum computer.

While it is a complex function, Shor's Algorithm can be broken down into a few simpler steps. Let us consider and integer *n* that we wish to factor. These first steps can be done on a classic computer:

1) Figure out if *n* is a prime number, an even number, or an integer power of a prime number. If any of those are true, then the problem isn't complex at all and we wouldn't need to use Shor's Algorithm.

2) Next, choose a number *q*. *q* <u>must</u> be a power of 2, where $n^2$≤*q*<$2n^2$.

3) Then, we need another number *x*, just as long as the greatest common divisor it shares with *n* is 1.

Now, we need our quantum computer. Any calculation for the appropriate number of 

4) 

If we are content with this explanation of the Shor's algorithm, click [here](https://amoderninvestor.netlify.app/) to return to the main page.

*Image from amazonaws.com*