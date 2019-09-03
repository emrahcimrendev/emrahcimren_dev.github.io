---
title: "Predicting Short Term Trucking Rates with Random Forests"
categories:
  - transportation rates
tags:
  - transportation rates
  - trucking rates
  - FTL
  - LTL
  - prediction
  - machine learning
  - random forests
  - python
  - plot.ly
--- 

In this post, we present a random forest model to predict short term trucking rates using Python.

![image-center](/images/dry_van.jpg){: .align-center}

Transportation rates are driven by different modes of transportation (air, road, rail, and ocean). 
In this work, we focus on trucking related transportation modes, Full Truckload (FTL) and Less than Truckload (LTL).

**Full Truckload (FTL):** An entire truck is used for transportation. 
In the FTL market, truck delivers to destination directly from shipper’s location using a dedicated truck. 
The rate is the same for the use of the full truck whether the truck is 100% full or 
25% full and may be different depending on where the shipment starts and ends. 
Capacity of a truck can be measured in total weight, total cube, or total number of pallets. 
Various truck types are used such as regular dry van, refrigerated, flatbed, tanker, and 48- and 53-foot trailers. 
An FTL carrier may hold 45,000 pounds of product.

**Less than Truckload (LTL):** Companies use LTL when they have a small load to ship to a destination. 
In this case, hiring an entire truck to make the delivery is not economical. 
Trucking company picks up the load, combines it with other companies’ pickup or deliveries, 
and makes the trip to complete a route of deliveries to customer locations. An LTL carrier may 
hold up to 15,000 pounds of product.

Different trailer types carry different products. 
The main types include *dry van*, *flatbed*, *refrigerated/temperature-controlled trailer* and *tank*. 
*Dry van* is the most popular one. The *flatbed trailer* does not have a side wall or ceiling and used to carry 
construction materials or large machinery. Food and medicines are normally hauled by *temperature-controlled*. 
*Tanks* are used to haul refined oil products or chemicals in the liquid form.

Two types of services exists to cover freight transportation requirements, through long-term contracts or 
on the spot market. Contract carriers is used most frequently. The contract is typically a one-year commitment, 
which consists of origin/destination, service requirement, volume and any other factors that affect the price. 
Spot market is used to obtain a rate when there is no availability in the contract market (lane does not exists or 
rate is not accepted).  

## Problem

Consider a network of customers and distribution centers (see Figure 1). 
Products are delivered to each customer from distribution centers using trucks. 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_customer_warehouse_network.jpg) | 
|:--:| 
| *Figure 1: Distribution centers and customers* |

Our objective is to determine FTL and LTL rates for each distribution center to each customer. 

## Data and Insights

The dataset consists of the European long-haul Truckload data, including
mode, distance covered per shipment, average shipment weight per truck, average shipment cost per truck,
and trailer type (see Figure 2). 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data.jpg) | 
|:--:| 
| *Figure 2: Transportation data set* |

Figure 3 shows the transportation data profiling by distance miles, average shipment per truck, 
transportation cost per truck, mode, and trailer type. We provide statistics by number of data points, 
minimum value, 25% quantile, mean, median, 75% quantile, and maximum. The following Python code 
can be used to generate those statistics.

{% highlight python %}

def quantile_25pct(x):
    return x.quantile(0.25)

def quantile_75pct(x):
    return x.quantile(0.75)

trans_costs_melted.groupby(['VARIABLE', 'MODE', 'TRAILER_TYPE'], as_index=False).agg({'VALUE': ['count', 'min', quantile_25pct, 'mean', 'median', quantile_75pct, 'max']})

{% endhighlight %}

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_profiling.jpg) | 
|:--:| 
| *Figure 3: Transportation data set profile* |

We now analyze relationships between distance miles, average shipment per truck, 
transportation cost per truck for each mode and trailer. 

Figure 4 shows relationships for FTL.

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter.png) | 
|:--:| 
| *Figure 4: FTL profile* |


