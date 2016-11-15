---
layout: post
comments: true
title:  "Vectorization in numpy (draft)"
excerpt: "Some examples for vectorization in machine learning algorithms"
date:   2016-11-02
mathjax: true
---

One common vectorization technique is useful in machine learning problem: assign one row of matrix $$A$$, let's say $$A[i]$$ to another column matrix $$B$$, let's say $$B[k]$$, where $$k = y(i)$$, $$y$$ is some index function.

### Example 1: SVM Gradient

For differential computation:

$$
dw_k = \frac{\partial L_i}{\partial w_k} =
					    \begin{cases}
					    -x_i^T & (k=y_j), i=1,\cdots, N \\
					    0      & (k \neq y_j)
					    \end{cases}
$$

Thus we get $$dw_k = -x_i^T$$, where $$k=y_j(=y(i))$$. To construct this assignment, we need to use the fact that, e.g. assign the first column of $$X[0]^T$$ to the $$y[0]$$st column of $$dw$$, we can use the formula

$$X[0]^T * [0, ..., 0, -1, 0, ..., 0]$$

where the position of $$-1$$ is equal to the value of $$y[0]$$.

Then, the whole computation will become

```python
mask = np.zeros((num_train, num_classes))
index = [np.arrange(num_train), y]
mask[index] = -1
np.dot(X.T, mask)
```

### Example 2: Softmax Loss

For differential $$dw_k=\frac{\partial L_i}{\partial w_k}= x_i^T$$, where $$k=\underset{j}{\operatorname{argmax}}f_j$$, we can use the fact that, e.g. assign the first column of $$X[0]^T$$ to the $$y[0]$$st column of $$dw$$, we can use the formula

$$X[0]^T * [0, ..., 0, 1, 0, ..., 0]$$

where the position of $$1$$ is equal to the position of $$f[0]$$'s  maximum element.

Then, the whole computation will become

```python
mask = np.zeros((num_train, num_classes))
maxPos = np.max(f, axis=1)
index = [np.arrange(num_train), maxPos]
mask[index] = 1
np.dot(X.T, mask)
```

### Example 3: Softmax Gradient

We can continue to use the characteristics of above discussion to construct the vectorization of softmax loss. It's loss formula is

$$L_i = -f_{y_i} + \log(\sum_j e^{f_j})$$

For the first term, it's the same as the first example that $$dw_k = -X_i^T$$, where $$k = y_i$$. Thus we can use the code

```python
mask = np.zeros((num_train, num_classes))
index = [np.arrange(num_train), y]
mask[index] = -1
np.dot(X.T, mask)
```

For the differential of 2nd term, we get

$$\sum_k \frac{e^{f_k}}{\sum_j e^{f_j}}\ f_k'$$

Thus, except the $$f_k'$$, which can be gotten by using, e.g.

$$X[0]^T * [0, ..., 0, 1, 0, ..., 0]$$

where the position of $$1$$ is equal to the value of $$y[0]$$, we only need to multiple the previous coefficients $$\frac{e^{f_k}}{\sum_j e^{f_j}}$$, i.e.

$$X[0]^T * [0, ..., 0, \frac{e^{f_k}}{\sum_j e^{f_j}}, 0, ..., 0]$$

This is the general term of the sum in the differential of 2nd term. Let's expand some beginning terms:

$$
\begin{align*}
X[0]^T &* [\frac{e^{f_0}}{\sum_j e^{f_j}}, 0, 0, ..., 0] \\

X[0]^T &* [0, \frac{e^{f_1}}{\sum_j e^{f_j}}, 0, ..., 0] \\

X[0]^T &* [0, 0, \frac{e^{f_2}}{\sum_j e^{f_j}}, 0, ..., 0]
\end{align*}
$$

So, the whole sum will become

$$X[0]^T * [\frac{e^{f_0}}{\sum_j e^{f_j}}, \frac{e^{f_1}}{\sum_j e^{f_j}}, \frac{e^{f_2}}{\sum_j e^{f_j}}, ..., \frac{e^{f_k}}{\sum_j e^{f_j}}, ...]$$


### Dimension Analysis

The above discussion can be treated as math proof of the following technique: *dimension analysis*, which is easier to remember. Let's analyze the typical vector multiplication in full-connected neural network computation. Assume the formula that we use is

$$a = X\cdot W + b$$

where, $$X$$ is a $$N\times D$$ matrix, $$W$$ has dimension $$D\times M$$, and the dimension of $$b$$ is $$M\times 1$$. And we've known the derivative of $$a$$ is $$da$$. Obviously, its dimension is the same as $$a$$, i.e. $$N\times M$$. Let's determine the derivatives of $$X, W, b$$.

According to chain rule, the derivative of $$X$$ must be some combinations of $$W$$ and $$da$$. As:

- dimension of $$dX$$ is the same as $$X$$, i.e. $$N\times D$$,
- dimension of $$W$$ is $$D\times M$$,
- and dimension of $$da$$ is $$N\times M$$.

In order to get the result with dimension $$N\times D$$, we need to use multiplication of matrices of dimension $$N\times M$$ and dimension $$M\times D$$. Thus, we can get the answer as $$dX = da \cdot W^T$$.

Also, to get $$dW$$, we know it must be the combination of $$X$$ and $$da$$. As:

- the dimension of $$dW$$ is $$D\times M$$,
- the dimension of $$X$$ is $$N\times D$$,
- the dimension of $$da$$ is $$N\times M$$

we get $$dW = X^T \cdot da$$.

Finally, for $$db$$, we know its dimension is $$M\times 1$$. And it is only related to $$da$$ with dimension $$N\times M$$. We just need to construct the matrix multiplication with all-one vector $$u = [1, 1, \cdots, 1]^T$$ with dimension $$N\times 1$$. Thus we get the result $$db = da^T\cdot u$$.

Superisely, we can find that the results of dimension analysis are the same as the previous discussion. That's the power of dimension analysis!






