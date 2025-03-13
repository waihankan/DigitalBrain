---
title: Induced Norm
tags:
  - cs189
  - eecs127
  - linear-algebra
  - machine-learning
  - statistics
---

> [!important] Theorem: Induced Norm Theorem
> A norm $\lVert.\rVert \text{ on } \mathbb{R}^n$ that is induced by an inner product, there exists a symmetric positive definite (PD) matrix A such that:
> $$
> \lVert x \rVert ^{2} = x^TAx, \; \forall x  \in \mathbb{R}^n
> $$
> * If A = I, the norm is **Euclidean Norm** 
> * If A is a **diagonal matrix with positive entries**, we get a weighted norm: $\lVert x \rVert _A = x^TAx = \sum_{i=1}^{N}a_{i}x_{i}^2$, where $a_{i}$ are diagonal elements of A (weights).
> * If A is a **covariance matrix**, this norm corresponds to the **Mahalanobis norm**, common in statistics and machine learning.

^ae513b

> [!important] **Ellipsoid Norm Theorem**
>  Every quadratic form defines an ellipsoid, showing a geometric interpretation of norms.
