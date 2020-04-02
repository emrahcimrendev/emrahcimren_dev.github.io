---
title: "Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python"
categories:
  - operations research
tags:
  - single depot capacitated vehicle routing problem with time windows
  - VRPTW
  - optimization
  - column generation
  - python
--- 

Vehicle routing problem (VRP) is identifying the optimal set of routes for a set of 
vehicles to travel in order to deliver to a given 
set of customers.  When vehicles have limited carrying capacity and 
customers have time windows within which the deliveries must be made, p
roblem becomes capacitated vehicle routing problem with time windows (CVRPTW). 
In this post, we will discuss how to tackle CVRPTW using Python and get a fast and 
robust solution.

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/truck_main.jpg)

## Problem 

We consider a pizza restaurant chain, **PPizza**, in the Los Angeles, CA area with 34 stores. 
Each store operates from 10am to 1am everyday. **PPizza** offers three pizza sizes
(small, medium, large) with various toppings and soft drinks. Pizzas are prepared using fresh
ingredients and baked in store on demand. 

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/pizza.PNG)

**PPizza** forecasts weekly demand of  food items for each store and identifies required ingredients
and soft drinks. Fresh ingredients are delivered to stores daily from the main depot once a day. 
Soft drinks are delivered and replenished by suppliers directly. 

Figure 1 shows location of stores and the depot. Each store has time windows between 
9am and 3pm where delivery needs to be
done within. Unloading time varies by store depending on location and parking availability. 

| ![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/store_depot_map.PNG) | 
|:--:| 
| *Figure 1: PPizza depot and stores* |

Trucks can leave from the depot at 6am and need to return the depot by 5pm. 
Each truck can be used once and has a limited capacity.
We want to identify the truck operating schedules 
to be able to deliver
fresh ingredients to each store with given time windows by minimizing the total cost. 

## Methodology

We use column generation to solve CVRPTW. Rousseau provides a tutorial on column generation and branch-and-price 
for [vehicle routing problems](https://symposia.cirrelt.ca/system/documents/000/000/254/Rousseau_original.pdf?1464701234).

### Mathematical Formulation

We develop a mixed integer model for the **PPizza** delivery problem as follows.










$$$ $$$



### Python Implementation

### Analysis of Results


### References
Desrochers, M., Lenstra, J.K., Savelsbergh, M.W.P., Soumis, F. (1988). 
Vehicle routing with time windows: Optimization and approximation. 
In: Golden, B.L., Assad, A.A. (Eds.), 
Vehicle Routing: Methods and Studies. North-Holland, Amsterdam, pp. 65–84.
