---
draft:
---

# Q1: Honor Code
![[Pasted image 20250420192038.png]]
* I worked on this homework alone. The following are the online resources used to understand some of the mathematics. 
* [The Complete Mathematics of Neural Networks and Deep Learning](https://www.youtube.com/watch?v=Ixl3nykKG9M&t=13472s) — This video is very helpful for understanding the math behind backpropagation. Note that it explains backpropagation for a single sample. Since we use batch samples in each iteration, we need to represent them as matrices to ensure the math works out correctly.
* [CNN Explainer](https://poloclub.github.io/cnn-explainer/) -- Interactive resource for understanding CNN layers.
* [Transfer Learning for Computer Vision Tutorial](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html) -- Pytorch Documentation

# Q4: Basic Network Layers

## 4.1: ReLU

### 4.1.1

![[Pasted image 20250420191555.png]]

### 4.1.2 Implementation of `activations.ReLU`

```python
class ReLU(Activation):
    def __init__(self):
        super().__init__()

    def forward(self, Z: np.ndarray) -> np.ndarray:
        """Forward pass for relu activation:
        f(z) = z if z >= 0
               0 otherwise
        
        Parameters
        ----------
        Z  input pre-activations (any shape)

        Returns
        -------
        f(z) as described above applied elementwise to `Z`
        """
        zeros = np.zeros_like(Z)
        return np.maximum(zeros, Z)

    def backward(self, Z: np.ndarray, dY: np.ndarray) -> np.ndarray:
        """Backward pass for relu activation.
        
        Parameters
        ----------
        Z   input to `forward` method
        dY  gradient of loss w.r.t. the output of this layer
            same shape as `Z`

        Returns
        -------
        gradient of loss w.r.t. input of this layer
        """
        return dY * (Z >= 0).astype(np.float32)
```

---


## 4.2: Fully-Connected Layer

### 4.2.1

![[Pasted image 20250420191622.png]]


### 4.2.2 Implementation of `layers.FullyConnected`:

```python
class FullyConnected(Layer):
    """A fully-connected layer multiplies its input by a weight matrix, adds
    a bias, and then applies an activation function.
    """

    def __init__(
        self, n_out: int, activation: str, weight_init="xavier_uniform"
    ) -> None:

        super().__init__()
        self.n_in = None
        self.n_out = n_out
        self.activation = initialize_activation(activation)

        # instantiate the weight initializer
        self.init_weights = initialize_weights(weight_init, activation=activation)

    def _init_parameters(self, X_shape: Tuple[int, int]) -> None:
        """Initialize all layer parameters (weights, biases)."""
        self.n_in = X_shape[1]

        ### BEGIN YOUR CODE ###

        W = self.init_weights((self.n_in, self.n_out))
        b = np.zeros((1, self.n_out))

        self.parameters = OrderedDict({"W": W, "b": b}) # DO NOT CHANGE THE KEYS
        self.cache: OrderedDict = OrderedDict()  # cache for backprop
        self.gradients: OrderedDict = OrderedDict({"W": np.zeros_like(W),
                                                   "b": np.zeros((1, self.n_out))})
        

    def forward(self, X: np.ndarray) -> np.ndarray:
        """Forward pass: multiply by a weight matrix, add a bias, apply activation.
        Also, store all necessary intermediate results in the `cache` dictionary
        to be able to compute the backward pass.

        Parameters
        ----------
        X  input matrix of shape (batch_size, input_dim)

        Returns
        -------
        a matrix of shape (batch_size, output_dim)
        """
        # initialize layer parameters if they have not been initialized
        if self.n_in is None:
            self._init_parameters(X.shape)

        W = self.parameters["W"]
        b = self.parameters["b"]

        Z = X @ W + b
        out = self.activation.forward(Z)

        # store information necessary for backprop in `self.cache`
        self.cache["X"] = X
        self.cache["W"] = W
        self.cache["Z"] = Z

        return out

    def backward(self, dLdY: np.ndarray) -> np.ndarray:
        """Backward pass for fully connected layer.
        Compute the gradients of the loss with respect to:
            1. the weights of this layer (mutate the `gradients` dictionary)
            2. the bias of this layer (mutate the `gradients` dictionary)
            3. the input of this layer (return this)

        Parameters
        ----------
        dLdY  gradient of the loss with respect to the output of this layer
              shape (batch_size, output_dim)

        Returns
        -------
        gradient of the loss with respect to the input of this layer
        shape (batch_size, input_dim)
        """
        # unpack the cache
        X = self.cache["X"]
        W = self.cache["W"]
        Z = self.cache["Z"]

        dldZ = self.activation.backward(Z, dLdY)
        
        # compute the gradients of the loss w.r.t. all parameters as well as the
        # input of the layer

        dW = X.T @ dldZ
        db = np.sum(dldZ, axis=0, keepdims=True) # 1^T dZ
        dX = dldZ @ W.T

        # store the gradients in `self.gradients`
        # the gradient for self.parameters["W"] should be stored in
        # self.gradients["W"], etc.

        self.gradients["W"] = dW
        self.gradients["b"] = db
        self.gradients["X"] = dX
		
        return dX
```

---
## 4.3: Softmax Activation

### 4.3.1
![[Pasted image 20250420191721.png]]

### 4.3.2: Implementation of `activations.SoftMax`:

```python
class SoftMax(Activation):
    def __init__(self):
        super().__init__()

    def forward(self, Z: np.ndarray) -> np.ndarray:
        """Forward pass for softmax activation.
        Hint: The naive implementation might not be numerically stable.
        
        Parameters
        ----------
        Z  input pre-activations (any shape)

        Returns
        -------
        f(z) as described above applied elementwise to `Z`
        """
        num = np.exp(Z - np.max(Z, axis=1, keepdims=True))
        denom = np.sum(num, axis=1, keepdims=True)
        return num/denom

    def backward(self, Z: np.ndarray, dY: np.ndarray) -> np.ndarray:
        """Backward pass for softmax activation.
        
        Parameters
        ----------
        Z   input to `forward` method
        dY  gradient of loss w.r.t. the output of this layer
            same shape as `Z`

        Returns
        -------
        gradient of loss w.r.t. input of this layer
        """
        # Option 1: re-run forward pass to get Y, or store Y in a cache during forward().
        Y = self.forward(Z)           # shape (B, k)
        B, k = Y.shape

        # We'll allocate dLdZ and fill it row by row.
        dLdZ = np.zeros_like(Y)       # shape (B, k)

        # Loop over the batch dimension (allowed for this question).
        for b in range(B):
            y_b = Y[b]                # shape (k,)
            dLdY_b = dY[b]          # shape (k,)
            dot = np.sum(y_b * dLdY_b)
            dLdZ[b] = y_b * (dLdY_b - dot)

        return dLdZ
```

---
## 4.4: Cross-Entropy Loss

### 4.4.1
![[Pasted image 20250420191745.png]]

### 4.4.2: Implementation of `losses.CrossEntropy`

```python
class CrossEntropy(Loss):
    """Cross entropy loss function."""

    def __init__(self, name: str) -> None:
        self.name = name

    def __call__(self, Y: np.ndarray, Y_hat: np.ndarray) -> float:
        return self.forward(Y, Y_hat)

    def forward(self, Y: np.ndarray, Y_hat: np.ndarray) -> float:
        """Computes the loss for predictions `Y_hat` given one-hot encoded labels
        `Y`.

        Parameters
        ----------
        Y      one-hot encoded labels of shape (batch_size, num_classes)
        Y_hat  model predictions in range (0, 1) of shape (batch_size, num_classes)

        Returns
        -------
        a single float representing the loss
        """
        return -1/(Y.shape[0]) * np.sum(Y * np.log(Y_hat))

    def backward(self, Y: np.ndarray, Y_hat: np.ndarray) -> np.ndarray:
        """Backward pass of cross-entropy loss.
        NOTE: This is correct ONLY when the loss function is SoftMax.

        Parameters
        ----------
        Y      one-hot encoded labels of shape (batch_size, num_classes)
        Y_hat  model predictions in range (0, 1) of shape (batch_size, num_classes)

        Returns
        -------
        the gradient of the cross-entropy loss with respect to the vector of
        predictions, `Y_hat`
        """

        tmp = Y * (1 / Y_hat)
        return -1/(Y.shape[0]) * tmp
```

---

## 4.5: BatchNorm Layers

### 4.5.1

![[Pasted image 20250420191814.png]]

### 4.5.2 Implementation of `BatchNorm1D`

```python
class BatchNorm1D(Layer):
    def __init__(
        self, 
        weight_init: str = "xavier_uniform",
        eps: float = 1e-8,
        momentum: float = 0.9,
    ) -> None:
        super().__init__()
        
        # instantiate the weight initializer
        self.init_weights = initialize_weights(weight_init,)

        self.eps = eps
        self.momentum = momentum

    def _init_parameters(self, X_shape: Tuple[int, int]) -> None:
        """Initialize all layer parameters (weights, biases)."""
        self.n_in = X_shape[1]

        gamma = self.init_weights((1, self.n_in))
        beta = self.init_weights((1, self.n_in))

        running_mu = np.zeros((1, self.n_in))
        running_var = np.zeros((1, self.n_in))

        self.parameters = OrderedDict({"gamma": gamma, "beta": beta}) # DO NOT CHANGE THE KEYS
        self.cache = OrderedDict({"X": None, "X_hat": None ,
                                  "mu": None , "var": None ,
                                  "running_mu": running_mu , "running_var": running_var})  
        # cache for backprop
        self.gradients: OrderedDict = OrderedDict({
            "gamma": np.zeros_like(gamma),
            "beta": np.zeros_like(beta)
        })  # parameter gradients initialized to zero # MUST HAVE THE SAME KEYS AS `self.parameters`


    def forward(self, X: np.ndarray, mode: str = "train") -> np.ndarray:
        """ Forward pass for 1D batch normalization layer.
        Allows taking in an array of shape (B, C) and performs batch normalization over it. 

        We use Exponential Moving Average to update the running mean and variance. with alpha value being equal to self.gamma

        You should set the running mean and running variance to the mean and variance of the first batch after initializing it.
        You should also make separate cases for training mode and testing mode.
        """
        ### BEGIN YOUR CODE ###
        gamma = self.parameters["gamma"]
        beta = self.parameters["beta"]
        running_mu = self.cache["running_mu"]
        running_var = self.cache["running_var"]


        if mode == "train":
            mu = np.mean(X, axis=0, keepdims=True)
            var = np.var(X, axis=0, keepdims=True)

            running_mu  = self.momentum * running_mu  + (1.0 - self.momentum) * mu
            running_var = self.momentum * running_var + (1.0 - self.momentum) * var

            X_hat = (X - mu) / np.sqrt(var + self.eps)

            output = gamma * X_hat + beta
            
            self.cache["X"] = X
            self.cache["X_hat"] = X_hat
            self.cache["mu"] = mu
            self.cache["var"] = var
            self.cache["running_mu"] = running_mu
            self.cache["running_var"] = running_var

        else:

            X_hat = (X - running_mu)/ np.sqrt(running_var + self.eps)
            output = gamma * X_hat + beta

        return output
```

---
# Q5: Two-Layer Fully Connected Networks
## 5.1 Fill in the forward, backward, and predict methods for the NeuralNetwork class in models.py.

#### Implementation of `models.NeuralNetwork.forward`:

```python
def forward(self, X: np.ndarray) -> np.ndarray:
	"""One forward pass through all the layers of the neural network.

	Parameters
	----------
	X  design matrix whose must match the input shape required by the
	   first layer

	Returns
	-------
	forward pass output, matches the shape of the output of the last layer
	"""
	# Iterate through the network's layers.
	a_l = X # first layer | a_0
	for layer in self.layers:
		a_l = layer.forward(a_l)
	return a_l
```

Implementation of `models.NeuralNetwork.backward`:

```python
def backward(self, target: np.ndarray, out: np.ndarray) -> float:
	"""One backward pass through all the layers of the neural network.
	During this phase we calculate the gradients of the loss with respect to
	each of the parameters of the entire neural network. Most of the heavy
	lifting is done by the `backward` methods of the layers, so this method
	should be relatively simple. Also make sure to compute the loss in this
	method and NOT in `self.forward`.

	Note: Both input arrays have the same shape.

	Parameters
	----------
	target  the targets we are trying to fit to (e.g., training labels)
	out     the predictions of the model on training data

	Returns
	-------
	the loss of the model given the training inputs and targets
	"""
	# Compute the loss.
	loss = self.loss.forward(target, out)
	a_l = self.loss.backward(target, out)
	
	for layer in reversed(self.layers):
		a_l = layer.backward(a_l)

	# Backpropagate through the network's layers.
	return loss
```

Implementation of `models.NeuralNetwork.predict`:

```python
def predict(self, X: np.ndarray, Y: np.ndarray) -> Tuple[np.ndarray, float]:
	"""Make a forward and backward pass to calculate the predictions and
	loss of the neural network on the given data.

	Parameters
	----------
	X  input features
	Y  targets (same length as `X`)

	Returns
	-------
	a tuple of the prediction and loss
	"""
	### YOUR CODE HERE ###
	# Do a forward pass. Maybe use a function you already wrote?
	Y_hat = self.forward(X)

	# Get the loss. Remember that the `backward` function returns the loss.
	loss = self.backward(Y, Y_hat)

	return (Y_hat, loss)
```

---
## 5.2: 2-layer neural network on Iris Dataset

> You must try at least 3 different combinations of these hyperparameters. Report the results of your exploration, including the values of the parameters you explored and which set of parameters gave the best test error. Provide plots showing the loss versus iterations for your best model and report your final test error.

####  Attempt 1: (==Best Model==) Learning Rate, Neurons in Hidden layer (0.01, 8)  

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

#### Attempt 2: Learning Rate, Neurons in Hidden layer (0.001, 8)

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

#### Attempt 3: Learning Rate, Neurons in Hidden layer (0.01, 10)

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

# Q6: CNN Layers
## 6.1 The Einsum Function

<figure style="text-align: center;">
    <img src="file:///Users/waihan/Downloads/cs189/hw6release/code/notes/images/image-2.png" width="650"/>
    <figcaption>Einsum Functions</figcaption>
  </figure>

---
## 6.2 Convolutional Layer

1. 
![[Pasted image 20250420191854.png]]

2. forward passes of the `Conv2D` layer in `layers.py`.
#### Implementation of `layers.Conv2D.forward`:

```python
def forward(self, X: np.ndarray) -> np.ndarray:
	"""Forward pass for convolutional layer. This layer convolves the input
	`X` with a filter of weights, adds a bias term, and applies an activation
	function to compute the output. This layer also supports padding and
	integer strides. Intermediates necessary for the backward pass are stored
	in the cache.

	Parameters
	----------
	X  input with shape (batch_size, in_rows, in_cols, in_channels)

	Returns
	-------
	output feature maps with shape (batch_size, out_rows, out_cols, out_channels)
	"""
	if self.n_in is None:
		self._init_parameters(X.shape)

	W = self.parameters["W"]
	b = self.parameters["b"]

	kernel_height, kernel_width, in_channels, out_channels = W.shape
	n_examples, in_rows, in_cols, in_channels = X.shape
	kernel_shape = (kernel_height, kernel_width)

	# output dimensions
	pad_h, pad_w = self.pad
	out_rows = (in_rows - kernel_height + 2 * pad_h) // self.stride + 1
	out_cols = (in_cols - kernel_width + 2 * pad_w) // self.stride + 1

	X_padded = np.pad(
		X,
		pad_width=((0, 0), (pad_h, pad_h), (pad_w, pad_w), (0, 0)),
		mode='constant'
	)

	X_windows = np.lib.stride_tricks.sliding_window_view(X_padded, window_shape=kernel_shape, axis=(1, 2))   # slide in h and w directions
	
	X_windows = X_windows[:, ::self.stride, ::self.stride, :, :, :]

	out = np.einsum('nijchw, hwck->nijk', X_windows, W)

	out += b  # shape (out_channels,) or (1, out_channels)

	out = self.activation(out)


	self.cache["X"] = X
	self.cache["Z"] = out

	return out
```

## 6.3 Pooling Layers

1. We need to keep track of which value in the pooling window get voted. For example, if we use max pooling method, the index that corresponds to the maximum value in the pooling window should be tracked. This is to ensure that the gradient at the pooled output is passed back only to that specific input element. The other indices will get the gradient of zero. If we have an average pooling, the gradient from the pool output should be distributed back into each individual input evenly.

2. Fill in the forward and backward passes of the Pool2D layer in `layers.py`.
#### Implementation of `layers.Pool2D`:

```python
class Pool2D(Layer):
    """Pooling layer, implements max and average pooling."""

    def __init__(
        self,
        kernel_shape: Tuple[int, int],
        mode: str = "max",
        stride: int = 1,
        pad: Union[int, Literal["same"], Literal["valid"]] = 0,
    ) -> None:

        if type(kernel_shape) == int:
            kernel_shape = (kernel_shape, kernel_shape)

        self.kernel_shape = kernel_shape
        self.stride = stride

        if pad == "same":
            self.pad = ((kernel_shape[0] - 1) // 2, (kernel_shape[1] - 1) // 2)
        elif pad == "valid":
            self.pad = (0, 0)
        elif isinstance(pad, int):
            self.pad = (pad, pad)
        else:
            raise ValueError("Invalid Pad mode found in self.pad.")

        self.mode = mode

        if mode == "max":
            self.pool_fn = np.max
            self.arg_pool_fn = np.argmax
        elif mode == "average":
            self.pool_fn = np.mean

        self.cache = {
            "out_rows": [],
            "out_cols": [],
            "X_pad": [],
            "p": [],
            "pool_shape": [],
        }
        self.parameters = {}
        self.gradients = {}

    def forward(self, X: np.ndarray) -> np.ndarray:
        """Forward pass: use the pooling function to aggregate local information
        in the input. This layer typically reduces the spatial dimensionality of
        the input while keeping the number of feature maps the same.

        As with all other layers, please make sure to cache the appropriate
        information for the backward pass.

        Parameters
        ----------
        X  input array of shape (batch_size, in_rows, in_cols, channels)

        Returns
        -------
        pooled array of shape (batch_size, out_rows, out_cols, channels)
        """
        batch_size, in_rows, in_cols, channels = X.shape
        pad_h, pad_w = self.pad

        X_pad = np.pad(
            X,
            pad_width=((0,0), (pad_h, pad_h), (pad_w, pad_w), (0, 0)),
            mode='constant'
        )        

        kernel_height, kernel_width  = self.kernel_shape

        out_rows = (in_rows - kernel_height + 2 * pad_h) // self.stride + 1
        out_cols = (in_cols - kernel_width + 2 * pad_w) // self.stride + 1

        # print(f"Out rows, cols: {out_rows, out_cols}")
 
        self.cache["X_shape"] = X.shape
        self.cache["X_pad"] = X_pad
        self.cache["X_pad_shape"] = X_pad.shape


        self.cache["out_rows"] = out_rows
        self.cache["out_cols"] = out_cols

        # print(f"\nX pad dimension: {X_pad.shape}")
        # print(f"Kernel dimension: {self.kernel_shape}")
        windows = np.lib.stride_tricks.sliding_window_view(X_pad,
                                                           window_shape=self.kernel_shape,
                                                           axis=(1, 2))
        
        # print(f"Windows shape after sliding window: {windows.shape}")
        windows = windows[:, ::self.stride, ::self.stride, :, :, :]

        if self.mode == "max":
            windows_flat = windows.reshape(batch_size, out_rows, out_cols, channels, -1)
            # print(f"windows flat shape: {windows_flat.shape}")

            max_values = np.max(windows_flat, axis=-1)
            max_indices = np.argmax(windows_flat, axis=-1)

            # convert back to 2D indices
            max_row_indices = max_indices // self.kernel_shape[1]
            max_col_indices = max_indices % self.kernel_shape[1]

            self.cache["max_row_indices"] = max_row_indices
            self.cache["max_col_indices"] = max_col_indices

            X_pool = max_values

        elif self.mode == "average":
            X_pool = np.mean(windows, axis=(4, 5))

        return  X_pool


    def backward(self, dLdY: np.ndarray) -> np.ndarray:
        """Backward pass for pooling layer.

        Parameters
        ----------
        dLdY  gradient of loss with respect to the output of this layer
              shape (batch_size, out_rows, out_cols, channels)

        Returns
        -------
        gradient of loss with respect to the input of this layer
        shape (batch_size, in_rows, in_cols, channels)
        """
        pad_h, pad_w = self.pad

        # retrieve from cache
        X_pad = self.cache["X_pad"]
        batch_size, padded_rows, padded_cols, channels = X_pad.shape
        out_rows = self.cache["out_rows"]
        out_cols = self.cache["out_cols"]

        original_rows = padded_rows - 2 * pad_w
        original_cols = padded_cols - 2 * pad_h

        kernel_height, kernel_width = self.kernel_shape
        # gradient wrt padded input
        dX_pad = np.zeros_like(X_pad)

        if self.mode == "max":
            max_row_indices = self.cache["max_row_indices"]
            max_col_indices = self.cache["max_col_indices"]

            b_indices, i_indices, j_indices, c_indices = np.meshgrid(
                np.arange(batch_size),
                np.arange(out_rows),
                np.arange(out_cols),
                np.arange(channels),
                indexing='ij'
            )

            row_positions = i_indices * self.stride + max_row_indices
            col_positions = j_indices * self.stride + max_col_indices

            np.add.at(dX_pad, (b_indices, row_positions, col_positions, c_indices), dLdY)

        elif self.mode == "average":
            pool_size = kernel_height * kernel_width
            grad_kernel = np.zeros((batch_size, out_rows, out_cols, channels, kernel_height, kernel_width))

            # distribute the gradient across all inputs in the pool
            grad_kernel += 1.0 / pool_size
            grad_kernel = grad_kernel * dLdY[:, :, :, :, np.newaxis, np.newaxis]

            for i in range(kernel_height):
                for j in range(kernel_width):
                    rows = np.arange(out_rows) * self.stride + i
                    cols = np.arange(out_cols) * self.stride + j
                    r, c = np.meshgrid(rows, cols, indexing='ij')
                    dX_pad[:, r, c, :] += grad_kernel[:, :, :, :, i, j] # 4D i and j are fixed 

        # trim padding
        if pad_h > 0 or pad_w > 0:
            gradX = dX_pad[:, pad_h:(pad_h + original_rows), pad_w:(pad_w + original_cols), :]
        else:
            gradX = dX_pad
        return gradX
```

---

# Q7: PyTorch

## 7.1 CNN for Fashion MNIST

### 7.1.1 Training an CNN on the fashion MNIST dataset (from code appendix)

---
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

## 7.2 Transfer Learning for CIFAR-10

### 7.2.1 From Code appendix for training CNN model

---
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






