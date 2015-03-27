---
layout: post
title: "Tracking survey"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Here I'll survey some important works on visual tracking.

# MIL

One of the most important recent tracking works is MIL, i.e. B. Babenko, et al. "Robust Object Tracking with Online Multiple Instance Learning". 
This method is from the class of "tracking-by-detection" method, which trains a discriminative classifier online. This paper proposes to use
Multiple Instance Learning to avoid drifting problems. 

## Basic idea

In concrete, the discriminative classifier is able to return $ p(y|x) $, where $x$ is the image patch and $y$ is a binary variable indicating the presence of the object. 
At every timestep $ t $, the tracker maintains the object location $ \ell^{*}_t $. Assume in the search window $ X^s $, the update is:
$$
\ell^{*}_t = \ell(\arg \max_{x \in X^s} p(y|x)).
$$

Once the tracker location is updated, appearance model should be updated. A set of patches from $ X^r $, where $ r < s $ (i.e. s is the search window range), are labelled as positive bag. Then an annular region $ X^{r, \beta} $ are labelled negative.

## Multiple instance learning

In MIL, the training data has the form $ \\{ (X_1, y_1), ..., (X_n, y_n) \\} $, where a bag $ X_i = \\{  x_{i1}, ..., x_{im} \\} $ and $ y_i $ is a bag label: $ y_i = \max_j(y_{ij}) $. I.e., a bag is considered positive if it contains at least one positive instance. 

MILBoosting aims to train a boosting classifier that maximizes the log-likelihood of bags: 
$$ 
L = \sum_i \log {p(y_i|X_i)}.
$$

To express 
$$ 
p(y_i|X_i)
$$ 
in terms of its instances, the Noisy-OR (NOR) model is adopted, so a bag has a high probability if one of its instances has a high probability.
However, it is not suitable for online application. Here the authors adopt online boosting for MIL. 

In the prior online adaboost method, for an incoming example $x$, each $h_k$ is updated sequencially and the weight of example $x$ is adjusted after update. Also, the average error of weak classifiers are being watched during run-time, which allows the algorithm to estimate both example weight and classifier weight. In the proposed online MIBoosting method, the weak classifiers are chosen sequentially to optimize the following criteria:
$$
(\textbf{h}_k, \alpha_k) = \arg \max_{\textbf{h} \in H, \alpha} J(\textbf{H}_{k-1} + \alpha \textbf{h})
$$


