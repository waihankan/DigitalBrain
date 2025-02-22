---
title: Building Classifiers
tags:
  - machine-learning
  - cs189
---
### Preface

I will start this note with not much understanding of the models, but by the end of the note, I will revise the different models again, hopefully - with a better understanding of the concept.

### 3 Ways to Build Classifiers 

1. Generative Models (Linear Discriminant Analysis)
> Core Idea: Try to learn the **underlying probability distributions** that generate the data for **each class separately**. Similar to figuring out how "each class" creates its data points.

2. Discriminative Models (Logistic Regression)
> This model **Directly Learn the Decision Boundary** between classes or the conditional probability $P(Y|X)$ without explicitly modeling the individual class distributions. "The Focus is on Finding WHAT separates the classes from each other ."

3. Find Decision Boundary (Support Vector Machine)
> No explicit calculations of **Probabilities**. Directly find the **Optimal Decision Boundary** that separates the classes.

<small style="color:red"> ⭕ Come back to recognize the advantages and disadvantages of different models.</small>

---

### Gaussian Discriminant Analysis 

Caution: Although the name is called Gaussian "Discriminant" Analysis, please note that this model is a **generative model**. The key factor is **"How it Learns"**. The overall technique is:

* Assume Gaussian Distributions for each class.
* Estimate the parameters (mean and covariance) of these distributions. (Maximum Likelihood Estimation)
* Use Bayes' theorem to get $P(Y | X)$, the probability of the class given features.

> First modeling the probability distribution of **each class separately.** And learns $P(X | Y)$ **The probability of observing features given a class** (Hallmark of a generative model). Model $P(X|Y) \rightarrow P(Y|X)$.


#### Fundamental Assumption: Each class has a Normal Gaussian Distribution.

$$ 
X \sim \mathbb{N}(u, \sigma^2)
$$

$$
f(x) = \frac{1}{(\sqrt{ 2\pi} \sigma)^d} \cdot \exp\left(-\frac{\lVert  x - \mu\rVert^2}{2\sigma^2}\right)
$$

$\mu, \sigma \; \text{and} \; \text{x are scalars and d = dimension}$ ^f1b0fd

>[!question] How did we get here?
> How did we get this [[Multivariate Gaussian Distribution]] with a scalar $\sigma^{2}$ instead of a covariance matrix $\Sigma$?

For each class C, **SUPPOSE** we know $\mu_C$ and variance $\sigma_{C}^2$ which gives us the PDF $f_{X | Y = C}(x)$ and we know prior probability $\pi_{C} = P(Y = C)$.

<p align = "center"> 
	<img src = "Pasted image 20250217214315.png" width="100%">
</p>

🚨 In this example, we are assuming that the **variance is a scalar**, which results in *circular iso-contours* and not ellipses. This is called **isotropic normal distribution** because the variance is the same in all directions. Anisotropic Gaussians = isosurfaces are ellipsoids. Also, the **Bayes Decision boundary is an ellipse**.

From [[Decision Theory]], we know that the optimal classifier $r^*(x)$ should pick the particular class `C` such that it **maximizes the expected loss** : $f_{(X|Y = C)}(x).\pi_{Y = C}$

Since we're considering `0-1 Loss Function`, we can also recall the main principle : **Pick the class with the highest Posterior Probability**

For the sake of simplifying the Math - Maximizing $Q_{c}(x) = \ln\left((\sqrt{ 2\pi })^df_{(X|Y = C)}(x).\pi_{Y = C}\right)$ is the same as maximizing $f_{(X|Y = C)}(x).\pi_{Y = C}$. The term $(\sqrt{ 2\pi })^d$ is just to cancel out the term in the Gaussian Distribution [[#^f1b0fd]].  ^25d885

Therefore, 

$$
Q_{c}(x) =  -\frac{ \left\lVert  x - \mu_{C}  \right\rVert^2}{2\sigma_{C}^2} - d\ln \sigma + \ln \pi_{C}
$$

🔺 Notice that $Q_{c}(x)$ is a quadratic function.

In a 2-class problem, we can also add asymmetric loss function by adding $ln\mathbb{L}(\text{not C}, C)$. In a multi-class though, it will be more harder to account for the loss function.

---

### Quadratic Discriminant Analysis (QDA)

Suppose we only have two classes `C` and `D`. Then, by Bayes Theorem: 

$$
r^*(x) = \begin{cases} C\:, 
& \text{if }  Q_{C}(x) - Q_{D}(x) \; \gt 0 \\
D, & \text{otherwise }
\end{cases}
$$
`Bayes Deicions Boundary is where x satisfies`  $Q_{C}(x) -Q_{D}(x) = 0$

$Q_{C}(x) -Q_{D}(x) = 0$ is a **quadratic function** and therefore, in 1-dimension, the **BDB** may have 1 or 2 points. In d-dimension, the **BDB** is a quadric

🔴 **Q: What about the probability that our prediction is correct?** This is basically the same as $P(Y = C| X)$

$$
P(Y=C|X) = \frac{f_{X|Y = C}.\pi_{C}}{f_{X}}
$$

By the law of total probability:
$$
P(Y=C|X) = \frac{f_{X|Y = C}.\pi_{C}}{f_{X|Y=C}.\pi_{{C}} + f_{X|Y=D}.\pi_{D}}
$$

Substitute equation [[#^25d885]],

$$
P(Y=C|X) = \frac{e^{Q_{C}(x)}}{e^{Q_{D}(x)} \, + \, e^{Q_{D}(x)}}
$$

$$
P(Y=C|X) = \frac{1}{1 + \frac {e^{Q_{D}(x)}} {e^{Q_{C}(x)}}}
$$

$$
P(Y=C|X) = \frac{1}{1 + e^{Q_{D}(x) - Q_{C}(x)}}
$$
>[!info] Definition
>**Logistic Function** / **Sigmoid Function** (Real-valued input $\rightarrow$ Probability) is in the form of : 
> $$
> s(\gamma) = \frac{1}{1 + e^{-r}}
> $$

Putting our probability equation into sigmoid function (monotonically increasing) form:

$$
s(Q_{C}(x) - Q_{D}(x)) = \frac{1}{1 + e^{Q_{D}(x) - Q_{C}(x)}}
$$


🔴 Answer: **Now, we can calculate our correctness of the prediction using the sigmoid function.**


$\rightarrow$ Recall the decision function is $Q_{C}(x) -Q_{D}(x)$. The output of the sigmoid function can be interpreted as the probability of the data point belonging to the correct class.

<p align="center">
	<img src="Pasted image 20250217233839.png">
</p>

**Multi-Class Quadratic Discriminant Analysis (QDA)** is quite natural. "multiple decision boundaries that adjoin each other at joints." The way we do this is by calculating $Q(x)$ of each class and choose the maximum.


<figure>
	<p align="center">
		<img src="Pasted image 20250221122321.png">
	</p>
	<figcaption align="center"> <b>Multi-Class QDA</b> partitioning the <b>Feature Space</b> into Regions. </figcaption>
</figure>

> [!attention] Notice the variance and the boundary
> * The dots are the means of each class, and the variances are the circular shapes in the graph. The circular shapes are not spread out equally across different classes, which means that they have **different variances**.
> * Also notice that the decision boundary are **not linear** since our $Q(x)$ are quadratic functions. <font style="color: red"> come back for updates! </font> [Ed Thread](https://edstem.org/us/courses/73019/discussion/6226516)


---

### Linear Discriminant Analysis (LDA)

Now that we know **QDA**, let's explore what happens IF **all** the Gaussians have the same variance $\sigma^{2}$. 

QDA allows **each class** to have its own covariance matrix $\Sigma_{k}$ where `k` is a class. LDA is a variant of QDA with **Linear Decision Boundaries**, where all classes have **same covariance matrix** $\Sigma_{k} = \Sigma, \; \forall k$.

LDA is also less likely to overfit since we assume the same covariance matrix (variance) for all class, reducing the number of parameters to estimate.


Recall that we defined Q(x) to be a natural log of the Gaussian PDF: [[#^25d885]]

$$
Q_{C}(x) = - \frac{\lVert x - \mu_{C} \rVert ^2}{2\sigma_{C}^2} - d\ln \sigma_{C} + \ln \pi_{C}
$$

But, we had made an important assumption in LDA that all $\sigma$ are the same. Therefore, the decision boundary $Q_{C}(x) - Q_{D}(x)$ is simplified to: 

$$
Q_{C}(x) - Q_{D}(x) =\underbrace{  \frac{(\mu_{C} - \mu_{D}) \cdot x}{\sigma^2} }_{ w \cdot x } - \underbrace{ \frac{\lVert \mu_{c} \rVert ^{2} - \lVert \mu_{D} \rVert ^{2}}{2\sigma^{2}} + \ln \pi_{C} - \ln \pi_{D} }_{ +\alpha }
$$

Now, the decision boundary equation is evidently in the familiar linear form of $w.x + \alpha$ as the **quadratic terms in $Q_{C}$ and $Q_{D}$ cancel out each other.**

Similar to what we did in **QDA**, we can find the correctness probability of our prediction (a.k.a) the posterior probability in the case of `0-1 Loss Function`. 

$$
P(Y = C | X = x) = \frac{1}{1 + e^{Q_{D}(x) - Q_{C}(x)}}
$$

$$
P(Y=C|X=x) = \mathbf{s}(Q_{C}(x) - Q_{D}(x))
$$

**To emphasize the linearity, we can rewrite this as:**

$$
P(Y=C|X=x) = \mathbf{s}(w.x + \alpha)
$$

<figure>
<p align="center"> 
	<img src = "Pasted image 20250221130550.png" >
</p>
<figcaption align="center"> Two Gaussians (red) and the logistic function (black)</figcaption>
</figure>

*If $Q_{C}(x)$ is the right Gaussian, the logistic (sigmoid) function is the right Gaussian divided by the sum of two Gaussians.* 🔺 Another observation is that the logistic function look 1D even though the Gaussians are 2D. In Higher Dimensions, the logistic function is essentially varying in only **one direction** and unchanging in all other directions.

A Side Note: In Logistic Regression, we are assuming that the posterior probability is in the "sigmoid form" and don't care about the underlying class conditional PDFs.

---

### Centroid Method -> Special Case of GDA: Same Prior Probability and Same Variance for all classes.

Class C and Class D have same variance as well as the same Prior Probability $\pi_{C} = \pi_{D} = \frac{1}{2}$

Then, the **Bayes decision boundary** is :

$$(
\mu_{C} - \mu_{D}).x - (\mu_{C}-\mu_{D}).\left( \frac{\mu_{c} + \mu_{D}}{2} \right) = 0
$$
This equation is the same as the "centroid method".

---

**Multi-Class LDA**: choose C that maximizes the **Linear Discriminant Function** 

$$
\frac{\mu_{C}\cdot x}{\sigma^{2}} - \frac{\lVert \mu_{C} \rVert^{2} }{2\sigma^{2}} + \ln \pi_{C}
$$

When we have Classes with **Same Variance and Same Prior Probabilities**, the decision boundary diagram becomes a **Voronoi Diagram**. 

If we only have **Same Variance but different Prior Probabilities**, the decision boundary diagram becomes a Power Diagram. ^824a75

<figure>
<p align="center">
	<img src="Pasted image 20250221230112.png">
</p>
<figcaption align =  "center"> A True <b>Voronoi Diagram</b> (Same Priors Probabilities)</figcaption>
</figure>

---

<small>
<b>Voronoi Diagram
:</b> A Voronoi diagram divides a space into regions where each region corresponds to a "generator" point. Every location within a region is closer to its corresponding generator than to any other generator.
</small>

<small>
<b>Power Diagram:</b> A power diagram, also known as a weighted Voronoi diagram, generalizes the concept of a Voronoi diagram by assigning a weight to each generator point. The distance metric is modified to account for these weights, leading to regions defined by the "power distance" rather than the standard Euclidean distance.

</small>

[[Maximum Likelihood Estimation]]
