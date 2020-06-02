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

After PCC receives orders from retailer, 
PCC prepares (picks) orders and retailer picks them
from PCC's warehouse. PCC and retailer has a 100% service level agreement
which is an order should be satisfied within 7 days. 
Therefore, PCC carries safety stock to be able to meet retailers service
level requirement. 

PCC receives a purchase plan from retailer and prepares a production plan
by product and week. PCC does not hold inventory at the manufacturing
facility and ships products immediately to its warehouse.

Both milk and dark chocolates are sold to retailer at $2.25 per unit and their
profit margins are 56% and 68%, respectively.

PCC's objective is to identify production and inventory management 
policy to maximize their
profit.

## Historical Data

We provide descriptive statistics on orders and lead times 
from historical data from last 200 days.

Figure 2 illustrates historical orders (200 days) for
the two products. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Orders Dash.png) | 
|:--:| 
| *Figure 2: Orders* |

| Product | Total Number of Days | Average Order Units | Order Units Standard Dev. | Order Units CoV |
|-------|-------|--------|--------|
| 3.5oz Dark Chocolate | 200 | 27,318 | 15,837 | 0.57 |
| 3.5oz Milk Chocolate | 200 | 47,837 | 45,426 | 0.94 |

Milk chocolate is a high volume product with 47,837 average order size. 
Similarly, dark chocolate has an average order size of 27,318.

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

The inter arrival time is the time between each arrival of an order and the next. 
Figure 4 illustrates interarrival times for both products.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/interarrival_times.png) | 
|:--:| 
| *Figure 4: Interarrival Times* |

Figure 5 illustrates interarrival time distribution for both products. 
Both chocolates are ordered average of every six days.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/intearrival_time_distribution.png) | 
|:--:| 
| *Figure 5: Interarrival Time Distribution* |

| Product | Average Interarrival Time Days | Interarrival Time Standard Dev. Days | Interarrival Time CoV | 
|-------|-------|-------|-------|
| 3.5oz Dark Chocolate | 6.8 | 4.4| 0.64 |
| 3.5oz Milk Chocolate | 6.3 | 4.5|	0.70 |

## Monte Carlo Simulation

Monte Carlo simulation involves using random number generators 
to simulate random effects. We follow the steps below to build
the Monte Carlo Simulation model.

1. Determine deterministic and stochastic components of the problem.

2. Calculate probability distribution for each stochastic component.

3. Generate random number for each of the stochastic component
from the corresponding probability distribution.

4. Computer the desired quantity using the generated
random value of the stochastic components. 

5. Steps 2 and 3 are repeated a large number of times 
to simulate the model which results in a large number of 
separate and independent values, each representing a possible outcome of the model.

6. After performing all trials, we assemble the result of 
all trials into the probability distribution of possible outcomes. 

As a result, the output of the model is a probability distribution 
of the desired quantity.

## Simulation Design

We now develop a Monte Carlo simulation model for the inventory management problem.
We first define model inputs. 

