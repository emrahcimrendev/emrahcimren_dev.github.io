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
by the replenishment amount from the manufacturing plant.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/replenishment_event.png) | 
|:--:| 
| *Figure 3: Replenishment event* |

### Retailer Order Event

A flowchart for the retailer order event is provided in Figure 4. 
First, arrival time and amount is generated for an order. 
Then, inventory is decreased by this amount.
Finally, the time of the
next order is scheduled into the event list. Note that this is the place where the
inventory level might become negative.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/retailer_order_event.PNG) | 
|:--:| 
| *Figure 4: Retailer order event* |

### Inventory Evaluation Event

Inventory evaluation event happened at the beginning of each time period
illustrated in Figure 5. 
If the inventory level $$I_{kt}$$ at the time $$t$$ is at least $$s_k$$, 
then no order is placed, and nothing is done except to
schedule the next evaluation into the event list. 
On the other hand, if $$I_{kt}\le s_k$$, then we
place an order for $$Z_{kt}=S_k-I_{kt}$$ items. 
Finally, we schedule the next inventory evaluation event.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/inventory_evaluation_event.PNG) | 
|:--:| 
| *Figure 5: Inventory evaluation event* |

## Monte Carlo Simulation

We use Monte Carlo simulation framework for the inventory simulation. 
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
all trials into the probability distribution of possible outcomes

## Kolmogorov-Smirnov Goodness-of-Fit Test

Second step in Monte Carlo simulation is
calculating probability distribution for each stochastic component in
the problem.
The Kolmogorov-Smirnov (K-S) test is used to decide 
if a sample comes from a population with a specific distribution.

An attractive feature of this test is that the distribution of the K-S test statistic itself does not depend on the underlying cumulative distribution function being tested. 
Another advantage is that it is an exact test. 

In addition to these advantages, the K-S test has several important limitations. 
It only applies to continuous distributions.
It tends to be more sensitive near the center of the distribution than at the tails.
Perhaps the most serious limitation is that the distribution must be fully specified.

The K-S test performs a test of the distribution 
$$F(x)$$ of an observed random variable against a given distribution $$G(x)$$. 

Under the null hypothesis, the two distributions are identical, $$F(x)=G(x)$$. 
The alternative hypothesis can be either ‘two-sided’, ‘less’ or ‘greater’. 

Let $$\alpha$$ be the level of significance. If $$p-value < \alpha$$, 
then it is very probable that the two distributions are different
and reject the null hypothesis.

We use the following Python function to run the K-S test
for all given distribution names as follows. 

{% highlight python %}
distribution_names = ['weibull_min', 'norm', 'weibull_max', 'beta', 
                      'invgauss', 'uniform', 'gamma', 'expon', 
                      'lognorm', 'pearson3', 'triang']
{% endhighlight %}

In the function, distribution_data is the data to be tested to see if
it is identical to the distribution provided by distribution names.
Function outputs the best distribution.

{% highlight python %}
from scipy.stats import *

def fit_distribution_using_ks(distribution_names, 
                              distribution_data):
    
    """
    Function to check if data fits to the given distributions

    Args:
        distribution_names (str): List of distribution names.
        distribution_data (float): Data set to be fitted.

    Returns:
        best distribution, all distributions
    """
    
    distribution = {}
    pvalues = {}
    for distribution_name in distribution_names:
        
        #print('Fitting {}'.format(distribution_name))
        
        args=eval('{}.fit(distribution_data)'.format(distribution_name))
        fitted_data=eval('{}.rvs(*args, size=100000)'.format(distribution_name))

        D, p_value = ks_2samp(distribution_data, fitted_data)
            
        distribution[distribution_name] = {
            'name': distribution_name,
            'p_value': p_value,
            'args': args,
            'fitted data with args': fitted_data
        } 
        
        pvalues[distribution_name] = p_value
    
    pvalues_sorted = dict(sorted(pvalues.items(), key=lambda x: x[1], reverse=True))
    distribution_with_max_p_value = next(iter(pvalues_sorted))
    
    print('Best distribution {}'.format(distribution_with_max_p_value))
    
    return distribution[distribution_with_max_p_value], distribution, pvalues_sorted
{% endhighlight %}

We apply fit_distribution_using_ks for all products and data types using the following
Python function.

{% highlight python %}
def fit_simulation_data(products, distribution_names, distribution_data_by_product, column_name):
    """
    Function to fit simulation data

    Args:
        products (str): List of products.
        distribution_names (str): List of distribution names.
        distribution_data_by_product (float): Data set to be fitted.
        column_name (str): Column name to be fitted.

    Returns:
        best distribution, p values
    """

    best_distribution_by_product = {}
    pvalues_by_product = {}

    for product in products:

        print('Fitting for {}'.format(product))

        distribution_data = distribution_data_by_product[distribution_data_by_product['Product']==product][column_name].to_list()
        best_distribution, all_distributions, pvalues = fit_distribution_using_ks(distribution_names, distribution_data)
        best_distribution_by_product[product] = best_distribution

        best_distribution_name = best_distribution_by_product[product]['name']
         
        pvalues_by_product[product] = pvalues
        
    return best_distribution_by_product, pvalues_by_product
{% endhighlight %}

## Calculating Probability Distributions

Order arrival time, order quantity, and manufacturing lead times are stochastic. 
In this section, we calculate probability
distributions for order arrivals, order quantity, and 
manufacturing lead times.

We analyze PCC's historical data for 200 days. 
Figure 6 illustrates historical orders arrivals with quantity for
the two products. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Orders Dash.png) | 
|:--:| 
| *Figure 6: Orders* |

### Interarrival Time Distribution

The interarrival time is the time between each arrival of an order and the next. 
Figure 7 illustrates interarrival times for both dark and milk chocolate bars.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/interarrival_times.png) | 
|:--:| 
| *Figure 7: Interarrival Times* |

Figure 8 illustrates interarrival time distribution for both products. 

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/intearrival_time_distribution.png) | 
|:--:| 
| *Figure 8: Interarrival Time Distribution* |

The coefficient of variation (CoV) is the ratio of the standard deviation to the mean. 
The higher CoV, 
the greater the level of dispersion around the mean. 
The lower the value of CoV, the more precise the estimate. 

The following table illustrates descriptive statistics for 
interarrival times. Dark chocolate is ordered every 6.8 
days and milk chocolate is ordered every 6.3 
days in average.

| Product | Average Interarrival Time Days | Interarrival Time Standard Dev. Days | Interarrival Time CoV | 
|-------|-------|-------|-------|
| 3.5oz Milk Chocolate | 6.3 | 4.5|	0.70 |
| 3.5oz Dark Chocolate | 6.8 | 4.4| 0.64 |

The following Python code calculates the distribution fitting for
interarrival times. 

{% highlight python %}
products = interarrivals['Product'].unique()
distribution_data_by_product = interarrivals.copy()
best_distribution_by_prKoduct, pvalues_by_product = fit_simulation_data(products, 
                                                                       distribution_names, 
                                                                       distribution_data_by_product,
                                                                      'Interarival Times in Days') 
{% endhighlight %}

Figure 9 illustrates pvalues for the distributions we test with the fit_simulation_data function.

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/Interarival Times in Days PValues.png) | 
|:--:| 
| *Figure 9: Interarrival time distribution fitting* |

Inverse Gaussian distribution fits the best to both products. 
Figure 

### Order Amount




### Lead Time





We provide descriptive statistics on orders and lead times 
from historical data from last 200 days.
 
 
 
 
 
 










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



| Product | Total Number of Days | Average Order Units | Order Units Standard Dev. | Order Units CoV |
|-------|-------|--------|--------|
| 3.5oz Dark Chocolate | 200 | 27,318 | 15,837 | 0.57 |
| 3.5oz Milk Chocolate | 200 | 47,837 | 45,426 | 0.94 |

Milk chocolate is a high volume product with 47,837 average order size. 
Similarly, dark chocolate has an average order size of 27,318.


In our case, both products have high CoV (greater than 0.5, 50%)
which can affect the forecast quality.

There is differences in the demand distributions for milk and dark chocolates 
(see Figure 4).

| ![_config.yml]({{ site.baseurl }}/images/Multi-Period Inventory Planning with Stochastic Demand Using Python/demand_distribution.png) | 
|:--:| 
| *Figure 4: Demand Distribution* 

As a result, the output of the model is a probability distribution 
of the desired quantity.




### 






