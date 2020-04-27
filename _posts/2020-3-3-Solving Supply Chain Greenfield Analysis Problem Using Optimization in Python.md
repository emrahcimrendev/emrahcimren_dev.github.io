---
title: "Solving Supply Chain Greenfield Analysis Problem Using Optimization in Python"
categories:
  - operations research
tags:
  - spatial clustering
  - optimization
  - facility location
  - python
--- 

Supply chain Greenfield Analysis helps 
determine where to locate your warehouses and factories based on
customer demand. In this post,
we provide an optimization based spatial clustering algorithm. 
We use this algorithm to solve a supply chain greenfield analysis 
problem.

![_config.yml]({{ site.baseurl }}/images//Solving Supply Chain Greenfield Analysis Problem Using Optimization in Python/GreenFields.jpg)

# Supply Chain Greenfield Analysis

A greenfield analysis is a great way to get started for 
long term strategic network planning. 
A greenfield analysis provides the optimal locations 
for your Distribution network based on customer demand footprint.

# Algorithm

In the previous post, we propose an unsupervised learning
based (K-means) algorithm to the greenfield analysis problem. With a given
number of clusters ($$K$$), this
algorithm starts with an arbitrary assignment of each cluster
to set of customers, calculates initial assignment cost, 
and iterates until there is no better assignment with smaller cost 
exists. The algorithm quickly provides solution. However, solution
quality depends on initial assignment of customers to clusters.

We now propose an optimization based algorithm for solving the greenfield
problem. We first formulate supply chain greenfield analysis 
problem as the K-Median problem and then solver the mathematical formulation.

## K-Median

Select a given number of facilities from possible points in a graph, 
in such a way that the sum of the distances 
from each customer to the closest facility is minimized.






Spatial data represents the location, size, and shape of an object 
on planet Earth such as a building, lake, mountain or township. 
Spatial clustering aims to partition spatial data into a series of 
meaningful subclasses, called spatial clusters. 

Facility location problem is classical optimization problem for 
determining the sites for factories and warehouses.

