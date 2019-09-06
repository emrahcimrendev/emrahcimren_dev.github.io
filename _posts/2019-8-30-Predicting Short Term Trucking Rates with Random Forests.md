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

## Analysis Steps

The following are the steps in the analysis. 

1. Data set cleanup (remove outliers)
2. Feature engineering
3. Create train and test data
4. Develop model baseline
5. Fit model
6. Put model into production
7. Use model for prediction

## 1. Data Set Cleanup

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

We fist analyze relationships between distance miles, shipment weight per truck, 
transportation cost per truck, and transportation cost per truck per mile 
for each mode and trailer. Then, we identify and remove outliers
from the data set. [The interquartile range (IQR)](https://en.wikipedia.org/wiki/Interquartile_range) 
rule is applied to be able to detect outliers. 

The following python functions
are used to generate plots and to detect outliers.

{% highlight python %}
import seaborn as sns

def plot_scatter(figure_data, x_axis_column, y_axis_column, legend_column, fig_width, fig_height, font_scale, grid_column, grid_row=None):
    sns.set(font_scale=font_scale)
    sns.set_style("white")
    if grid_row is None:
        g = sns.FacetGrid(figure_data, col=grid_column, hue=legend_column)
    else:
        g = sns.FacetGrid(figure_data, col=grid_column, row=grid_row, hue=legend_column)
    g = (g.map(sns.scatterplot, x_axis_column, y_axis_column, edgecolor="w").add_legend())
    g.fig.set_size_inches(fig_width, fig_height)

{% endhighlight %}

{% highlight python %}

import numpy as np

def detect_outlier(data):
    
    Q1 = data.quantile(0.25)
    Q3 = data.quantile(0.75)
    IQR = Q3 - Q1
    
    outliers = np.full(len(data), False)
    outliers[data < (Q1 - 1.5 * IQR)] = True
    outliers[data > (Q3 + 1.5 * IQR)] = True
    
    return outliers

{% endhighlight %}

Figure 4 shows relationships for FTL. We observe that there is a linear relationship between distance traveled
and transportation cost per truck. It means the FTL rate is the same whether the truck is 100% full or 25% full
and changes depending on where the shipment starts and ends. We also see that FTL shipments in temperature controlled
trailers are $0.7 more expensive than the shipments in dry van (Figure 3). 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_ftl.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ftl.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_weight_cost_ftl.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_weight_distance_ftl.png) | 
|:--:| 
| *Figure 4: FTL profiles* |

There are FTL shipments where shipment weight per truck is less than 10,000 LBS. We also identified points with a large 
transportation cost per truck per mile using the IQR rule. 
We consider those points as outliers and remove from the data set (see Figure 5). 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ftl_outliers.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ftl_outliers_cleaned.png) | 
|:--:| 
| *Figure 5: FTL outliers* |

Figure 6 shows relationships between distance miles, shipment weight per truck,
transportation cost per truck, and transportation cost per truck per mile for LTL. 
There is no clear distinct relationship between any of those variables. 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_ltl.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ltl.png) |
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_weight_cost_ltl.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_weight_distance_ltl.png) | 
|:--:| 
| *Figure 6: LTL profiles* |

We treat shipments
more than 5,000 mile distance as outliers since firms prefer FTL shipments for long distance
since it is more economical comparing to LTL.  
After removing outlier points, we see a clear relationship between distance miles and transportation 
cost per truck for LTL (Figure 7).

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_no_outliers_ltl.png) | 
|:--:| 
| *Figure 7: LTL outliers* |

Similar in FTL, there exists shipments with a large transportation cost per mile. We use the IQR rule to detect 
those outliers (see Figure 8). 

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ltl_outliers.png) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_input_data_flt_scatter_distance_cost_per_mile_ltl_outliers_cleaned.png) | 
|:--:| 
| *Figure 8: LTL outliers* |

## 2. Feature Engineering

Feature engineering is the process of using domain knowledge of the data to 
create features that make machine learning algorithms work.

We use one-hot encoding to convert categorical data into to a numerical format without losing any information.
Figure 9 shows how FTL data is transformed as an example. We also provide Python code below for one-hot encoding.

| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_ftl_model_data.jpg) | 
| ![_config.yml]({{ site.baseurl }}/images/trans_rate_random_forest_ftl_one_hot_model_data.jpg) | 
|:--:| 
| *Figure 9: FTL one-hot encoding* |

{% highlight python %}

import pandas as pd

trans_cost_ftl_with_one_hot = pd.get_dummies(trans_cost_ftl.drop(['MODE', 'OUTLIER', 'TRANS_COST_PER_TRUCK_USD_PER_MILE'], 1))

{% endhighlight %}


## 3. Create Train and Test Data

At this step, we split data into training and testing sets to evaluate performance of the model. 
We randomly select 75% of the data for training and 25% of data for testing using the following Python function.

{% highlight python %}

from sklearn.model_selection import train_test_split

def create_train_test_splits(labels_data, features_data, test_size):
    
    labels = np.array(labels_data)
    features_list = list(features_data.columns)
    features = np.array(features_data)

    train_features, test_features, train_labels, test_labels = train_test_split(features, labels, test_size = test_size, random_state = 42)

    return train_features, test_features, train_labels, test_labels

{% endhighlight %}

## 4. Develop Model Baseline

Before making predictions, we need to develop a model baseline to level set performance of the model. 
If the model can not improve the baseline, we need to try a new model. 

In our problem, the baseline prediction is the average transportation cost 
per mile by mode and trailer type. We calculate Mean Absolute Error (MAE), Mean Percentage Error (MAPE),
and accuracy as performance metrics. 

We now define MAE, MAPE, and accuracy. Let $$y_i$$ be the prediction and $$x_i$$ be the actual value for $$i=1, \dots, n$$. 

$$MAE = \frac{1}{n}\sum_{i=1}^n|y_i-x_i|$$

$$MAPE = 100\frac{1}{n}\sum_{i=1}^n\frac{|y_i-x_i|}{x_i}$$

$$Accuracy = 100 - MAPE $$

The following Python code is used to calculate baseline and
accuracy metrics.

{% highlight python %} 
def apply_baseline_and_calculate_performance(trans_cost_with_one_hot, test_features, features_list, test_labels):
    
    baseline = trans_cost_with_one_hot.groupby(['MODE', 'TRAILER_TYPE_DRY VAN', 'TRAILER_TYPE_TEMPERATURE CONTROLLED'], as_index=False).agg({'TRANS_COST_PER_TRUCK_USD': 'sum', 'DISTANCE_MILES': 'sum'})
    baseline['TRANS_COST_PER_TRUCK_USD_PER_MILE'] = baseline['TRANS_COST_PER_TRUCK_USD'] / baseline['DISTANCE_MILES']
    baseline.drop(['DISTANCE_MILES'], 1, inplace=True)
    
    baseline_costs = pd.DataFrame(test_features)
    baseline_costs.columns = features_list
    
    baseline_costs = baseline_costs.merge(baseline, how='left', on=['TRAILER_TYPE_DRY VAN', 'TRAILER_TYPE_TEMPERATURE CONTROLLED'])
    baseline_costs['BASELINE_TRANS_COST_PER_TRUCK_USD'] = baseline_costs['DISTANCE_MILES'] * baseline_costs['TRANS_COST_PER_TRUCK_USD_PER_MILE']
    baseline_costs['ACTUAL_TRANS_COST_PER_TRUCK_USD'] = test_labels
    baseline_costs['ABSOLUTE_ERROR'] = abs(baseline_costs['BASELINE_TRANS_COST_PER_TRUCK_USD'] - baseline_costs['ACTUAL_TRANS_COST_PER_TRUCK_USD'])
    baseline_costs['MAPE_PCT'] = 100 * (baseline_costs['ABSOLUTE_ERROR'] / baseline_costs['ACTUAL_TRANS_COST_PER_TRUCK_USD'])
    
    mean_absolute_error = round(np.mean(baseline_costs['ABSOLUTE_ERROR']), 2)
    accuracy = round(100 - np.mean(baseline_costs['MAPE_PCT']), 2)
    
    return baseline_costs, mean_absolute_error, accuracy
{% endhighlight %}

The performance metrics for FTL and LTL is as follows. 
Those provide us goals which is model performance should be better than baseline performance.

<center>
| | FTL | LTL |
|-------|-------|--------|
| MAE | 434.46 | 352.96 | 
| MAPE | 12.38% | 66.9% | 
| Accuracy | 87.62% | 33.1% |
</center>




