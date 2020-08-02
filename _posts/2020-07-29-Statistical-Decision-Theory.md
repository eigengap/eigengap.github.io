---
layout: post
title: Statistical Decision Theory
date: 2020-08-02
summary: A quick review of statistical decision theory from the HTF book.
categories: machine-learning
tag: machine learning
---

I am reviewing my ML knowledge starting from the basic with "The Elements of Statistical Learning" 
book by Hastie, Tibshirani and Friedman.

One fundamental theory is the statistical decision theory, which is the principle guide to how to
predict the outcome of new data given previously observed data. It revolves around the concept of
*Expected Prediction Error* ($$\mathrm{EPE}$$).

Let's review [the notations](https://en.wikipedia.org/wiki/Notation_in_probability_and_statistics) 
first because I usually find it hard to follow the notations by statisticians.

Using the squared loss, given a function $$f$$ to predict the value $$y$$ for the input $$x$$,
the $$\mathrm{EPE}$$ of $$f$$ is:

$$\mathrm{EPE}(f) = \mathrm{E}[(f(X) - Y)^2]$$

The expectation is over the join distribution of $$(X, Y)$$. The goal is to find $$f$$ such that
$$\mathrm{EPE}(f)$$ is minimized. By the [law of total expectation](https://en.wikipedia.org/wiki/Law_of_total_expectation),
we have:

$$\mathrm{EPE}(f) = E_X[E_{Y|X}[(f(X) - Y)^2|X]],$$

where $$E_X$$ is the expectation over $$X$$. From this, 

$$\hat{f}(x) = \mathrm{argmin}_{t}~E_{Y|X}[(t - Y)^2|X=x],$$

for every $$x$$ is the desired function. The solution of this is:

$$f(x) = \mathrm{E}[Y|X=x].$$

Note that this is the solution for any distributions (not just normal distribution). Let

$$\begin{eqnarray}
g(t) &=& \int (t - y)^2p(y)dy \\
&=& \int (t^2 - 2ty + y^2)p(y)dy \\
&=& t^2 - 2t\mathrm{E}[Y] + \mathrm{E}[Y^2].
\end{eqnarray}$$

Clearly, $$g(t)$$ attains its minimum at $$t = \mathrm{E}[Y]$$.

The $$\mathrm{EPE}$$ based on the squared loss also relates to the Bias-Variance decomposition
for the additive noise model $$Y = f(X) + \epsilon$$, where $$\mathrm{E}(\epsilon) = 0$$ and
$$\mathrm{Var}(\epsilon) = \sigma^2$$.
Let $$f_*$$ be the true function, $$\hat{f}$$ be the estimate of $$f_*$$
given the training data $$D$$ and

$$\begin{eqnarray}
\mathrm{Bias}[\hat{f}(x)] &=& \mathrm{E}_D[\hat{f}(x)] - f_*(x)~~\text{and} \\
\mathrm{Var}[\hat{f}(x)] &=& \mathrm{E}_D[(\hat{f}(x) - \mathrm{E}_D[\hat{f}(x))^2]= \mathrm{E}_D[\hat{f}(x)^2] - \mathrm{E}_D[\hat{f}(x)]^2.
\end{eqnarray}$$

Then,

$$\mathrm{EPE}(f(x)) = \sigma^2 + \mathrm{Bias}^2[\hat{f}(x)] + \mathrm{Var}[\hat{f}(x)].$$

Note that the $$\mathrm{EPE}$$ of $$f$$ here is computed for a given input $$x$$.

One last note is an interesting exercise (E.x 2.9) which states that the expected training error of
a linear least square model is less than its expected test error.

$$\mathrm{E}[R_{tr}(\hat{\beta})] \leq \mathrm{E}[R_{te}(\hat{\beta})],$$

where $$\hat{\beta}$$ is the estimate of the linear model $$y = \beta^Tx$$ and

$$\mathrm{E}[R_{tr}(\hat{\beta})] = 1/N\sum_{i=1}^{N}(y_i - \hat{\beta}^Tx_i)^2),$$

$$\mathrm{E}[R_{tr}(\hat{\beta})] = 1/M\sum_{i=1}^{M}(\tilde{y}_i - \hat{\beta}^T\tilde{x}_i)^2.$$

for random sets of training data $$\{(x_i,y_i)\}$$ and testing data $$\{(\tilde{x}_i, \tilde{y}_i)\}$$.

This is a good exercise for playing with expectation over different random variables and for a lack
of ultimate effort, I found a solution on [stats.stackexchange](https://stats.stackexchange.com/questions/310687/prove-that-the-expected-mse-is-smaller-in-training-than-in-test).

---
[^1]: **Disclaimer**: This material is for informational purposes only, and should not be construed as legal advice or opinion.  For actual legal advice, you should consult with professional legal services.
[^2]: This list of privileges are derived from the four freedoms of "The Free Software Definition" published by the GNU project <https://www.gnu.org/philosophy/free-sw.en.html>.
[^3]: Using a different name from "Pixyll" for your derivate work helps avoid misdirected questions from people who are using your version.  It's similar to using version numbers to discrimate the revisions of software when troubleshooting issues.
