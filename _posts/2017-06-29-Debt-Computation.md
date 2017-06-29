---
layout: post
comments: true
title:  "Mortgage Debt Computation"
excerpt: "One record for computing debt rate"
date:   2017-06-29
mathjax: true
---

### Capital and Interest Computation

Capital | Interest 
 ------------------ | ------------- 
 $$a$$ | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0 
 $$a\times (1-r)$$ | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$$a\times r$$ 
 $$a\times (1-r)^2$$ | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$$a\times (1-r)\times r$$ 
 $$a\times (1-r)^3$$ | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$$a\times (1-r)^2\times r$$ 
 ... | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;... 
 
 
 - Total Interest
 
$$
\begin{align*}
a \times r [ 1 + (1-r) + (1-r)^2 + (1-r)^3 + \cdots + (1-r)^{35} ] &= I \\
a \times r \cdot \frac{(1-r)^{36}-1}{1-r-1} &= I \\
a \cdot [1-(1-r)^{36}] &= I \\
a \cdot (1-r)^{36} &= a - I \\
(1-r)^{36} &= \frac{a-I}{a} \\
1-r &= \sqrt[36]{\frac{a-I}{a}} \\
r &= 1 - \sqrt[36]{\frac{a-I}{a}} 
\end{align*}
$$

or  

$$
\begin{align*}
(1-r)^{36} &= \frac{a-I}{a} \\
36 \ln{(1-r)} &= \ln{\frac{a-I}{a}} \\
\ln{(1-r)} &= \frac{\ln{(\frac{a-I}{a})}}{36} \\
1-r &= \exp{[\frac{\ln{\frac{a-I}{a}}}{36}]} \\
r &= 1 - \exp{[\frac{\ln{\frac{a-I}{a}}}{36}]}
\end{align*}
$$


### Fixed monthly payment for a fixed rate mortgage

- **c**: montly payments.
- **r**: montly interest rate (*since the quoted yearly percentage rate is not a compounded rate, the montly percentage rate is simple the yearly percentage rate divided by 12*.)
- **N**: number of monthly payment, called the loan "term".
- **p**: amount borrowed, known as loan's principal.

In the standardized calculations used in US:

$$c = \frac{r\cdot p}{1-(1+r)^{-N}}=\frac{p\cdot r\cdot (1+r)^N}{(1+r)^N-1}$$

**Derivation**

> Amount owned this month = amount owned from the previous month + interest on this amount - fixed amount paid every month

1st term:

$$(1+r)\cdot p - c$$

2nd term:

$$[(1+r)\cdot p - c](1+r)-c = (1+r)^2p-[1+(1+r)]\cdot c$$

3rd term:

$$[(1+r)^2p-[1+(1+r)]\cdot c](1+r)-c=(1+r)^3p-[1+(1+r)+(1+r)^2]c$$

...

Nth term:

$$(1+r)^N\cdot p - [1+(1+r)+(1+r)^2 + \cdots + (1+r)^{N-1}]\cdot c = (1+r)^Np-\frac{(1+r)^N-1}{1+r-1}\cdot c$$

As at the Nth term, the payment is done, we have

$$
\begin{align*}
0 &= (1+r)^Np-\frac{(1+r)^N-1}{1+r-1}\cdot c \\
\frac{(1+r)^N-1}{r}\cdot c &= (1+r)^N\cdot p \\
c &= \frac{p\cdot r\cdot (1+r)^N}{(1+r)^N-1}
\end{align*}
$$

or 

$$\frac{r\cdot(1+r)^N}{(1+r)^N-1}-\frac{c}{p} = 0$$






