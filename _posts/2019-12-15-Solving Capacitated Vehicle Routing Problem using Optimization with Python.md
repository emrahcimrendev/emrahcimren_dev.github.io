---
title: "Solving Capacitated Vehicle Routing Problem Using Optimization with Python"
categories:
  - operations research
tags:
  - capacitated vehicle routing problem with time windows
  - CVRPTW
  - optimization
  - mixed integer programming
  - python
--- 

Vehicle routing problem (VRP) is identifying the optimal set of routes for a set of 
vehicles to travel in order to deliver to a given 
set of customers. When vehicles have limited carrying capacity and 
customers have time windows within which the deliveries must be made, problem becomes
capacitated vehicle routing problem with time windows (CVRPTW). 
In this post, we will discuss how to tackle CVRPTW using Python and 
get a fast and robust solution.

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Capacitated Vehicle Routing Problem Using Optimization with Python/truck_main.jpg)

## Problem 


We consider a pizza restaurant chain, **PPizza**, in the Los Angeles, CA area with 34 stores. 
Each store operates from 10am to 1am everyday. **PPizza** offers three pizza sizes
(small, medium, large) with various toppings and soft drinks. Pizzas are prepared using fresh
ingredients and baked in store on demand. 

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Capacitated Vehicle Routing Problem Using Optimization with Python/pizza.PNG)

forecasts weekly demand of 
food items for each store and identifies required ingredients. 
 
 and one depot. 
Figure 1 shows location of stores and a depot.  

| ![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Capacitated Vehicle Routing Problem with Python/store_depot_map.PNG) | 
|:--:| 
| *Figure 1: PPizza depot and stores* |

## Analysis

### Mathematical Formulation

### Python Implementation

### Analysis of Results


