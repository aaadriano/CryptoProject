---
title: Market Analysis
author: ''
date: '2021-01-22'
---

<center>
</br>
<img src="https://www.schwab.com/resource-center/insights/sites/g/files/eyrktu156/files/orange-bar-chart-960x537_3.jpg">
</center>

```{r, echo=FALSE, warning=FALSE, message=FALSE, include=FALSE}
library(tidyverse)
library(dplyr)
library(ggplot2)
library(forecast)
library(zoo)
library(lubridate)
library(tsibble)
library(knitr)
library(feasts)
library(tsbox)
library(seasonal)
library(fable)
library(KFAS)
Crypto <- read.csv("content/post/2021-01-22-market-analysis/crypto.csv")
Crypto$Date <- as.Date(Crypto$Date, "%m/%d/%Y")
Crypto <- as_tsibble(Crypto, key = Currency)
```
Data: All data gathered from https://www.coindesk.com/price/ (updated: 08/04/2021). Variables of interest are Currency, Date, Closing Price, the 24h High, and the 24h Low.

In current conditions, Bitcoin is so comparatively expensive that it is hard to gauge the other currencies. As a result, it won't be included in the following price chart.

```{r, echo=FALSE}
Crypto %>% filter(Currency != "BTC") %>% filter(Date >= "2018-05-31") %>%
ggplot(aes(x = Date, y = ClosingPrice, color=Currency)) + geom_line() + 
  labs(title = "Top Cyrptocurrencies: Daily Closing Prices",
       
            subtitle = "End of Day Adjusted Prices") +
  
  xlab("Date") + ylab("Price")
```

Visually, we notice that after a slump over the last two years, cryptocurrency is again regaining popularity very quickly. The questions are (1) how quickly?, (2) for how long?, and (3) with which type of growth (we see a steep curve, but is it... Quadratic? Exponential? Polynomial?)? Is now a good time to be investing in or mining cryptocurrency?

Most human activities have some sort of seasonality. However, it can be inconsistent when it comes to the stock market. 

```{r, echo= FALSE}
Crypto %>% filter(Currency == "XLM" | Currency == "LTC" | Currency == "ETH") %>% gg_season(ClosingPrice) + 
  labs(title = "Seasonal Trends for Ethereum, Litecoin, & Stellar", subtitle = "End of Day Adjusted Prices") + xlab("Date") + ylab("Price")
```

Regardless of which currency we choose, it is hard to parse out a consistent seasonal pattern. In some years, it looks like price picks up in the middle of the year, while in others the growth is seen at the tails. Since trading is highly responsive to the social climate of the time, it makes sense that season alone isn't enough to predict pricing. Consecutive closing values appear not to follow one another closely, suggesting an autoregression model for prediction would likely be appropriate, as it often is for stocks. 

The most simplistic models for stocks are 3 easy, nonseasonal forecasting methods, and they are often used as benchmarks for future models. The three forecasting methods are:
</br>
1. Average method,  literally the mean of all selected historic data, expressed as
</br>
AM = $\frac{\Sigma^{T}_{t=1} x_{t}}{T}$ 
</br>
where $x_t$ is the observation at a given time *t* and *T* is the total number of observations
</br>
2. Na??ve method, also know as the value for the most recent observation, expressed as
</br>
NM = $x_{T}$ 
</br>
where $x_T$ is the final observation
</br>
& 3. Drift method, dependent on the average slope between the first and final observations, expressed as
</br>
DM = $x_T + h(\frac{x_T-x_1}{T-1})$ 
</br>
where $x_T$ is the final observation, $x_1$ is the first observation, *T* is the total number of observations, and *h* is the number of days included in the forecast.

This is what they look like when applied to Etherium.

```{r, echo=FALSE, warning=FALSE, message=FALSE}
test <- Crypto %>% filter(Currency == "ETH") %>% select(Date, ClosingPrice) %>% filter(Date >= "2021-01-01")

test2 <- test %>% filter(Date < "2021-04-01")

Crypto.ts <- ts_ts(ts_long(test))
Crypto.ts2 <- ts_ts(ts_long(test2))
autoplot(Crypto.ts) +
  autolayer(meanf(Crypto.ts2, h=30),
    series="Mean", PI=FALSE) +
  autolayer(rwf(Crypto.ts2, h=30),
    series="Na??ve", PI=FALSE) +
  autolayer(rwf(Crypto.ts2, drift=TRUE, h=30),
    series="Drift", PI=FALSE) +
  ggtitle("Forecasts for Closing Price for Ethereum") +
  xlab("Date") + ylab("Closing Price (US$)") +
  guides(colour=guide_legend(title="Forecast"))
```

We can measure to see how well these models performed in comparison against the actual value and against each other using the MAPE, or mean absolute percentage error. We want the lowest error values, so the "drift" forecast seems to be the best. However, these are not good estimates, and we expect to outperform them even with simple time series models.

|Average Method|  MAPE|
|:------------|--------:|
|Training set | 17.88|
|Test set     | 29.67|
|Na??ve Method|  |
|:------------|--------:|
|Training set | 4.70|
|Test set     | 29.67|
|Drift Method|  |
|:------------|--------:|
|Training set | 4.59|
|Test set     | 6.01|

## Identity Crisis: What am I?

We can tackle questions (1) & (3) from above (what kind of growth is this?, what is the slope?) fairly quickly by fitting a few models and finding the best $R^2$ value. We ask this question because the rate of change for the price variable can help us determine which predictive models would be most effective.

The outcomes are as follows:

| Model       | $R^2$ (Adj.) |
|-------------|--------------|
| Linear      | 0.005        |
| Exponential | 0.071        |
| Quadratic   | 0.004        |
| Polynomial  | 0.055        |

There are many more regression-type models we could apply, but these are the most common and would give better insight into the type of growth. As a good practice, we would usually check the outlier and multicollinearity assumptions on our best model, but we already know that currency prices do not have a normal distribution, the variance has odd patterning to it, the mean of the residuals is not zero, and price variables are correlated.

Ultimately, attributing cryptocurrency price to exponential growth only explains about 7% of the variance. We didn't expect time alone to be enough to unlock the secrets of the stock market, so it is hardly surprising that we get poor results when conditioning solely on time. Note: the $R^2$ values would improve if we only consider data from 2020 forward, since financial data at any particular time is most related to the time period immediately following. With this observation, we can move into the first of our time series models.

# Can we make money?

The first thing we can do to answer question (2) (how long is the market trending up/down?) is create a *simple moving average (MA)* or *rolling mean*  for each stock. This is done to make identifying trends easier and to smooth noise from the data set. As we increase the number of days we average over, the MA's responsiveness to price variation decreases. The formula for the MA is as follows:

MA = $\frac{1}{q}\Sigma^{q-1}_{i=0} x_{t-1}$
</br>
where *q* is the number of days considered, from a series $x_t$ during time period *t*.

If we choose a smaller window, the MA can adapt faster to changes. Let's try 7, 15, and 30 day versions of the MA:

```{r echo=FALSE}
Crypto2 <- Crypto %>% 
  group_by(Currency) %>%
  mutate (MA7 = rollmean(ClosingPrice, k=7, fill = list(NA, NULL, NA)), 
          MA10 = rollmean(ClosingPrice, k=15, fill = list(NA, NULL, NA)),
          MA30 = rollmean(ClosingPrice, k=30, fill = list(NA, NULL, NA))) %>%
  ungroup()
```

Here is an example of what this would look like for Bitcoin in 2021:

```{r, echo=FALSE, warning=FALSE}
Crypto2 %>% filter(Currency == "BTC") %>% filter(Date >= "2021-01-01") %>%
  ggplot(aes(x = Date)) +
  geom_line(aes(y = ClosingPrice, color = "Actual"))  +
 geom_line(aes(y = MA7, color = "MA7")) +
  geom_line(aes(y = MA10, color = "MA10")) +  
  geom_line(aes(y = MA30, color = "MA30")) + 
  labs(title = "Top Cyrptocurrencies: MA's Comapred to Daily Closing Prices for Bitcoin", subtitle = "End of Day Adjusted Prices") +
  xlab("Date") + ylab("Price")
```

Using this data, we could build a very simple buy/sell indicator. We would "buy" when MA7 crosses over MA30 upwards and sell when MA7 crosses MA30 downwards. We could build this trigger using Python, and it would work well, for a while. 

### Demonstration:
We create a variable for the difference between MA7 and MA30, and watch for a sign change. This model is considered very "unintelligent", as it cannot learn and adapt from historic data outside the set time period. Sometimes that focus might be useful, but not in all circumstances.

```{r, echo=FALSE, include=FALSE}
#Buy/Sell Verification
Signal <- Crypto2 %>% filter(Date >= "2021-01-01") %>%
  group_by(Currency) %>%
  mutate (Diff = MA7-MA30) %>%
  drop_na() %>%
  select(Date, Currency, Diff, ClosingPrice)
kable(tail(Signal %>% filter(Currency == "BTC") %>% arrange(Date)))
```

Continuing the Bitcoin example: 
With this this buy/sell model, let's see what happened if we followed the model's buy/sell predictions. Say we  first purchase Bitcoin on  1/02, sold on 01/19, purchased again on 02/07, sold on 02/24, purchased on 03/07, sold on 03/20, purchased on 03/29, and finally sold again on 04/05. 

To be more realistic, let's assume we bought 3.11% of a Bitcoin for 1k on 01/02 of this year.

| Date  | Buy/Sell | Cost    | Timing        |
|-------|----------|----------|---------------|
| 01/02 | Buy      | -1,000.00 | Given|
| 01/19 | Sell     | 1,137.56 | Ideal         |
| 02/07 | Buy      | -1,137.56 | Ideal   |
| 02/24 | Sell     | 2,632.25 | Ideal         |
| 03/07 | Buy      | -2,632.25 | Ideal   |
| 03/20 | Sell     | 2,953.40 | Ideal    |
| 03/29 | Buy      | -2,953.40 | 4 Days Early |
| 04/05 | Sell     | 2,971.66 | Ideal    |
| | **Total  Earned**   |1,971.65 |

Overall, this model made 1,971.65 or 197% in profit. For reference, if we purchased the same amount of Bitcoin on that first day and left it in until the last day, I would have withdrawn 1,805.05, or about 805 dollars of profit.

This totals 8 transactions (4 each of Buy/Sell). Of these 8 transactions, the model would have made the correct choice (as compared to the actual market rise/fall) 7/8 (87.5%) times.

**In Sum:** If this (a most simplistic forecasting model) can make money, imagine what we could do with a more complex model (or even better, multiple aggregated complex models). Given current market conditions, this MA model outperforms even Simple Exponential Smoothing (SES) or Autoregressive Integrated Moving Average (ARIMA) models. Overall, it looks like we are expecting an upward trend for cryptocurrency for a little while longer. Good Luck!

**Warning** The MA made money because it did not have to account for a sign change in the slope. If the market turned and Bitcoin prices declined while the algorithm was running, the MA model would have lost money. In order to be a safe way to make money, there has to be a built-in trigger. If the slope has changed sign for three consecutive measurements, we would instead buy when the difference was negative and sell when the difference turned positive.

Learn more about how you can acquire cryptocurrency without purchasing it [here](https://wucrypto-project.netlify.app/) or how quantum computers can help miners work more efficiently [here](https://wucrypto-project.netlify.app/).

*Image from schwab.com*