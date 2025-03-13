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

1. **Covariance Matrix:** For LDA, we want *pool-covariance within-class matrix* $\hat{\Sigma}$. **Note: This is a weighted sum with prior probabilities. **That means: 

$$
\hat{\Sigma} = \frac{1}{n}\sum_{c}\sum_{i:y_{i} = c} (X_{i} - \hat{\mu}_{c})(X_{i} - \hat{\mu}_{c})^T
$$

$$
\hat{\Sigma} = \sum_{c}\frac{n_{c}}{n} . \frac{1}{n_{c}} \sum_{i:y_{i} = c} (X_{i} - \hat{\mu}_{c})(X_{i} - \hat{\mu}_{c})^T
$$

Note that $\frac{n_{c}}{n}$ is the same as `prior probability of class C`. And the rest of the terms is covariance of `class C`.

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
		C}(x) - Q_{D}(x)) = \frac{1}{1+ e^{Q_{D}(x) - Q_{C}(x)}}
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

## GDA vs LDA 

For Two-classes Classification:

1. LDA has `d+1` parameters (w, $\alpha$).
2. QDA has $\frac{d(d+3)}{2} + 1$ parameters.
3. LDA is more likely to *underfit* (small number of parameters).
4. QDA is more likely to overfit (the danger is much bigger wiith larger dimensions d) as the parameters grow with squared of the dimensions.



Cautions: 
*  that QDA or LDA on data doesn't find the True Baye's Classifier. In fact, it is not possible to get a true Baye's Classifier since we do not know the true parameters / distribution of real world data. Most of the time, if not all the time, we use the estimated distributions from *finite data*. Moreover, real-world data might not fit the Gaussian perfectly.
* Changing Prior Probabilities or Loss Functions is the same as adding constants (actually `ln(constants)`) to our discriminant functions. Thus, in a two-class classifiers, changing these values would be the same as simply changing isovalue.
* Posterior Probability gives us some sort of confidence levels.
* Decision boundaries are drawn at different probability thresholds (e.g 10%, 50%, 90%) to indicate how confident we need to be before making a classification decision. 50% for 0-1 loss functions.
* Setting the decision boundary at probability `p` is the same as setting asymmetric loss values for false positives and false negatives. (Similarly, the prior probability follows the same rule).
* LDA can result in non-linear decision boundaries if we introduce new features or transformations.


> [!cite]+
> _LDA & QDA are the best method in practice for many applications. In the STATLOG project, either LDA_ _or QDA were among the top three classifiers for 10 out of 22 datasets. But it’s not because all those datasets_ _are Gaussian. LDA & QDA work well when the data can only support simple decision boundaries such as_ _linear or quadratic, because Gaussian models provide stable estimates._

[[Decorrelating the Design Matrix]]