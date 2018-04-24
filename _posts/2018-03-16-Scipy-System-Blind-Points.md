---
layout: post
comments: true
title:  "Scipy System Blind Points"
excerpt: "Kernel features of Scipy system."
date:   2018-03-16
mathjax: true
---

- A figure in matplotlib means the whole window in the user interface.


---

Each array has attributes `ndim` (the number of dimensions), `shape` (the size of each dimension), and `size` (the total size of the array):

```Python
print("x3 ndim: ", x3.ndim)
print("x3 shape:", x3.shape)
print("x3 size: ", x3.size)

x3 ndim:  3
x3 shape: (3, 4, 5)
x3 size:  60
```

Another useful attribute is the `dtype`, the data type of the array (which we discussed previously in [Understanding Data Types in Python](https://jakevdp.github.io/PythonDataScienceHandbook/02.01-understanding-data-types.html)):

```Python
print("dtype:", x3.dtype)
```

```Python
dtype: int64
```

Other attributes include `itemsize`, which lists the size (in bytes) of each array element, and `nbytes`, which lists the total size (in bytes) of the array:

```Python
print("itemsize:", x3.itemsize, "bytes")
print("nbytes:", x3.nbytes, "bytes")

itemsize: 8 bytes
nbytes: 480 bytes
```

In general, we expect that `nbytes` is equal to `itemsize` times `size`.

---

```Python
x = np.arange(10)
x

# array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

```Python
x[::2]  # every other element

# array([0, 2, 4, 6, 8])
```

```Python
x[1::2]  # every other element, starting at index 1

# array([1, 3, 5, 7, 9])
```



A potentially confusing case is when the `step` value is negative. In this case, the defaults for `start` and `stop` are swapped. This becomes a convenient way to reverse an array:

```Python
x[::-1]  # all elements, reversed

# array([9, 8, 7, 6, 5, 4, 3, 2, 1, 0])
```

```Python
x[5::-2]  # reversed every other from index 5

# array([5, 3, 1])
```
