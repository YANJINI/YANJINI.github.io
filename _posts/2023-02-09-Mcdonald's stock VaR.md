---
title: How Risky is McDonald's Stock?
subtitle: I estimate the daily Value at Risk (VaR) of McDonald's stock by using a GARCH model that takes into account heavy-tailed White Noise innovations.
layout: default
date: 2023-02-09
keywords: blogging, writing
published: true
---
In finance, Value at Risk (VaR) is a crucial metric for asessing the risk of an investment portfolio. VaR provides an estimate of rare but potential loss an investment may face over a specific time horizon and confidence level. To calculate VaR, it's necessary to have an understanding of the underlying distribution of investment returns.

To accurately determine this distribution, various models from time series analysis can be used. The GARCH model, in particular, is known for its effectiveness in analyzing financial data. One of the main reasons to choose GARCH over other models, such as empirical distribution (actually it is even model-free), random walk, or ARIMA, is that it can capture the volatility dynamics of the stock return, which is essential for accurate VaR calculation of financial asset return.

## Prices, simple returns, log returns
Whose distribution exactly do we want to figure out for VaR calculation?  Long story short, we indentify the distribution of log returns of McDonald's stock due to the desirable characteristics of log returns for statistical or practical analysis purpose.

The terms "prices", "simple returns" and "log returns" refer to different ways of measuring the performance of an investment over time. Price is the most common form of financial asset data we can encounter and the other two are transformed version of this. Let $P_{t}$, $R_{t}$ and $r_{t}$ respectively denote the price, simple return and log return of McDonald's stock at time $t$. Then $R_{t}$ and $r_{t}$ are defined as

$R_{t}=\dfrac{P_{t}-P_{t-1}}{P_{t-1}}\tag{1}$

$r_{t}=log \left(\dfrac{P_{t}}{P_{t-1}}\right)=log(1+R_{t})\tag{2}$

Both  $R_{t}$ and $r_t$ are measure of the relative movements of price, which is a useful property for comparison of performance between assets. Another technical reason we might be more interested in simple return and log return than raw price is that they could be used as a remedy for non-stationarity of price time-series (differencing).

<div class='figure'>
    <img src="/image/VaR/McDonald's price, return and log return.png"
         style="width: 100%; display: block; margin: 0 auto;"/>
    <div class='caption'>
        <span class='caption-label'>Figure 1.</span> Closing prices (top), simple returns (middle) and log returns (bottom) of McDonald's stock for past five years.
    </div>
</div>

 As seen on the chart of McDonald's stock price, it is impossible to conduct a statiscal analysis with this due to non-stationarity. The substantial similarity simple return and log return charts exhibit is linked with the fact that the simple return metric is an approximation for  log return by a first-order Talyor expansion.

The first order Taylor Expansion of $log(x)$ around $x=1$ is as follows.

$$\begin{align}
log(x)
&\simeq log(1)+log^{'}(1)(x-1)\\
&\simeq 0+\dfrac{1}{1}(x-1)
\end{align} \tag{3}$$

So when $\dfrac{P_{t}}{P_{t-1}}\simeq 1$

$$\begin{align}
r_{t}
&\simeq log \left(\dfrac{P_{t}}{P_{t-1}}\right)\\
&\simeq \dfrac{P_{t}}{P_{t-1}}-1\\
&\simeq \dfrac{P_{t}-P_{t-1}}{P_{t-1}}\\
&\simeq R_{t}
\end{align} \tag{4}$$


## Value at Risk (VaR)

Cool!

add equations and Let's say the company is mcdonald and  change the title more attractive to general public (for example how much can we expect to lose from investment in mcdonald stock)
