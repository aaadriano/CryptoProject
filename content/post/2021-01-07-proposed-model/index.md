---
title: Proposed Model
author: ''
date: '2021-01-07'
slug: proposed-model
categories: []
tags: []
Categories:
  - Development
  - GoLang
Description: ''
Tags:
  - Development
  - golang
menu: main
---

<center>
</br>
<img src="https://images-global.nhst.tech/image/QjJSOC9UZ2dQY25EY3RManFqc0grZ1Vja2FvQ1hTU3dlcVZadmlGSm9BMD0=/nhst/binary/358d239815f9af49ca6409c4962e868c">
</center>

Now that we have covered [what cryptocurrency is](https://wucrypto-project.netlify.app/) and [how mining it works](https://wucrypto-project.netlify.app/), we apply our two special tools ([Quantum Computers](https://wucrypto-project.netlify.app/) and [Grover's Algorithm](https://wucrypto-project.netlify.app/)).

**Theory**
We need to find a specific (or smaller) sequence of 64 letters and numbers. Cryptocurrency mining involves  repeatedly changing our guess until we find a sequence that works. On a classic computer, this can only be done using exhaustive, brute force methods. This takes a long time and is difficult to do, hence the high rewards for success.

For each position of the 64, there are 16 possibilities: 0 though 9 and a, b, c, d, e, and f. At the moment, we also know that the first 8 to 64 positions must be "0". As of 08/04/2021, the number of leading zeros is 17. This means that we are realistically trying to compute 48 digits. 

How many combinations is that? Well, you have probability $\frac{1}{16}$ of randomly guessing the first digit correctly. You have a  $\frac{1}{16}$*$\frac{1}{16}$=$\frac{1}{256}$ chance of guessing the first and second digits correctly. From here, we infer that our chances of guessing every digit correctly is $\frac{1}{16^{48}}$, which is a massive, massive number that I am not sure what we can call. For a your typical home computer to find this number, it would take would take thousands of years to mine a new block alone. 

Now, imagine that we can try 0-f on our first digit at the same time. That would require a quentum computer that 

Whew. Now that we can build an oracle and run a Grover Algorithm, we can move on to the next portion of this project [here](https://wucrypto-project.netlify.app/).

*Image from nhst.tech*