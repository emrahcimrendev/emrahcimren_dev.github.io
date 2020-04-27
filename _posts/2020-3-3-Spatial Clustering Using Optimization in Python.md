---
title: "Spatial Clustering Using Optimization for Supply Chain Greenfield Analysis"
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
We use this algorithm to solve a Supply chain Greenfield Analysis 
problem.

![_config.yml]({{ site.baseurl }}/images//Spatial Clustering Using Optimization in Python/GreenFields.jpg)

# Supply Chain Greenfield Analysis


# Algorithm

In the previous post, we propose an unsupervised learning
based (K-means) algorithm to the greenfield analysis problem. With a given
number of clusters ($$K$$), this
algorithm starts with an arbitrary assignment of each cluster
to set of customers, calculates initial assignment cost, 
and iterates until there is no better assignment with smaller cost 
exists. The algorithm quickly provides solution. However, 

Spatial data represents the location, size, and shape of an object 
on planet Earth such as a building, lake, mountain or township. 
Spatial clustering aims to partition spatial data into a series of 
meaningful subclasses, called spatial clusters. 

Facility location problem is classical optimization problem for 
determining the sites for factories and warehouses.

