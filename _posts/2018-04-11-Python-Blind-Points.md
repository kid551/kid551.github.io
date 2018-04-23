---
layout: post
comments: true
title:  "Python Blind Points"
excerpt: "Some distinguish features of Python."
date:   2018-04-11
mathjax: true
---


### 2.1.1. Argument Passing

When known to the interpreter, the script name and additional arguments there after are turned into a list of strings and assigned to the `argv` variable in the `sys` module.

The length of the list is at least one; when no script and no arguments are given, `sys.argv[0]` is an empty string. **When the script name is given as `'-'` (meaning standard input), `sys.argv[0]` is set to `'-'`.** When [`-c`](../using/cmdline.html#cmdoption-c) *command* is used, `sys.argv[0]`is set to `'-c'`. When [`-m`](../using/cmdline.html#cmdoption-m) *module* is used, `sys.argv[0]` is set to the full name of the located module. Options found after [`-c`](../using/cmdline.html#cmdoption-c) *command* or [`-m`](../using/cmdline.html#cmdoption-m) *module* are not consumed by the Python interpreter’s option processing but left in `sys.argv`for the command or module to handle.

---

A second way of starting the interpreter is `python -c command [arg] ...`, which executes the statement(s) in *command*, analogous to the shell’s [`-c`](../using/cmdline.html#cmdoption-c) option. Since Python statements often contain spaces or other characters that are special to the shell, it is usually advised to quote *command* in its entirety with single quotes.

Some Python modules are also useful as scripts. These can be invoked using `python -m module [arg] ...`, which executes the source file for *module* as if you had spelled out its full name on the command line.

---

For continuation lines it prompts with the *secondary prompt*, by default three dots (`...`).

Continuation lines are needed when entering a multi-line construct. As an example, take a look at this [`if`](../reference/compound_stmts.html#if) statement:

```Python
>>> the_world_is_flat = True
>>> if the_world_is_flat:
...     print("Be careful not to fall off!")
...
Be careful not to fall off!
```

---

### 2.2.1. Source Code Encoding

By default, Python source files are treated as encoded in UTF-8. In that encoding, characters of most languages in the world can be used simultaneously in string literals, identifiers and comments — although the standard library only uses ASCII characters for identifiers, a convention that any portable code should follow. To display all these characters properly, your editor must recognize that the file is UTF-8, and it must use a font that supports all the characters in the file.

To declare an encoding other than the default one, __a special comment line should be added as the *first* line of the file__. The syntax is as follows:

```python
# -*- coding: encoding -*-
```

where *encoding* is one of the valid [`codecs`](../library/codecs.html#module-codecs) supported by Python.

For example, to declare that Windows-1252 encoding is to be used, the first line of your source code file should be:

```python
# -*- coding: cp-1252 -*-
```

One exception to the *first line* rule is when the source code starts with a [UNIX “shebang” line](appendix.html#tut-scripts). In this case, the encoding declaration should be added as the second line of the file. For example:

```python
#!/usr/bin/env python3
# -*- coding: cp-1252 -*-
```

---

Division (`/`) always returns a float. To do [floor division](../glossary.html#term-floor-division) and get an integer result (discarding any fractional result) you can use the `//` operator; to calculate the remainder you can use `%`:

```Python
>>> 17 / 3  # classic division returns a float
5.666666666666667
>>>
>>> 17 // 3  # floor division discards the fractional part
5
>>> 17 % 3  # the % operator returns the remainder of the division
2
```

In interactive mode, the last printed expression is assigned to the variable `_`. This means that when you are using Python as a desk calculator, it is somewhat easier to continue calculations, for example:

```Python
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```

This variable should be treated as read-only by the user. Don’t explicitly assign a value to it — _you would create an independent local variable with the same name masking the built-in variable with its magic behavior._

---

If you don’t want characters prefaced by `\` to be interpreted as special characters, you can use *raw strings* by adding an `r` before the first quote:

```Python
>>> print('C:\some\name')  # here \n means newline!
C:\some
ame
>>> print(r'C:\some\name')  # note the r before the quote
C:\some\name
```

---

String literals can span multiple lines. One way is using triple-quotes: `"""..."""` or `'''...'''`. End of lines are automatically included in the string, but it’s possible to prevent this by adding a `\` at the end of the line. The following example:

```Python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

produces the following output (**note that the initial newline is not included**):

```Python
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```

---

> From: https://www.codecademy.com/en/forum_questions/505ba3cfc6addb000200e33c
>
> As part of the Python course it is taught that in order to do a multiline comment one should use `"""triple quotes"""`. This is wrong. Python only has one way of doing comments and that is using `#`.
>
> Triple quotes are treated as regular strings with the exception that they can span multiple lines. By regular strings I mean that if they are not assigned to a variable they will be immediately garbage collected as soon as that code executes. hence are **not ignored** by the interpreter in the same way that `#a comment` is.
>
> The only exception to the garbage collection fact is when they are placed **immediately after a function or class definition or on top of a module**, in which case they are called **docstrings** and made available via the special variable `myobj.__doc__`.

----

In addition to indexing, *slicing* is also supported. While indexing is used to obtain individual characters, *slicing* allows you to obtain substring:

```Python
>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
'Py'
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
'tho'
```

Note how the __start is always included, and the end always excluded__. This makes sure that `s[:i] + s[i:]` is always equal to `s`:

```Python
>>> word[:2] + word[2:]
'Python'
>>> word[:4] + word[4:]
'Python'
```

Slice indices have useful defaults; an omitted first index defaults to zero, an omitted second index defaults to the size of the string being sliced.

```Python
>>> word[:2]   # character from the beginning to position 2 (excluded)
'Py'
>>> word[4:]   # characters from position 4 (included) to the end
'on'
>>> word[-2:]  # characters from the second-last (included) to the end
'on'
```

---

Attempting to use an index that is too large will result in an error:

```Python
>>> word[42]  # the word only has 6 characters
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

However, out of range slice indexes are handled gracefully when used for slicing:

```Python
>>> word[4:42]
'on'
>>> word[42:]
''
```

---

```Python
>>> # Fibonacci series:
... # the sum of two elements defines the next
... a, b = 0, 1
>>> while b < 10:
...     print(b)
...     a, b = b, a+b
```

In Python, like in C,

- __any non-zero integer value is true__; zero is false.

The condition may also be a string or list value, in fact:

- __any sequence; anything with a non-zero length is true__, empty sequences are false.

The test used in the example is a simple comparison.

The standard __comparison operators__ are written the same as in C:

1. `<` (less than),
2. `>` (greater than),
3. `==` (equal to),
4. `<=`(less than or equal to),
5. `>=` (greater than or equal to) and
6. `!=` (not equal to).

>  If you use other operations like assignment, you'll get an error:

```Python
>>> if(n = 2):
   File "<stdin>", line 1
    if(n = 2):
         ^
SyntaxError: invalid syntax
```

---

If you need to __modify the sequence__ you are iterating over while inside the loop (for example to duplicate selected items), **it is recommended that you first make a copy**. Iterating over a sequence does not implicitly make a copy. The slice notation makes this especially convenient:

```Python
>>> for w in words[:]:  # Loop over a slice copy of the entire list.
...     if len(w) > 6:
...         words.insert(0, w)
...
>>> words
['defenestrate', 'cat', 'window', 'defenestrate']
```

With `for w in words:`, the example would attempt to create an infinite list, inserting `defenestrate` over and over again.

---

A strange thing happens if you just print a range:

```Python
>>> print(range(10))
range(0, 10)
```

In many ways the object returned by [`range()`](../library/stdtypes.html#range) behaves as if it is a list, but in fact it isn’t. It is an object which returns the __successive items of the desired sequence__ when you iterate over it, but it **doesn’t really make the list**, thus saving space.

We say such an object is *iterable*, that is, suitable as a target for functions and constructs that expect something from which they can obtain successive items until the supply is exhausted. We have seen that the [`for`](../reference/compound_stmts.html#for) statement is such an *iterator*. The function [`list()`](../library/stdtypes.html#list) is another; **it creates lists from iterables**:

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```

---

Loop statements may have an `else` clause; it is executed when the loop terminates through exhaustion of the list (with [`for`](../reference/compound_stmts.html#for)) or when the condition becomes false (with [`while`](../reference/compound_stmts.html#while)), but not when the loop is terminated by a [`break`](../reference/simple_stmts.html#break) statement. This is exemplified by the following loop, which searches for prime numbers:

```Python
>>> for n in range(2, 10):
...     for x in range(2, n):
...         if n % x == 0:
...             print(n, 'equals', x, '*', n//x)
...             break
...     else:
...         # loop fell through without finding a factor
...         print(n, 'is a prime number')
...
2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

(Yes, this is the correct code. Look closely: the `else` clause belongs to the [`for`](../reference/compound_stmts.html#for) loop, **not** the [`if`](../reference/compound_stmts.html#if) statement.)

---

The [`pass`](../reference/simple_stmts.html#pass) statement does nothing. It can be used when a statement is required syntactically but the program requires no action.

Another place [`pass`](../reference/simple_stmts.html#pass) can be used is as a place-holder for a function or conditional body when you are working on new code, allowing you to keep thinking at a more abstract level. The [`pass`](../reference/simple_stmts.html#pass) is silently ignored:

```Python
>>> def initlog(*args):
...     pass   # Remember to implement this!
```

---

The actual parameters (arguments) to a function call are introduced in the local symbol table of the called function when it is called; thus, arguments are passed using *call by value* (where the *value* is always an object *reference*, not the value of the object).

[**Actually, *call by object reference* would be a better description, since if a mutable object is passed, the caller will see any changes the callee makes to it (items inserted into a list).**]

When a function calls another function, a new local symbol table is created for that call.

A function definition introduces the function name in the current symbol table. The value of the function name has a type that is recognized by the interpreter as a user-defined function. This value can be assigned to another name which can then also be used as a function. This serves as a general renaming mechanism:

```Python
>>> fib
<function fib at 10042ed0>
>>> f = fib
>>> f(100)
0 1 1 2 3 5 8 13 21 34 55 89
```

Coming from other languages, you might object that `fib` is not a function but a procedure since it doesn’t return a value. In fact, even functions without a [`return`](../reference/simple_stmts.html#return) statement do return a value, albeit a rather boring one. This value is called `None`(it’s a built-in name). Writing the value `None` is normally suppressed by the interpreter if it would be the only value written. You can see it if you really want to using [`print()`](../library/functions.html#print):

```Python
>>> fib(0)
>>> print(fib(0))
None
```

---

The default values are evaluated at the point of function definition in the *defining* scope, so that

```Python
i = 5

def f(arg=i):
    print(arg)

i = 6
f()
```

will print `5`.

**Important warning:** The default value is evaluated only once. This makes a difference when the default is a _mutable object_ such as a list, dictionary, or instances of most classes. For example, the following function accumulates the arguments passed to it on subsequent calls:

```Python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
```

This will print

```Python
[1]
[1, 2]
[1, 2, 3]
```

If you don’t want the default to be shared between subsequent calls, you can write the function like this instead:

```Python
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```

---

Functions can also be called using [keyword arguments](../glossary.html#term-keyword-argument) of the form `kwarg=value`. For instance, the following function:

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
```

accepts one required argument (`voltage`) and three optional arguments (`state`, `action`, and `type`). This function can be called in any of the following ways:

```python
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

but all the following calls would be invalid:

```python
parrot()                     # required argument missing
parrot(voltage=5.0, 'dead')  # non-keyword argument after a keyword argument
parrot(110, voltage=220)     # duplicate value for the same argument
parrot(actor='John Cleese')  # unknown keyword argument
```

In a function call, keyword arguments must follow positional arguments. All the keyword arguments passed must match one of the arguments accepted by the function (e.g. `actor` is not a valid argument for the `parrot` function), and **their order is not important**.

---

- When a final formal parameter of the form `**name` is present, it receives a **dictionary** (see [Mapping Types — dict](../library/stdtypes.html#typesmapping)) containing all keyword arguments except for those corresponding to a formal parameter.
- This may be combined with a formal parameter of the form `*name` (described in the next subsection) which receives a **tuple containing** the positional arguments beyond the formal parameter list. (`*name` must occur before `**name`.)

For example, if we define a function like this:

```python
def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)

    # Here, the 'arguments' variable is a tuple.
    for arg in arguments:
        print(arg)
    print("-" * 40)

    # Here, the 'keywords' variable is a dictionary.
    for kw in keywords:
        print(kw, ":", keywords[kw])
```

It could be called like this:

```python
cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")
```

and of course it would print:

```Txt
-- Do you have any Limburger ?
-- I'm sorry, we're all out of Limburger
It's very runny, sir.
It's really very, VERY runny, sir.
----------------------------------------
shopkeeper : Michael Palin
client : John Cleese
sketch : Cheese Shop Sketch
```

Note that the order in which the keyword arguments are printed is guaranteed to match the order in which they were provided in the function call.

---

Normally, these `variadic` arguments will be last in the list of formal parameters, because they scoop up all remaining input arguments that are passed to the function. Any formal parameters which occur after the `*args` parameter are ‘keyword-only’ arguments, meaning that they can only be used as keywords rather than positional arguments.

```Python
>>> def concat(*args, sep="/"):
...     return sep.join(args)
...
>>> concat("earth", "mars", "venus")
'earth/mars/venus'
>>> concat("earth", "mars", "venus", sep=".")
'earth.mars.venus'
```

---

### 4.7.4. Unpacking Argument Lists

The reverse situation occurs when the arguments are already in a list or tuple but need to be unpacked for a function call requiring separate positional arguments. For instance, the built-in [`range()`](../library/stdtypes.html#range) function expects separate *start* and *stop*arguments. If they are not available separately, write the function call with the __`*`-operator__ to unpack the arguments out of a **list or tuple**:

```Python
>>> list(range(3, 6))            # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]
```

In the same fashion, **dictionaries** can deliver keyword arguments with the `**`-operator:

```Python
>>> def parrot(voltage, state='a stiff', action='voom'):
...     print("-- This parrot wouldn't", action, end=' ')
...     print("if you put", voltage, "volts through it.", end=' ')
...     print("E's", state, "!")
...
>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> parrot(**d)
-- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !
```

To summarize:

- use _star operation_ (*) to unpack list or tuple argument.
- use _double star operation_ (**) to unpack dictionary argument.

---

### 4.7.5. Lambda Expressions

Small anonymous functions can be created with the [`lambda`](../reference/expressions.html#lambda) keyword. This function returns the sum of its two arguments: `lambda a, b: a+b`. Lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. __Semantically, they are just syntactic sugar for a normal function definition__. Like nested function definitions, lambda functions can reference variables from the containing scope:

```Python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```

The above example uses a lambda expression to return a function. Another use is to pass a small function as an argument:

```Python
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

---

### 4.7.7. Function Annotations

[Function annotations](../reference/compound_stmts.html#function) are completely optional metadata information about the types used by user-defined functions (see [**PEP 484**](https://www.python.org/dev/peps/pep-0484) for more information).

Annotations are stored in the `__annotations__` attribute of the function as a dictionary and have no effect on any other part of the function.

- __Parameter annotations__ are defined by a colon after the parameter name, followed by an expression evaluating to the value of the annotation.
- __Return annotations__ are defined by a literal `->`, followed by an expression, between the parameter list and the colon denoting the end of the [`def`](../reference/compound_stmts.html#def) statement.

The following example has a positional argument, a keyword argument, and the return value annotated:

```Python
>>> def f(ham: str, eggs: str = 'eggs') -> str:
...     print("Annotations:", f.__annotations__)
...     print("Arguments:", ham, eggs)
...     return ham + ' and ' + eggs
...
>>> f('spam')
Annotations: {'ham': <class 'str'>, 'return': <class 'str'>, 'eggs': <class 'str'>}
Arguments: spam eggs
'spam and eggs'
```

> Here, the original form of  `eggs: str = 'eggs'` is `eggs = 'eggs'`. Thus the only annotation part is `: str`. So in all, the added parts are just three: `: str`, `: str`, and `-> str`.

---

#### `list.insert(i, x)`

Insert an item at a given position.

- The first argument is the index of the element before which to insert, so `a.insert(0,x)` inserts at the front of the list,
- and `a.insert(len(a), x)` is equivalent to `a.append(x)`.

> If we think deeper, there're many considerations behind the meaning of first argument. Why will we set the first argument as the index of element __before__ which to insert, instead of __after__ which to insert?
>
> Because, it's more intuitive to append one element using `a.insert(len(a), x)`. If we use the __after__ mechanism, how can we construct one expression to add element at the head of this list? Should we use `a.insert(-1, x)`? But in Python, the `-1` position commonly means the last element. This will introduce confusions in internal mechanisms.

---

#### `list.index(x[, start[, end]])`

Return __zero-based index__ in the list of the first item whose value is *x*. Raises a [`ValueError`](../library/exceptions.html#ValueError) if there is no such item.

The optional arguments *start* and *end* are interpreted as in the __slice notation__ and are used to limit the search to a particular subsequence of the list. The __returned index__ is computed _relative to the beginning of the full sequence_ rather than the *start* argument.

---

You might have noticed that methods like `insert`, `remove` or `sort` that only modify the list have no return value printed – they return the default `None`. [[1\]](#id3) This is a design principle for all mutable data structures in Python.

---

The built-in [`sorted()`](#sorted) function is guaranteed to be stable. A **sort is stable** if it _guarantees not to change the relative order of elements that **compare equal**_ — this is helpful for sorting in multiple passes (for example, sort by department, then by salary grade).

---

It is also possible to use a list as a queue, where the first element added is the first element retrieved (“first-in, first-out”); however, **lists are not efficient** for this purpose. While appends and pops from the end of list are fast, _doing inserts or pops from the beginning of a list is slow_ (because **all of the other elements have to be shifted by one**).

To implement a queue, use [`collections.deque`](../library/collections.html#collections.deque) which was designed to have fast appends and pops **from both ends**. For example:

```Python
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

---

#### *class* `collections.``deque`([*iterable*[, *maxlen*]])

Returns a new deque object initialized left-to-right (using [`append()`](#collections.deque.append)) with data from *iterable*. If *iterable* is not specified, the new deque is empty.

Deques are a generalization of stacks and queues (the name is pronounced “deck” and is short for **“double-ended queue”**). Deques support thread-safe, memory efficient appends and pops from either side of the deque with approximately the same O(1) performance in either direction.

If *maxlen* is not specified or is `None`, deques may grow to an arbitrary length. Otherwise, the deque is bounded to the specified maximum length.

Once a bounded length deque is full, _when new items are added, a corresponding number of items are discarded from the **opposite end**_. Bounded length deques provide functionality similar to the `tail` filter in Unix. They are also useful for tracking transactions and other pools of data where only the most recent activity is of interest.

---

### 5.1.3. List Comprehensions

List comprehensions provide a concise way to create lists. Common applications are to make new lists where each element is the result of some operations applied to each member of another sequence or iterable, or to create a subsequence of those elements that satisfy a certain condition.

For example, assume we want to create a list of squares, like:

```Python
>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Note that this creates (or overwrites) a variable named `x` that **still exists** after the loop completes. We can calculate the list of squares without any side effects using:

```Python
squares = list(map(lambda x: x**2, range(10)))
```

or, **equivalently**:

```Python
squares = [x**2 for x in range(10)]
```

which is more concise and readable.

---

A list comprehension consists of brackets containing an expression followed by a [`for`](../reference/compound_stmts.html#for) clause, then zero or more [`for`](../reference/compound_stmts.html#for) or [`if`](../reference/compound_stmts.html#if)clauses. The result will be a new list resulting from evaluating the expression in the context of the [`for`](../reference/compound_stmts.html#for) and [`if`](../reference/compound_stmts.html#if) clauses which follow it. For example, this listcomp combines the elements of two lists if they are not equal:

```Python
>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

and it’s equivalent to:

```Python
>>> combs = []
>>> for x in [1,2,3]:
...     for y in [3,1,4]:
...         if x != y:
...             combs.append((x, y))
...
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

Note __how the order of the [`for`](../reference/compound_stmts.html#for) and [`if`](../reference/compound_stmts.html#if) statements is the same in both these snippets__.

---

```Python
SyntaxError: invalid syntax
>>> # flatten a list using a listcomp with two 'for'
>>> vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

List comprehensions can contain complex expressions and nested functions:

```python
>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

---

For Python's `zip` code:

```Python
def myzip(*iterables):
    # zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]

    # if the iterators is not empty,
    # the loop can always keep running
    while iterators:
        result = []
        for it in iterators:
            # return 'sentinel' if 'it' is exhausted
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            # once run 'next(it, sentinel)', it
            # will deal with another item of iterators.
            result.append(elem)

        # i.e. return the same position list.
        yield tuple(result)


cc = myzip('ABCD', 'xy')
# the 'zip' won't execute,
# until this 'list' function call
list(cc)
```

It's a piece of elaborate code to implement _zip_ function.

- `elem = next(it, sentinel)` pop ups the current `it` element, and move to the next position. The second optional parameter is used to _return_ a specified element `sentinel` when the iterator `it` is exhausted. If the second optional parameter is not specified, it'll return empty object, i.e. the `object()`.
- `result.append(elem)`: pay attention, after this line is executed, the `elem` will deal with next element of `iterators`, instead of next element of `it`. Because there's no loop to deal with `it`.
- `yield tuple(result)`: `yield` is almost the same as `return`. It returns, let's say position `i`, the array of each `i`th position of `iterators` element.

---

## 27.3. [`pdb`](#module-pdb) — The Python Debugger

> Authtor | Terence Xie

我想，我之前把`pdb`的定位给弄错了。我一直以为，它会像是一般的IDE那样，带领你去一行行**显示地**查看代码运行到哪里。可事实上是，`pdb`是一款针对于命令行的工具，或者更准确的说，是针对于REPL(read-evaluate-print loop)的工具。

你不能够指望它一次性把所有东西做完。你脑海中的模型，应该是把它当做一个命令行的交互式工具。如同shell，终端必须在读取了你的命令行，才能够运行处理，进而打印出你要的数据。之后，再一次进入等待读取的状态。所以：

- 当你想要让程序运行到下一个步骤，你输入`n(ext)`这个指令，它就带领你到下一行代码去。
- 当你想要查看在当前状态下，某个变量的值，你便在命令行里输入变量的名字。
- 当你想要查看当前状态下这一行，到底是怎么被运行到这个地方的，你只需要输入`w(here)` 就可以了。
- 当你想要为某一行加入断点，你只需要输入`b(reak)`就可以了。

也即是，`pdb`是一个完全为命令行所定制的工具。在命令行下，没有GUI，有的只是你与终端之间的不断交流。你不能够像GUI那样，妄图通过一个界面，了解多方面的信息。

在命令行这个环境下，你一次只能了解某一方面的一个信息。并且，这还需要你使用相应的命令，才能够调取所需要的信息。而不是像GUI那样，通过着色、或者多变量展示窗口，可以一次性展示多种信息。

认清楚了这个本质的不同，再去使用`pdb`，便能体会其精妙之处。在命令行这个大环境下，`pdb`的设计，堪称精湛。使用过程流畅顺滑，是一款出色的命令行下的产品。

---

A tuple consists of a number of values separated by commas, for instance:

```Python
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
>>> # Tuples may be nested:
... u = t, (1, 2, 3, 4, 5)
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
>>> # Tuples are immutable:
... t[0] = 88888
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> # but they can contain mutable objects:
... v = ([1, 2, 3], [3, 2, 1])
>>> v
([1, 2, 3], [3, 2, 1])
```

---

A special problem is the construction of tuples containing 0 or 1 items: the syntax has some extra quirks to accommodate these. Empty tuples are constructed by an empty pair of parentheses; a tuple with one item is constructed by __following a value with a comma__ (it is **not sufficient** to enclose a single value in parentheses). Ugly, but effective. For example:

```Python
>>> empty = ()
>>> singleton = 'hello',    # <-- note trailing comma
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```

---

The statement `t = 12345, 54321, 'hello!'` is an example of *tuple packing*: the values `12345`, `54321` and `'hello!'` are packed together in a tuple. The reverse operation is also possible:

```Python
>>> x, y, z = t
```

This is called, appropriately enough, *sequence unpacking* and works for any sequence on the right-hand side. Sequence unpacking requires that there are as many variables on the left side of the equals sign as there are elements in the sequence. Note that multiple assignment is really just a combination of tuple packing and sequence unpacking.

---

## 5.4. Sets

Python also includes a data type for *sets*. A set is an unordered collection with no duplicate elements. Basic uses include __membership testing__ and __eliminating duplicate entries__. Set objects also support mathematical operations like union, intersection, difference, and symmetric difference.

Curly braces or the [`set()`](../library/stdtypes.html#set) function can be used to create sets. **Note: to create an empty set you have to use `set()`, not `{}`;** the latter creates an empty dictionary, a data structure that we discuss in the next section.

Here is a brief demonstration:

```Python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # show that duplicates have been removed
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # fast membership testing
True
>>> 'crabgrass' in basket
False

>>> # Demonstrate set operations on unique letters from two words
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # unique letters in a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # letters in a but not in b
{'r', 'd', 'b'}
>>> a | b                              # letters in a or b or both
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # letters in both a and b
{'a', 'c'}
>>> a ^ b                              # letters in a or b but not both
{'r', 'd', 'b', 'm', 'z', 'l'}
```

---

Calling `d.keys()` will return a *dictionary view* object. It supports operations like membership test and iteration, but its contents are not independent of the original dictionary – it is only a *view*.

---

The [`dict()`](../library/stdtypes.html#dict) constructor builds dictionaries directly from sequences of key-value pairs:

```Python
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

---

When the keys are simple strings, it is sometimes easier to specify pairs using keyword arguments:

```Python
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

---

## 5.6. Looping Techniques

1. When looping through dictionaries, the key and corresponding value can be retrieved at the same time using the `items()` method.

```Python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
...
gallahad the pure
robin the brave
```

2. When looping through a sequence, the position index and corresponding value can be retrieved at the same time using the [`enumerate()`](../library/functions.html#enumerate) function.

```Python
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe
```

3. To loop over two or more sequences at the same time, the entries can be paired with the [`zip()`](../library/functions.html#zip) function.

```Python
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

---

Note that in Python, unlike C, **assignment cannot occur inside expressions**. C programmers may grumble about this, but it avoids a common class of problems encountered in C programs: typing `=` in an expression when `==` was intended.

---

Sequence objects may be compared to other objects with the same sequence type. The comparison uses *lexicographical*ordering: first the first two items are compared, and if they differ this determines the outcome of the comparison; if they are equal, the next two items are compared, and so on, until either sequence is exhausted.

…...

If one sequence is an initial sub-sequence of the other, the shorter sequence is the smaller (lesser) one.

Note that comparing objects of different types with `<` or `>` is legal provided that the objects have appropriate comparison methods. For example, mixed numeric types are compared according to their numeric value, so 0 equals 0.0, etc. Otherwise, rather than providing an arbitrary ordering, the interpreter will raise a [`TypeError`](../library/exceptions.html#TypeError) exception.

---

If you quit from the Python interpreter and enter it again, the definitions you have made (functions and variables) are lost.

Therefore, if you want to write a somewhat longer program, you are better off using a text editor to prepare the input for the interpreter and running it with that file as input instead. This is known as creating a *script*.

As your program gets longer, you may want to split it into several files for easier maintenance. You may also want to use a handy function that you’ve written in several programs without copying its definition into each program.

To support this, Python has a way to __put definitions in a file and use them in a script or in an interactive instance of the interpreter__. Such a file is called a *module*; definitions from a module can be *imported* into other modules or into the *main*module (the collection of variables that you have access to in a script executed at the top level and in calculator mode).

A **module** is _a file containing Python definitions and statements_. **The file name is the module name with the suffix `.py`appended.**

For instance, use your favorite text editor to create a file called `fibo.py` in the current directory with the following contents:

```Python
# Fibonacci numbers module

def fib(n):    # write Fibonacci series up to n
    a, b = 0, 1
    while b < n:
        print(b, end=' ')
        a, b = b, a+b
    print()

def fib2(n):   # return Fibonacci series up to n
    result = []
    a, b = 0, 1
    while b < n:
        result.append(b)
        a, b = b, a+b
    return result
```

Now enter the Python interpreter and import this module with the following command:

```Python
>>> import fibo
```

This does not enter the names of the functions defined in `fibo` directly in the current symbol table; it only enters the module name `fibo` there. Using the module name you can access the functions:

```Python
>>> fibo.fib(1000)
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

# Here, '__name__' stores the module name, which is
# a verification of above 'module name' discussions.
>>> fibo.__name__
'fibo'
```

If you intend to use a function often you can assign it to a local name:

```Python
>>> fib = fibo.fib
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

---

# 29.4. [`__main__`](https://docs.python.org/3/library/__main__.html#module-__main__) — Top-level script environment[¶](https://docs.python.org/3/library/__main__.html#module-__main__)

------

`'__main__'` is the name of the scope in which top-level code executes. A module’s __name__ is set equal to `'__main__'` when read from standard input, a script, or from an interactive prompt.

A module can discover whether or not it is running in the main scope by checking its own `__name__`, which allows a common idiom for conditionally executing code in a module when it is run as a script or with `python -m` but not when it is imported:

```Python
if __name__ == "__main__":
    # execute only if run as a script
    main()
```

---

Each module has its **own private symbol table**, which is used as the global symbol table by all functions defined in the module. Thus, the author of a module can use global variables in the module without worrying about accidental clashes with a user’s global variables.

Modules can import other modules. It is customary but not required to place all [`import`](../reference/simple_stmts.html#import) statements at the beginning of a module (or script, for that matter). **The imported module names are placed in the importing module’s global symbol table**.

There is a variant of the [`import`](../reference/simple_stmts.html#import) statement that imports names from a module directly into the importing module’s symbol table. For example:

```Python
>>> from fibo import fib, fib2
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

**This does not introduce the module name** from which the imports are taken in the local symbol table (so in the example, `fibo` is not defined).

There is even a variant to import all names that a module defines:

```Python
>>> from fibo import *
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

This imports all names **except** those beginning with an underscore (`_`). In most cases Python programmers do not use this facility since it introduces an unknown set of names into the interpreter, **possibly hiding some things you have already defined**.

> **Note**:
>
> For efficiency reasons, each module is **only imported once per interpreter session**. Therefore, **if you change your modules, you must restart the interpreter** – or, if it’s just one module you want to test interactively, use[`importlib.reload()`](../library/importlib.html#importlib.reload), e.g. `import importlib; importlib.reload(modulename)`.

---

### 6.1.1. Executing modules as scripts

When you run a Python module with

```Bash
python fibo.py <arguments>
```

the code in the module will be executed, just as if you imported it, but with the `__name__` set to `"__main__"`. That means that by adding this code at the end of your module:

```Python
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
```

you can make the file usable as a script as well as an importable module, because the code that parses the command line only runs if the module is executed as the “main” file:

```Python
$ python fibo.py 50
1 1 2 3 5 8 13 21 34
```

If the module is imported, the code is not run:

```Python
>>> import fibo
>>>
```

This is often used either to **provide a convenient user interface** to a module, or **for testing purposes** (_running the module as a script executes a test suite_).

---

### 6.1.2. The Module Search Path

When a module named `spam` is imported, the interpreter

- first searches for a **built-in module** with that name.
- If not found, it then searches for a file named `spam.py` in a list of **directories given by the variable [`sys.path`](../library/sys.html#sys.path)**. [`sys.path`](../library/sys.html#sys.path) is initialized from these locations:
  - The directory containing the input script (or the current directory when no file is specified).
  - [`PYTHONPATH`](../using/cmdline.html#envvar-PYTHONPATH) (a list of directory names, with the same syntax as the shell variable `PATH`).
  - The installation-dependent default.

> Note
>
> On file systems which support symlinks, the directory containing the input script is calculated after the symlink is followed. In other words the directory containing the symlink is **not** added to the module search path.

After initialization, Python programs can modify [`sys.path`](../library/sys.html#sys.path).

The directory containing the script being run is placed at the beginning of the search path, **ahead of the standard library path**.

_This means that scripts in that directory will be loaded instead of modules of the same name in the library directory_.

This is an error unless the replacement is intended. See section [Standard Modules](#tut-standardmodules) for more information.

---

### 6.1.3. “Compiled” Python files

To speed up loading modules, Python caches the compiled version of each module in the `__pycache__` directory under the name `module.*version*.pyc`, where the version encodes:

- the format of the compiled file;
- it generally contains the Python version number.

For example, in **CPython** release **3.3** the compiled version of spam.py would be cached as `__pycache__/spam.cpython-33.pyc`.

This naming convention allows compiled modules from different releases and different versions of Python to coexist.

---

## 6.2. Standard Modules

Python comes with a library of standard modules, described in a separate document, the Python Library Reference (“Library Reference” hereafter). Some modules are built into the interpreter; these provide access to operations that are not part of the core of the language but are nevertheless built in,

- either for efficiency or to
- provide access to _operating system primitives_ such as system calls.

One particular module deserves some attention: [`sys`](../library/sys.html#module-sys), which is built into every Python interpreter.

- The variables `sys.ps1` and
-  `sys.ps2` define the strings used as primary and secondary prompts:

```Python
>>> import sys
>>> sys.ps1
'>>> '
>>> sys.ps2
'... '
>>> sys.ps1 = 'C> '
C> print('Yuck!')
Yuck!
C>
```

These two variables are only defined if the interpreter is in interactive mode.

The variable `sys.path` is a list of strings that determines the interpreter’s search path for modules. It is initialized to

- a default path taken from the environment variable [`PYTHONPATH`](../using/cmdline.html#envvar-PYTHONPATH), or from
- a built-in default if [`PYTHONPATH`](../using/cmdline.html#envvar-PYTHONPATH) is not set.

You can modify it using standard list operations:

```Python
>>> import sys
>>> sys.path.append('/ufs/guido/lib/python')
```

---

The built-in function [`dir()`](../library/functions.html#dir) is used to find out **which names a module defines**. It returns a sorted list of strings:

```Python
>>> import fibo, sys


# For this example, it'll show which
# names this 'fibo' module defines.
>>> dir(fibo)
['__name__', 'fib', 'fib2']


# Another example of showing which names
# the default python module 'sys' defines.
>>> dir(sys)  
['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
 '__package__', '__stderr__', '__stdin__', '__stdout__',
 '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
 '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
 'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
 'call_tracing', 'callstats', 'copyright', 'displayhook',
 'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
 'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
 'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
 'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
 'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
 'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
 'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
 'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
 'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
 'thread_info', 'version', 'version_info', 'warnoptions']
```

---

## 6.4. Packages

Packages are a way of structuring Python’s module namespace by using “dotted module names”. For example, the module name `A.B` designates a submodule named `B` in a package named `A`.

- Just like the use of **modules** saves the *authors of different modules* from having to worry about each other’s global variable names,
- the use of **dotted module names** saves the *authors of multi-module packages* like NumPy or the Python Imaging Library from having to worry about each other’s module names.

Suppose you want to design a collection of modules (a “package”) for the uniform handling of sound files and sound data.

- There are many different **sound file formats** (usually recognized by their extension, for example: `.wav`, `.aiff`, `.au`), so you may need to create and maintain a growing collection of modules for the conversion between the various file formats.
- There are also many **different operations** you might want to perform on sound data (such as mixing, adding echo, applying an equalizer function, creating an artificial stereo effect), so in addition you will be writing a never-ending stream of modules to perform these operations.

Here’s a possible structure for your package (expressed in terms of a hierarchical filesystem):

```Python
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

When importing the package, Python searches through the directories on `sys.path` looking for the package subdirectory.

The `__init__.py` files are:

-  required to make Python **treat the directories as containing packages**;
- this is done to **prevent directories** with a common name, such as `string`, **from unintentionally hiding valid modules** that occur later on the *module search path*. (As the exist of `__init__.py` file, this directory will be treated as *package*, which will not be treated as *module*.)

In the simplest case, `__init__.py` can just be an empty file, but it can also execute initialization code for the package or set the `__all__` variable, described later.

---

Note that when using `from package import item`, the **item** can be:

- either a **submodule** (or **subpackage**) of the package,
- or some other name defined in the package, like a **function**, **class** or **variable**.

The `import` statement

- first tests whether the item is *defined in the package*;
- if not, it assumes it is a *module and attempts to load it*.
- If it fails to find it, an [`ImportError`](../library/exceptions.html#ImportError)exception is raised.

Contrarily, when using syntax like `import item.subitem.subsubitem`,

- each item except for the last must be a **package**;
- the last item can be a module or a package but can **NOT be a class or function or variable** defined in the previous item.

Thus, it's really different usage of those two *import* action.

---

### 6.4.1. Importing * From a Package

Now **what happens** when the user writes `from sound.effects import *`?

Ideally, one would hope that this somehow goes out to the filesystem, finds which submodules are present in the package, and imports them all. **This could take a long time** and importing sub-modules might have unwanted side-effects that should only happen when the sub-module is explicitly imported.

The only solution is *for the package author* to **provide an explicit index of the package**. The [`import`](../reference/simple_stmts.html#import) statement uses the following convention:

- if a package’s `__init__.py` code defines a list named `__all__`, it is taken to be the list of module names that should be imported when `from package import *` is encountered.

It is up to the package author to keep this list up-to-date when a new version of the package is released. Package authors may also decide not to support it, if they don’t see a use for importing * from their package. For example, the file `sound/effects/__init__.py` could contain the following code:

```Python
__all__ = ["echo", "surround", "reverse"]
```

This would mean that `from sound.effects import *` would import the three named submodules of the `sound` package.

- If `__all__` is not defined, the statement `from sound.effects import *` does *not* import all submodules from the package `sound.effects` into the current namespace; it **only ensures that the package `sound.effects` has been imported** (possibly running any initialization code in `__init__.py`) and then **imports whatever names are defined in the package**.
  - This includes any names defined (and submodules explicitly loaded) by `__init__.py`.
  - It also includes any submodules of the package that were explicitly loaded by previous [`import`](../reference/simple_stmts.html#import) statements.

Consider this code:

```Python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```

In this example, the `echo` and `surround` modules are imported in the current namespace because they are defined in the `sound.effects` package when the `from...import` statement is executed. (This also works when `__all__` is defined.)

Although certain modules are designed to export only names that follow certain patterns when you use `import *`, *it is still considered **bad practice** in production code*.

Remember, there is **nothing wrong** with using `from Package import specific_submodule`! In fact, this is the **recommended notation** unless the importing module needs to use submodules with the same name from different packages.

---

You can also write relative imports, with the `from module import name` form of import statement. These imports use leading dots to indicate the current and parent packages involved in the relative import. From the `surround` module for example, you might use:

```Python
from . import echo
from .. import formats
from ..filters import equalizer
```

Note that **relative imports** are based on the **name of the current module**.

> **<u>Note</u>**: Since the name of the main module is always `"__main__"`, **modules intended for use as the main module** of a Python application **must always use absolute imports**.
>
> (Explanation: ususally, each module's name is stored in the variable '\__name\__', which is used to locate the position of *relative import*. But when this module is used as  main module, its variable  '\__name\__' will always be assigned as valule `"__main__"`, which can NOT be used to locate relative import.)

---
