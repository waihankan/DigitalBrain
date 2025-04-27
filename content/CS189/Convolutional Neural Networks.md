

Learn Edge Detector

Local connectivity: hidden units connect only to a small patch of units in the previous layer.

Shared Weights: groups of hidden units share same set of weights (called filters / masks/ kernels). 

dot product. not a matrix.
We learn the weights in the filter by 'backpropagation'


---


Here are detailed notes about channels, layers, filters, and how dimensions work in Convolutional Neural Networks (CNNs), along with proper notations, based on the provided source [19.pdf]:

### Convolutional Neural Networks (ConvNets/CNNs)

- ConvNets are neural networks that have driven a resurgence of interest in neural nets, often referred to as **deep learning** (neural nets with many layers).
- Most image recognition networks are deep and convolutional.

### Core Concepts

#### 1. Channels

- A **channel** can be an **activation map** produced by a filter, or it can represent another dimension of the input, such as the **red, green, and blue channels of a color image**.
- The **input to a convolutional layer can have multiple channels**, for instance, the RGB channels of an image.
- A **convolutional layer learns multiple filters**, and for each filter, it produces **one activation map**, which is also considered a channel.
- Therefore, the **output of a convolutional layer has multiple channels**, with the number of output channels equal to the number of filters learned by that layer. These output channels then become the input channels for subsequent convolutional layers.
- **Notation:** If a convolutional layer $l$ has $C(l-1)$ input channels and $C(l)$ output channels (meaning it learns $C(l)$ filters), then:
    - $C(l-1)$ represents the number of channels coming into layer $l$.
    - $C(l)$ represents the number of channels going out of layer $l$ (and also the number of filters in layer $l$).
#### 2. Layers

CNNs typically consist of different types of layers:

- **Convolutional Layers (Conv Layers)**:
    
    - These are the core building blocks of CNNs.
    - They utilize **filters** (also known as **masks** or **kernels**) with **shared weights** to perform convolution operations on the input.
    - Each hidden unit in a convolutional layer connects only to a **small, local patch** of units in the previous layer. This is called **local connectivity**. A typical patch size might be $9$ ($3 \times 3$), $25$ ($5 \times 5$), or $49$ ($7 \times 7$) pixels.
    - The same set of weights (a filter) is applied across every patch of the input image to produce an **activation map**. This is the concept of **shared weights**.
    - A convolutional layer learns **multiple filters**, each designed to detect different features (e.g., edges, textures). Each filter generates its own activation map, resulting in multiple output channels.
    - **Notation:** A filter has a size of $M \times M$ in spatial dimensions. If the input to the layer has $C(l-1)$ channels, the filter is a 3D array of size $C(l-1) \times M \times M$. The set of all $C(l)$ filters in the layer can be thought of as a 4D array.
    - The output of a convolutional layer is a set of activation maps, each of size $(J - M + 1) \times (K - M + 1)$ if the input image size is $J \times K$ and the filter size is $M \times M$. The number of such activation maps is equal to the number of filters (output channels), $C(l)$.
- **Pooling Layers (Downsampling Layers)**:
    
    - These layers are used to **reduce the spatial size** of the feature maps (activation maps) coming from the convolutional layers.
    - Pooling helps to make the network more **invariant to small translations** in the input.
    - Common types of pooling include **max pooling** (taking the maximum value within a local region) and **average pooling** (taking the average value). LeNet 5 used average pooling, but max pooling is more popular now.
    - Pooling layers have **no weights** to be trained.
    - **Notation:** If a $J \times K$ activation map is subjected to $2 \times 2$ max pooling, the output size becomes $\lceil J/2 \rceil \times \lceil K/2 \rceil$.
- **Fully Connected Layers**:
    
    - After several convolutional and pooling layers, the high-level reasoning in the network is often done using fully connected layers.
    - The output from the final pooling or convolutional layer is **flattened** into a one-dimensional vector and then fed into one or more fully connected layers.
    - In these layers, each neuron is connected to every neuron in the previous layer, similar to traditional multi-layer perceptrons.
    - The final fully connected layer often uses a **softmax activation function** for classification, outputting probabilities for each class.
- **Other Layers:** Modern CNN architectures often include other layers like **ReLU activation** layers (applying the Rectified Linear Unit activation function), **Batch Normalization** layers (for regularization and stabilization of training), and **Dropout** layers (another regularization technique to prevent overfitting).
    

#### 3. Filters (Kernels/Masks)

- A **filter** is a small weight matrix (or a 3D tensor in the case of multi-channel input) that slides across the input data (image or feature map) performing a **convolution operation**.
- The size of the filter (e.g., $3 \times 3$, $5 \times 5$) determines the **local receptive field** of the hidden units in the convolutional layer.
- Each filter is designed to detect a specific feature in the input, such as edges, corners, or textures.
- **Shared weights** mean that the same filter (same set of weights) is used at every spatial location of the input, which significantly reduces the number of learnable parameters and makes the network more efficient and less prone to overfitting. This also allows a feature detector learned in one part of the image to be useful in another part.
- A convolutional layer learns a set of these filters during the training process through backpropagation.
- **Notation:**
    - The spatial dimensions of a filter are typically $M \times M$.
    - If the input to a convolutional layer has $C(l-1)$ channels, then each filter in that layer is a $C(l-1) \times M \times M$ tensor.
    - A convolutional layer with $C(l)$ filters has a total of $C(l-1) \times C(l) \times M \times M$ weights (excluding bias terms).

#### 4. Dimensions and How They Work Out

Let's consider a convolutional layer $l$:

- **Input:** Assume the input to this layer is a volume of size $C(l-1) \times J \times K$, where $C(l-1)$ is the number of input channels, $J$ is the height, and $K$ is the width. For the first convolutional layer, this input might be an RGB image, so $C(0) = 3$, and $J$ and $K$ are the image dimensions (e.g., $400 \times 400$).
- **Filters:** The convolutional layer has $C(l)$ filters (kernels), each with a spatial size of $M \times M$ and extending across all $C(l-1)$ input channels. So, each filter has dimensions $C(l-1) \times M \times M$.
- **Convolution Operation:** Each of these $C(l)$ filters is convolved with the input volume. The convolution operation involves sliding the filter across the spatial dimensions (height and width) of the input, computing the element-wise product between the filter and the corresponding local patch of the input, and then summing the results to produce a single output value for the corresponding location in the output feature map.
- **Activation Map (Output Channel):** For each of the $C(l)$ filters, this sliding and computing process generates a 2D activation map (a single output channel) of size $(J - M + 1) \times (K - M + 1)$. The reduction in spatial size ($J-M+1$ and $K-M+1$) occurs because the filter cannot be perfectly centered on the border pixels of the input image.
- **Output Volume:** The output of the convolutional layer $l$ is a volume of size $C(l) \times (J' \times K')$, where $C(l)$ is the number of filters (and thus the number of output channels), and $J' = J - M + 1$ and $K' = K - M + 1$ are the dimensions of each activation map. This output volume then serves as the input to the next layer (which could be another convolutional layer or a pooling layer).

**Bias Terms:**

- A bias term can be added to the output of each convolution operation.
- There are different options for bias terms:
    - **Untied bias:** One bias term per output unit: $C(l) \times (J - M + 1) \times (K - M + 1)$ bias terms.
    - **Tied bias:** One bias term per filter/output channel: $C(l)$ bias terms.
    - **No bias terms:** This is sometimes used, especially if a ReLU activation follows immediately.

**Strides:**

- The **stride** defines how many pixels the filter is shifted over the input image during the convolution operation.
- A stride of 1 means the filter moves one pixel at a time, resulting in overlapping patches.
- A stride greater than 1 reduces the spatial size of the output activation map more significantly because the filter moves in larger steps, covering fewer overlapping patches. If the stride is $S$, the output dimension becomes $(\lfloor (J - M)/S \rfloor + 1) \times (\lfloor (K - M)/S \rfloor + 1)$.

**Padding:**

- **Padding** is a technique used to add extra layers of pixels (usually with value 0) around the input image.
- Padding is often used to ensure that the filter fits perfectly onto the input image, especially at the borders, and to control the output size of the convolutional layer. For example, with "same" padding, the output size is the same as the input size (when stride is 1).

**Pooling Layer Dimensions:**

- A pooling layer reduces the spatial dimensions of its input. For example, with $2 \times 2$ max pooling, a $J \times K$ input is reduced to $\lceil J/2 \rceil \times \lceil K/2 \rceil$. The number of channels remains the same.

Understanding these concepts of channels, layers, filters, and how their dimensions transform through the network is crucial for comprehending the architecture and functionality of Convolutional Neural Networks. The notation helps to formalize these ideas and enables precise descriptions of network structures.


--- 

Based on the sources, an **activation map** is a **2D array of hidden units** that is the **output of a convolutional layer** when a filter (or kernel) is applied to the input image.

Here's a more detailed breakdown:

- A convolutional layer **learns multiple filters**.
- For **each filter**, the convolutional layer produces **one activation map**. This means if a convolutional layer learns, for example, four filters, it will produce four activation maps.
- An activation map can also be considered a **channel**. The output of a convolutional layer has multiple channels, with **one channel per filter**. These output channels then become the input channels for subsequent convolutional layers.
- The activation map is generated by **applying a filter to every patch of the input image** through a process called convolution. The filter, which contains a set of shared weights, slides across the input image, performing a computation at each location to produce a single value in the activation map.
- If the input image has a size of $J \times K$ and the filter has a size of $M \times M$, the resulting activation map will have dimensions of $(J - M + 1) \times (K - M + 1)$. This is because the filter can only be centered on these many locations within the input image.
- Figure 4 in the source illustrates how applying a filter to an image results in an activation map.
- Figure 5 shows an example where different filters (Sobel horizontal, Laplacian, Sobel vertical) are applied to an image, yielding three different activation maps, which are considered three channels. These filters in the example were not learned by a CNN but could have been.
- Figure 6 illustrates a convolutional layer where the input has three channels (red, green, blue), and the layer learns four filters. Each filter (which is a 3D array in this case) is applied to the 3-channel input to produce one activation map. The output of this layer is therefore four activation maps, or four channels.
- Visualizations of trained CNNs show that the activation maps in the first layer often correspond to the detection of basic features like edges and colors. Subsequent layers learn to detect more complex features based on the activation patterns in the preceding layers.

In essence, an activation map highlights the regions in the input image that strongly activate a particular learned feature as defined by the filter. Each filter is designed to detect a specific feature, and its corresponding activation map shows where that feature is present in the input.

---



* involved in research labs
* Labs
* tips for internship 
* gaining experience in this field
* considering grad school or industry roles in ML, what steps would you recommend?
info 159 


