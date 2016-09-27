---
layout: post
comments: true
title:  "Computations in LeNet-5"
excerpt: "some computation work in LeNet-5."
date:   2016-09-27
mathjax: true
---

Yann LeCun's classic paper [Gradient-Based Learning Applied to Document Recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf) is worth every Deep Learning researcher for reading. It contains about 43 pages(except the reference pages), which is some kind of challenge for your patience and focus. Based on this price, you can get the detailed explanations and intuitive insights behind the deep learning development.

When I read the **II-B** part, I found there're lots of summary numbers about the famous LeNet-5 network. But in this compact paper, it didn't give detailed steps for those computations. But I thinik these computing steps are interesting and are able to give us deeper understanding about how this network works. So, I'd like to complement those details.


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

- number of trainable parameters: $$(120+1)\times 84=10,164$$.
- number of connections: $$10,164\times 1=10,164$$.


---

to be continued ...






