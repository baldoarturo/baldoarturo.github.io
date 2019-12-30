---
id: 311
title: Machine Learning, Classification and Regression Analysis Using Python
date: 2019-01-30T15:30:02-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/306-revision-v1/
permalink: /306-revision-v1/
---
Machine Learning is the science and art of programming computers so they can learn from data.

For example, your spam filter is a Machine Learning program that can learn to flag spam given examples of spam emails (flagged by users, detected by other methods) and examples of regular (non-spam, also called “ham”) emails.

The examples that the system uses to learn are called the training set. The new ingested data is called the test set. The performance measure of the prediction model is called accuracy and it&#8217;s the <span class="wsc-spelling-problem" data-spelling-word="objetive" data-wsc-lang="en_US">objetive</span> of this project.

<p class="has-medium-font-size">
  <strong>The tools</strong>
</p>

I will us Python (version 3) and the Python package _scikit-learn_ to tackle this. You can find more info about this package on the official page.

<https://scikit-learn.org/stable/tutorial/basic/tutorial.html>

**Supervised learning**

In general, a learning problem considers a set of n samples of data and then tries to predict properties of unknown data. If each sample is more than a single number and, for instance, a multi-dimensional entry (aka multivariate data), it is said to have several attributes or features.

Supervised learning consists in learning the link between two datasets: the observed data X and an external variable y that we are trying to predict, usually called “target” or “labels”. Most often, y is a 1D array of length n_samples.

All supervised estimators in <span class="wsc-spelling-problem" data-spelling-word="scikit-learn" data-wsc-lang="en_US">scikit-learn</span> implement a fit(X, y) method to fit the model and a predict(X) method that, given unlabeled observations X, returns the predicted labels y.

If the prediction task is to classify the observations in a set of finite labels, in other words to “name” the objects observed, the task is said to be a&nbsp;**classification**&nbsp;task. On the other hand, if the goal is to predict a continuous target variable, it is said to be a&nbsp;**regression**&nbsp;task.

When doing classification in <span class="wsc-spelling-problem" data-spelling-word="scikit-learn" data-wsc-lang="en_US">scikit-learn</span>, `y` is a vector of integers or strings.

<p class="has-medium-font-size">
  <strong>The Data</strong>
</p>