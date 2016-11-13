---
layout: post
comments: true
title:  "The scenario behind Bayesian analysis"
excerpt: "exploring the Bayesian scenario in machine learning"
date:   2016-10-25
mathjax: true
---

For lots of applied machine learning algorithms, Bayesian analysis is one of the seldom really understood by engineers even the researchers. It contains massive mathematical details and abstraction framework. But to be honest, it's one kind of powerful weapon you may use to analyze your algorithms. 

When we mention Bayesian analysis in machine learning, the most popular junior example is the *naive bayesian classifier*, which can be competitive as popular algorithm as decision tree or neural network in some cases. But from the perspective of the real Bayesian analysis, it contains lots of implict information that few people may notice. In my opinion, it's not a good example to introduce the Bayesian analysis, but a good example to apply it with ignored bias. 

### Introduction to statistical learning

For a learner that we want to build is a function $$f: \mathcal{X} \rightarrow \mathcal{Y}$$, which maps from the input space $$\mathcal{X}$$ to the outut space $$\mathcal{Y}$$. Often, the dataset $$D$$ we observe is the subset of input space $$\mathcal{X}$$. And the predicted result $$y$$ is the value in output space $$\mathcal{Y}$$. The whole process of machine learning is to build learner. And this is the so-called **discriminative model**. Because our output here is directly the data $$y$$ we want. 

We can revisit the **Machine Learning** definition given by Tom M.Mitchell:

> A computer program is said to **learn** from experience $$E$$ with respect to some class of tasks $$T$$ and performance measure $$P$$, if its performance at tasks in $$T$$, as measured by $$P$$, improves with experience $$E$$.

The experience $$E$$ is the data that we learn from, the task $$T$$ is often treated as prediction or classification. And the performance $$P$$ is the loss function that we used to measure how good our classifier is.

One quick question is where is the probability, i.e. what's the probability framework of this leanring problem? Thus we need to introduce the **probabilistic model**, which is a different perpsective from *discriminative model*.

Instead of directly predicting the data $$y$$, we'll predict the *probability* that the data $$y$$ appearing under the condition observing existing data $$X$$. To be more rigorous, let's build the probabilistic model from random variable definitions. From the probability perspective, define the input/observed data $$X \in \mathcal{X}$$ as input random variable. So is the output value $$Y\in\mathcal{Y}$$. For each new result $$y$$, we want to get the probability of $$P(Y=y \vert X)$$.

To achieve this goal, we need to estimate the conditional distribution $$P(Y \vert X)$$. And if we get the distribution, we can indirectly give the predicting $$\hat{y}$$ we want by

$$\hat{y} = \underset{y}{\operatorname{argmax}} P(Y=y \vert X) $$

So the whole problem is boiled down to estimate the distribution. 

### Bayesian School

In statistics, each estimation should be built under some hypothesis, i.e. the statistical parameters in the estimated distribution. We define $$\mathcal{H}$$ as the hypothesis space, and each concrete parameter as $$h\in\mathcal{H}$$.

Once we've determined which distribution we want to use for prediction, the remaining part is to estimate the hypothese $$h$$ in our chosen distribution. And how to deal with the hypothese estimation divides the **Bayesian** and **non-Bayesian** schools.

- For *non-Bayesian* school members, they'll estimate the distribution hypothese $$\hat{h}$$ by **Maximum Likelihood Estimation** (*MLE*), i.e. $$\hat{h}=\underset{h}{\operatorname{argmax}} P(X \vert h)$$.
- While for Bayesian school members, they think the maximum argument is not only relevant to the likelihood that $$X$$ being gotten under hypothese $$h$$, but **also** relevant to the probability that hypothese $$h$$ appearing, i.e. $$\hat{h}=\underset{h}{\operatorname{argmax}} P(X \vert h) P(h)$$, which is the so-called **Maximum A Posterior** (*MAP*) estimation.

It's easy to see that these two different schools only differ in one term $$P(h)$$, i.e. the admission of **prior knowledge**, which gives totally different perspective to treat the whole world!


### Review of Naive Bayesian Classifier

The process of naive Bayesian classifier is simple. But the point here is to dissect the *simple* algorithm under above probabilistic perspective. Let's go through the most common naive Bayesian algorithm first.

Assume $$Y$$ is any discrete random variable, and $$X_1, \cdots, X_n$$ are the observed data. Our goal is to train a classifer that will output the probability distribution $$P(Y \vert X)$$ as we discussed above. The expression for the probability that $$Y$$ takes on its $$k^{th}$$ possible value based on Bayesian rule is

$$P(Y=y_k \vert X_1, \cdots, X_n) = \frac{P(Y=y_k)P(X_1, \cdots, X_n \vert Y=y_k)}{\sum_j P(Y=y_j)P(X_1, \cdots, X_n \vert Y=y_j)}$$

Then, use the naive Bayesian assumption that $$X_i$$ are conditionally independent given $$Y$$, we get

$$P(Y=y_k \vert X_1, \cdots, X_n) = \frac{P(Y=y_k)\Pi_i P(X_i \vert Y=y_k)}{\sum_j P(Y=y_j)\Pi_i P(X_i \vert Y = y_j)}$$

So the chosen $$Y$$ should be

$$Y \leftarrow \underset{y_k}{\operatorname{argmax}}\frac{P(Y=y_k)\Pi_i P(X_i \vert Y=y_k)}{\sum_j P(Y=y_j)\Pi_i P(X_i \vert Y = y_j)}$$

then, simplify it ignoring non-$$y_k$$ terms

$$Y \leftarrow \underset{y_k}{\operatorname{argmax}}P(Y=y_k)\Pi_i P(X_i \vert Y=y_k)$$

As $$X, Y$$ are the discrete random variables, we can get the $$P(Y=y_k)$$ and $$P(X_i \vert Y=y_k)$$ by frequencies counting. That's the whole story.

The whole process seems so natural that we may ignore the key points in probabilistic model. The direct two questions can be proposed under the probabilistic perspective are:

- Where or what is the hypothesis $$h$$ in above naive Bayesian algorithm?
- And how could we distinguish we're using the *MLE* or *MAP* to estimate $$h$$? 

TK

### Discrete-valued Naive Bayes

In order to solve these two questions, I should clarify one trick part here. In above discussion, we've used one assumption implictly that $$X_i, Y$$ *are discrete-valued random variables*. Why does it care? Because, the probabilistic parameters, i.e. the hypothesis $$h$$, are in different from under these two different cases. Usually, we'd like to talk about the parameters in dense function, which is the continuous-valued random variable case. But for discrete-valued model, once we know the probability of different random variable situation, i.e. to know $$P(X = x_i)$$, we'd know the distribution of this random variable. Thus in this case, the estimated value is just the probability that random variable assigned with different value.

Now, we can answer the first question, what's the hypothesis $$h$$? In above Bayesian discussion, the estimated hypotheses are the values of $$P(Y = y_k)$$ and $$P(X_i \vert Y = y_k)$$.

Let's discuss this part formally. Assume random variable $$Y$$ can take $$K$$ different values: $$Y = y_k$$, where $$k$$ takes from 1 to $$K$$.  Assume the $$n$$ different random variables $$X_i$$ can take $$J$$ different values: $$v_j$$, where $$j$$ takes from 1 to $$J$$. Define $$x_i^{(j)}$$ as the situation $$X_i = v_j$$.

So, for the first part $$P(X_i \vert Y = y_k)$$, we need to estimate $$n \times J \times K $$ parameters, which we can define as

$$\theta_{ijk} \equiv P(X_i = x_i^{(j)} \vert Y = y_k)$$

As the $$X_i$$ can only take $$J$$ different values, we have $$\sum_{j=1}^J P(X_i = x_i^{(j)} \vert Y = y_k) = 1 $$. Thus there're only $$n \times (J-1) \times K$$ independent $$\theta_{ijk}$$ values.

For the second part $$P(Y = y_k)$$, we can define the $$K$$ different parameters as:

$$\pi_k \equiv P(Y = y_k)$$

As $$Y$$ only takes $$K$$ different values, we have $$P(Y = y_k) = 1$$. Thus there're only $$(K-1)$$ independent $$\pi_k$$ values.

TK

### Regularization and MAP

On technique to avoid overfitting is using regularization. It adds one penalty term to objective funtion to control the complexity of weights $$W$$. From the perspective of statistics, this regularization term is equivalent to the prior distribution $$P(W)$$ in MAP. Let's see the details.

Assume the weight $$W$$ is governed by the normal distribution $$N(0, 1/N^2)$$, the objective function we want to optimize is

$$W \leftarrow \underset{W}{\operatorname{argmax}}P(Y|X, W)$$

Then, if we add the prior distribution, it becomes:

$$W \leftarrow \underset{W}{\operatorname{argmax}}P(Y|X, W)P(W)$$

As it's an optimization problem, it's also fine to add the function $$ln(\cdot)$$ for the objective function, i.e.

$$W \leftarrow \underset{W}{\operatorname{argmax}} lnP(Y|X, W) + lnP(W)$$

As $$W \sim N(0, 1/N^2)$$, where

$$ N(0, 1/N^2) = \frac{N}{\sqrt{2\pi}}exp(-\frac{N^2}{2}x^2)$$

Thus the term $$ln P(W)$$ becomes:

$$ln P(W) = ln\frac{N}{\sqrt{2\pi}}-\frac{N^2}{2}W^2=ln\frac{N}{\sqrt{2\pi}}-\frac{N^2}{2}\lvert W \rvert^2$$

Ignoring the constant term in optimization problem, the original objective function with prior distribution becomes

$$W \leftarrow \underset{W}{\operatorname{argmax}}P(Y|X, W)-\frac{N^2}{2}\lvert W \rvert^2$$

which is what we want to prove.







