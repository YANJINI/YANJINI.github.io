---
title: Why Differentiation for Optimization?
subtitle: Optimization involves finding the minimum or maximum of unknown functions, and is closely related to differentiation. I explore the connection between differentiation and optimization by deriving the so-called First Order Condition (FOC) and Second Order Condition (SOC) from Taylor series.
layout: default
date: 2023-03-26
keywords: blogging, writing, project
published: true
categories: [all, opt]
---

## Taylor's Series
Taylor's Series is a useful mathematical tool for approximating an unknown function with polynomial terms around a specific point. Let $f(x)$ be an infinitely-differentiable function of a variable $x$ and $a$ be the point around which we want to approximate the function. The function can be represented as

$f(x) = c_0+c_{1}(x-a)+c_{2}(x-a)^{2}+c_{3}(x-a)^{3}+...\tag{1}$

Here, we subtract the constant $a$ from all polynomial terms for notational convenience. We can find the coefficients for the polynomials $c_0, c_1, c_2,$ and so on, by taking the value of the derivatives of both sides at the point $x=a$ as follows.

$$\begin{align}
f(a) &= c_{0}\\
f^{'}(a) &= 1! \cdot c_{1}\\
f^{''}(a) &= 2! \cdot c_{2}\\
f^{'''}(a) &= 3! \cdot c_{3}\\
& \vdots
\end{align} \tag{2}$$

Then we can rewrite Equation $1$ as follows.

$$\begin{align}
f(x) &= f(a) + \dfrac{f^{'}(a)}{1!}(x-a) + \dfrac{f^{''}(a)}{2!}(x-a)^{2} + \dfrac{f^{'''}(a)}{3!}(x-a)^{3} + ...\\
&= \sum^{\infty}_{i=0}\frac{f^i(a)}{i!}(x-a)^i
\end{align} \tag{3}$$

Here, $f^{(i)}$ denotes the $i$-th order derivative of $f$, and $f^{(0)} = f$. The approximation is well-behaved when $x$ is close enough to $a$. The plot of the log function approximation around $x=1$ below shows this property well. The approximations get poorer as $x$ gets away from $1$.

<a id="Figure-1"></a>

<div class='figure'>
    <img src="/image/why_differentiation/log_taylor.png"
         style="width: 80%; display: block; margin: 0 auto;"/>
    <div class='caption'>
        <span class='caption-label'>Figure 1. Taylor approximations of log function around $x=1$.</span> 
    </div>
</div>

See [A1](#code-1) for the code to generate this figure.

We can further generalize Equation $3$ by setting $x = a+h$, where $h$ is a small enough constant.

$$\begin{align}
f(a+h) &= f(a) + \dfrac{f^{'}(a)}{1!}h + \dfrac{f^{''}(a)}{2!}h^{2} + \dfrac{f^{'''}(a)}{3!}h^{3} + ...\\
&= \sum^{\infty}_{i=0}\frac{f^i(a)}{i!}h^i
\end{align} \tag{4}$$

Equation $4$ is useful because it explicitly shows how the output changes as the input varies between $a+h$ and $a$, which can be calculated as $f(a+h) - f(a)$. This is fundamental for numerical optimization, where we try to find the optimal point of an unknown function given only input and output. 


## First Order Condition (FOC)

## Second Order Condition (SOC)

## Complete Second Order Condition (CSOC)

## Appendix

<a id="code-1"></a>
### A1: Plot Taylor approximations of log

```R
# plot log function
xx = seq(0, 4, by = 0.01)
yy = log(xx)

plot(xx, yy, type = "l",
     xlab = "x", ylab = "log(x)", 
     main = "The log function", 
     ylim = c(-2, 2),
     xlim = c(0, 2))



# plot Talor approximations of log function around x = 1
x = 1
hh = seq(-1, 3, by = 0.01)

first_ord = log(x) + (x)^(-1) * hh
second_ord = first_ord - factorial(2) * (x)^(-2) * hh^2
third_ord = second_ord + factorial(3) * (1/2)*x^(-3) * hh^3
fourth_ord = third_ord - factorial(4) * (1/6)*x^(-4) * hh^4

lines(x+hh, first_ord, col = "blue")
lines(x+hh, second_ord, col = "red") 
lines(x+hh, third_ord, col = "orange")
lines(x+hh, fourth_ord, col = "purple")

legend("topleft", legend=c("Log function", "First order", "Second order", "Third order", "Fourth order"), 
       col=c("black", "blue", "red", "orange", "purple"), lty=1)
  

```