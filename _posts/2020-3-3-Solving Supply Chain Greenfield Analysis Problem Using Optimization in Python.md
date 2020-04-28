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

Greenfield analysis answers the following supply chain design questions:

- How many distribution centers do we need to reach our service level?
- Where should distribution centers be geographically located to minimize cost?
- Which customers will be supplied from each distribution center?

# Algorithm

In the previous post, we define an unsupervised learning
based (K-means) algorithm to the greenfield analysis problem. With a given
number of clusters ($$K$$), this
algorithm starts with an arbitrary assignment of each cluster
to set of customers, calculates initial assignment cost, 
and iterates until there is no better assignment with smaller cost 
exists. The algorithm quickly provides solution. However, solution
quality depends on initial assignment of customers to clusters.

We now propose an optimization based algorithm for solving the greenfield
problem. We first formulate supply chain greenfield analysis 
problem as the K-Median problem and then solve the mathematical formulation.

## K-Median

Select a given number of facilities from possible points in a graph, 
in such a way that the sum of the distances 
from each customer to the closest facility is minimized.

The following is the mixed integer programming formulation 
of the K-Median problem.

![_config.yml]({{ site.baseurl }}/images//Solving Supply Chain Greenfield Analysis Problem Using Optimization in Python/kmedian_inputs.PNG)



![_config.yml]({{ site.baseurl }}/images//Solving Supply Chain Greenfield Analysis Problem Using Optimization in Python/kmedian_model.PNG)


## Formulating Greenfield Problem as K-Median

Note that we select $$K$$ number of facilities among given
possible facility locations in the K-Median problem. 
However, greenfield
problem starts with no set of possible facility locations.

Since we can open up distribution centers at physical addresses
and each physical address has an associated zipcode, we can define
each zipcode in the given region 
as a possible facility location at the greenfield problem. In this case,
K-median formulation can be used for solving greenfield problem.

## Solving a Greenfield Problem

We consider a network of customers where demand for each customer is satisfied by a distribution center. 
Figure 1 shows the customer locations and corresponding annual demand. 

| ![_config.yml]({{ site.baseurl }}/images/2019-7-30_customer_demand_points.jpg) | 
|:--:| 
| *Figure 1: Customer demand map* |

It is assumed that any location can be selected for a distribution center location. 


### Obtaining Zipcodes as Candidate Facility Locations with Python

## Solution to the Problem


# Extensions to Brownfield Analysis


# Extensions to Spatial Clustering

# Conclusion

