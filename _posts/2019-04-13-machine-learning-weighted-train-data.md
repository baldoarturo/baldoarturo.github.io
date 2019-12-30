---
id: 327
title: 'Machine Learning &#8211; Weighted Train Data'
date: 2019-04-13T15:59:47-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=327
permalink: /machine-learning-weighted-train-data/
categories:
  - Projects
tags:
  - machine learning
  - numpy
  - pandas
  - python
  - sklearn
---
 

Last post talked about an introduction to Machine Learning and how outcomes can be predicted using _sklearn&#8217;s LogisticReggression._

Sometimes, the input data could require additional processing to prefer certain classes of information, that it considered more valuable or more representative to the outcome.

The _LogisticRegression_ model allows to set the preference, or weight, at the time of being created, or later when being fitted.

The data used on the previous entry had four main classes: DRAFT, ACT, SLAST and FLAST. Once it is encoded and fitted, it can be selected by its index. I prefer to initialize some mnemonics selectors to ease the coding and make the entire code more human friendly.

<pre class="wp-block-code"><code>x_columns_names = ['DRAFT', 'ACT', 'SLAST', 'FLAST']
y_columns_names = ['PREDICTION']

# Indexes for columns, used for weighting
DRAFT = 0
ACT = 1
SLAST = 2
FLAST = 3

# Weights
DRAFT_WEIGHT = 1
ACT_WEIGHT = 1
SLAST_WEIGHT = 1
FLAST_WEIGHT = 1</code></pre>

The model can be initialized lated using the following method, where the _class_weight_ parameter is used referencing the previous helpers.

<pre class="wp-block-code"><code>model = LogisticRegression(
    solver='lbfgs',
    multi_class='multinomial',
    max_iter=5000,
    class_weight={
        DRAFT: DRAFT_WEIGHT,
        ACT: ACT_WEIGHT,
        SLAST: SLAST_WEIGHT,
        FLAST: FLAST_WEIGHT,
    })</code></pre>