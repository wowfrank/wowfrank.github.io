---
layout: post
title: "Machine Learning Algorithms Advantages and Disadvantages Summary"
date: "2023-10-16 01:09:00 +0800"
description: "Machine Learning Algorithms Advantages and Disadvantages Summary" # (optional)
img: "2023-10-16-cover-image-ml-algorithms-summary.jpeg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Machine Learning', 'Algorithm']
categories: ['Machine Learning']
---

## Purpose

Take notes of the advantages and disadvantages of machine learning algorithms, it is very important to realize their weaknesses to implement proper algorithm for specific problems.

## Regression: Linear Regression

The goal of linear regression is to find the best-fit line that describes the relationship between the dependent and the independent variable.

Advantages:

- It works well for binary classification problems, where the output variable has only two possible values.
- It can handle both continuous and categorical input variables.

Disadvantages:

- Logistic Regression assumes that the relationship between the input variable and the output variable is linear, which may not be true in all cases.
- It can be sensitive to outliers or biased data, which affects predictions.

## Regression: Polynomial Regression

Polynomial regression is an algorithm that allows us to model nonlinear relationships between input features and output labels. It can be used in real-time business problems, such as sales forecasting, where the relationship between variables is not linear.

Advantages:

- Polynomial regression can model a wide range of nonlinear relationships between input and output variables. It can capture complex patterns that are difficult to model with linear regression.
- Polynomial regression is a simple algorithm that can be easily implemented and understood. It does not require advanced mathematical knowledge or complex algorithms.

Disadvantages:

- Polynomial regression can easily overfit the data if the degree of the polynomial curve is too high. It can lead to poor generalization and inaccurate predictions on new data.
- Polynomial regression can be sensitive to outliers in the data. Outliers can significantly affect the shape of the polynomial curve and lead to inaccurate predictions.

## Regression: Ridge and Lasso Regression

Ridge Regression, also known as L2 regularization, adds a penalty term proportional to the square of the magnitude of the coefficients. It encourages the model to have smaller but non-zero coefficients. It helps in reducing the impact of high-value coefficients and, in turn, the risk of overfitting.

Lasso Regression, also known as L1 regularization, adds a penalty term proportional to the absolute values of the coefficients. It is a type of regression algorithm used for feature selection and regularization. It is similar to ridge regression, but it adds an L1 penalty term to the regression equation, resulting in a sparse model where some of the coefficients are set to zero.

Ridge and Lasso Regression are techniques that add regularization to linear regression models. Ridge encourages small coefficients and can prevent multicollinearity, while Lasso can perform feature selection by setting some coefficients to zero.

## Linear Classification: Logistic Regression

Logistic Regression is used to predict the probability of an outcome based on the input features. It uses a sigmoid function to map the input features to output the probability.

Advantages:

- It works well for binary classification problems, where the output variable has only two possible values.
- It can handle both continuous and categorical input variables.

Disadvantages:

- Logistic Regression assumes that the relationship between the input variable and the output variable is linear, which may not be true in all cases.
- It can be sensitive to outliers or biased data, which affects predictions.

## Linear Classification: Linear Discriminant Analysis (LDA)

A classifier with a linear decision boundary, generated by fitting class conditional densities to the data and using Bayes’ rule.

## Linear Classification: Stochastic Gradient Descent Classifier

## Naive Bayes: 

Naive Bayes is an algorithm that uses probabilities to make predictions. It is used for classification problems, where the goal is to predict the class an input belongs to.

Advantages:

- It can handle both continuous and categorical input variables.
- It is less prone to overfitting than other algorithms, which means it can generalize well on new data.

Disadvantages:

- It assumes that the input features are independent, which may not be true in all cases.
- It can be sensitive to the quality of the input data, such as missing values or noisy data.

## SVM and KNN: Support Vector Machines

Support Vector Machine (SVM) is a popular supervised learning algorithm used for classification and regression problems in Machine Learning. The basic idea behind SVM is to find a decision boundary that separates data points of different classes with the maximum margin.

## Decision Trees and Ensemble Methods

Decision tree algorithm is like a flowchart that makes decisions based on a series of questions and answers. It starts at a “root” node with a question, branches to subsequent questions based on the answers, and finally reaches a “leaf” node representing a decision.

Advantages:

- Decision trees produce easy-to-interpret decision rules, allowing stakeholders to understand and trust the decision-making process.
- Decision trees can effectively handle missing data by making decisions based on the data available for each division.

Disadvantages:

- Decision trees are prone to overfitting, especially when the tree becomes too deep or complex.
- Decision trees are sensitive to small changes in input data, which can result in different tree structures and decision rules.

## Random Forest

The Random Forest algorithm is an ensemble learning method for classification and regression tasks. It is composed of multiple decision trees, where each tree is built on a random subset of the input features and a random subset of the training data.

Advantages:

- Random Forest can handle a large number of input features, including both categorical and numerical features.
- It is less prone to overfitting compared to other machine learning algorithms, such as decision trees.

Disadvantages:

- Random Forest is a black box model, which means that it is difficult to understand how it makes its predictions.
- It may take longer to train compared to other machine learning algorithms, especially on large datasets.

## Boosting: Gradient Boosting

## Boosting: Adaptive Boosting

## Clustering: K-Means Clustering

K-Means is a clustering algorithm in machine learning that can group an unlabeled dataset very quickly and efficiently in just a few iterations. It works by labelling all instances on the cluster with the closest centroid. When the instances are centred around a particular point, that point is called a centroid.

The computational complexity of the K-Means clustering algorithm is generally linear concerning:

- the number of instances m,
- the number of clusters k,
- and the number of dimensions n.

It is important to scale the input features before running the K-means, otherwise, the clusters can get very stretched and therefore the algorithm will perform poorly. However, scaling the features does not guarantee that the clusters will become nice and spherical, but it usually improves them a lot.

## Clustering: DBSCAN Clustering 

DBSCAN stands for Density-Based Spatial Clustering for Applications with Noise. This is an unsupervised clustering algorithm which is used to find high-density base samples to extend the clusters.

The DBSCAN Clustering algorithm is based on the concept of core samples, non-core samples, and outliers:

- Core Samples: The samples present in the high-density area have minimum sample points with the eps radius.
- Non-core samples: The samples close to core samples but are not core samples but are very near to the core samples. The no-core samples lie within the eps radius of the core samples but they don’t have minimum samples points.
- Outliers: The samples that are not part of the core samples and the non-core samples and are far away from all the samples.

The DBSCAN clustering algorithm works well if all the clusters are dense enough and are well represented by the low-density regions.


DBSCAN clustering algorithm is a very simple and powerful clustering algorithm in machine learning. It can identify any cluster of any shape. It is robust to outliers and has only two hyperparameters. It may be difficult for it to capture the clusters properly if the cluster density increases significantly.

## Clustering: Agglomerative Clustering

Agglomerative clustering is one of the clustering algorithms where the process of grouping similar instances starts by creating multiple groups where each group contains one entity at the initial stage, then it finds the two most similar groups, merges them, repeats the process until it obtains a single group of the most similar instances. 

Some of the advantages of using this algorithm for clustering are:

- It adapts very well to a large number of instances
- It can capture the clusters of different shapes
- It forms flexible and informative clusters
- It can also be used with any pairwise distance

## Clustering: BIRCH Clustering

It stands for Balanced Reducing and Clustering using Hierarchies. 

This is best while clustering on a very large dataset having less than 20 features.

## Clustering: Mean Shift Clustering

Below is the complete process of the Mean Shift algorithm:

1. It starts by placing a circle centered on each sample
1. Then for each circle, it calculates the mean of all the samples located in the circle
1. Then it moves the circle so that it is centered on the mean
1. Then it iterates the mean shift step until all of the circles stop moving
1. Then it shifts the circles in the direction of the highest density until each circle reaches a maximum of local density
1. Then all the instances whose circles have settled in the same place are assigned to the same cluster

The mean shift algorithm is a nonparametric clustering algorithm that does not require prior knowledge of the number of clusters. It is not a popular clustering algorithm because it is not suitable while working with large datasets.