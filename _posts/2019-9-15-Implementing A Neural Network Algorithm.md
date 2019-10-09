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

Neural Networks consist of the following components:

- An input layer, $$x$$, 
- Hidden layers, 
- An output layer, $$\hat{y}$$,
- A set of weights and biases between each layer, $$W$$ and $$b$$,
- Activation function for each hidden layer, $$\sigma$$.

A neuron computes a linear function 

$$z = Wx + b = w_1x_1 + \dots + w_nx_n + b.$$

An output of a neuron is

$$\hat{y} = \sigma(z) = \sigma(Wx + b).$$

z = Wx + b)

