---
title: Quantum Mining Model
author: ''
date: '2021-01-22'
---
<center>
</br>
<img src="https://images-global.nhst.tech/image/QjJSOC9UZ2dQY25EY3RManFqc0grZ1Vja2FvQ1hTU3dlcVZadmlGSm9BMD0=/nhst/binary/358d239815f9af49ca6409c4962e868c">
</center>

Now that we have covered [what cryptocurrency is](https://amoderninvestor.netlify.app/30/2021/01/) and [how mining it works](https://amoderninvestor.netlify.app/27/2021/01/), we apply our two special tools ([quantum computers](https://amoderninvestor.netlify.app/29/2021/01/) and [Grover's Algorithm](https://amoderninvestor.netlify.app/26/2021/01/)).

As of this post, the reward is 6.25 BTC for finishing a block. Since the price is at 39,766.64 dollars, this amounts to a total of 248,541.50 USD. The time it takes to find a hash is controlled for difficulty ever 2016 blocks, but it take a single miner (on average) about 5 years to finish a block. It is important to keep in mind that this number includes people with complex mining rigs and those with a more simplistic approaches to hashing. Still, the potential to make 49,708.30 dollars a year (before expenses) is enough to incentivize many hopeful miners.

**Theory**
</br>
In order to acquire cryptocurrency, we need to find a specific (or smaller) sequence of 64 letters and numbers. Cryptocurrency mining involves repeatedly changing our guess until we find a sequence that works. On a classic computer, this can only be done using exhaustive, brute force methods. This takes a long time and is difficult to do, hence the high rewards for success.

For each position of the 64, there are 16 possibilities: 0 though 9 and a, b, c, d, e, and f. At the moment, we also know that the first 8 to 64 positions must be "0". As of 08/04/2021, the number of leading zeros is 17. This means that we are realistically trying to compute only 48 digits out of th 64. 

How many combinations is that? Well, you have probability $\frac{1}{16}$ of randomly guessing the first digit correctly. You have a  $\frac{1}{16}$*$\frac{1}{16}$=$\frac{1}{256}$ chance of guessing the first and second digits correctly. From here, we infer that our chances of guessing every digit correctly is $\frac{1}{16^{n}}$, where n=48. This is a tiny probability (it is about 0.0000000000000000000000000000000000000000000000000000000159%, if you were curious). For a your typical home computer to find the right hash guessing randomly, it would take would take thousands of years to mine a new block alone. However, with mining camps and supercomputers, the real number of iterations can be fewer. Nowadays, Bitcoin is designed to be adaptive in difficulty, and one new block is found on average every 10 minutes or 600 seconds. 

Let's figure out just home much of an advantage a quantum computer can provide. Recall, Grover offers us a speedup from for a database of N to ???N. Imagine that we can try 0-f on our first digit at the same time, or, in parallel. That would require a quantum computer that could consider XXXXX qubits at a time.

Since the number of leading zeros can change, we will consider a scenario where we have to generate the full sequence of 64 characters for simplicity. Since we have 64 positions, with each digit having 16 possibilities, we have $16^{64}$ combinations. 16 can be written as a power of 2, $2^{4}$, which will allow us to simplify this number to $2^{256}$. This is where the Bitcoin cryptographic hashing function"SHA-256" gets its name.

Classically, the probability of successfully mining a block via random guess is given by $\frac{Trt}{2^{256}}$, where r is the hash rate (the number of guesses made per second), t is the time (seconds), and T is the "adaptive difficulty" mentioned above, which can be computed using the number of hashes equal or less than the target. It is intuitively understood that the faster you can guess, the faster you can find a solution.

My personal laptop uses an Intel i7 processor, which can reach a hash rate of 1811.35 H/s, so we will use that number for further calculations. Also, at the time of this writing, T= 14,496,442,856,349. Difficulty and other statistics can be found on the Bitcoin [website](https://btc.com/stats/diff).


On a quantum computer using Grover, we can represent the equation for Bitcoin mining as $sin^{2}(2r_{q}t\sqrt{T/2^{256}})$
where $r_{q}$ is the quantum hash rate, determined by the number of Grover iterations per second. Remember, T is adaptive, so for Grover to work we need to make sure that we run the loop and take a measurement before T changes. Now, the interval between blocks shows an exponential distribution, which means that the chance that a block is still discoverable after a time *t* is $e^{-t/600}$.

WE can then represent our earning potential on a quantum computer with $Re^{-t/600}sin^{2}(2r_{q}t\sqrt{T/2^{256}})-Ct$ where C is the constant cost of running the computer and R is the 6.25 BTC reward.

The number of qubits available to a quantum computer would change the outcome, but after a certain point, the advantage begins to level out and will not exceed $\sqrt{2^{256}/T}$.

One could ask, how many qubits will be available in the near future? To answer this, we have to look at the rate of growth quantum computers have shown in recent years and generate a new model for predicting qubit availability. Qubit growth has been historically exponential, and we verify that the model is still the best fit in this case. We found that the estimate for ambitions growth will see the number of qubits double every year while the conservative estimate believes it to double every two years.

Here is an example of the data:
| Number of Qubits | Year     |Quantum Computer |
|--------|--------------|-------------|
| 2    | 2013      |IBM |
| 5    | 2014 |UC Santa Barbara|
| 3    | 2014      |IBM|
| 5    | 2016      |IBM |
| 16    | 2017 |IBM|
| 20   | 2017      |Google|
| 49   | 2018      |Google|
| ...   | ...      |...|

```{r}
x= c(2013,2014,2014,2016,2017,2017,2018)
y= c(2,5,3,5,16,20,49)
xy <- data.frame(x, y)

Conservative_Growth <- lm(y ~ x, data = xy)
Ambitious_Growth <- lm(y ~ x + I(x^2), data = xy)

prd <- data.frame(x = seq(2020, 2040, by = 1))

result <- prd
result$Conservative_Growth <- predict(Conservative_Growth, newdata = prd)
result$Ambitious_Growth <- predict(Ambitious_Growth, newdata = prd)

library(reshape2)
library(ggplot2)

result <-  melt(result, id.vars = "x", variable.name = "Model",
                value.name = "Qubits")
ggplot(result, aes(x = x, y = Qubits)) +
  theme_bw() + 
  geom_point(data = xy, aes(x = x, y = y)) +
  geom_line(aes(colour = Model), size = 1) +
  xlim(2013, 2040) + 
  scale_y_continuous(limits=c(0, 1644), breaks=c(0, 200, 1100, 1644)) + labs(x = "Year")
  
```

By our computation, we expect:
| Number of Qubits (Min) | Number of Qubits (Max)     | Year |
|--------|--------------|-------------|
| 98    | 196 |2025|
| 3    | 2014      |2030|
| 5    | 2016      |2035 |
| 16    | 2017 |2040|
| ...   | ...      |...|

Along with the number of qubits, we need to model hashing power of each computer.

We can demonstrate this effect on a classic computer. Suppose we generate our own 64 bit hexadecimal number that follows the rules of a Bitcoin hash. Let our target hash be *0000000000000000057fcc708cf0130d95e27c5819203e9f967ac56e4df598ee*. 

*Limitations*

Current iterations of quantum computers have a lot of calculation error. Quantum error correction is required in order to counteract the issue, but it uses a large number of qubits.

Another limitation is specific to Bitcoin. Because miners have become highly specialized, they can be extremely fast compared to even quantum computers. Bitcoin is then relatively resistant to the speedup granted by quantum computers.

Finally, it should be mentioned that we talk about using quantum computers in a **legal** way to honestly mine cryptocurrency. There is a real danger that quantum computers will enable hackers to *steal* finished blocks from individuals with real computers. That is a different type of security problem that is outside the scope of this inquiry.

What are your impressions? Read some closing thoughts on quantum computing and cryptocurrency [here](https://amoderninvestor.netlify.app/21/2021/01/).

*Image from nhst.tech*