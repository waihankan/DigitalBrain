
# 5.2: 2-layer neural network on Iris Dataset

> You must try at least 3 different combinations of these hyperparameters. Report the results of your exploration, including the values of the parameters you explored and which set of parameters gave the best test error. Provide plots showing the loss versus iterations for your best model and report your final test error.

### Attempt 1: (==Best Model==) Learning Rate, Neurons in Hidden layer (0.01, 8)  

* validation accuracy: 1.0 
* training accuracy: 0.984 
* test error: 0.077
* test accuracy: 0.96

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_8-lr0.01_mom0.9_seed0/error.png" width="700"/>
    <figcaption>Error Plot | 8 Neurons in Hidden Layers, Learning rate: 0.01</figcaption>
  </figure>

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_8-lr0.01_mom0.9_seed0/loss.png" width="700"/>
    <figcaption>Loss Plot | 8 Neurons in Hidden Layers, Learning rate: 0.01</figcaption>
  </figure>

---

### Attempt 2: Learning Rate, Neurons in Hidden layer (0.001, 8)

* validation accuracy: 0.9333
* training accuracy: 0.912
* test error: 0.5137
* test accuracy: 0.94

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_8-lr0.001_mom0.9_seed0/error.png" width="700"/>
    <figcaption>Error Plot | 8 Neurons in Hidden Layers, Learning rate: 0.001</figcaption>
  </figure>
<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_8-lr0.001_mom0.9_seed0/loss.png" width="700"/>
    <figcaption>Loss Plot | 8 Neurons in Hidden Layers, Learning rate: 0.001</figcaption>
  </figure>

---

### Attempt 3: Learning Rate, Neurons in Hidden layer (0.01, 10)

* validation accuracy: 1.0
* training accuracy: 0.976
* test error: 0.1349
* test accuracy: 0.96

<figure style="text-align: center;">
    <img src=" file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_10-lr0.01_mom0.9_seed0/error.png " width="700"/>
    <figcaption>Error Plot | 10 Neurons in Hidden Layers, Learning rate: 0.01</figcaption>
  </figure>

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/experiments/feed_forward_2layers_10-lr0.01_mom0.9_seed0/loss.png" width="700"/> 
    <figcaption>Loss Plot | 10 Neurons in Hidden Layers, Learning rate: 0.01</figcaption>
  </figure>

---

# Q6
## 6.1 The Einsum Function

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/notes/images/image-2.png" width="650"/>
    <figcaption>Einsum Functions</figcaption>
  </figure>






## 6.2 Convolutional Layer

1. handwritten
2. code write up
3. optional 

## 6.3 Pooling Layers

1. We need to keep track of which value in the pooling window get voted. For example, if we use max pooling method, the index that corresponds to the maximum value in the pooling window should be tracked. This is to ensure that the gradient at the pooled output is passed back only to that specific input element. The other indices will get the gradient of zero. If we have an average pooling, the gradient from the pool output should be distributed back into each individual input evenly.

2. code write up 


---

# Q7


## 7.1

### 7.1.1 CNN for Fashion MNIST
1. Training an CNN on the fashion MNIST dataset (from code appendix)
### 7.1.2 A plot of the training and validation loss for each epoch of training for at least 8 epochs.
1. <figure style="text-align: center;">
    <img src="Pasted image 20250420131936.png" height="600" />
    <figcaption>Training and Validation Loss</figcaption>
  </figure>
---

### 7.1.3 A plot of the training and validation accuracy for each epoch, achieving a final validation accuracy of at least 84%.

<figure style="text-align: center;">
	<img src="Pasted image 20250420132145.png" height="600" />
    <figcaption>Training and Validation Accuracy</figcaption>
  </figure>

---

## 7.2

### 7.2.1

From Code appendix for training CNN model

### 7.2.2

<figure style="text-align: center;">
	<img src="Pasted image 20250420132932.png" height="600" />
    <figcaption>Kaggle Report</figcaption>
  </figure>

---
### 7.2.3

> Provide at least 1 training curve for your model, depicting loss per epoch or step after training for at least 5 epochs. In addition to validation accuracy after every epoch of training, also include the validation accuracy before training the network in your plot.

<figure style="text-align: center;">
	<img src="Pasted image 20250420140200.png" height="600" />
    <figcaption>Plots for training and validation (Loss and Accuracy)</figcaption>
	<p>The validation plot has an earlier start in the plot since we include the validation loss and accuracy <strong>before</strong> the training starts. Please see the next plot for "<i>without pre-training data</i>".</p>
  </figure>

<figure style="text-align: center;">
	<img src="Pasted image 20250420140124.png" height="600" />
    <figcaption>Plots for training and validation (Loss and Accuracy)</figcaption>
	<p><strong>Without</strong> pre-training validation.</p>
  </figure>


---

### 7.2.4

> Briefly explain your network structure and training regime, and how you think your design choices contributed to its performance. Please also explicitly state the number of trainable parameters in your model.

My model that generated the image in `7.2.3` is based on `Resnet-18`. I used it as a feature extractor by freezing all the pretrained layers and only finetune the last fully connect layer. The final layer from `Resnet-18` is replaced with a `nn.Linear` layer so that it maps to `10 classes`, (what we need for CIFAR-10 dataset). Using a pretrained model helps with convergence and improve efficiency overall. Although the accuracy is not the best with this model, the accuracy per compute cost is relatively high compared to training from scratch.
* This frozen weights model has `5130` Trainable parameters. 

In my Kaggle model, I used `Resnet-50` as base model, and I replaced the original fully connected layer with a custom head consisting of a `Dropout(0.3)`layer (to prevent overfitting since I ran it for 10 epochs) followed by a `nn.Linear` mapping to `10 classes`. Additionally, I added weight decay of `1e-4` to prevent overfitting. The learning rate was scheduled with Cosine Annealing, that starts high, gradually decreases and resets. However, as I set `T_max = 10`, I believe the learning rate never got reset in my case.

* My Kaggle model has `11181642` Trainable parameters. 


---
### 7.2.5
 
 > Initialize a new network with the same structure as your CNN model, and unfreeze all weights. Now train this network on CIFAR-10 again using your training script for the same number of epochs as your transfer learning model, and compare their performance. Does it give better computational efficiency or learn a more effective model?

The performance of the model is better in the sense that it can predict better than feature extraction model. (The validation accuracy increases from `80.43%` to `92.51%`) This is because we now have a much larger weights to train compared to the previous model. As a result, the finetuned model requires more compute cost.


<figure style="text-align: center;">
	<img src="Pasted image 20250420140525.png" height="600" />
    <figcaption>Finetuned Model (Weights Unfrozen)</figcaption>
    <p>The validation plot has an earlier start in the plot since we include the validation loss and accuracy <strong>before</strong> the training starts. Please see the next plot for "<i>without pre-training data</i>".</p>
  </figure>
<figure style="text-align: center;">
	<img src="Pasted image 20250420142652.png" height="600" />
    <figcaption>Finetuned Model (Weights Unfrozen)</figcaption>
    <p><strong>Without</strong> pre-training validation.</p>
  </figure>


