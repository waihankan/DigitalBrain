---
title: Decorrelating the Design Matrix
tags:
  - cs189
  - machine-learning
---
>[!abstract] 
> We will go through "centering", "decorrelating", and "sphering", which are the preprocessing steps for machine learning or statistical analysis.

#### Understanding the Design Matrix X
* Suppose X is a $n\times d$ design matrix. It contains n data points (samples) with d dimensions (features).
* Each row represents a single sample (point in d-dimensional space) $x_{i}^T$.

#### Centering the Matrix X
* Centering is basically the same as subtracting the mean from all data points. In other words, we subtract the **mean of all rows from each row**.
* The new matrix $\dot{X}$ has approximately zero mean.
	$$
	\dot{X} = X - \mu^T
	$$
>[!note]+ Why do we do this?
>* Many statistical methods assume that the data is centered around zero. In PCA, for example, it's important to center the data before **computing the eigenvalues**.
>* It also simplifies the estimation of covariance matrix.
>* [ - ] It prevents Bias in machine learning models that might otherwise be influenced by the scale of the data.

#### Sample Covariance Matrix
* Measures how different features in the data vary together.
* **With a centered Matrix $\dot{X}$**, the sample covariance matrix is defined as:
 $$
 Var(R) = \frac{1}{n}\dot{X}^T\dot{X}
$$

#### Decorrelating $\dot{X}$
* *When we **decorrelate** a dataset, we are transforming it into a new coordinate system where the features become **uncorrelated**. This is useful because many machine learning algorithms work better when features are independent.*
* We want to decorrelate to make the covariance matrix diagonal. To remove correlation, we apply an eigenvector transformation:
$$
Z = \dot{X}V
$$
where, V is the eigenvectors of Var(R), that is $Var(R) = V \Lambda V^T$. The diagonal values of $\Lambda$ represent the variance along the eigenvector axes.
Thus, Variance of Z will be:

$$
Var(Z) = \frac{1}{n}Z^TZ
$$
$$
=\frac{1}{n}(V^T\dot{X}^T\dot{X}V)
$$
$$
= V^TVar(R)V
$$

$$
= V^TV\Lambda V^TV
$$

$$
Var(Z) = \Lambda
$$
dd
#### Sphering / Whitening 
* Make all features have unit variance.
* We get this by applying:
  $$
  W = \dot{X}Var(R)^{-1/2}
  $$

$$
Var(W) = I
$$
>[!note]+ Why do we do this?
>* In algorithms like SVM and neural networks, some features might have much larger values than others.
>* These algorithms could give larger importance to larger valued features.
>* Thus, by sphering / whitening, we normalize all the features, ensuring that all the features contribute equally.

