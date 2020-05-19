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

## Historical Data

We provide descriptive statistics on orders and lead times 
from historical data from last 200 days.

### Orders and Lead Time

Figure 2 illustrates historical orders (200 days) for
the two products. Note that retailer does not order everyday. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Orders Dash.png) | 
|:--:| 
| *Figure 2: Orders* |

Let $$p$$ be probability of placing an order on any given day which
is calculated by dividing the number of 
orders last year by the number of working days (see the following table).

| Product | Total Number of Days | Total Number of Days with Nonzero Order | $$p$$ | Average Order Units | Standard Dev. | CoV |
|-------|-------|--------|--------|--------|--------|
| 3.5oz Milk Chocolate | 200 | 30 | 0.15 | 47,837 | 45,426 | 0.94 |
| 3.5oz Dark Chocolate | 200 | 24 | 0.14 | 27,318 | 15,837 | 0.57 |

Milk chocolate is a high volume product 
that gets ordered every week and the mean order size is 47,837. 
Similarly, dark chocolate is ordered every week with an order size of 27,318.

The coefficient of variation (CoV) is the ratio of the standard deviation to the mean. 
The higher CoV, 
the greater the level of dispersion around the mean. 
The lower the value of CoV, the more precise the estimate. 
In our case, both products have high CoV (greater than 0.5, 50%)
which can affect the forecast quality.

There is differences in the demand distributions for milk and dark chocolates 
(see Figure 3).

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/demand_distribution.png) | 
|:--:| 
| *Figure 3: Demand Distribution* |

TODO: Fit those to distributions.

### Lead Time

Figure 4 illustrates lead time distributions. 
Lead time for both products is around 19 days. 
Note that lead time is the time between order place time
and retailer pick time.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/lead_time_distribution.png) | 
|:--:| 
| *Figure 4: Lead Time Distribution* |

| Product | Lead Time Days | Standard Dev. | CoV | 
|-------|-------|-------|-------|
| 3.5oz Milk Chocolate | 18.7 | 6.9 | 0.48 |
| 3.5oz Dark Chocolate | 19.3 | 9.4 | 0.37 |


### Onhand Inventory



