---
title: Quantum Mining Model
author: ''
date: '2021-01-22'
---
<center>
</br>
<img src="https://images-global.nhst.tech/image/QjJSOC9UZ2dQY25EY3RManFqc0grZ1Vja2FvQ1hTU3dlcVZadmlGSm9BMD0=/nhst/binary/358d239815f9af49ca6409c4962e868c">
</center>

Now that we have covered [what cryptocurrency is](https://amoderninvestor.netlify.app/30/2021/01/) and [how mining it works](https://amoderninvestor.netlify.app/27/2021/01/), we apply our two special tools ([Quantum Computers](https://amoderninvestor.netlify.app/29/2021/01/) and [Grover's Algorithm](https://amoderninvestor.netlify.app/26/2021/01/)).

As of this post, the reward is 6.25 BTC for finishing a block. Since the price is at 39,766.64 dollars, this amounts to a total of 248,541.50 USD. The time it takes to find a hash is controlled for difficulty ever 2016 blocks, but it take a single miner (on average) about 5 years to finish a block. It is important to keep in mind that this number includes people with complex mining rigs and those with a more simplistic approaches to hashing. Still, the potential to make 49,708.30 dollars a year (before expenses) is enough to incentivize many hopeful miners.

**Theory**
</br>
In order to acquire cryptocurrency, we need to find a specific (or smaller) sequence of 64 letters and numbers. Cryptocurrency mining involves repeatedly changing our guess until we find a sequence that works. On a classic computer, this can only be done using exhaustive, brute force methods. This takes a long time and is difficult to do, hence the high rewards for success.

For each position of the 64, there are 16 possibilities: 0 though 9 and a, b, c, d, e, and f. At the moment, we also know that the first 8 to 64 positions must be "0". As of 08/04/2021, the number of leading zeros is 17. This means that we are realistically trying to compute only 48 digits out of th 64. 

How many combinations is that? Well, you have probability $\frac{1}{16}$ of randomly guessing the first digit correctly. You have a  $\frac{1}{16}$*$\frac{1}{16}$=$\frac{1}{256}$ chance of guessing the first and second digits correctly. From here, we infer that our chances of guessing every digit correctly is $\frac{1}{16^{n}}$, where n=48. This is a tiny probability (it is about 0.0000000000000000000000000000000000000000000000000000000159%, if you were curious). For a your typical home computer to find the right hash guessing randomly, it would take would take thousands of years to mine a new block alone. However, with mining camps and supercomputers, the real number of iterations can be fewer. Nowadays, Bitcoin is designed to be adaptive in difficulty, and one new block is found on average every 10 minutes or 600 seconds. 

Let's figure out just home much of an advantage a quantum computer can provide. Recall, Grover offers us a speedup from for a database of N to √N. Imagine that we can try 0-f on our first digit at the same time, or, in parallel. That would require a quantum computer that could consider XXXXX qubits at a time.

Since the number of leading zeros can change, we will consider a scenario where we have to generate the full sequence of 64 characters for simplicity. Since we have 64 positions, with each digit having 16 possibilities, we have $16^{64}$ combinations. 16 can be written as a power of 2, $2^{4}$, which will allow us to simplify this number to $2^{256}$. This is where the Bitcoin cryptographic hashing function"SHA-256" gets its name.

Classically, the probability of successfully mining a block via random guess is given by $\frac{Trt}{2^{256}}$, where r is the hash rate (the
number of guesses made per second), t is the time (seconds), and T is the "adaptive difficulty" mentioned above, which can be computed using the number of hashes equal or less than the target. It is intuitively understood that the faster you can guess, the faster you can find a solution.

On a quantum computer using Grover,



We can demonstrate this effect on a classic computer. Suppose we generate our own 64 bit hexadecimal number that follows the rules of a Bitcoin hash. Let our target hash be *0000000000000000057fcc708cf0130d95e27c5819203e9f967ac56e4df598ee*. 

*Limitations*

Current iterations of quantum computers have a lot of calculation error. Quantum error correction is required in order to counteract the issue, but it uses a large number of qubits.

Another limitation is specific to Bitcoin. Because miners have become highly specialized, they can be extremely fast compared to even quantum computers. Bitcoin is then relatively resistant to the speedup granted by quantum computers.

Finally, it should be mentioned that we talk about using quantum computers in a **legal** way to honestly mine cryptocurrency. There is a real danger that quantum computers will enable hackers to *steal* finished blocks from individuals with real computers. That is a different type of security problem that is outside the scope of this inquiry.

What are your impressions? Read some closing thoughts on quantum computing and cryptocurrency [here](https://amoderninvestor.netlify.app/21/2021/01/).

*Image from nhst.tech*