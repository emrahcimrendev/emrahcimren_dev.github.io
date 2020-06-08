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
PCC has a manufacturing facility and a warehouse in Portland, OR (see Figure 1).
PCC works with a retailer who sells PCC's chocolates in their stores in US.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/supply_chain.PNG) | 
|:--:| 
| *Figure 1: Supply chain* |

After PCC receives orders from retailer, 
first PCC prepares orders and 
then,
retailer picks them from the PCC's warehouse. 

PCC receives a purchase plan from retailer and prepares a production plan
by product and week. PCC does not hold inventory at the manufacturing
facility and ships products immediately to its warehouse.

PCC and retailer has a 100% service level agreement
which is an order should be satisfied within 7 days. 
Therefore, PCC carries safety stock to be able to meet retailers service
level requirement. 

When PCC inventory falls to a particular level, 
this triggers an action to replenish that particular inventory. 
In this case, PCC creates a manufacturing order
from the manufacturing plant for replenishment.

At the beginning of each day, PCC reviews the inventory level and
decides how many items to order from the manufacturing plant.
PCC's objective is to identify inventory management 
policy to maximize their
profit.

## Problem Formulation

We now define the inventory management problem formulation. 
Let $$P$$ be the set of products and $$T$$ be the set of time periods in days.
Let $$I_{kt}$$ be be the inventory level of product $$k \in P$$ at time $t\in T$, 
$$s_k$$ be the reorder point of product $$k \in P$$, and
$$S_k$$ be the minimum safety stock level of product $$k\in P$$ (see Figure 2). 
For all $$k\in P$$, 
PCC orders $$Z_{kt}$$ units of product $$k$$ from the manufacturing plant
at time $$t\in T$$ where

![_config.yml]({{ site.baseurl }}/images//Multi-Period Inventory Planning with Stochastic Demand Using Python/ordering_equation.PNG)

When PCC places an order from the manufacturing plant, 
the time required for it to arrive is called the manufacturing lead time.
Let $$\tau_{kt}$$ be the manfacturing lead time for the PCC order $$Z_{kt}$$.

This policy is called $$(s, S)$$ policy.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/inventory_policy.PNG) | 
|:--:| 
| *Figure 2: Inventory policy* |

Let $$D_{kt}$$ be the retailer order amount of product $$k\in P$$ at time $$t\in T$$.
If $$D_{kt}\le I_{kt}$$, then it is satisfied immediately.
If $$D_{kt} > I_{kt}$$, then the excess of order amount over supply ($$D_{kt}-I_{kt}$$) is backlogged and satisfied by future deliveries. 
In this case, the new inventory level is equal to the old inventory level minus the order amount, resulting in a negative inventory level. 

When an PCC order arrives from the manufacturing plant, 
it is first used to eliminate as much of the backlog (if any) as possible; the remainder of the order (if any) is added to the inventory.

For each PCC's order, $$K$$ amount of set up cost incurs. Also, there exists $$c_{k}$$ unit cost
for $$k\in K$$ when PCC orders from the manufacturing plant.

PCC incurs a holding cost of $$h$$ per item per time period held in (positive) inventory. 
The holding cost includes such costs as warehouse rental, insurance, taxes, and maintenance, as well as the opportunity cost of having capital tied up in inventory rather than invested elsewhere. 

Similarly, PCC incurs a backlog cost of $$b$$ per item per time period in backlog; this accounts for the cost of extra record keeping when a backlog exists, as well as loss of customers’ goodwill.

## Simulation Design

We have four types of events in the simulation as in the following table.

| **Event Name** | **Event Description** | **Event Type**|
|-------|-------|-------|
| Replenishment | Arrival of an order to the PCC's warehouse from the plant | 1 | 
| Retailer order | The retailer order from PCC | 2 | 
| End | End of simulation | 3 |
| Inventory evaluation | Inventory evaluation (possible ordering from the plant) at the beginning of each day | 4 |

We now describe the logic for event types 1, 2, and 4, which actually involve state changes.

### Replenishment Event

Figure 3 illustrates the replenishment event which increases the on hand inventory
by the replenishment amount.

<style type="text/css">
  p {
    .width-half_logo {width: 80%}
  }
</style>

 ![](/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/replenishment_event.PNG){: .align-right .width-half_logo}

| ![](/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/replenishment_event.PNG){: .align-right .width-half_logo}| 
|:--:| 
| *Figure 3: Replenishment event* |

### Retailer Order Event



| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/retailer_order_event.PNG) | 
|:--:| 
| *Figure 4: Retailer order event* |

### Inventory Evaluation Event


 
 
 
 
 
 
 
 
 










Inventory at the warehouse decreases as orders are picked by 
the retailer. 
When inventory reach at a level (called reorder point), PCC
orders product from the manufacturing plant. Warehouse receives the 
orders from the manufacturing plant and available inventory 
reaches to the maximum
level. 



Both milk and dark chocolates are sold to retailer at $2.25 per unit and their
profit margins are 56% and 68%, respectively.



When an order is
placed for the manufacturing plant, 
the time required for it to arrive called lead 


The following is the algorithm steps. 

1. For each product, determine order arrivals based on arrival distribution.

2. For each arrival, determine order quantity.

3. If there is available inventory, 
the replenish orders based on the incoming order sequence.

4. Monitor safety stock and re-order at the reordering point
if on hand inventory reaches at the reorder point.

We assume that there is manufacturing capacity for each product and inventory
hold at the warehouse is limited.

## Historical Data

We provide descriptive statistics on orders and lead times 
from historical data from last 200 days.

Figure 3 illustrates historical orders (200 days) for
the two products. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Orders Dash.png) | 
|:--:| 
| *Figure 3: Orders* |

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
(see Figure 4).

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/demand_distribution.png) | 
|:--:| 
| *Figure 4: Demand Distribution* |

The inter arrival time is the time between each arrival of an order and the next. 
Figure 5 illustrates interarrival times for both products.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/interarrival_times.png) | 
|:--:| 
| *Figure 5: Interarrival Times* |

Figure 6 illustrates interarrival time distribution for both products. 
Both chocolates are ordered average of every six days.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/intearrival_time_distribution.png) | 
|:--:| 
| *Figure 6: Interarrival Time Distribution* |

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




### 






