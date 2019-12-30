---
id: 317
title: 'Machine Learning &#8211; Classification nd Regression Analysis'
date: 2019-01-30T15:41:18-03:00
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

To tackle this, Python (version 3) will be used, among the package _<span class="wsc-spelling-problem" data-spelling-word="scikit-learn" data-wsc-lang="en_US">scikit-learn</span>_. You can find more info about this package on the official page.

<https://scikit-learn.org/stable/tutorial/basic/tutorial.html>

**Supervised learning**

In general, a learning problem considers a set of n samples of data and then tries to predict properties of unknown data. If each sample is more than a single number and, for instance, a multi-dimensional entry (aka multivariate data), it is said to have several attributes or features.

Supervised learning consists in learning the link between two datasets: the observed data X and an external variable y that we are trying to predict, usually called “target” or “labels”. Most often, y is a 1D array of length n_samples.

All supervised estimators in <span class="wsc-spelling-problem" data-spelling-word="scikit-learn" data-wsc-lang="en_US">scikit-learn</span> implement a fit(X, y) method to fit the model and a predict(X) method that, given unlabeled observations X, returns the predicted labels y.

If the prediction task is to classify the observations in a set of finite labels, in other words to “name” the objects observed, the task is said to be a&nbsp;**classification**&nbsp;task. On the other hand, if the goal is to predict a continuous target variable, it is said to be a&nbsp;**regression**&nbsp;task.

When doing classification in <span class="wsc-spelling-problem" data-spelling-word="scikit-learn" data-wsc-lang="en_US">scikit-learn</span>, `y` is a vector of integers or strings.

<p class="has-medium-font-size">
  <strong>The Models</strong>
</p>

**<span class="wsc-spelling-problem" data-spelling-word="LinearRegression" data-wsc-lang="en_US">LinearRegression</span>**, in its simplest form, fits a linear model to the data set by adjusting a set of parameters in order to make the sum of the squared residuals of the model as small as possible.

<span class="wsc-spelling-problem" data-spelling-word="LogisticRegression" data-wsc-lang="en_US"><strong>LogisticRegression</strong></span>, which has a very counter-intuitive model, is a better choice when linear regression is not the right approach as it will give too much weight to data far from the decision frontier. A linear approach is to fit a sigmoid function or logistic function.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-19.png?w=640&#038;ssl=1" alt="" class="wp-image-313" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-19.png?w=447&ssl=1 447w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-19.png?resize=300%2C39&ssl=1 300w" sizes="(max-width: 447px) 100vw, 447px" data-recalc-dims="1" /></figure>
</div>

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i0.wp.com/scikit-learn.org/stable/_images/sphx_glr_plot_logistic_001.png?w=640&#038;ssl=1" alt="../../_images/sphx_glr_plot_logistic_001.png" data-recalc-dims="1" /></figure>
</div>

<p class="has-medium-font-size">
  <strong>The Data</strong>
</p>

Data is presented on a CSV file. It has around 2500 rows, with 5 columns. Correct formatting and integrity of values cannot be assured, so additional processing will be needed. The sample file is like this.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-20.png?w=640&#038;ssl=1" alt="" class="wp-image-316" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-20.png?w=330&ssl=1 330w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-20.png?resize=240%2C300&ssl=1 240w" sizes="(max-width: 330px) 100vw, 330px" data-recalc-dims="1" /></figure>
</div>