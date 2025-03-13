---
title: Logistic Regression (1958)
tags:
  - cs189
  - machine-learning
---

> Despite the name “regression,” logistic regression is mainly used for **classification**, not regression.

Logistic Regression Function + Logistic Loss Function + Mean Loss Cost Function

* the inputs y's can be probabilities, can also be 0 or 1 (in most applications).
* usually used for classification
* Fits probabilities in range [0, 1].

In QDA and LDA (generative models) we estimate the underlying data first. In **logistic regression**, we only care about the posterior probability. (No estimation of the data distribution).

We will use the same `X` and `w` as in linear regression. That is we include the fictitious dimension for the bias term $\alpha$.

--- 
**The essence of logistic regression**: Find w that minimizes

$$
\begin{aligned}
J &= \sum_{i=1}^{n} L(s(X_{i}.w), y_{i}) \\
&= - \sum_{i=1}^{n} (y_{i}\ln (s(X_{i}.w)) + (1 - y_{i})\ln(1 - s(X_{i}.w)))
\end{aligned}
$$
The minus term comes from the logistic lost function. We will learn more on this later.

---
### Into the Logistic Loss Function

Before we proceed to finding the minimum of the `cost function`, we should try to understand both the loss function and the cost function. Recall that the `logistic loss` function is defined as:

$$
L(\hat{y}, y) = -[y\ln \hat{y} + (1-y)\ln(1-\hat{y})]
$$
**Important recall**: $\ln(x) = -\infty , \quad x = 0$. It does not make sense for us to have a negative infinity loss in general. This is why earlier we said that the loss function must follow -- $0 < \hat{y} < 1$, and $0 \leq y \leq 1$. This aligns with the logistic function a.k.a `sigmoid` function as the sigmoid function compresses $+\infty, -\infty$ to **approach** +1 and -1 respectively but never +1 or -1. 

<p align="center">
	<img src="Pasted image 20250312173321.png">
</p>
>[!help] Observation
> A good observation here would be to see what the logistic loss is doing intuitively. Evidently, the loss function penalizes harder (larger) if it has a high confident that the label is wrong.

---

### Into the Logistic Regression Function

The logistic regression function is defined as:
$$
f(w, x, \alpha) = s(w.x + \alpha) \quad \text{where} s(\gamma) = \frac{1}{1 + e^{-\gamma}}
$$

Now, let's look at the derivative of the logistic regression function:
$$
	\begin{aligned}
	s'(\gamma) &= \frac{d}{dr} \frac{1}{1 + e^{-\gamma}} \\
	&= \frac{e^{-\gamma}}{(1+e^{-\gamma})^{2}}
	&= s(\gamma)(1-s(\gamma))
	\end{aligned}
$$

<p align="center">
	<img src="Pasted image 20250312174410.png">
	 Sigmoid and Derivative of Sigmoid
</p>

---


Now that we have these information, we can continue our main goal of finding the minimum cost. (i.e $\nabla J_{w} = 0$).

For simplicity of notation, let $s_{i} = s(X_{i}\cdot w)$


$$
\begin{aligned}
J &= - \sum_{i=1}^{n} (y_{i}\ln (s(X_{i}.w)) + (1 - y_{i})\ln(1 - s(X_{i}.w))) \\
J &= - \sum_{i=1}^{n} (y_{i}\ln (s_{i})) + (1 - y_{i})\ln(1 - s_{i})) \\ 
\nabla_{w} J &= -\sum_{i=1}^{n} \left( y_{i} . \frac{1}{s_{i}}. \nabla_{w} s_{i} + (1-y_{i}). \frac{1}{1-s_{i}} . \nabla_{w}(1-s_{i}) \right) \\ 

\nabla_{w} J &= -\sum_{i=1}^{n} \left( y_{i} . \frac{1}{s_{i}}. \nabla_{w} s_{i} - (1-y_{i}). \frac{1}{1-s_{i}} . \nabla_{w}(s_{i}) \right) \\ 

&= -\sum_{i=1}^{n} \left( \frac{y_{i}}{s_{i}} - \frac{1-y_{i}}{1-s_{i}} \right)\nabla_{w}s_{i}


\end{aligned} 
$$

Computation of $\nabla_{w}s_{i} = \nabla_{w} s (X_{i}\cdot w)$

$$
\begin{aligned}
\nabla_{w}s_{i} &= \nabla_{w} s(X_{i}\cdot w) \\
&= s_{i}(1-s_{i}) \cdot X_{i}


\end{aligned}

$$

So, the previous equation becomes: 

$$
\begin{align}
\nabla_{w} J &= -\sum_{i=1}^{n} \left( \frac{y_{i}}{s_{i}} - \frac{1-y_{i}}{1-s_{i}} \right)\nabla_{w}s_{i}
 \\
&= -\sum_{i=1}^{n} \left( \frac{y_{i}}{s_{i}} - \frac{1-y_{i}}{1-s_{i}} \right)s_{i}(1-s_{i})\cdot X_{i}
 \\
&= - \sum_{i=1}^{n} (y_{i} - s_{i})X_{i} \\
\nabla_{w} J&= -X^T(y-s) \quad \text{,  where } \;  s(Xw)= \begin{bmatrix}
s_{1} \\
s_{2} \\
\vdots \\
s_{n}
\end{bmatrix}

\end{align}

$$
The gradient is in `d+1` dimension for both logistic regression and linear regression. A sanity check is that you will be subtracting the gradient from the weight vector, so the two must match the dimensions.

*Some notes: $X_{i}$ is a `d x 1` column vector. $y_{i} - s_{i}$ is a scalar. Transformation to matrix is by sum of linear combinations of $X^T$ columns.*

>[!question]+ Can we simply set the gradient to zero? 
>**Answer** : No. The logistic cost function is non-linear and non-quadratic although it is still convex. Therefore, unlike linear regression, which has a nice quadratic convex bowl, we need to use numerical method like *Gradient descent, Newton's method, etc. to find the global minimum.* 

---
### Using Gradient Descent to find the global minimum.

* The gradient points to the direction of **maximum ascent**. 
* The update rule in gradient descent would be: 
$$
\begin{align}
w&:= w - \epsilon.\nabla_{w}J \\ 
&= w + \epsilon (X^T(y-s))
\end{align}
$$

Generally start w = 0 in practice.

* The update rule in **Stochastic gradient descent**: 
$$
\begin{align}
w := w + \epsilon(y_{i} - s(X_{i}.w)).X_{i}
\end{align}
$$
Stochastic gradient descent works best if we shuffle the data beforehand. In cases with large samples, it might converge before we visit all points.

<p align="center">
	<img src="Pasted image 20250312222243.png">
</p>

#### Final Thoughts:
- Logistic regression (classification) gives you a **probability** that the **output is class 1 (true label 1)** (assuming binary classification).
- Logistic regression **does separate** linearly separable points.
- It achieves **perfect separation** by making the decision boundary **infinitely confident**—which corresponds to w getting infinitely large.
- w **diverges**(goes to infinity), but J(w) **converges** to zero.
