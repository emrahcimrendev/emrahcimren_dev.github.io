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
Each mile driven generates 0.0009 tons of CO2 on an edge. 

### Demand

Each day there is a demand at each vertex that needs to be served by dispatching a vehicle.
Figure 1 shows total demand locations for the years 2018 and 2019. 

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

For each demand point, a delivery time window exists between 8am and 4pm. 

### Depots

Figure 3 illustrates locations of alternative depots. Each location has an
an estimated daily fixed cost of maintaining a depot at that location. 

| ![_config.yml]({{ site.baseurl }}/images/MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/depot_locations.PNG) | 
|:--:| 
| *Figure 3: Alternative depot locations* |

### Vehicles

Each depot has an initial number of vehicles assigned. These vehicles remain in place
even if they are idle. We assume that vehicles move at an average speed of 40 miles/hour.

There is a fixed cost of $30 per day per vehicle which represents fixed annual 
costs expressed per day (lease or purchase payments, insurance, licenses, 
driver wages and benefits). 

### Routing

A vehicle stops at demand points and delivers demand units at each stop
with given time windows.

A vehicle can leave its depot after 6am and return until 5pm.
For a driver, a maximal total driving time is 11 hours. 

The maximum loading per vehicle is limited to a quantity of 60. 
However, a vehicle can reload each time it passes by its depot.

### Question

Design a distribution network by identifying location of depots and
the number of vehicles at each depot with daily delivery schedules with total minimum
cost consisting of depots fixed cost, 
fixed costs of the vehicles, and the variable costs of the routings subject to
demand, vehicle capacity, and time windows constraints.

Make sure that the vehicles can always be routed to serve the 2018 demand. 
However, evaluate the robustness of the solution by testing them on the 2019 demand data.

## Methodology

We first create mixed-integer formulation of the problem (LRPTW). 
Then, we use column generation to solve LRPTW. 

Rousseau provides a tutorial on column generation and branch-and-price 
for [vehicle routing problems](https://symposia.cirrelt.ca/system/documents/000/000/254/Rousseau_original.pdf?1464701234).

### Mathematical Formulation

In this section, we provide a general formulation of the LRPTW problem. The following are
definition of model inputs.

![_config.yml]({{ site.baseurl }}/images//MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/general_model_definitions.PNG)

We now provide general model formulation.

![_config.yml]({{ site.baseurl }}/images//MOPTA 2020 Case Competition Vehicle Fleet Sizing Positioning and Routing/general_model_formulation.PNG)

The formulation contains $|M| + |A||L| + |V||L|$ number of variables  
and $|N| + 2|L| + |N||L| + |A||L| + 2|V||L|$ constraints.

### Column Generation








