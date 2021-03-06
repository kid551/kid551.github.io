---
layout: post
comments: true
title:  "Python与计算机编程"
excerpt: "Python及编程入门"
date:   2018-05-16
mathjax: true
published: false
---

## 前言

真正意义上的使用计算机，必须要使用编程。而要写程序，就必然要选择一门编程语言。而程序员的作用，就是将现实的想法，通过编程语言表述出来，让计算机可以接受。

编程的学习，是一个漫长的过程。特别是最开始，有十分陡峭的学习曲线。这不仅是由编程自身的难度造成的，更是由于前期的编程学习，如同练武功的扎马步，你很难看到这些前期基础知识的意义。成天接触黑色的对话框，无法对图形界面施加相应的影响，必然会让你对自己的学习过程充满疑惑。这种没有现实反馈、看不到前路的探索，在大部分情况下，都会让一个初学者走向放弃。

而我要做的，就是尽可能地去发现、挖掘出仅凭黑色命令行，也能够操作的、对你有现实意义的项目，来给你足够的反馈，让你知晓，你的学习路径是有意义的。



## 编程语言

或许你很难相信，对Python这门编程语言来讲，掌握了：

1. 变量、数组
2. 流程控制if、else
3. 循环for、while
4. 函数
5. 类

仅仅掌握这几个方面后，就能够实现你所见到的拥有各种功能的各种程序。

没错，就是这么五项基本东西的组合，却足以覆盖你要拥有的众多需求。这确实是一件神奇的事情！但这是初学者容易忽略的问题。因为一个摆在眼前的实际问题是：我就学习了这么几个东西，if、for这些看似简单到不能再简单的东西，难道就可以构建大千世界吗？！

在程序的世界里，这个答案是肯定的。

有了这个肯定的答复后，我想你会更有信心去学习这些前期单调乏味的基础知识。随着学习时间的积累，我们会从各个方面向你展示，你确实可以通过这些简单操作的各种神奇组合，去实现一个个让人惊讶的功能，包括那些让你眼花缭乱的图形界面。



## 编程与数学

写代码和与做数学，都是高强度的脑力劳动。但区别在于，编程追求的是可操作性的解答，而数学追求的是逻辑上的解答。一个简单的例子是，对于问题：整数2是否可以被开根号，表示为一个有理数？

- 数学的处理方式，或者关注重点是：通过反证法，可以说明，如果结果可以表示为有理数，则它会出现自相矛盾的情况。于是，数学的答案是，它不可以。
- 编程的侧重点是：计算机只能表示一个有限的数，于是要通过一些操作，尽可能地找到一个接近答案的数。



这里的重要区别点在于，编程追求的，是一些列可操作的步骤，所能解决的问题。这堆可操作的步骤，就是指令集，是你可以命令计算机做的操作。



## Python的计算操作

编程的目的，就是为了让计算机按照自己的想法干活。也即是，通过计算机语言，把自己想让计算机”干的活“，表达为计算机源代码(一堆操作指令)。

那什么是想让计算机干的活呢？例如：

- 让计算机为你挑选出一堆美女照片
- 让计算机修改一个游戏的代码，让游戏角色永生不死
- 让计算机写出一个财务系统，帮助自己管理财务

这些一个个让计算机具体做的事情，就是你想让计算机干的活儿。但是，你直接把这个想法告诉计算机，它是听不懂的。你必须把这些想法，转换为计算机可以理解的、精确无误的指令。这一**转换过程**就是编程。

那么，最近单的指令是什么呢？

那无疑就是把计算机当做计算器来使用。

那么，你可以在Python里面尝试各种复杂的四则运算：

```python
>>> 1 + 1

>>> sqrt(3**2 + 4**2)

>>> 3.14 * (3**2)
```

我们可以看出：

- 第二个例子，似乎是在运用勾股定理计算直角三角形的斜边。
- 而第三个例子，似乎是在计算一个半径为3的圆的面积。



那么，一个直接的问题是，每一次用勾股定理做计算、用圆面积公式做计算，其“模式”都是一样的，我能否把它提取出来？

这就是需要引入“变量”，来提取这个模式。

变量，代表的是计算机内存所分配给它的一块区域，它可以存储数据。你可以把它想象为一个空盒子，里面可以放数字、文字等等。

那么，如果将上面的勾股定理求值过程`sqrt(3**2 + 4**2)` 用变量表示，我们可以得到：

```Python
a = 3
b = 4

sqrt(a**2 + b**2)
```

那么，这么做有什么好处呢？

好处是：当你的两边长度不再是3和4这两个值，例如5和6，你不必再一次去把完整的过程写出来`sqrt(5**2 + 6**2)`，你只需要将变量的值更改一下就可以了。

或许你会说，这并没有什么工作的减少啊。那么，让我们再看一个复杂一些的例子。例如，让一个数3，做一下操作：

```Python
(3**2 + 3**3 + 3**4) / (3**3 + 3**6 + 3**9)
```

这时候，如果你要将3更换为5，可以想象，你不得不仔细地一个个去小心核对、并把3改成5。

但如果我们使用变量呢？

```python
a = 3
(a**2 + a**3 + a**4) / (a**3 + a**6 + a**9)
```

你可以直接把这段代码复制（引入函数后，连复制都不需要）了，只把第一行修改为`a=5`就行了。这时候，你是否能体会有变量的好处呢？

回过头去看，我们似乎还没有解释`a = 3` 这条语句。它的意思表示：把右边的值3，赋值给`a` 这个变量，也即是把3这个数据放进名为`a` 的内存分配各它的盒子里。

同理，`b = 5` 表示把5这个数据，放进名为`b` 的盒子里。



那么，问题来了，如果我想交换变量a和b的值呢？也就是让a保存b的值5，而让b保存a的值3。或许你会说，直接使用：

```Python
a = 5
b = 3
```

不可以吗？答案是，不可以！因为这里a之前的值是3，b之前的值是5仅仅是一个特例。我的要求是，无论之前a与b是什么值，你能否通过同样的代码，让a和b交换它们所保存的值？



或许，你会使用：

```Python
a = b
b = a
```

这样的语句。但是，让我们看看上面发生了什么事。

- 第一行，是把变量`b` 的当前值5，赋值给了变量`a` 。此时a的值就变成了5。
- 而下一句，再把a的值（此时，已经更新为5而不是3）赋值给b，也就是把5赋值给b。此时，b依旧为5。

也即是，你只是让a得到了b的值5，而b并未能够得到a的值3。

> 练习1：写一段程序，交换上述变量`a` 和`b` 的值。


