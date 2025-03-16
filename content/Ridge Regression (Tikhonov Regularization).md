---
title: Ridge Regression (Tikhonov Regularization)
tags:
  - cs189
  - machine-learning
---
>[!danger] Optimization Problem
> Find `w` that minimizes:
>$$
> \lVert Xw - y \rVert ^{2} + \lambda \lVert w' \rVert ^{2} = J(w)
>$$
>where $w'$ is the weight without the bias term or bias term 0.

* $l_{2}$ penalization promotes shrinkage of the weights. i.e. it will encourage our solutions to have smaller weights.
* Also makes the least squares problems to always have a unique minimum solution because of the $\lambda$ term -- making the matrix to be **Positive Definite** instead of just Positive Semidefinite.

<p align="center">
	<img src="Pasted image 20250316125130.png">
</p>

Left figure: ill-pose, many minima.
Right figure: well-pose, one unique minima.

$l_{2}$ regularization reduces **overfitting** by reducing the variance while increasing the bias. In general, we don't like big weights if the data and label are relatively smaller. 

<p align="center">
	<img src="Pasted image 20250316125736.png">
</p>

==The Ridge Regression solutions lie at the tangent between the red isocontour and the blue isocontour==.  Notice how higher $\lambda$ would pull the solution towards the $\lVert w \rVert^{2}_{2}$ minimum. 

---

If we were to solve the optimization problem:

$$
argmin_{w} \lVert Xw -y \rVert ^{2}_{2} + \lambda \lVert w' \rVert ^{2}_{2}
$$

The optimal solution would be: 

$$
(X^TX + \lambda I')w^* = X^Ty
$$

==Note== that $X^TX + \lambda I'$ is always **Positive Definite** and thus invertible - leading to a unique solution. $I'$ is an identity matrix with the bottom right element being 0 (due to the fact that we don't penalize the bias term). 

---

### Connection of Ridge Regression and Variance 

Assume that the true data model is defined as $y = Xv + e$, where e is noise from a Normal Distribution. Then, the variance of the Ridge Regression at a test (arbitrary) point is:
$$
Var(z^T(X^TX + \lambda I')^{-1}X^Te)
$$
As $\lambda$ goes to $\infty$, variance approaches 0, but the bias increases. In practice, we need to tune the $\lambda$ by cross-validation.

> Important Note: Features should be normalized so that the weights get penalized in the same amount, and the features will have same variance. 

> An Alternative way to achieve is this to use a different diagonal matrix instead of $\lambda I'$, which will weight dissimilar amount to different features.


<p align="center">
	<img src="Pasted image 20250316131748.png">
	<p align="center"> Bias Squared vs Variance as lambda increases. <p>
</p>

>[!warning] 
> $$
> \text{Variance}\propto \frac{1}{\text{\# of sample points}} \propto  \frac{1}{\lambda}
> $$

---


