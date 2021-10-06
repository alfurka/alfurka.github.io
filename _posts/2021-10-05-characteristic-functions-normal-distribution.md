---
layout: post
title: Understanding the Characteristic Function
subtitle: Example: Characteristic Function of a Normal Distribution
tags: [statistics, characteristic function, normal distribution, method of moments]
image: /img/hoca.jpg
bigimg: /img/table.jpg
---

I knew the textbook definition and some uses of the characteristic function. But I have never used it before. You know what they say; if you do not use it, you cannot learn it well! I have been revisiting the asymptotic theory recently, and I have come across with characteristic functions again. I did proofs by hand and did some examples with it so that I could grasp it well. I expect to use it later. This article summarizes my understanding of the characteristic function and how it relates to some fundamental theorems in statistics. There is also an example with the normal distribution. 

## The Background of Characteristic Functions

Sometimes it is difficult to show the converge in the distribution of a sequence of random vectors. In such cases, we `transform` the random vectors where that transformation is easier to work (easier to use in proofs). Such transformation is done with a `characteristic function.` It is actually a `characterization` of the Portmanteau lemma:

> For any random vectors $X_n$ and $X$, $E[f(X_n)] \to E[f(X)]$ for all bounded and continuous functions $f$. 

The Portmanteau lemma has some other equivelant statements. It basically says that if $X_n \to X$ then  $E[f(X_n)] \to E[f(X)]$  for all bounded and continuous functions. Here we characterize this function with:

$$\varphi _{X}(t)=E [e^{itX}]$$

Let $X\in R^k$. The Portmanteau lemma says that $E[e ^{it^TX_n] \to E[e ^{it^TX]$ for every $t\in R^k$ if $X_n$ converges in distribution to $X$. More importantly, by Levy's Continuity Theorem, the converse is true. 

> Let $X_n$ and $X$ be random vectors in $R^k$. Then $X_n$ converges in distribution to $X$ if and only if $E[e ^{it^TX_n] \to E[e ^{it^TX]$ for every $t\in R^k$. Moreoever, if $E[e ^{it^TX_n]$ converges pointwise to a function $\varphi (t)$ that is continuous at zero, then $\varphi $ is the characteristic function of a random vector $X$ and $X_n \to X$ (in distribution).

