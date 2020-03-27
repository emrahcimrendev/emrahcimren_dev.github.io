---
title: "MOPTA 2020 Case Competition: Vehicle Fleet Sizing, Positioning and Routing"
categories:
  - operations research
tags:
  - capacitated vehicle routing problem with time windows
  - optimization
  - MOPTA
  - python
--- 

The goal of the competition this year is to solve an
optimal vehicle routing problem where in addition, the
location of depots for the vehicles and the fleet size
should be optimized. In this post, we provide a solution
to this problem.

![_config.yml]({{ site.baseurl }}/images//MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/transport-fleet.jpg)

## Problem

Determine set of depots, the number of vehicles, and routing of each vehicle per depot 
to be able to serve the demand at nodes by minimizing the total cost. 
This problem is classified
as "location-routing problem" in the literature.

The detailed problem description is provided at the 
[competition web page](https://coral.ise.lehigh.edu/~mopta/mopta2020/AIMMS_MOPTA_case_2020.pdf).

We now look at problem components.

### Transportation Network

The transportation network is described as an undirected graph with vertices and edges with weights.
The vertices are given as 5-digit zip codes and the edges represent routes between locations.
For each edge, distance is given in miles. There exists cost, $0.70/mile, 
comprises the variable costs such as fuel, tires, repairs, tolls. 
Ech mile driven generates 0.0009 tons of CO2 on an edge. 

### Demand

Each day there is a demand at each vertex that needs to be served by dispatching a vehicle.
Figure 1 shows total demand locations for 2018 and 2019. 

| ![_config.yml]({{ site.baseurl }}/images/MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/demand_map.PNG) | 
|:--:| 
| *Figure 1: Customer demand map* |

There are total of 1451 demand points each of them has a daily demand. Note that 
daily demand can be zero for some demand points. Figure 2 illustrates daily total demand.
Although there are total of 1451 demand points, daily positive demand points varies between
30 and 175. 

| ![_config.yml]({{ site.baseurl }}/images/MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/demand_daily.PNG) | 
|:--:| 
| *Figure 2: Daily total demand* |

For each demand point, a delivery time window exists which is between 8am and 4pm. 

### Depots

### Vehicles

### Routing

### Questions

## Methodology









