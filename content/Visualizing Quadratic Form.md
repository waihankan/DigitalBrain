---
title: Visualizing Quadratic Form
tags:
  - cs189
  - machine-learning
---
[quadratic form](https://math.libretexts.org/Bookshelves/Linear_Algebra/Understanding_Linear_Algebra_(Austin)/07%3A_The_Spectral_Theorem_and_singular_value_decompositions/7.02%3A_Quadratic_forms)

* The **quadratic form of M** (M is symmetric) is $x^TM x$

A symmetric matrix M 
* is positive definite if $w^TMw > 0$ for all w $\neq$ 0 $\iff$ all eigenvalues are **positive**.
* A symmetric matrix M is positive semi-definite if $w^TMw \geq 0$ for all w $\neq$ 0 $\iff$ all eigenvalues are **non-negative**. (Not invertible if eigenvalue 0)
* Indefinite: Saddle -> if positive eigenvalues and negative eigenvalues.
* Invertible if there is no 0 eigenvalue.
---

### Transformation by the matrix in quadratic form

<p align=center>
<img src="Pasted image 20250301143108.png">
</p>

*Goal: Want the matrix such that it transforms the left graph to the right graph.*

$$
q_{2}(Az) = q_{1}(z)
$$
Thus, $x = Az \;\text{and}\; z = A^{-1}x$

$q_{2}(x) = q_{1}(z)= q_{1}(A^{-1}x) = \lVert A^{-1}x \rVert^{2} =x^TA^{-1}A^{-1}x = x^TA^{-2}x$

Now that we have this equation, $q_{2}(x) = x^TA^{-2}x$, we can plot iso-contours of $q_{2}(x)$

> [!important] Isocontours of a Quadratic Form
> Given a symmetric PSD $\mathbf{M}$ and $q(x) = x^TMx$, then the isocontours of $q(x)$ are
> $$
> x^TMx = c, \; \text{for} \; c \in R
> $$
> * The **axes** of the ellipsoid are given by the **eigenvectors of M**
> * The radii are determined by the square roots of **the eigenvalues of $M^{-1/2}$.** This is the same as radii = $\frac{1}{\sqrt{\lambda(M) }}$ because $\lambda(A^{-1/2}) = \frac{1}{\sqrt{\lambda(A)}
>   }$
> * For Identity Matrix, isocontours are simply **spheres**.
> * For Diagonal Matrix, isocontours are axis-aligned (same as coordinate axes).
> * For General PSD, the isocontours are ellipses / ellipsoids.

In the case of $x^TA^{-2}x$,  radii of ellipsoid = $\frac{1}{\sqrt{ \lambda(A^{-2}) }} = \lambda(A)$. The pattern here is therefore:

$M \to M^{-1/2}$
$M^{-2} \to M$

> (multiply the exponent with $-\frac{1}{2}$) for eigenvalues (radii of ellipsoids)
---

{TODO: Discussion 5}


### Recap on Symmetric Matrix and relate to the Quadratic form plot.

A symmetric matrix M 

* positive definite $\iff$$w^TMw > 0$ for all $w \neq 0$ $\iff$ $\lambda's > 0$.
* positive semi-definite $\iff$ $w^TMw \geq 0$ for all $w\iff \lambda's \geq 0$ .
* indefinite $\iff$ have both +ve and -ve eigenvalues.
* invertible $\iff$ no 0 eigenvalue.

<p align="center">
	<img src="Pasted image 20250301151936.png">
</p>
> [!cite] Shewchuk
> If M is only positive semidefinite, but not positive definite, the isosurfaces are cylinders instead of ellipsoids. These cylinders have ellipsoidal cross sections spanning the directions with nonzero eigenvalues, but they run in straight lines along the directions with zero eigenvalues.

---
{TODO:  }
> Every square matrix **has to be** Positive Semidefinite, including $A^{-2}$. If $A^{-2}$ exists, it is **Positive Definite**.

---

Isotropic = Variance is the same in all directions.

--- 
### Anisotropic Gaussians

$X \sim \mathbf{N}(\mu, \Sigma)$

PDF is 
$$\frac{1}{\sqrt{ (2\pi)^2\lvert \Sigma \rvert  }}\exp\left( -\frac{1}{2} (x-\mu)^T \Sigma^{-1}(x - \mu) \right)$$

$\lvert \Sigma \rvert \text{determinant}$

$\Sigma$ is the d x d PSD **Covariance Matrix**
$\Sigma^{-1}$ is the d x d PSD **Precision Matrix**