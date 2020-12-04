---
layout: post
comments: true
title: Robust Resource Allocation
date: 2020-12-03
summary: Some notes on robust resource allocation
categories: machine-learning robust-optimization
tag: machine learning
tag: robust optimization
---

Consider the problem of optimizing the return of a portfolio of $$n$$ items, with a reward vector $$r \in \mathbb{R}^n$$.
The total reward given the allocation vector $$w \in [0..1]^n$$, where $$\sum_i w_i = 1$$, is $$w^Tr$$.

If there are no uncertainties in the rewards, the allocation is trivially choosing the best item, i.e. $$w_i = 1$$
for $$i = \mathrm{argmax}_i~r_i$$, and $$w_i = 0$$ otherwise.

The problem is more interesting if we are not certain about $$r$$. One approach is to assume $$r$$ belongs to an uncertainty
set $$U$$, which will be discussed later. Given, $$r \in U$$, one way to deal with this uncertainty is to find $$w$$ to:

$$\mathrm{maximize}_w~\mathrm{min}_{r\in U}~w^Tr~~~~(*),$$

which means to find $$w$$ that minimizes the worst case scenario of the total reward among all $$r$$ in the uncertainty set.
This is a very special case of the *Uncertain Linear Optimization* problem which is addressed in depth in the book
["Robust Optimization" by Aharon Ben-Tal, Laurent El Ghaoui and Arkadi Nemirovski](https://www2.isye.gatech.edu/~nemirovs/FullBookDec11.pdf). 
What follows is also a very special treatment of the problem compared to those discussed in the book.

Now, what are some reasonable uncertainty sets that render this problem tractable, i.e. can be solved efficiently?
A simple but pretty reasonable one is the *ellipsoidal uncertainty*:

$$U(\rho) = \{r~|~\|\Sigma^{-1/2}(r-\tilde{r})\|_2 \le \rho \}.$$

This comes from assuming $$r \sim N(\tilde{r}, \Sigma)$$ where $$\tilde{r}$$ and $$\Sigma$$ are the mean and
covariance matrix respectively, and taking a region of certain confidence.

With this uncertainty set, the inner minimum of problem (*) has a closed form solution.

$$\mathrm{min}_{r\in U(\rho)}~w^Tr = w^T\tilde{r} - \rho\|\Sigma^{1/2}w\|_2.$$

This is derived by applying Cauchy-Schwarz on $$w^T(r-\tilde{r})$$. Problem (*) becomes:

$$\mathrm{maximize}_w~w^T\tilde{r} - \rho\|\Sigma^{1/2}w\|_2.$$

This can be converted to a SOCP and can be solved efficiently.

