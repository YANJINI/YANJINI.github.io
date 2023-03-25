---
title: Proof of Consistency using Markov's Inequality
subtitle: Markov's inequality is a useful statistical tool used to prove the consistency of an estimator. I discuss the proof of Markov's inequality and demonstrate its use in proving the consistency of an example estimator.
layout: default
date: 2023-02-15
keywords: blogging, writing
published: true
categories: [all, econ]
---

During the first class of my advanced Econometrics course at graduate school, I volunteered to solve some exercise problems that involved proving certain estimators were unbiased and consistent mathematically. While proving whether estimators were unbiased was trivial, demonstrating consistency was more challenging. However, I remembered from my self-study in Econometrics that if the variance of the estimator converges to zero probabilistically, we can conclude that the estimator is consistent. The proof is closely related to Markov's Inequality, but I had difficulty formulating it during the class. After the class, I revisited [an old post]((https://blog.naver.com/boadoboado11/222684138583)) I had written about Markov's Inequality in Korean from a year ago and decided to write this post.

## Markov's Inequality
Markov's Inequailty is fomulated as follows.

$P(X \geq a) \leq \dfrac{E(X)}{a} \quad \forall_X \geq 0, \forall_a \geq 0 \tag{1}$

Here $X$ is a positive random variable with a finite mean and variance, and $a$ is a positive constant. The inequality always holds regardless of the distribution of $X$. The proof is quite straightforward.

When there is a continuous random variable $X$ and $f(X)$ is a probability density function of $X$,

$$\begin{align}
E(X)
&= \displaystyle{\int_{0}^{\infty}} xf(x)dx\\
&= \displaystyle{\int_{0}^{a}} xf(x)dx + \displaystyle{\int_{a}^{\infty}} xf(x)dx\\
&\geq \displaystyle{\int_{a}^{\infty}} xf(x)dx\\
&\geq \displaystyle{\int_{a}^{\infty}} af(x)dx = aP(X \geq a) \quad \because x \geq a
\end{align} \tag{2}$$

It is easy to show the Equation $1$ holds by dividing the both sides of the Equation $2$. We can use this inequality to prove consistency of estimators. Consistency of estimators means an estimator probabilistically converges to its true parameter as the sample size goes to infinity. The mathematical definition of consistency is as follows.

When $\hat{\theta}_{N}$ is an estimator with sample size of $N$ and the true parameter is $\theta$,

$P\left(\|\hat{\theta}\_{N} - \theta\| \geq \epsilon \right) \xrightarrow{N \to \infty} 0 \tag{3}$

Here $\epsilon$ is any positive number. Let's square both sides and apply the Markov's Inequality.

$P\left((\hat{\theta}\_{N} - \theta)^{2} \geq \epsilon^{2} \right) \leq \dfrac{E\left[ (\hat{\theta}_{N} - \theta)^{2} \right]}{\epsilon^{2}}\tag{4}$

$E\left[ (\hat{\theta}_{N} - \theta)^{2} \right]$ on the right-hand side in Equation $4$ is just a variance notation when $\hat{\theta}\_{N}$ is an unbiased estimator. So, if we show the variance of the estimator with its true mean converges to zero, we can say that the estimator consistent.



## Proof of consistency
For $iid$ random variable $X_{i}$ from the normal distribution with mean $\mu$ and variance $\sigma^{2}$,

$$\hat{\theta}_{N} = \dfrac{1}{N} \sum_{i=1}^{N} X\_{i} \tag{5}$$

Let's calculate the variance of the estimator.

$$\begin{align}
Var\left(\hat{\theta}_{N}\right) 
&= Var\left(\dfrac{1}{N} \sum_{i=1}^{N}X\_{i}\right)\\
&= \dfrac{1}{N^{2}} \sum_{i=1}^{N} Var(X_i) \quad \because iid\\
&= \dfrac{1}{N}\sigma^{2}
\end{align} \tag{6}$$

The variance definitely converges to zero when $N \to \infty$. 

$$\begin{align}
P\left(|\hat{\theta}_{N} - \mu| \geq \epsilon \right) 
&= P\left((\hat{\theta}_{N} - \mu)^{2} \geq \epsilon^{2} \right)\\
&\leq \dfrac{E\left[ (\hat{\theta}_{N} - \mu)^{2} \right]}{\epsilon^{2}} = \dfrac{Var\left(\hat{\theta}_{N}\right)}{\epsilon^{2}}

\xrightarrow{N \to \infty} 0 
\end{align} \tag{7}$$
