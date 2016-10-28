---
layout: post
comments: true
title:  "Notes in LeNet-5"
excerpt: "notes of my understanding in LeNet-5."
date:   2016-09-27
mathjax: true
---

Yann LeCun's classic paper [Gradient-Based Learning Applied to Document Recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf) is worth every Deep Learning researcher for reading. It contains about 43 pages(not counting the reference pages), which is some kind of challenge for your patience and focus. Based on this price, you can get the detailed explanations and intuitive insights behind the deep learning development.

## Gap between testing error and training error

This part references classic formula to explain the trade-off between $$E_{test}$$ and $$E_{train}$$, which is the trade-off between bias and variance, or synonym underfitting and overfitting.

$$E_{test}-E_{train}=k(h/P)^{\alpha}$$

where $$k$$ is one constant, $$h$$ is one index to evaluate the complexity of the model, $$P$$ is the number of training samples, $$\alpha$$ is a constant in range $$[0.5, 1.0]$$.


## LeNet-5 Details

When I read the **II-B** part, I found there're lots of summary numbers about the famous LeNet-5 network. But in this compact paper, it didn't give detailed steps for those computations. In my opinion these computing steps are interesting and are able to give us deeper understanding about how this network works. So, I'd like to complement those details.


### Layer $$C_1$$ (6@28x28).

- $$6$$ feature maps.
- $$5 \times 5$$ size filter.
- size of feature map: one dimension is $$32-(5-1)=28$$, thus the size is $$28 \times 28$$.
- number of trainable parameters:
    - each feature map requires $$1$$ same filter, which contains $$5 \times 5$$ trainable coefficients.
	- and $$1$$ trainable bias parameter.
	
	So in all, each feature map require $$5 \times 5 + 1 = 26$$ trainable parameters
	- as there're $$6$$ layers' feature map in $$C_1$$,
	
	The whole $$C_1$$ requires $$26 \times 6 = 156$$ trainable parameters.
- number of connections: when we compute the above parameters, we just treat each point in $$C_1$$ with same filter, which smooths $$28 \times 28$$ different cases with the same $$1$$ compute pattern. Thus, the number of connections can be get by recovering this process as: $$156 \times (28 \times 28) = 122,304$$.


### Layer $$S_2$$ (6@14x14).

- $$6$$ feature maps.
- size of feature map: as there's non-overlapping and shrinking each filter dimension by $$2$$, i.e. $$28 \div 2 = 14$$, we get the feature size as $$14 \times 14$$.
- number of trainable parameters:
    - considering the pooling process as $$sigmoid[(\sum_{i=1}^4 \pi_i) \times \omega + b]$$, only $$\omega$$ and $$b$$ are trainable, and each feature map use the same trainable parameters, so we only need $$2$$ trainable parameters in each feature map.
    - as there're $$6$$ layers, in totally we require $$2 \times 6=12$$ trainable parameters.
- number of connections: as there're $$2 \times 2$$ inputting sampling connections, plus $$1$$ bias connection within each feature map. Thus we get the total number of connections as $$(2\times 2+1)\times (14\times 14\times 6)=5,880$$.


### Layer $$C_3$$ (16@10x10).
This layer is much more special and differs from common convolutional layer or sampling(pooling) layer. As the paper says, it combines *neighborhoods at identical locations in a subset of $$S_2$$'s feature maps*, which means the neighborhoods in the 3rd dimension, layer dimension. If we image the $$S_2$$ feature maps are heaped from bottom to top. Then the **neighborhoods** means the local layers in the height dimension. So for each layer group in $$C_3$$, the filter size may be different.

- number of training parameters:
  - number of parameters in **first** $$6$$ layers: $$((5\times 5)\times 3+1)$$ for each layer in this group (the $$3$$ indicates the number of neighbors), and the total $$6$$ layers require $$(25\times 3+1)\times 6=456$$ training parameters.
  - number of parameters in **next** $$6$$ layers(with the same above thinking): $$(25\times 4 + 1)\times 6=606$$ training parameters (the $$4$$ indicates the number of neighbors). 
  - number of **next** 3 layers: $$(25\times 4+1)\times 3=303$$ training parameters.
  - number of **last** 1 layer: $$(25\times 6+1)\times 1=151$$ training parameters.
  Summarizing all the numbers, the whole $$C_3$$ requires $$1,516$$ parameters.
- number of connections: same as discussion in $$C_1$$, each feature map's element uses the same filter, and there're $$10\times 10$$ elements in each feature map of $$C_3$$, we get the whole number of connections $$1,516\times 100=151,600$$.


### Layer $$S_4$$ (16@5x5).

- number of trainable parameters: $$2\times 16=32$$.
- number of connections: $$(4+1)\times 5\times 5\times 16=2,000$$.


### Layer $$C_5$$ (120@1x1).

- number of trainable parameters: $$(5\times 5\times 16+1)\times 120=48,120$$.
- number of connections: $$48,120\times 1=48,120$$.


### Layer $$F_6$$ (84@1x1).

This layer contains the same classic neural network structure. It would get the input from all the layers from $$C_5$$, which means $$120$$ layers with $$1x1$$ size. And to generate the $$i^{th}$$ unit of $$F_6$$, it'll weight the $$120$$ elements in $$C_5$$ and add one bias. It means each unit of $$F_6$$ requires $$120+1=121$$ trainable parameters.

And this weighted sum, called $$a_i$$ for unit $$i$$, will be passed to a sigmoid squashing function. This is the final result of unit $$i$$ in $$F_6$$: $$x_i=f(a_i)$$, where $$f(a)=A\ tanh(Sa)$$.

- number of trainable parameters: $$(120+1)\times 84=10,164$$.
- number of connections: $$10,164\times 1=10,164$$.


### Output Layer

Each unit in *OUTPUT* layer computes from the Euclidean distance between its input vector and its parameter vector: $$y_i=\sum_j (x_j - \omega_{ij})^2$$, where $$i$$ is index of output class, i.e. totally $$10$$ classes here, and $$j$$ is the index of input vector, i.e. totally $$84$$ units in $$F_6$$ here. Thus we can get:
- number of trainable parameters: $$84\times 10=840$$


**Remarks**: 

1. The distance used above is called Euclidean Radical Basis Function (RBF).
2. As the paper discussed, although there're lots of trainable parameters, but the value of each parameter is $$-1$$ or $$1$$. 
3. As the component in $$F_6$$ is the result of sigmoid function, it's value range is $$[-1, 1]$$. So, the design of this *OUTPUT* layer is used as a *penalty term* measuring the fit between the *input pattern* and a *model* of the class associated with the RBF. *The further away is the input from the parameter vector, the larger is the RBF output*.
4. The rationale of treating RBF as penalty term can be traced from probability theory. In probabilistic terms, the RBF output can be interpreted as the unnormalized negative log-likelihood of a Gaussian distribution in the space of configurations of layer $$F_6$$, which is the common definition of loss function.
5. As the role of loss function, the configuration of $$F_6$$ is supposed to be as close as possible to parameters in RBF, which corresponds to the pattern's designed class. 
6. The choice of RBF parameters is designed to represent a stylized image of the corresponding character class drawn on a $$7\times 12$$ bitmap (hence the number $$84$$).
7. As it represents a **stylized** image, it's very powerful to recognize groups of objects, which have common confusable parts, especially there's post-processor to correct the confusion. On the other hand, it's not useful to recognized isolated object, which is full of special characters far from *stylized* trend.
8. From a higher perspective, here we deal with the classier by using *distributed codes* (i.e. use the codes of pixels) instead of common-used one-hot encoding format. The reason is: **non-distributed** codes tend to behave **badly** when the *number of classes* is **larger** than a few dozens. That's why we often see one-hot encoding appearing in distinguish digits, instead of pictures. (Here the number of set ASCII, i.e. the number of classes, is already large enough for one-hot encoding.) 

