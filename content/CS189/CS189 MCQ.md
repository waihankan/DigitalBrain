training accuracy. vs validation accuracy vs test accuracy!
## Decision Trees.

* The misclassified points cost function often assigns the same weighted average cost to several different splits.
* The entropy cost is strictly concave, so if the children have different distributions of classes (and are nonempty), the information gain is positive.
* The misclassified points cost is not strictly concave, so we encounter more circumstances where the weighted average of the children’s costs is equal to the parent’s cost.
* Decision Trees can guarantee 100% training accuracy on any training set where no two identical points have different labels.
* build them top-down, starting with the entire training set and dividing it at each step.
* Pruning improves validation accuracy but decreases training accuracy.
* **A random forest** decreases the variance (compared to a single decision tree).
* We restrict which features can be used for splitting in each treenode to **decrease the correlation between the trees’ predictions.**
* In a typical tree in the forest, **some training points are not used.** (Because we only allow a subset of features).
* Don't use **Bagging**: (bagging reduces variance, but does not improve bias)
	* You want your model to be interpretable.
	* Your model has high bias and low variance.
* to increase the likelihood that the decision trees in your random forest differ from one another:
	* Use deeper decision Trees: Shorter decision trees have less variance whereas deeper decision trees have **greater variance**.
	* Considering only a subset of the features for splitting at a treenode.
	* Bagging
* Decreasing the number of randomly selected features we consider for splitting at each treenode tends to increase the bias.
* Increasing the number of decision trees tends to decrease the variance.
* **FALSE: Pruning is a technique used to reduce tree depth by removing nodes that don’t reduce entropy enough.** Truth: it's a technique to reduce overfitting. Pruning is used to remove treenodes if removing them improves validation performance. It isn’t based on entropy at all.
* Decision trees with all their leaves pure are prone to overfitting.
* Calculating the best split among quantitative features for a treenode can be implemented so it takes asymptotically the same amount of time as calculating the best split among binary features.
* Choosing a split for n points among d binary features takes O(nd) time, which is the same as the time for d quantitative features (assuming you use radix sort and the algorithm we discussed in class).
* If you increase the maximum depth of the decision tree, which of the following are possible effects?:
	* The number of pure leaves is reduced. False. only increase.
	* The time to classify a test point increases.
	* The **training** accuracy goes down. False. it will go up.
	* The test accuracy goes up. Possible if the original one is underfit.
## Adaboost
* After enough iterations, AdaBoost can **NOT** always obtain 100% training accuracy, regardless of what classifier it uses.
* AdaBoost with decision trees typically uses different criteria/hyperparameters to build the trees than random forests do. (**Higher Bias, shorter trees, compared to Random Forest Trees).
* ---2023
* Sequential
* After a weak learner is trained, the weights associated with the training points it misclassifies are increased.
* : The coefficient βT assigned to weak learner GT is -infinity if the weighted error rate errT of GT is 1. infinity if error is 0.
* **AdaBoost makes progress if it trains a weak learner only to discover that its weighted error rate is substantially greater than 0.5. It doesn't not make progress only if the error is exactly 0.5.** $\beta = \frac{1}{2}\ln\left( \frac{1-err}{err} \right)$
* AdaBoost is faster to train. (assuming no parallel training in random forests).
* AdaBoost is better at reducing bias than a random forest.
* False: AdaBoost is a natural good fit with the 1-nearest neighbor classifier. {Both wants to overfit! high variance}
* AdaBoost with an ensemble of soft-margin linear SVM classifiers allows the metalearner to learn **nonlinear decision boundaries.**
* AdaBoost can give you a rough estimate of the posterior probability that a test point is in the predicted class.
## k-nearest neighbor classification.
* As k increases, the decision boundary tends to look smoother
* Even the slow, Θ(nd)-time exhaustive nearest neighbor algorithm is often used in practice.
* As the number of neighbors grows as k → ∞ and the number of training points grows even faster, so n/k → ∞, the k-nearest neighbor error rate converges to the Bayes risk.
* ---2023
* It is possible to implement the algorithm to use the ℓ1 metric as your distance function.
* **There is a k-nearest neighbor algorithm that classifies a test point in at most O(nd + n log k) time, even if k is much larger than d.
* A 1-NN classifier is more likely to overfit and less likely to underfit than a 10-NN classifier.
* Sometimes finding k approximate nearest neighbors can be done much faster than finding the k exact nearest neighbors.
* In the special case of a two-dimensional feature space, it is possible to preprocess a data set so that nearest neighbor queries can be answered in O(log n) time. **Yes, build a planar point location data structure on a Voronoi diagram.**

## k-means clustering
* k-means is a **unsupervised learning** algorithm.
* **The k-medoids algorithm with the l1 distance is less sensitive to outliers than standard k-means with the Euclidean distance.**
* **k-means is sensitive to initialization and converges to a local minimum which might not be the global minimum.**
* Increasing k can never increase the optimal value of the k-means cost function.


## Neural Network

* The neural network cost function is **not even close to quadratic**.

### Activation Functions
* ReLUs may be preferred over sigmoids (logistic functions) as activation functions for the hidden layers of a neural network because:
	* The forward and backward passes are computationally cheaper with ReLUs than with sigmoids.
	* ReLUs are less vulnerable to the vanishing gradient problem than sigmoids.
	* **ReLU network rarely have convex cost functions.**
* Fixes for vanishing gradient problem with sigmoid function:
	* Make the network shallower (fewer layers).
	* Use ReLU activations instead of sigmoids. Although ReLU gradients can be zero, it is rarely true in practice that most of the components are zero for most of the training points. A substantial number of components of the gradients will be 1, thereby helping gradient descent to make progress.
	* **(The derivative of a sigmoid is between 0 and 0.25).**
	* Using larger initial weights will not necessarily increase the norm of gradients. The gradient of a sigmoid vanishes as the input deviates from zero, so larger initial weights could lead to smaller gradients.




### Backpropagation
* (usually) part of training a neural network’s weights with backpropagation
	* Computing the partial derivatives of a cost function or loss function with respect to each weight.
	* Computing the partial derivatives of a cost function or loss function with respect to each hidden unit value.
	* **We can’t change the training points, so these derivatives w.r.t inputs are not useful.**
	* **Weights do not depend on each other.**




## Principal Components Analysis (PCA)
* Unsupervised (no need labels)
* The first principal component is always orthogonal to the second principal component.
* If the sample covariance matrix has an eigenvalue of zero, the corresponding eigenvector (principal component) is orthogonal to a hyperplane that passes through all the training points.
* We want to find a subspace that minimizes the mean of the squared projection distances.
* We want to find a subspace that maximizes the sample variance of the projected sample points.
* **The principal components are columns of V.**
* When k is much less than d, we can find k principal components faster by computing a partial SVD than we can by computing the eigenvectors of the sample covariance matrix.
* **SVD is faster, it takes O(ndk) time vs. O($nd^2$) to compute $X^TX$ alone in the original PCA algorithm.**
* The diagonal entries "**Squared**"of D are the eigenvalues that correspond to the principal components.
* The principal coordinates for sample point are given by **UD.**
* **the principal coordinates are the inner product of a sample point with each eigenvector.**
* --- 2023 
* PCA can be used to avoid overfitting.
* PCA tends to reduce the variance of your classification algorithm.
* **PCA produces features (principal coordinates) that are linear combinations of the input features.**
* **The principal components are chosen to maximize the variance in the projected data.**
* The principal coordinates are NOT the eigenvalues of the sample covariance matrix.
* PCA is not a clustering algorithm, though PCA can help with clustering algorithms.


## K-NN
* If all the training points are at distinct locations, the 1-nearest neighbor algorithm obtains 100% training accuracy.



## Centroid Method
* If we have only one training point for class A and only one training point (at a different location) for class B, the centroid method classifier obtains 100% training accuracy


## Least Squares
* n ≥ d is a necessary condition for X ⊤X to be invertible.
* n ≥ d is **NOT** a sufficient condition for X ⊤X to be invertible.
* If X is invertible, then there is a solution w∗ such that RSS(w∗ ) = 0.
* If we instead use ridge regression with a positive λ, then ridge regression always has a unique solution regardless of the invertibility of X ⊤X.
* **False**: Least squares regression can be derived from maximum likelihood estimation if we assume that **the sample points have a multivariate normal distribution.** Truth: **least squares regression assumes that the label noise is modeled by a Gaussian, not the sample points.**
* False: **Adding a feature with no predictive value to least squares regression is unwise because it increases the bias of the method.** It is unwise but because it may increase the variance. 
* Weighted least squares regression can be derived from maximum likelihood estimation if we assume that some points’ labels are noisier than others.

## LASSO
* **Does not have a closed form solution unlike Ridge Regression.**
* Lasso becomes least-squares linear regression when λ = 0.
* The isocontours of the ℓ1-regularization term are **cross polytopes**, **l-infinity will be (high-dimensional cubes).**
* The main motivation of Lasso is that it tends to select weights of zero for weakly predictive features, which may reduce overfitting and improve interpretability.
* \


# CNN (Review)

* Pooling layers (of edges) reduce the number of hidden units in the subsequent layer (of units).
* For a convolutional layer, increasing the filter height and width decreases the the number of hidden units in the subsequent layer.
* For a convolutional layer, increasing the number of filters **increases** the the number of hidden units in the subsequent layer.
* Each unit in a convolutional layer is connected to all units in the previous layer. False. Truth is Convolutional layers take advantage of local connections; that is, a unit should be connected to only a small patch of units in the previous layer.
* --- 2023
* Learned convolutional masks can act as edge detectors or line detectors.
* A convolutional layer of connections from a layer of m hidden units to a layer of m 0 hidden units has fewer weights than a fully-connected layer of connections from a layer of m hidden units to a layer of m 0 hidden units.
* 
* **False Let X be a 4 × 4 image that is fed through an average-pooling layer with 2 × 2 masks, producing a 2×2 output layer Y. Then for every unit/pixel Xi j and every unit/pixel Yk` , the partial derivative ∂Yk`/∂Xi j is equal to 1/4.**  --- Although every non-zero entry of the matrix is equal to 1 4 , there are also many zeros within the ∂Y ∂X tensor stemming from the fact that some Y values do not depend at all on some other X values.
* 
* Some research on CNNs made a strong enough impact to win the Alan M. Turing Award.
* 

## Ensemble Learning
* typical benefits of ensemble learning in its basic form (that is, not AdaBoost and not with randomized decision boundaries):
	* **Ensemble learning tends to reduce the variance of your classification algorithm.**
	* **Ensemble learning can be used to avoid overfitting.**
	* **basic ensembling can be used to avoid overfitting but would generally not be used to avoid underfitting. (By contrast, AdaBoost reduces bias.)**
* Bagging tries to reduce the variance / overfitting
* 


## SVD and Eigendecomposition
* The eigendecomposition applies only to square matrices.
* The right singular vectors of a matrix X ∈ R n×d are eigenvectors of$X^TX$.
* Consider a non-square matrix X ∈ R n×d and the vector w ∈ R d \ {0} that maximizes the Rayleigh quotient (w ⊤X ⊤Xw)/(w ⊤w). The singular values of X are no greater than the (positive) square root of the maximum Rayleigh quotient.
* $Q(X, w) = \frac{w^TX^YXw}{w^Tw}$
	* Q(X,w) is maximized when w is the eigenvector of $X^TX$ corresponding to the greatest eigenvalue.
	* Q(X, w) >= 0 for all X and w
	* $X^TX$ is always PSD.
	* $\lVert Xw \rVert^{2} = w^TX^TXw \geq 0$
	* Q(X,w) ≥ 0 for all X and w
	* Q(X,w) ≤ σ 2 max(X) for all X and w, where σmax(X) is the greatest singular value of X.

## K-Means Clustering
* In the algorithm’s output, any two clusters are separated by a linear decision boundary.
* FALSE: k-means is guaranteed to find clusters that minimize its cost function, as the steps updating the cluster assignments yj can be solved optimally, and so can the steps updating the cluster means µ is. k-means is NP hard.
* 

## Miscellaneous
* A Nobel Prize in Physiology was awarded for characterizing action potentials in squid axons.
* A Nobel Prize in Physiology was awarded for discoveries about how neurons in the visual cortex process images.
* A Turing Award was awarded for work on deep neural networks.
* A Godel Prize was awarded for the paper on AdaBoost.
* the output of artificial neuron is analogous to the firing rate, not the voltage.
* The visual cortex has neurons primarily devoted to detecting lines or edges.
* A connection in an artificial neural network is roughly analogous to a synapse in the brain.
* The brain has many functional regions that are specialized to do very different things. (Not generalized).

SVD AND PCA Review








1. Decision trees
2. Neural Networks
3. Biology
4. CNN
5. PCA
6. SVD
7. K-Means Lloyd's algorithm, hierarchical clustering
8. Generalization for neural network
9. 10. Adaboost


10. Batch Normalization
11. KNN
12. 1. Decision trees Runtimes 


**gradient matrix**

Past exams

kd tree video
