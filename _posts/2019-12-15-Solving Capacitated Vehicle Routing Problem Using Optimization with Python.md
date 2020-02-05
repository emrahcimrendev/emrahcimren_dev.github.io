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

**PPizza** forecasts weekly demand of  food items for each store and identifies required ingredients
and soft drinks. Fresh ingredients are delivered to stores daily from the main depot once a day. 
Soft drinks are delivered and replenished by suppliers directly. 

Figure 1 shows location of stores and the depot. Each store has time windows where delivery needs to be
done within. Unloading time varies by store depending on location and parking availability. 

| ![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Capacitated Vehicle Routing Problem Using Optimization with Python/store_depot_map.PNG) | 
|:--:| 
| *Figure 1: PPizza depot and stores* |

Trucks leave from the depot at 6am in the morning. We can allocate a helper to a truck to speed up
unloading. Drivers cannot drive for longer than 11 hours due to 
Department of Transportation (DOT) regulations.
We can add a second driver to a truck to be able to deliver longer distances. This case is called
"team driver" and truck can be operated up to 22 hours.

We want to identify the required number of trucks and truck operating schedules 
to be able to deliver
fresh ingredients to each store with given time windows and DOT constraints by minimizing the total cost. 

### Mathematical Formulation

We develop a mixed integer model for the **PPizza** delivery problem 
by extending Desrochers et al., (1988) formulation. 

Let $G=(V, A)$ be a directed graph where $|V|=n+2$, 
and the depot is represented by the two vertices $0$ and $n+1$. 





$$$ $$$



### Python Implementation

### Analysis of Results


### References
Desrochers, M., Lenstra, J.K., Savelsbergh, M.W.P., Soumis, F. (1988). 
Vehicle routing with time windows: Optimization and approximation. 
In: Golden, B.L., Assad, A.A. (Eds.), 
Vehicle Routing: Methods and Studies. North-Holland, Amsterdam, pp. 65–84.
