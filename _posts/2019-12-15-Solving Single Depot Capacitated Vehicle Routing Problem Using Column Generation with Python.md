---
title: "Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python"
categories:
  - operations research
tags:
  - single depot capacitated vehicle routing problem with time windows
  - VRPTW
  - optimization
  - column generation
  - python
--- 

Vehicle routing problem (VRP) is identifying the optimal set of routes for a set of 
vehicles to travel in order to deliver to a given 
set of customers.  When vehicles have limited carrying capacity and 
customers have time windows within which the deliveries must be made, p
roblem becomes capacitated vehicle routing problem with time windows (CVRPTW). 
In this post, we will discuss how to tackle CVRPTW using Python and get a fast and 
robust solution.

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/truck_main.jpg)

## Problem 

We consider a pizza restaurant chain, **PPizza**, in the Los Angeles, CA area with 34 stores. 
Each store operates from 10am to 1am everyday. **PPizza** offers three pizza sizes
(small, medium, large) with various toppings and soft drinks. Pizzas are prepared using fresh
ingredients and baked in store on demand. 

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/pizza.PNG)

**PPizza** forecasts weekly demand of  food items for each store and identifies required ingredients
and soft drinks. Fresh ingredients are delivered to stores daily from the main depot once a day. 
Soft drinks are delivered and replenished by suppliers directly. 

Figure 1 shows location of stores and the depot. Each store has time windows between 
9am and 3pm where delivery needs to be
done within. Unloading time varies by store depending on location and parking availability. 

| ![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/store_depot_map.PNG) | 
|:--:| 
| *Figure 1: PPizza depot and stores* |

Trucks can leave from the depot at 6am and need to return the depot by 5pm. 
Each truck can be used once and has a limited capacity.
We want to identify the truck operating schedules 
to be able to deliver
fresh ingredients to each store with given time windows by minimizing the total cost. 

## Analysis

We first formulate the problem as a mixed integer program. Then, we solve the problem various
number of available trucks.

We also develop column generation based algorithm to solve the problem. 
Rousseau provides a tutorial on column generation and branch-and-price 
for [vehicle routing problems](https://symposia.cirrelt.ca/system/documents/000/000/254/Rousseau_original.pdf?1464701234).

### General Formulation

We develop a mixed integer model for the **PPizza** delivery problem as follows.

![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/general_model_inputs.PNG)



![_config.yml]({{ site.baseurl }}/images//2019-12-15-Solving Single Depot Capacitated Vehicle Routing Problem Using Column Generation with Python/general_model_formulation.PNG)

We solve the mixed integer model using Python with PuLp. The following is the 
implementation.  

{% highlight python %}
import pandas as pd
import timeit
import time
from threading import Thread, currentThread
import queue
from cvrptw_optimization import single_depot_general_model_pulp as sm

# Read input data
customers = pd.read_pickle(r'data/customers.pkl')
depots = pd.read_pickle(r'data/depots.pkl')
transportation_matrix= pd.read_pickle(r'data/transportation_matrix.pkl')
vehicles = pd.read_pickle(r'data/vehicles.pkl')

# Model parameters
bigm_input=transportation_matrix.DRIVE_MINUTES.max()*20
solver_time_limit_minutes_input = 480

# Calculate range for vehicles
min_vehicles = int(round(customers.DEMAND.sum()/60)+2)
max_vehicles = len(vehicles)+1

# Define functions
def run_single_depot_general_model(vehicle,
                                   depots, 
                                   customers, 
                                   transportation_matrix, 
                                   vehicles,
                                   bigm_input,
                                   solver_time_limit_minutes_input):
    """
    Run general model
    """
    start = timeit.default_timer()
    vehicles_sub = vehicles.head(int(vehicle))
    print(len(vehicles_sub))
    solution_objective, solution_paths = sm.run_single_depot_general_model(depots, 
                                                                           customers, 
                                                                           transportation_matrix, 
                                                                           vehicles_sub,
                                                                           bigm=bigm_input,
                                                                           solver_time_limit_minutes=solver_time_limit_minutes_input)
    solution_paths['OBJECTIVE'] = solution_objective
    solution_paths['NUMBER_OF_VEHICLES'] = vehicle
    stop = timeit.default_timer()
    solution_paths['MODEL_RUN_TIME_MINUTES'] = (stop - start)*60
    solution_paths.to_csv(r'general model solutions/{}_.csv'.format(str(vehicle)), index=False)
    return 'ok'
    
q = queue.Queue()
def worker():
    """
    Worker function to process vehicles from a queue (q).
    """
    while True:
        print("Start thread worker.")
        vehicle = q.get()
        print("Starting vehicle: {}".format(str(vehicle)))
        run_single_depot_general_model(vehicle,
                                       depots, 
                                       customers, 
                                       transportation_matrix, 
                                       vehicles,
                                       bigm_input,
                                       solver_time_limit_minutes_input)
        print("Finishing vehicle: {}".format(str(vehicle)))
        q.task_done()
        print("End thread worker")
        
def create_and_process_queue(vehicle_range_list, max_num_threads):
    """
    Creates a queue of vehicles to process. Creates threads to process the queue. 
    The number of threads are limited by max_num_threads.
    """
    # add the vehicles to the queue
    for vehicle in vehicle_range_list:
        print("Adding vehicle {} to queue".format(str(vehicle)))
        q.put(vehicle)
    print("Create threads")
    for i in range(max_num_threads):
        time.sleep(10)
        t = Thread(target=worker)
        t.daemon = True
        t.start()
    q.join()  # blocks until all queue items have been processed
    
min_vehicles = int(round(customers.DEMAND.sum()/60)+2)
max_vehicles = len(vehicles)+1

vehicle_range_list = []
for vehicle in range(min_vehicles, max_vehicles):
    vehicle_range_list.append(vehicle)
vehicle_range_list.reverse()

create_and_process_queue(vehicle_range_list, 5)

{% endhighlight %}

You can install cvrptw_optimization package to your conda environment using the following code. 

```
pip install cimren-cvrptw-optimization
```



### Column Generation

We develop a column generation approach based on [Dantzig-Wolfe decomposition](https://pubsonline.informs.org/doi/abs/10.1287/opre.8.1.101). 
CVRPTW is decomposed into two problems, the master problem, and the subproblem 
to provide better bound when linear relaxation of the problem is solved.

#### Master Problem



#### Subproblem


### Implementation


### Analysis

### References
Desrochers, M., Lenstra, J.K., Savelsbergh, M.W.P., Soumis, F. (1988). 
Vehicle routing with time windows: Optimization and approximation. 
In: Golden, B.L., Assad, A.A. (Eds.), 
Vehicle Routing: Methods and Studies. North-Holland, Amsterdam, pp. 65–84.
