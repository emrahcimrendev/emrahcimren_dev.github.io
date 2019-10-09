---
title: "Implementing A Neural Network Algorithm"
categories:
  - neural networks
tags:
  - neural networks
  - algorithm implementation
  - python
--- 

In this post, we will implement a simple three-layer neural network from scratch to explain how neural
networks work.

![image-center](/images/2019-9-15_neurons.jpg){: .align-center}

## What is a Neural Network?

Neural networks use an architecture inspired by the neurons in the brain. 
A brain neuron receives an input and based on that input and fires off an output that is used by another neuron. 
Neurons interconnected to other neurons forms a network. 
The neural network simulates this behavior in learning about collected data and then predicting outcomes.

Neural networks can be read from left to right.
Inputs are entered to the first layer. 
Internal layers (hidden layers) perform calculations, 


and one last layer that contains all the possible outputs. Don’t bother with the “+1”s at the bottom of every columns. It is something called “bias” and we’ll talk about that later
