---
id: 306
title: 'Machine Learning &#8211; Classification and Regression Analysis'
date: 2019-01-30T19:21:18-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=306
permalink: /machine-learning-classification-and-regression-analysis/
categories:
  - Projects
tags:
  - machine learning
  - numpy
  - pandas
  - python
  - sklearn
  - upwork
---
 

Machine Learning is the science and art of programming computers so they can learn from data.

For example, your spam filter is a Machine Learning program that can learn to flag spam given examples of spam emails (flagged by users, detected by other methods) and examples of regular (non-spam, also called “ham”) emails.

The examples that the system uses to learn are called the training set. The new ingested data is called the test set. The performance measure of the prediction model is called accuracy and it&#8217;s the objetive of this project.

<p class="has-medium-font-size">
  <strong>The&nbsp;tools</strong>
</p>

To tackle this, Python (version 3) will be used, among the package _scikit-learn_. You can find more info about this package on the official page.

<https://scikit-learn.org/stable/tutorial/basic/tutorial.html>

**Supervised learning**

In general, a learning problem considers a set of n samples of data and then tries to predict properties of unknown data. If each sample is more than a single number and, for instance, a multi-dimensional entry (aka multivariate data), it is said to have several attributes or features.

Supervised learning consists in learning the link between two datasets: the observed data X and an external variable y that we are trying to predict, usually called “target” or “labels”. Most often, y is a 1D array of length n_samples.

All supervised estimators in scikit-learn implement a fit(X, y) method to fit the model and a predict(X) method that, given unlabeled observations X, returns the predicted labels y.

If the prediction task is to classify the observations in a set of finite labels, in other words to “name” the objects observed, the task is said to be a&nbsp;**classification**&nbsp;task. On the other hand, if the goal is to predict a continuous target variable, it is said to be a&nbsp;**regression**&nbsp;task.

When doing classification in scikit-learn,&nbsp;`y`&nbsp;is a vector of integers or strings.

<p class="has-medium-font-size">
  <strong>The Models</strong>
</p>

**LinearRegression**, in its simplest form, fits a linear model to the data set by adjusting a set of parameters in order to make the sum of the squared residuals of the model as small as possible.

**LogisticRegression**, which has a very counter-intuitive model, is a better choice when linear regression is not the right approach as it will give too much weight to data far from the decision frontier. A linear approach is to fit a sigmoid function or logistic function.

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

<p class="has-medium-font-size">
  <strong>The Code</strong>
</p>

We need three main libraries to start:

  * **numpy,** which basically is a N-dimensional array object. It also has tools for linear algebra, Fourier transforms and random numbers.  
    It can be used as an efficient multi-dimensional container of generic data, where arbitrary data-types can be defined.
  * **pandas,** which provides high-performance and easy-to-use data structures and data analysis tools simple and efficient tools for data mining and data analysis
  * <strong style="">sklearn,</strong> the main machine learning library. It has capabilities for classification, regression, clustering, dimensionality reduction, model selection and data preprocessing.

A non essential, but useful library is **matplotlib**, to plot sets of data.

In order to provide data for _sklearn_ __models to work, it has to be encoded first. As the sample data has strings, or _labels,_ a _LabelEncoder_ is needed. Next, the prediction model is declared, where a _LogisticRegression_ model is used. 

The input data file path is also declared, in order to be loaded with _pandas.read_csv()._

<pre class="wp-block-code"><code>import pandas as pd
import numpy as np
import matplotlib.pyplot as pyplot

from sklearn.preprocessing import LabelEncoder
from sklearn.linear_model import LogisticRegression

encoder = LabelEncoder()
model = LogisticRegression(
    solver='lbfgs', multi_class='multinomial', max_iter=5000)

# Input dataset
file = "sample_data.csv"</code></pre>

The CSV file can be loaded into a pandas dataframe in a single line. The library also provides a convenient method to remove any rows with missing values.

<pre class="wp-block-code"><code># Use pandas to load csv. Pandas can eat mixed data with numbers and strings
data = pd.read_csv(file, header=0, error_bad_lines=False)
# Remove missing values
data = data.dropna()

print("Valid data items : %s" % len(data))</code></pre>

Once loaded, the data needs to be encoded in order to be fitted into the prediction model. This is handled by the previously declared _LabelEncoder_. Once encoded, the _x_ and _y_ datasets are selected. The _pandas_ library provides a way to drop entire labels from a dataframe, which allows to easily select data.

<pre class="wp-block-code"><code>encoded_data = data.apply(encoder.fit_transform)
x = encoded_data.drop(columns=['PREDICTION'])
y = encoded_data.drop(columns=['DRAFT', 'ACT', 'SLAST', 'FLAST'])</code></pre>

The main objective is to test against different lengths of train and test data, to find out how much data provides the best accuracy. The lengths of data will be incremented in steps of 100 to get a broad variety of results.

<pre class="wp-block-code"><code>length = 100
scores = []
lenghts = []
while length &lt; len(x):
    x_train = x[:length]
    y_train = y[:length]
    x_test = x.sample(n=length)
    y_test = y.sample(n=length)
    print("Fitting model for %s training values" % length)
    trained = model.fit(x_train, y_train.values.ravel())
    score = model.score(x_test, y_test)
    print("Score for %s training values is %0.6f" % (length, score))
    length = length + 100
    scores.append(score)
    lenghts.append(length)</code></pre>

Finally, a plot is made with the accuracy scores.

<pre class="wp-block-code"><code>pyplot.plot(lenghts,scores)
pyplot.ylabel('accuracy')
pyplot.xlabel('values')
pyplot.show()</code></pre><figure class="wp-block-image">

![](https://agora-file-storage-prod.s3-us-west-1.amazonaws.com/workplace/attachment/7108570604843560901?response-content-disposition=inline%3B%20filename%3D%22Pasted%2520File%2520at%2520January%252030%252C%25202019%25201%253A50%2520PM.png%22%3B%20filename%2A%3Dutf-8%27%27Pasted%2520File%2520at%2520January%252030%252C%25202019%25201%253A50%2520PM.png&X-Amz-Security-Token=FQoGZXIvYXdzEDcaDDolUjmkdc8snAN0HyK3A9K9JNVtH6TqQZo7jVbOtXakCg3IDQt33GMAJrRktzbqzUnDa5Eh5OVj1BqChQqA14ifKg53FS8qJVIxXNgnXJFOoG0bvse%2BO9vZe9bHEWhwoqIT65qGbLHvcKbRe8i2R0gxn%2BNEHMhmYCHGwjueJ9R3F1hx1Fsi5v3Y53Dom2v8fLwHw4eq0lFPfvNU83AnP8cxy8KNqhLDjYgnuQs%2F8lsuYM3Xp1k%2BjHBGdpopqgIIhqsfY3CJSvDWvXzb%2BHcYZVN3aCmG%2Fi1fIVpiR9dteHgLkQY3Xe2f%2B7bYkCtDZ2QJJZoUufmTnEMY85x0PBQR51uHyhtJObN%2BVI64LIdDccyvZzTdsgQ%2FHGR%2Bi%2FjVPqYO5pazVJKADCxqrTiIpBd%2BJ4mvUVp8b0yMUQdR%2FBxwzAS%2BbJKSxQ4ofCKSrUxJdNc1xzzG0016KFuBkr6hOO00OPPk0PDjHc%2F5PZuYAPbgEkgfa1bMuVVF6zJSoI4LWXCtoKEzYFZPSA8R8rpoSo4pPpkXOVUk2BQcq4VL7srJ2os%2BuBbwjG43mrFyIk5YvrgiXxKkh6PG0lIHfZUgrEw6RLyuN7spiFcoi6TI4gU%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20190130T222042Z&X-Amz-SignedHeaders=host&X-Amz-Expires=599&X-Amz-Credential=ASIA2YR6PYW5R5OLEC6P%2F20190130%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Signature=d299c064944f9d2a2be70c6e0bda065cc9529ce80bd10de5241af5dd32b91b8c) </figure>