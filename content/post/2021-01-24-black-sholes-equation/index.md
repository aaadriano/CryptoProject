---
title: Black-Scholes Equation
author: ''
date: '2021-01-24'
---

<center>
</br>
<img src="https://images.fineartamerica.com/images-medium-large-5/derivatives-financial-debacle-black-scholes-equation-daniel-hagerman.jpg">
</center>

The Black-Scholes Model was developed as a way to use mathematics to price stock options. It is a differential equation that estimates price over time, and won the authors a Nobel prize in economics. You can find their original work [here](https://www.cs.princeton.edu/courses/archive/fall09/cos323/papers/black_scholes73.pdf).

Model Components: asset price; strike price; interest rates; time to expiration; and volatility. Most of these terms should be familiar, but the **strike price** is the set price at which an option can be bought/sold. The *time to expiration* is the the exact date and time when the contract to buy/sell is rendered void.

The mathematical formula can be expressed as follows:
$$C=S_tN(d_1)-Ke^{-rt}N(d_2)$$
\textbf{where:}
$$d_1=\frac{\ln\big(\frac{S_t}{K}\big)+\big(r+\frac{\sigma_v^2}{2}\big)t}{\sigma_s\sqrt{t}}\quad\text{and}\quad d_2=d_1-\sigma_s\sqrt{t}$$
\textbf{where:}
\begin{align*}
C&=\text{Call option price}\\
S&=\text{Current stock (or other underlying) price}\\
K&=\text{Strike price}\\
r&=\text{Risk-free interest rate}\\
t&=\text{Time to maturity}\\
N&=\text{A normal distribution}\\
\end{align*}

The Black-Scholes theory was that stocks and futures contracts have a lognormal distribution of prices, with constant drift and volatility.

The Black-Scholes model requires several assumptions:
- The only options being mapped are European options exercised at the moment of expiration.
- The option pays no dividends
- Market movements themselves cannot be predicted
- Transactions are cost-free
- Asset returns are log-normally distributed
- And, the rate of return is risk-free and the option volatility is known

As anyone familiar with stocks should note, some of the assumptions are less than ideal. We would be able to shed certain assumptions if we were able to adapt the model for use on in quantum system.

*A quantum version of the Black-Scholes Model is under development by the author of this site, pending completion in 2022. There will be a link available "here" when the content is available.*

If we are content with this explanation of the Black-Scholes Equation, click [here](https://amoderninvestor.netlify.app/) to return to the main page.

*Image from fineartamerica.com*