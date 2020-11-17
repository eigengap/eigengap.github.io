---
layout: post
comments: true
title: Random Projection
date: 2020-08-02
summary: Random Projection
categories: machine-learning
tags: machine-learning random-projection
---

Random projection is a very useful tool to deal with massive amounts of high dimensional data.
It is an efficient way to project the data from a high dimensional space to a much lower
dimensional space without much sacrifice of the $$L_2$$ distances between data points.

Given a dataset $$D$$ of $$n$$ points in $$\mathrm{R}^{K}$$. Let $$A$$ be a matrix of size $$k \times K$$
whose entries are i.i.d as $$N(0, 1)$$. It can be shown that, for $$k = O(\log n)$$, with high probability,
such matrix has the following property:

$$(1-\epsilon)\|u-v\|^2 \le \frac{1}{k}\|Au - Av\|^2 \le (1+\epsilon)\|u-v\|^2,$$

for all $$u, v \in D$$.

*Lemma.* Let $$x = Au$$ and $$x_i$$ be the $$i$$-th element of $$x$$, then $$x_i \sim N(0, \|u\|^2)$$, i.e. $$\mathrm{E}[x_i] = 0$$ 
and $$\mathrm{E}[x_i^2] = \|u\|^2$$.

*Proof.* $$x_i = \sum_{j=1}^{K}A_{ij}u_j$$ is a linear combination of normally distributed random variables with 0 mean $$A_{ij}$$,
hence it is also normally distributed with 0 mean. Also,

$$\mathrm{E}[x_i^2] = \mathrm{E}[(\sum_{j=1}^{K}A_{ij}u_j)^2] = \mathrm{E}[\sum_{j=1}^{K}A_{ij}^2u_j^2] = \|u\|^2. \square$$

*Lemma.* With probability of at least $$1-2e^{-(\epsilon^2-\epsilon^3)k/4}$$:

$$(1-\epsilon)\|u\|^2 \le \|\frac{1}{\sqrt{k}}Au\|^2 \le (1+\epsilon)\|u\|^2.$$

*Proof.* The proof is a generic [Chernoff bounding](https://en.wikipedia.org/wiki/Chernoff_bound) method.
Consider the random variable $$z_i = x_i / \|u\|$$, then $$z_i \sim N(0,1)$$.

$$\begin{eqnarray}
\mathrm{P}[\sum_{i=1}^{k} z_i^2 \geq (1+\epsilon)k] &=& \mathrm{P}[e^{\lambda\sum_{i=1}^{k} z_i^2} \geq e^{(1+\epsilon)\lambda k}] \\
&\leq& \mathrm{E}[e^{\lambda\sum_{i=1}^{k} z_i^2}].e^{-(1+\epsilon)\lambda k}~~~~&(1) \\
&=& (\mathrm{E}[e^{\lambda z_1^2}])^k.e^{-(1+\epsilon)\lambda k}&(2) \\
&=& (1-2\lambda)^{-k/2}.e^{-(1+\epsilon)\lambda k}.&(3)
\end{eqnarray}$$

where (1) is by [Markov's inequality](https://en.wikipedia.org/wiki/Markov%27s_inequality), (2) is by product of expectations,
and (3) is by the [moment generating function](https://en.wikipedia.org/wiki/Moment-generating_function) of the 
[$$\chi_k^2$$ distribution](https://online.stat.psu.edu/stat414/lesson/15/15.8#paragraph--771) for $$0 < \lambda < \frac{1}{2}$$
($$z_i^2$$'s are i.i.d with $$\chi_k^2$$). $$\lambda=\frac{\epsilon}{2(1+\epsilon)}$$ ($$<\frac{1}{2}$$) is the minimizer of the last term, therefore:

$$\mathrm{P}[\sum_{i=1}^{k} z_i^2 \geq (1+\epsilon)k] \leq [(1+\epsilon)e^{-\epsilon}]^{-k/2} \leq e^{-(\epsilon^2-\epsilon^3)k/4},$$

where the last inequality follows $$1+\epsilon \leq e^{\epsilon-\frac{\epsilon^2-\epsilon^3}{2}}$$. Similarly for the other side. $$\square$$

Finally, using [the union bound](https://en.wikipedia.org/wiki/Boole%27s_inequality) on $$\frac{n(n-1)}{2} < \frac{n^2}{2}$$ pairs $$(u, v)$$ in $$D$$, we have:

$$\begin{eqnarray}
\mathrm{P}((1-\epsilon)\|u-v\|^2 \le \frac{1}{k}\|Au - Av\|^2 \le (1+\epsilon)\|u-v\|^2,~\forall u,v\in D) \\
> 1 - n^2e^{-(\epsilon^2-\epsilon^3)k/4}.
\end{eqnarray}$$

Choosing $$k=\frac{20\log(n)}{\epsilon^2}$$, the above probability if at least $$1 - n^{\epsilon - 3}$$.

The Chernoff method is generic enough to extend this results to iid cases that are not normal, e.g. bounded random variables
(similar to [Hoeffding's inequality](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)).

For more results, e.g., on inner products under random projection, see reference [1].

A couple of things to remember:
- [Markov's inequality](https://en.wikipedia.org/wiki/Markov%27s_inequality) and [Chernoff bounding](https://en.wikipedia.org/wiki/Chernoff_bound) method
is a frequently used tool for concentration of measure bounds for iid random variables.
- [Moment generating function](https://en.wikipedia.org/wiki/Moment-generating_function) is needed for the Chernoff method.
- Inequalities involve $$e$$ and $$\log$$
- [The union bound](https://en.wikipedia.org/wiki/Boole%27s_inequality)

References.
- [1] This post is based on [Sham Kakade's lecture on JL](https://ttic.uchicago.edu/~gregory/courses/LargeScaleLearning/lectures/jl.pdf)
- [2] [Inequalities cheat sheet](http://www.lkozma.net/inequalities_cheat_sheet/ineq.pdf)