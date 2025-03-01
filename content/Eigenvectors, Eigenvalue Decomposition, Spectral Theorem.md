---
title: Eigenvectors, Eigenvalue Decomposition, Spectral Theorem
draft: 
tags:
  - cs189
  - linear-algebra
---
Useful prerequisite: [[Miscellaneous Facts about Matrices and Decomposition]]

Given a square matrix A, if $Av = \lambda v$ for some vector v $\neq$ 0, then v is and eigenvector of A and $\lambda$ is the eigenvalue of A associated with vector.

**Theorem** If v is eigenvector of A with eigenvalue $\lambda$, then v is eigenvector of $A^k$ with eigenvalue $\lambda^k$

Proof: $A^2v = A(Av) = A(\lambda v) = \lambda Av =\lambda^{2}v$

Theorem: If A is invertible, then v is eigenvector of $A^{-1}$ with eigenvalues $\frac{1}{\lambda}$

Proof: $A^{-1}v = A^{-1}\frac{Av}{\lambda} = \frac{1}{\lambda}v$

---


> [!important] Spectral Theorem
>  **Every REAL, Symmetric** n x n matrix has **Real** eigenvalues and n eigenvectors that are **mutually orthogonal** to each other. 
>  * If there is no multiplicity in eigenvalues, the **directions** of the eigenvectors are unique. If there exists multiplicity, choose the eigenvectors that are orthogonal. 
>  * **Orthogonality** is important for the Spectral Theorem
>  * We can use them as a basis for $\mathbb{R}^n$

### Building a matrix with specified eigenvectors

Choose `n` mutually orthogonal **unit** n vectors $v_{1}, \dots, v_{n}$ of a Symmetric Matrix $\mathbf{A}$. Let $V = [v_{1} \,  v_{2} \, \dots v_{n}] \in \mathbf{R}^{n\times n}$  then, $V^TV = VV^T = I$.

This V is called **orthogonal matrix** (in math) **orthonormal matrix**. Orthonormal matrix acts like a rotation / reflection.  Choose some eigenvalues $\lambda_{i}$:

 $$
 \Lambda = \begin{bmatrix}
\lambda_{1} &  \\
 & \ddots  \\
 &  &  \lambda_{n} 
\end{bmatrix}

$$$$Av_{i} = \lambda_{i}v_{i}$$

$$AV = V\Lambda $$

$$A = V\Lambda V^T$$

**Spectral Theorem**: $$A = V\Lambda V^T = \sum_{i}^n\lambda_{i}v_{i}v_{i}^T $$
Note that each $v_{i}v_{i}^T$ is a n x n matrix with rank at most 1. 

This is a matrix factorization called Eigen Decomposition. Every real symmetric matrix will have this decomposition (so called **spectral theorem**)

--- 

**Practice Exam Problem**

$$
\begin{bmatrix}
\frac{1}{\sqrt{ 2 }} & \frac{1}{\sqrt{ 2 }} \\
\frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }}
\end{bmatrix}
\begin{bmatrix}
2 & 0 \\
0 & -\frac{1}{2}
\end{bmatrix}
\begin{bmatrix}
\frac{1}{\sqrt{ 2 }} & \frac{1}{\sqrt{ 2 }} \\
\frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }}
\end{bmatrix}
=
\begin{bmatrix}
\frac{3}{4} & \frac{5}{4} \\
\frac{5}{4} & \frac{3}{4}
\end{bmatrix}


$$


Also Note that $$A^2 = V\Lambda^2V^T$$
Same Eigenvectors, different eigenvalues.

$$A^{-1} = V\Lambda^{-1}V^T$$
Same Eigenvectors, different eigenvalues.

---


> [!important] Theorem: Symmetric Square Root
> Given a Symmetric PSD matrix $\Sigma$, we can find a **symmetric square root** A such that $\Sigma = A^2$. Equivalently, $A = \Sigma^{1/2}$
> $$ 
> A = \Sigma^{1/2} = V\Lambda^{1/2} V^T
> $$


> [!warning] Cholesky Decomposition
> Don't get confused Symmetric Square root with Cholesky Decomposition. Cholesky Decomposition is that Given that $\Sigma$ is PD (**Positive Definite**) (i.e. all eigenvalues are strictly greater than 0), then $\Sigma = LL^T$, where L is a lower triangular matrix.

Given a symmetric **PSD** matrix $\Sigma$, we can find a **Symmetric Square Root** $A = \Sigma^{1/2}$
* Compute eigenvectors / values of $\Sigma$
* Take square root of $\Sigma$ eigenvalues
* Reassemble Matrix A
* $\Sigma^{1/2} = V\Lambda^{\frac{1}{2}}V^T$

---

[[Visualizing Quadratic Form]]



