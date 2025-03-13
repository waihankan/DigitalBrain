
Logistic Regression Function + Logistic Loss Function + Mean Loss Cost Function

* he inputs y's can be probabilities, can also be 0 or 1 (in most applications).
* usually used for classification
* Fits probabilities in range [0, 1].

In QDA and LDA (generative models) we estimate the underlying data first. In **logistic regression**, we only care about the posterior probability. (No estimation of the data distribution).

We will use the same `X` and `w` as in linear regression. That is we include the fictitious dimension for the bias term $\alpha$.

Find w that minimizes

$$
\begin{aligned}
J &= \sum_{i=1}^{n} L(s(X_{i}.w), y_{i}) \\
&= - \sum_{i=1}^{n} (y_{i}\ln s(X_{i}.w) + (1 - y_{i})\ln(1 - s(X_{i}.w)))

\end{aligned}
$$
The minus term comes from the logistic lost function. We will learn more on this later.

