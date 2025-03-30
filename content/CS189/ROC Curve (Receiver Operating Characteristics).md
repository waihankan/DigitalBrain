---
title: ROC Curve (Receiver Operating Characteristics)
tags:
  - cs189
  - machine-learning
---

<p align="center">
	<img src="Pasted image 20250315213839.png">
</p>

> [!error]  Key Facts:
> * Nothing to do with training. It is in fact a way of evaluating the trained class
> * Made by running the trained classifier on validation or test set.
> * Horizontal Axis = False Positive Rate.
> * Vertical Axis = True Positive Rate (Sensitivity).
> * ROC Curve is discrete.
> * The area under the curve determines 
> * Random Classifier will have an area of 0.5
> * The Knob is not in the diagram*


Important: In practice, the trade-off between false negatives and false positives is usually negotiated by choosing a point on this plot, based on real test data.




