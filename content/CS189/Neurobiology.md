
* Neuron : Cell in brain / nervous system for thinking.
* Action potential or spike: electrochemical impulse fired by a neuron to communicate.
* Axon: Limbs along which the action potential propagates (output) 
* Dendrite: Smaller limb by which neuron receives into (input)
* Synapse: Connection from one neuron axon to another's dendrite
* Neurotransmitter: Chemical released by axon terminal to stimulate dendrite.


#### Analogies

* Output of unit -Firing Rate of neuron (Not voltage, it's about how fast the neuron is firing)
* 0 - 1000 times per second.
* weights of connection <-> Synapse strength
* Positive weights and negative weights <-> exatatory neurotransmitter (glutamine)
* Negative weights <-> inhibitory neurotransmitter  
* Linear combinations of inputs <-> Summation 
* Logistic / Sigmoid Function <-> Firing Rate Saturation (0 - 1000 per second)
* Weight change / learning <-> Synaptic plasticity
* "Cells that fire together, wire together" 


---

## Heuristics for faster training

1. ReLUs
2. Stochastic Gradient Descent (SGD): faster than batch on large, redundant data sets



> One epoch presents every training point once.

* In Batch Gradient Descent, one epoch is just one iteration.
* In Stochastic Gradient Descent, one epoch means shuffling the data and going through all the points in it. 
* Training usually takes many epochs, if the training sample size is huge, SGD might converge in less than one epoch.

### Mini-batch Gradient descent

1. choose a mini-batch size b (e.g. 256)
2. Repeatedly perform gradient descent on the **sum** of the loss functions of b randomly choose points. 

Advantages:

* Less bouncy; usually converges more quickly.
* Can use parallelism, vectorization, GPUs efficiency.
* Better Speed because of memory hierarchy

Typically, we shuffle training points, partition into ceil(n/b) minibatches.

An epoch present each mini-batch once.


Continue here!!!
