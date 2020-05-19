---
title: "Multi-Period Inventory Planning with Stochastic Demand Using Python"
categories:
  - operations research
tags:
  - inventory planning
  - stochastic demand
  - multi-period
  - python
--- 

In this post, we consider a supply chain were product demand is stochastic over time. 
We determine plans for inventory management using Monte Carlo Simulation in
Python.
 
![_config.yml]({{ site.baseurl }}/images//Multi-Period Inventory Planning with Stochastic Demand Using Python/chocolate_main.jpg)

## Portland Chocolate Company

Portland Chocolate Company (PCC) is a craft chocolate 
supplier offering two 3.5oz chocolate bars; milk, and dark.
PCC has a manufacturing facility and a warehouse where a retailer picks up 
their orders and
transfers to their regional warehouse (see Figure 1). 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/supply_chain.PNG) | 
|:--:| 
| *Figure 1: Supply chain* |

### Orders and Lead Time

Figure 2 illustrates historical orders (200 days) for
the two products. Note that retailer does not order everyday. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Orders Dash.png) | 
|:--:| 
| *Figure 2: Orders* |

Let $$p$$ be probability of placing an order on any given day which
is calculated by dividing the number of 
orders last year by the number of working days (see the following table).

| Product | Total Number of Days | Total Number of Days with Nonzero Order | $$p$$ | Average Order Units | 
|-------|-------|--------|--------|--------|
| 3.5oz Milk Chocolate | 200 | 30 | 0.15 | 47837 |
| 3.5oz Dark Chocolate | 200 | 24 | 0.14 | 27318 |

Demand Distribution for each Product. 

There is differences in the demands for milk and dark chocolates. 
Figure 3 illustrates demand distribution.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/demand_distribution.png) | 
|:--:| 
| *Figure 3: Demand Distribution* |

Milk chocolate is a high volume product 
that gets ordered every week and the mean order size is 47837. 
Similarly, dark chocolate is ordered every week with an order size of 27318.

### Onhand Inventory



