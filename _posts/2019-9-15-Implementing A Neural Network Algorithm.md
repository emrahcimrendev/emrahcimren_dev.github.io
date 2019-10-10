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
networks work. We consider the 
[Predicting Short Term Trucking Rates problem](https://emrahcimren.github.io/transportation%20rates/Predicting-Short-Term-Trucking-Rates-with-Random-Forests/).
and develop a neural network model to predict transportation rates using the the European long-haul Truckload data.

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
- Activation function for each hidden layer, $$g$$.

The number of nodes in the input layer is determined by 
the dimensionality of our data, i.e.
features we use in prediction. 
The dimensionality (the number of nodes) of the hidden layer are given. 
The number of nodes in the output layer is determined by the number of prediction classes.

## A Neuron 

A neuron computes a linear function 

$$z = w_1x_1 + \dots + w_nx_n + b = Wx + b.$$ 

An output of a neuron is

$$\hat{y} = g(z) = g(Wx + b).$$

The process of calculating $$W$$ and $$b$$ from the input data is known as training the Neural Network. 
Each iteration of the training process consists of the following steps:

- **Forward propagation**: Calculating the predicted output, $$\hat{y}$$,
- **Backward propagation**: Updating $$W$$ and $$b$$.

## Algorithm

The following are the steps to build the model:

1. Define the network structure, i.e. number of input features.

2. Initialize parameters.
 - Number of iterations
 - Number of layers $$L$$ in the neural network
 - Size of the hidden layers
 - Learning rate, $$\alpha$$
 
3. For each iteration:
 - Forward propagation (calculate current loss)
 - Compute cost function
 - Backward propagation (calculate current gradient)
 - Update parameters (using parameters, and gradients from backpropagation)
 
4. Use trained parameters to predict labels.

## Problem

We consider the [Predicting Short Term Trucking Rates problem](https://emrahcimren.github.io/transportation%20rates/Predicting-Short-Term-Trucking-Rates-with-Random-Forests/).
and develop a neural network model to predict transportation rates using the the European long-haul Truckload data.

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_customer_warehouse_network.jpg) | 
|:--:| 
| *Figure 1: Distribution centers and customers* |

Our objective is to determine FTL and LTL rates for each distribution center to each customer. We develop
a model to predict transportation rates. 

## 1. Define Network Structure



## 2. Initialize Parameters 

## 3. Iterations

### Forward Propagation

### Loss function

### Backward Propagation

### Activation Functions 

## 4. Predict Labels


