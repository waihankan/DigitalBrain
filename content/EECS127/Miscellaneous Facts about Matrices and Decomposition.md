---
title: Miscellaneous Facts about Matrices and Decomposition
tags:
  - linear-algebra
---
> [!important] Symmetric Matrix
> If the matrix A is real, then a square matrix A is symmetric if $A = A^T$. However, for complex matrices, the matrix is "symmetric | Hermitian" $\iff A = A^H$ is a **Hermitian** Matrix (conjugate transpose of $\mathbf{A}$).

Characteristics of **Real** Symmetric Matrices:
* They have **real** eigenvalues.
*  **A symmetric matrix can have negative eigenvalues**.
* Their eigenvectors corresponding to *distinct* eigenvalues are orthogonal.
* They are always diagonalizable. (*Spectral Theorem*)

> [!important] Positive Semi Definite Matrix
> A symmetric or Hermitian matrix is PSD if for any vector x, $\vec{x}^TA \vec{x}\geq 0$. 

**Positive Semi Definite** need not to be Symmetric and obviously, symmetric matrices need not to be Positive Semi Definite.

Positive Semi Definite Matrix ($A\geq{0}$)
* $\iff$ all eigenvalues are **non-negative**.
* Cholesky Decomposition: $\iff$ there exists a matrix B such that $B^TB = A$. B is a lower triangular matrix.
* Singular values = Eigen values $\sigma = |\lambda|$

*For general matrices (non-symmetric, non-Hermitian), there's no direct equality between eigenvalues and singular values.*

