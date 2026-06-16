---
layout: post
title: "Unsupervised Learning in Python"
date: 2025-12-07 14:24:00
description: Incomplete!
tags: machine-learning
categories: Statistics

title: ""
date: 2025-12-26
layout: single
mathjax: true
---

My friend asked me to help on his thesis work, so I thought this was the perfect opportunity to get familiar with actually using Python to run my machine learning things (aka do a clustering analysis) and understand the math behind this method.

Unsupervised learning is attempting prediction without an outcome variable. What usually happens is that based on a set of characteristics, different groups of data start to emerge. A key component to recognize is that different runs will give different results. I will explain why this is the case by exploring algorithm. Right now, I will discuss k-means clustering because that's what I have to do for my friend. Before starting anything, I needed to install packages in my terminal. 

## Basic Data Preparation in Python

I am using the GeeksforGeeks tutorial for EDA in python. I am copying down what was relevant + worked for me, and what I needed to fix some issues.

```python
pip install pandas
```

I'm using PyCharm, and I don't know how to view data like you can in RStudio (I gave up trying to code in python there). Thus, I'm kind of forced to look into data with some code. The things that I came up with are the following (used pandas; use snake_case): 

```python
print(df.head()) #gets the first few observations to print
df.shape #gets the number of columns and rows
df.info() #gets important information about variable types
print(df.columns) #gets our variable names
```

A bit more detailed summaries include the equivalent function for summary

```python
df.describe().T
```

For data cleaning (aka what the relevant features are for clustering analysis), this is the code I have:

```python
my_data = my_data[['id', 'author_id']] #keep track of everything
```

For data visualizations, this is the code I used, along with extensive comments to add customization!

```python
# Also using NumPy and Pandas
import matplotlib.pyplot as plt
import seaborn as sns
```

Visualization 1: Multiple distributions 

```python
#INSERT CODE HERE
```

Visualization 2: Correlation plot for Multicollinearity

```python
#INSERT CODE HERE
```

Saving these visualizations as a jpg

```python
#INSERT CODE HERE
```

## K-means algorithm

The steps of k-means clustering are as follows
1) Randomly select k points in the dataset to be our cluster centers
2) Calculate the distance of each datapoint to the cluster center (using Euclidean distance metric in R^n)
3) Assign any given datapoint to the cluster it is closest to (you can formalize this in set theory, but it should be quite self-explanatory)
4) From each cluster, we re-compute the center by taking the mean of the datapoints
5) Repeat steps 2-4 until we have a stopping criteria for convergence

The random selection in step 1 creates different results for each run. That is why we need to set our seed so our results are reproducible, even if our data does not change! The 5th step has multiple criteria, from the data not changing clusters, the cluster center staying the same, or if we reach the max number of iterations. Some other criteria are to choose a threshold change of the **within-cluster sum of squares** (WCSS) between the $t$th iteration and the $(t-1)$th iteration. This stuff is easily google-able for a formal treatment, so I'm forgoing some notation.

To apply this in python, the code is 

```python
#INSERT CODE HERE
```

## What kind and number of features are we allowed to have?

Any simulation studies that give a good number?

## Selecting the optimal number of clusters

A popular method (and the one I learned) is the elbow plot to see how much variance we explain. The more clusters, the more variance we explain, but the harder it becomes to interpret. 

```python
#INSERT CODE HERE
```