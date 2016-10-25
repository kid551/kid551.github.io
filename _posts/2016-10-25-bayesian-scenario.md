---
layout: post
comments: true
title:  "The scenario behind Bayesian analysis"
excerpt: "exploring the Bayesian scenario in machine learning"
date:   2016-10-25
mathjax: true
---

For lots of applied machine learning algorithms, Bayesian analysis is one of the seldom really understood by engineers even the researchers. It contains massive mathematical details and abstraction framework. But to be honest, it's kind of powerful weapon you may use to analyze your algorithms. 

When we mention Bayesian analysis in machine learning, the most popular junior example is the *naive bayesian classifier*, which can be competitive as popular algorithm as decision tree or neural network in some cases. But from the perspective of the real Bayesian analysis, it contains lots of implict information that few people may notice. In my opinion, it's not a good example to introduce the Bayesian analysis, but a good example to apply it with ignored bias.

### Introduction to statistical learning

For a learner that we want to build is a function $$f: \mathcal{X} \rightarrow \mathcal{Y}$$, which maps from the input space $$\mathcal{X}$$ to the outut space $$\mathcal{Y}$$. Often, the dataset $$D$$ we observe is the subset of input space $$\mathcal{X}$$. And the predicted result $$y$$ is the value in output space $$\mathcal{Y}$$. The whole process of machine learning is to build learner.

We can revisit the **Machine Learning** definition given by Tom M.Mitchell:

> A computer program is said to **learn** from experience $$E$$ with respect to some class of tasks $$T$$ and performance measure $$P$$, if its performance at tasks in $$T$$, as measured by $$P$$, improves with experience $$E$$.

The experience $$E$$ is the data that we learn from, the task $$T$$ is often treated as prediction or classification. And the performance $$P$$ is the loss function that we used to measure how good our classifier is.

One quick question is where is the probability, i.e. what's the probability framework of this leanring problem? From the probability perspective, the input/observed data $$X \in \mathcal{X}$$ can be treated as random variable. So is the output value $$Y\in\mathcal{Y}$$. For each new result $$y$$, we want to get the probability of $$P(Y=y \vert X)$$. To achieve this goal, we need to estimate the conditional distribution $$P(Y \vert X)$$, which is equivalent to the function $$f$$ mentioned above. 

In statistics, each estimation should be built under some hypothesis, i.e. the parameters in the estimated distribution. We define $$\mathcal{H}$$ as the hypothesis space, and each concrete parameter as $$h\in\mathcal{H}$$.


### Review of Naive Bayesian Classifier



