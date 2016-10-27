---
layout: post
comments: true
title:  "One efficient way to compute functional differential"
excerpt: ""
date:   2016-10-27
mathjax: true
---

In many advanced optimization field, you need to compute the 1st or 2nd differential of a functional. The rigor analysis of functional calculus is complex and difficult. But luckily, in most application, we just need to get the 1st order differential of a functional to compute the stable "point" (in fact it's a function in the functional case). And further, to compute the 2nd order differential functional to check if the stable is the optimized point that we're searching. From this perspective, what we need to do is making the process of computing 1st and 2nd order differential as convenient as possible.

Lots of books introduce the calculus of functional by variational method. But that's a great amount of work. One efficient computation way is through the Taylor expansion in appendix of G.H. Fredrickson's book *The equilibrium theory of inhomogeneous polymers*. I'd like to give some details of this great method.

### Introduction to Dirac function

One useful method in the functional Taylor expansion is the Dirac $$\delta$$ function, which is defined by

$$
\delta(x) =
             \begin{cases}
	         +\infty, & x =   0 \\
		 0,       & x \ne 0
             \end{cases}
$$

and also constrained to satisfy the identity $$\int_{-\infty}^\infty \delta(x) \, dx = 1$$.

From Lebesgue integral with respect to the measure $$\delta$$, Dirac function satisfies

$$\int f(x')\delta(x-x')\ dx'=f(x)$$

This function is used widely in engineering field. But its rigorous explanation involves massive knowledge about *generalized function* and *advanced functional analysis*, which is beyond the scope of this post. Here, we only treat it as some useful mid tool to help us compute the functional calculus.

### Taylor Expansion

Let's begin with the normal one dimension formula:

$$f(x+x_0) = f(x_0) + f'(x_0)x + \frac{f''(x_0)}{2!}x^2 + ... + \frac{f^{(n)}(x_0)}{n!}x^n + ...$$

Then, let's treat $$f$$ as $$F$$, $$x$$ as $$\delta f$$ and $$x_0$$ as $$f$$, we can get

$$F[f + \delta f]=F[\delta f + f] = F[f] + F'[f]\delta f + \frac{F''[f]}{2!} \delta f^2 + ... + \frac{F^{(n)}[f]}{n!}\delta f^n + ...$$

Then considering the characteristics of Dirac $$\delta$$ function, the functional can be transformed like the book format:

$$F[f+\delta f]=F[f] + \int^{b}_a \Gamma_1(x)\ \delta f(x)\ dx + \frac{1}{2!}\int^b_a\int_a^b\Gamma_2(x,x')\ \delta f(x)\delta f(x') \ dx'dx + ...$$

where the $$\Gamma_1(x)$$ and $$\Gamma_2(x, x')$$ corresponding to 1st and 2nd order functional differentials. Also I guess, the general $$n^{th}$$ term may be like (it's welcome to discuss if anyone disagree with this):

$$\frac{1}{n!}\int_a^b\int_a^b\cdots\int_a^b\Gamma_n(x_1, ..., x_n)\delta f(x_1)...\delta f(x_n)\ dx_1dx_2\cdots dx_n$$

Now, we only need to compare the expressions with our own functional form, we can get the 1st and 2nd order differential functional.

### Example of computing functional

Let's first try to compute functional $$F[f] = \int_a^b\ [f(x)]^n\ dx \stackrel{abbr.}{=} \int_a^b[f]^n\ dx$$.

$$
\begin{align*}
    F[f + \delta f] &= \int_a^b [f+\delta f]^n\ dx  \\
                    &= \int_a^b [f^n + nf^{n-1}\delta f + C_n^2 f^{n-2}(\delta f)^2 + ...]\ dx \\
                    &= \int_a^b[f^n]\ dx + \int_a^b [n f^{n-1}\ \delta f] dx + \int_a^b[(C_n^2 f^{n-2}\delta f)\ \delta f]\ dx + ... \\
		    &= F[f] + \int^{b}_a \Gamma_1(x)\ \delta f(x)\ dx + \frac{1}{2!}\int^b_a\int_a^b\Gamma_2(x,x')\ \delta f(x)\delta f(x') \ dx'dx + ...
\end{align*}
$$

For the 1st order differential, we just need to scrutinize the equation $$\int_a^b [n f^{n-1}\ \delta f] dx = \int^{b}_a \Gamma_1(x)\ \delta f(x)\ dx$$. It's obvious that

$$\Gamma_1(x)=n f^{n-1}=n[f(x)]^{n-1}$$

For the 2nd order differential, we need to consider the equation:

$$\frac{1}{2!}\int^b_a\int_a^b\Gamma_2(x,x')\ \delta f(x)\delta f(x') \ dx'dx = \int_a^b[(C_n^2 f^{n-2}\delta f)\ \delta f]\ dx$$

as the left side can be

$$\frac{1}{2!}\int^b_a\int_a^b\Gamma_2(x,x')\ \delta f(x)\delta f(x') \ dx'dx = \frac{1}{2!}\int^b_a\int_a^b\Gamma_2(x,x')\ \delta f(x') \ dx'\delta f(x)\ dx$$

the equation can be reduced as 

$$\int_a^b\Gamma_2(x,x')\ \delta f(x')\ dx'= 2\ C_n^2 f^{n-2}\delta f $$

Use the measure characteristics of Dirac function, and treat the $$C_n^2 f^{n-2}\delta f$$ as $$x$$'s function, the right side can be

$$
\begin{align*}
2\ C_n^2 f^{n-2}\delta f &= \int_a^b [2\ C_n^2 [f(x')]^{n-2}\delta f(x')]\ \delta(x-x')\ dx' \\
                         &= \int_a^b [2\ C_n^2 [f(x')]^{n-2}\ \delta(x-x')]\ \delta f(x')\ dx'
\end{align*}
$$

Now, it's easy to see the 2nd order differential of our functional:

$$\Gamma_2(x, x') = 2\ C_n^2 [f(x')]^{n-2}\ \delta(x-x') = n(n-1)[f(x)]^{n-2}\delta(x-x')$$

where we used the symmetry of Dirac function.




