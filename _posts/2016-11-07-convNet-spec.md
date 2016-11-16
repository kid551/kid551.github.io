---
layout: post
comments: true
title:  "Convolutional Neural Network Specifications (draft)"
excerpt: "different specs applying in convNet"
date:   2016-11-07
mathjax: true
---

Notes from cs231n.

### Mini-batch SGD

**Loop:**

1. **Sample** a batch of data instead of using all data.
2. **Forward** prop it through the graph, get its loss (the final *data loss* and each layer's *regularization loss*).
3. **Backprop** to calculate the gradients.
4. **Update** the weights by using gradients.

#### Notes

Usually, in above situation, the number of loops that we apply is provided by ourselves, e.g. `num_iterations`. But in some literature, there's another form to use `num_epochs` instead to express this single number indirectly.

What's the meaning of a *epoch*. It often means one round of going through all training data. But in every loop above, our mini-batch can only go through `batch_size` number of training data.

So in order to finish *one epoch* quantity execution, we need to go through `num_train / batch_size`, which is also defined as `iterations_per_epoch`, times of above loop.

Correspondingly, in order to satisfy `num_epochs` quantity standard, we need to apply `num_epochs * iterations_per_epoch` times of above loops. (We only use the middle variable `iterations_per_epoch` here once.) That's why I said if we provide `num_epochs` only, it's just another way to express the `num_iterations` indirectly.

---

### One Time Setup

> 1. Activation functions
> 2. Preprocessing
> 3. Weight initialization
> 4. Regularization
> 5. Gradient Checking


#### 1. Activation functions

`sigmoid`, `tanh`, `ReLu`, `LeakReLu`, `Maxout`, and `ELU`.

Three problems should be considered. Use `sigmoid function` as example.

- **Problem 1**: saturated neuraons *kill* the gradients. (squashes numbers to range $$[0, 1]$$)

- **Problem 2**: sigmoid outputs are not zero centered.

As its formula is $$f(\sum_i w_ix_i + b)$$, if all the $$x_i$$ are positive, which is the case of sigmoid function's output, the gradient on $$w_i$$ will be

$$\frac{df}{dw_i} = x_i \times f'(\cdot)$$

Now, all the gradient on $$w$$ will be all positive or negative, depending on if $$f'$$ is positive or negative. This means, the updating path of $$w_i$$ will follow the zigzag path! And **this is also why we need the data to be zero mean**!

- **Problem 3**: $$exp(\cdot)$$ is a bit computation expensive.

**In practice**:

- Use `ReLu`, be careful with your learning rates.
- Try out `Leaky Relu`, `Maxout`, `ELU`.
- Try out `tanh` but don't expect much.
- Don't use `sigmoid`


#### 2. Data Preprocessing

- *zero-centered, normalized* data.
- *decorrelated, whitened* data. (**PCA** and **Whitening**)

In practice for image: centered only.


#### 3. Weight Initialization

If all $$W$$ are zero, then the network can't just startup. So the first idea is just giving it small random numbers:

```python
W = 0.01 * np.random.randn(D, H)
```

But it only works fine for small networks, but leads to non-homogeneous distributions of activations across the layers of a network.

The reason is ...

TK

---

### Training Dynamics

> 1. Babysitting the learning process
> 2. Parameter updates
> 3. Hyperparameter optimization

#### 1. Babysitting the Learning Process

TK

---

### Evaluation

> 1. Model Ensembles

#### 1. Model Ensembles

TK