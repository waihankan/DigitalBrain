---
title: Multivariate Gaussian Distribution
tags:
  - statistics
  - cs189
  - machine-learning
---

The multivariate Gaussian Distribution is commonly expressed in terms of $\mu$ and $\Sigma$. The probability density function (PDF) of the multivariate normal (Gaussian) distribution is defined as:

$$
f(x) = \frac{1}{\sqrt{ (2\pi)^d\lvert \Sigma \rvert  }}\exp\left( -\frac{1}{2}(x-u)^T\Sigma^{-1}(x-u) \right)
$$
$$
X \sim \mathbf{N}(\mu, \Sigma)
$$


* $X \text{ and } u \; \text{are vectors in } \mathbb{R}^d$.
* $X\; \text{is a random variable with mean } \mu$.
* $\Sigma \; \text{is the d x d PSD Covariance Matrix}$.
* $\Sigma^{-1} \; \text{is the d x d PSD Precision Matrix}$.
* $\lvert \Sigma \rvert\; \text{is the determinant of } \Sigma$.

---

### Multivariate Normal Log PDF (with prior probability)

$$
Q_{c}(x) = \ln \left( \sqrt{ (2\pi)^d } \cdot f_{X|Y=C}(x).\pi_{Y=c} \right)
$$

#### General logPDF with Prior Probability*
$$
Q_{c}(x) =  -\frac{1}{2} \ln  \lvert \Sigma_{c} \rvert  -\frac{1}{2}(x-\mu_{c})^T\Sigma_{c}^{-1}(x-\mu_{c}) + \ln \pi_{c} 
$$

---

#### Anisotropic QDA logPDF (different covariances for each class)

$$
Q_{c}(x) = -\frac{1}{2}\ln \lvert \Sigma_{c} \rvert  -\frac{1}{2}(x-\mu_{c})^T\Sigma_{c}^{-1}(x-\mu_{c}) + \ln \pi _{c}
$$

---
#### Isotropic QDA logPDF

*Isotropic* $\implies \Sigma_{c} = \sigma_{c}^2I$

The logpdf is simplified to: 

$$
Q_{c}(x) = -\frac{1}{2}\ln(|\sigma_{c}^2I|) - \frac{1}{2}(x - \mu_{c})^T\left( \frac{1}{\sigma_{c}^2}I \right)(x - \mu_{c}) + \ln \pi_{c}
$$
$$
Q_{c}(x) = -\frac{1}{2}(2d)\ln(\sigma_{c}) - \frac{1}{2\sigma_{c}^{2}}(x-u_{c})^TI(x-u_{c}) + \ln \pi_{c}
$$
Reference [[#^ae513b]]
$$
Q_{c}(x) = -\frac{\lVert x-\mu_{c} \rVert ^2}{2\sigma_{c}^2}  -d\ln \sigma_{c}  + \ln \pi_{c}
$$
---

#### Anisotropic LDA logPDF (pool covariance)

Let $\Sigma$ be the pool covariance of all the conditional covariances. (sum of expected covariances)

$$
Q_{c}(x) = -\frac{1}{2}\ln \lvert \Sigma \rvert  -\frac{1}{2}(x-\mu_{c})^T\Sigma^{-1}(x-\mu_{c}) + \ln \pi_{c}
$$
**Decision Boundary** (include prior probability)

$$
Q_{c}(x) - Q_{d}(x) = (\mu_{c} - \mu_{d})^T\Sigma^{-1}x - \frac{\mu_{c}^T\Sigma^{-1}\mu_{c} - \mu_{d}^T\Sigma^{-1}\mu_{d}}{2} + \ln \pi_{c} - \ln \pi_{d}
$$

---

#### Isotropic LDA logPDF

Let $\sigma^2$ be the pool variance of all the conditional variances. (sum of expected variances)

**The Decision Boundary**: (include prior probability)

$$
Q_{c}(x) - Q_{d}(x) = \frac{(\mu_{c} - \mu_{d}).x}{\sigma^2} - \frac{\lVert u_{c} \rVert ^{2} - \lVert \mu_{d} \rVert ^{2}}{2\sigma^{2}} + \ln \pi_{c} - \ln \pi _{d}
$$

---
#### Isotropic LDA with Same Prior logPDF

**The Decision Boundary**: (*Centroid Method*)

$$
Q_{c}(x) - Q_{d}(x) = \frac{(\mu_{c} - \mu_{d}).x}{\sigma^2} - \frac{\lVert u_{c} \rVert ^{2} - \lVert \mu_{d} \rVert ^{2}}{2\sigma^{2}}
$$

---

**Anisotropic** -> Covariance Matrix is not Diagonal.
**Isotropic** -> Covariance Matrix is Diagonal.

[[Visualizing Quadratic Form]]
[[Induced Norm]]

---
