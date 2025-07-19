
surprise = information content

$X = \{x_{1}, x_{2}, \dots, x_{n}\}$

#### Bootstrap

$$
\text{Initial Dataset} = \begin{bmatrix}
1 & 2 \\
1 & 3 \\
1 & 4
\end{bmatrix}
$$

one bootstrap sample:

$$
\text{Bootstrap sample 1 = }\begin{bmatrix}
1 & 4 \\
1 & 4 \\
1 & 2
\end{bmatrix}
$$

Before bootstrap, I can have like one decision tree.
After bootstrap, I can have a lot of independent decision trees.

During prediction: $\hat{y} =\text{mode}(DT^{(1)}(x), \dots , DT^{(\tau)}(x))$

mode = value that appears most frequently in a dataset.


Bootstrapping doesn't increase the BIAS because **the expected value of the bootstrap sample is the same as the original dataset.** But decrease the variance.

After bootstrap, the decision trees won't be correlated (hopefully). i.e. correlation won't be 1.

Variance of ( the average of the decision trees on bootstrap samples) will be lower.

The problem in real life though is that the decision trees on the bootstrap samples are still somewhat correlated.


> There's also something called feature bagging. 

---

### 🌳 Random Forests

> Question: Consider n training points in a feature space of d dimensions. Consider building a random forest with T binary trees, each having exactly h internal nodes. Let m be the number of features randomly selected (from among d input features) at each treenode. For this setting, compute the probability that a certain feature (say, the first feature) is never considered for splitting in any treenode in the forest.

<u>Solution:</u>

Prob of not considering feature i in a single node  = $1 - \frac{m}{d}$

Prob of not considering feature i in the forest = $\left( 1 - \frac{m}{d} \right)^{hT}$

---

**Stump**: Decision Tree with One Split.






