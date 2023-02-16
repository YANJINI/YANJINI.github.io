---
title: How Risky is McDonald's Stock?
subtitle: I estimate the daily Value at Risk (VaR) of McDonald's stock in R by using a GARCH model that takes into account heavy-tailed White Noise innovations.
layout: default
date: 2023-02-15
keywords: blogging, writing, project
published: false
categories: [all, qf]
---
In finance, Value at Risk (VaR) is a crucial metric for assessing the risk of an investment portfolio. VaR provides an estimate of rare but potential loss an investment may face over a specific time horizon and confidence level. To calculate VaR, it's necessary to have an understanding of the underlying distribution of investment returns.

To accurately determine this distribution, various models from time series analysis can be used. The GARCH model, in particular, is known for its effectiveness in analyzing financial data. One of the main reasons to choose GARCH over other models, such as empirical distribution (actually it is even model-free), random walk, or ARIMA, is that it can capture the volatility dynamics of the stock return, which is essential for accurate VaR calculation of financial asset return.

## Prices, simple returns, log returns
Whose distribution exactly do we want to figure out for VaR calculation?  Long story short, we identify the distribution of log returns of McDonald's stock due to the desirable characteristics of log returns for statistical analysis purpose.

The terms "prices", "simple returns" and "log returns" refer to different ways of measuring the performance of an investment over time. Price is the most common form of financial asset data we can encounter and the other two are transformed version of this. Let $P_{t}$, $R_{t}$ and $r_{t}$ respectively denote the price, one-period simple return and one-period log return of McDonald's stock at time $t$. Then $R_{t}$ and $r_{t}$ are defined as

$R_{t}=\dfrac{P_{t}-P_{t-1}}{P_{t-1}}\tag{1}$

$r_{t}=log \left(\dfrac{P_{t}}{P_{t-1}}\right)=log(1+R_{t})\tag{2}$

The log return is frequently referred to as the continuously compounded return on account of the following mathematical principle.

$$\begin{align}
& e^{r_{t}}=\dfrac{P_{t}}{P_{t-1}}\\
& P_{t}=P_{t-1}e^{r_{t}}
\end{align} \tag{3}$$

Both  $R_{t}$ and $r_t$ are measure of the relative movements of price, which is a useful property for comparison of performance between assets. Another technical reason we might be more interested in simple return and log return than raw price is that they could be used as a remedy for non-stationarity of price time-series (differencing).

<div class='figure'>
    <img src="/image/VaR/McDonald's price, return and log return.png"
         style="width: 100%; display: block; margin: 0 auto;"/>
    <div class='caption'>
        <span class='caption-label'>Figure 1.</span> Closing prices (top), simple returns (middle) and log returns (bottom) of McDonald's stock for past five years.
    </div>
</div>

See [A1](#code-1) for the code to generate this figure.

As seen on the chart of McDonald's stock price, it is impossible to conduct a statistical analysis with this due to the non-stationarity.

The substantial similarity simple return and log return charts exhibit is linked with the fact that the simple return metric is an approximation for  log return by a first-order Talyor expansion. The first order Taylor Expansion of $log(x)$ around $x=1$ is as follows.

$$\begin{align}
log(x)
&\simeq log(1)+log^{'}(1)(x-1)\\
&\simeq 0+\dfrac{1}{1}(x-1)
\end{align} \tag{4}$$

So, when $\dfrac{P_{t}}{P_{t-1}}\simeq 1$

$$\begin{align}
r_{t}=log \left(\dfrac{P_{t}}{P_{t-1}}\right)
&\simeq \dfrac{P_{t}}{P_{t-1}}-1\\
&\simeq \dfrac{P_{t}-P_{t-1}}{P_{t-1}}=R_{t}
\end{align} \tag{5}$$

When $R_{t}$ is small enough, the difference between simple return and log return is negligible. Log return, however, is often chosen over simple return at least for two practical reasons.

First, log returns are additive, which makes it simpler to calculate the total return of an investment over time. Let $r_{k,\ t}$ denote log return over $k$ period at time $t$, then it is defined as

$$\begin{align}
r_{k,\ t}
&=log \left(\dfrac{P_{t}}{P_{t-k}}\right)\\
&=log \left(\dfrac{P_{t}}{P_{t-1}}\right)+log \left(\dfrac{P_{t}}{P_{t-2}}\right)+\ ...\ +log \left(\dfrac{P_{t}}{P_{t-k}}\right)\\
&=r_{1,\ t}+r_{1,\ t-1}+\ ...\ +r_{1,\ t-k}\\
&=r_{t}+r_{t-1}+\ ...\ +r_{t-k}
\end{align} \tag{6}$$

Second, simple returns are inherently asymmetric due to the limited liability issue (simple returns are bounded by $-100\%$) and positive skewness, and logarithmic transformation remedies this problem by mapping the range of $(-1, 0)$ to the range of $(-\infty, 0)$ and limiting big positive values.

To summarize, log returns are often preferred in the statistical analysis of financial assets due to their suitability for comparison, symmetric nature, and ease of calculation for different time horizons. So, I will figure out the distribution of the log returns of McDonald's stock for calculating Value at Risk (VaR).


## Value at Risk (VaR)
Value at Risk (VaR) is the most broadly used risk metric in finance and is defined as follows.

$P(r_{k,\ t} \leq VaR_{k;\ 1-\alpha})=\alpha \tag{7}$

In plain English, equation (7) means that the probability of the return of an asset in a given $k$ period being less than or equal to the value of the VaR for that $k$ period at a confidence level of $1 - \alpha$ is equal to $\alpha$. The most common values for $\alpha$ are $1\%$ and $5\%$, which are also universal values of significance level for hypothesis testing.

<div class='figure'>
    <img src="/image/VaR/Visualization of empirical VaR of McDonald's stock.png"
         style="width: 65%; display: block; margin: 0 auto;"/>
    <div class='caption'>
        <span class='caption-label'>Figure 1.</span>The empirical 1st period VaR of log returns of McDonald's stock with confidence level of $95\%$ is $-3.05\%$. In mathematical notation, $P(r_{t} \leq VaR_{95\%})=5\%$
    </div>
</div>

See [A2](#code-2) for the code to generate this figure.

What we are trying to figure out with VaR is also called "tail risk" and why it goes by the name is intuitively visualized on Figure $2$. The term "tail risk" is used to describe the potential for losses or extreme events that fall outside the normal range of outcomes and occur in the tails of a probability distribution.

The histogram drawn on Figure $2$ is like an empirical distribution, which is a probability distribution based on historical data rather than a theoretical model, which is why it is also called "model-free" distribution. The main assumption behind the choice to use empirical distribution for VaR calculation is that the probability distribution of returns in the past is a good representation of the probability distribution of returns in the future. This is not always valid, especially when the market is showing high volatility. Accurately characterizing the distribution of the data is crucial for VaR calculation, as the choice of distribution significantly impacts the accuracy of the calculated VaR. The issue poses the question, "What is the appropriate model that can accurately reflect the underlying distribution of the data?"


## GARCH model with $t$-distribution
GARCH stands for Generalized AutoRegressive Conditional Heteroskedasticity, in which conditional variance of the data is modeled in an autoregressive way. Let $X_{t}$ denote the time-series (in our case, log returns of McDonald's stock), then we can assume the following GRACH $(p, q)$ structure.

$X_{t}=\mu+E_{t}=\mu+\sigma_{t}W_{t} \tag{8}$

$\sigma_{t}^{2} = \alpha_0 + \sum_{i=1}^{p}{\alpha_i E_{t-i}^{2}} + \sum_{i=1}^{q}{\beta_i \sigma_{t-i}^2} \tag{9}$

$W_{t} \sim t_{v} \tag{10}$

$\mu$ is the constant conditional expectation and $\sigma_{t}^{2}$ is the conditional variance varying with GARCH $(p, q)$ structure. E_t는 뭔가 $W_{t}$ is a White Noise innovation that is $iid$ from a standardized Student's $t$-distribution with $v$ degrees of freedom, $t_{v}(0, 1)$ or simply $t_{v}$ instead of the commonly used standard Gaussian distribution to incorporate the heavy-tailed property of the financial log returns.

여기에 GARCH 구조

## McDonald's stock log returns distribution
Now finally I am going to construct the McDonald's stock log returns distribution from the above GARCH structure with $t$-distribution. This distribution is time-dependent due to the time-dependent structure of volatility (GARCH). It is intuitive, because what we want to know is not a general time-independent distribution of log returns but a distribution of log returns at some time $t$ so that we can take into account a dynamic volatility structure.

Once we estimate and substitute $\hat{\sigma}\_{t}$ for $\sigma_{t}$, $\hat{\mu}$ for $\mu$ and $\hat{v}$ for $v$ so $\hat{W}\_{t}$, we can think of our log return at time $t$, $X_{t} = \hat{\mu} + \hat{\sigma}_{t}\hat{W}\_{t}$, as a random variable from a non-standardized $t$-distribution $t\_{\hat{v}}(\hat{\mu}, \hat{\sigma}\_{t}^{2})$. It is because the estimated White Noise term, $\hat{W}\_{t}$ follows a standardized $t$-distribution with $\hat{v}$ degrees of freedom, $t\_{\hat{v}}$.

실제 R코드에 넣어보고 결과를 가지고와서 위 식으로 그대로 재현하고 plot도 해놓기





## Appendix

<a id="code-1"></a>
### A1: Prices, simple returns and log returns plots
I imported the closing price data of McDonal's stock for the past five years from Yahoo Finance through `getSymbols` function in `quantmod` library.
```R
# load Mcdonald's stock data
library(tidyquant)

options("getSymbols.warning4.0"=FALSE)
options("getSymbols.yahoo.warning"=FALSE)

getSymbols("MCD", from = '2018-01-01',
           to = "2023-01-01",warnings = FALSE,
           auto.assign = TRUE)


# price, return, log return into one xts object
MCD <- MCD[, "MCD.Close"]
MCD_sreturn <- diff(MCD)/stats::lag(MCD)  ## dplyr::lag(MCD) generates error
MCD_lreturn <- diff(log(MCD))

MCD_perform <- merge(MCD, MCD_sreturn, MCD_lreturn)
colnames(MCD_perform) <- c("Price", "Simple_return", "Log_return")


# plot the three
library(ggplot2)

autoplot(MCD_perform) +
  xlab("Year") +
  scale_x_date(date_breaks = "1 year") +
  facet_grid(Series ~ ., scales="free_y",
             switch="y") +
  theme(strip.background = element_blank(),
        panel.background = element_blank(),
        panel.grid.major =  element_line(colour="#E7E7E7"),
        panel.grid.minor = element_line(colour="#E7E7E7"),
        axis.line = element_line(colour = "black"),
        strip.placement = "outside",
        strip.text = element_text(size = 14),
        axis.title.x = element_text(size = 14),
        axis.text = element_text(size=14)) +
  geom_line(color="#1F77B4", size=0.5)
```

<a id="code-2"></a>
### A2: Empirical VaR plot

```R
# plot 1-period empirical VaR with confidence of 95%
MCD_lreturn <- diff(log(MCD))
VaR <- quantile(MCD_lreturn, 0.05)

hist(returns, breaks = 50, col = "#1F77B4", main = "", xlab = "Daily Log Return",
     ylab = "Frequency", ylim=c(0, 650))
abline(v = VaR, col = "#D62728", lwd = 2, lty = 2)
legend("topright", legend = paste0("VaR (95% confidence): ", round(VaR * 100, 2),
                                   "%"), col = "#D62728", lty = 2, cex = 0.8)
```
