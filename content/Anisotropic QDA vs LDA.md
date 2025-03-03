---
title: Anisotropic QDA vs LDA
tags:
  - cs189
  - machine-learning
---
Agenda:
1. Anisotropic QDA
2. Anisotropic LDA
3. Isotropic QDA (already learned) (spherical) $\sigma_{c}^2I$
4. Isotropic LDA (spherical) $\sigma^2I$


## Maximum Likelihood Estimation for Anisotropic Gaussians
Given training points $X_{1}, \dots X_{n}$ and classes $y_{1}\dots y_{n}$, we need to find the best-fit Gaussians.

Let $n_{c}$ be the number of training points in class C.

#### Quadratic Discriminant Analysis (QDA)

1. **Covariance Matrix**: The best estimate covariance matrix (conditional covariance of points in class C) , $\hat{\Sigma_{c}}$ is:
	$$
	\hat{\Sigma}_{c} = \frac{1}{n_{c}}\sum_{i: y_{i} \in C}(X_{i} - \hat{\mu}_{c})(X_{i} - \hat{\mu}_{c})^T
	$$
2. **Discriminant Function:**
	$$
	Q_{c}(x) = -\frac{1}{2}(x - \mu_{c})^T\Sigma^{-1}(x-\mu_{c})-\frac{1}{2}\ln \lvert \Sigma_{c} \rvert + \ln \pi_{c}
	$$
3. **Decision Boundary:**
		$$
		Q_{ C}(x) - Q_{D}(x) = 
		0
		$$
4. **Posterior Probability (for two class):**
		$$
		P(Y = C | X = x) = s(Q_{
		C}(x) - Q_{D}(x)) = \frac{1}{1-e^{Q_{D}(x) - Q_{C}(x)}}
		$$
5. **Visualization**:


<figure>
<p align="center">
	<img src="Pasted image 20250302194504.png">
</p>
<figcaption align="center"><b>Two-Class QDA</b></figcaption>
</figure>

<figure>
<p align=center>
	<img src="Pasted image 20250302194555.png">
</p>
<figcaption align="center"><b>Multi-Class QDA</b></figcaption>
</figure>


---

#### Linearly Discriminant Analysis (LDA)

1. **Covariance Matrix:** For LDA, we want *pool-covariance within-class matrix* $\hat{\Sigma}$. That means: 

$$
\hat{\Sigma} = \frac{1}{n}\sum_{c}\sum_{i:y_{i} = c} (X_{i} - \hat{\mu}_{c})(X_{i} - \hat{\mu}_{c})^T
$$
2. **Discriminant Function**:
		$$
		Q_{c}(x) = \mu_{c}^T\Sigma^{-1}x - \frac{\mu_{c}^T\Sigma^{-1}\mu_{c}}{2} + \ln \pi_{c}
		$$
		
3. **Decision Boundary** 
		$$
		Q_{ C}(x) - Q_{D}(x) = 
		0
		$$
	$$
	Q_{c}(x) - Q_{D}(x) = \underbrace{ (\mu_{c} - \mu_{D})^T\Sigma^{-1}x }_{ w^Tx = w.x } - \underbrace{ \frac{\mu_{c}\Sigma^{-1}\mu_{c} - \mu_{D}^T\Sigma^{-1}\mu_{D}}{2} + \ln \pi_{C} - \ln \pi_{D} }_{ \alpha }
	$$
4. **Posterior Probability (for two class)**:
		$$
		P(Y = C | X = x) = s(w^Tx + \alpha)=s(Q_{
		C}(x) - Q_{D}(x)) = \frac{1}{1-e^{Q_{D}(x) - Q_{C}(x)}}
		$$
5. **Visualization**:

<figure>
<p align="center">
<img src="Pasted image 20250302194807.png">
</p>
<figcaption align ="center" > <b>Two-Class LDA</b></figcaption>
</figure>

<figure>
	<p align="center">
		<img src="Pasted image 20250302194918.png">
	</p>
	<figcaption align ="center" > <b>Multi-Class Real World Data LDA</b></figcaption>
</figure>

---
