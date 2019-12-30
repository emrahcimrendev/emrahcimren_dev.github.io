---
title: "Customer Segmentation For An Online Apparel Retailer"
categories:
  - data science
tags:
  - data science
  - operations research
  - retail planning
  - customer segmentation
--- 

In this post, we provide a customer segmentation application for an online apparel retailer. 
Our objective is to divide the customer groups into sub-groups
where each member in each group is very similar
by some measure to each other and very dissimilar to members
in all other groups.

![_config.yml]({{ site.baseurl }}/images/2019-12-11-Customer Segmentation/ecommerce.png)

Segmentation is the part of the assortment planning process where we decide *right products*
at the *right quantity* at the *right price* in the *right place* at the *right time* (see Figure 1). We classify customers into groups (segments) based on 
observed or inferred characteristics, behaviors, and preferences.

| ![_config.yml]({{ site.baseurl }}/images/2019-12-11-Customer Segmentation/assortment planning.PNG) |
|:--:|
| *Figure 1: Assortment planning process* |

## Analysis

The objective of this work is first to understand 

- Who is buying the product, 
- How they buy,
- What attributes they value (and don’t value),
- What price they are willing to pay.

Then, we classify customers into groups based on these characteristics.

## Problem

We consider an online woman apparel retailer, called XYZ. XYZ has four selling seasons; fall (FA), winter (WA), spring (SP), and summer (SU). At the beginning of each season, they offer an assortment consisting of new products and carryover products from the previous seasons. 



