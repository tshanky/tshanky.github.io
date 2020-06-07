---
title: What are Indeterminate Forms?
author: tshanky
permalink: /2020/06/07/what-are-indeterminate-forms/
modified:
excerpt: "An Indeterminate form is an expression involving two functions whose limit cannot be determined solely from the limits of the individual functions."
tags: [mathematics, calculus]
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

The limit of a function at a point **a** in its domain is the value it approaches as its argument approaches **a**. Intutively and informally, one could say that the limit of a function of a variable **x** at **a** is **L** if the function gets closer and closer to **L** as **x** gets closer to **a**.

$$
\lim_{x \to a} f(x) = L
$$

For more on limits please read [Paul's Online Notes on the subject](https://tutorial.math.lamar.edu/classes/calci/thelimit.aspx){:target="_blank"}.

The concept of limit is fundamental to calculus and serves as the building block to define derivatives and definite integrals.

## Limit of an expression involving multiple functions

The limit of an expression involving multiple functions can be obtained by taking the limit of the individual functions and then combining them to get the limit of the expression. Here is a simple example:

$$
\lim_{x \to a} f(x) = L1
$$

$$
\lim_{x \to a} g(x) = L2
$$

$$
\lim_{x \to a} (f(x)+g(x)) = L1 + L2
$$

$$
\lim_{x \to a} \frac{f(x)}{g(x)} = \frac{L1}{L2}
$$

In some cases though this trick leads to unresolvable expressions. Common among such unresolvable expressions are limits of functions 

$$
\lim_{x \to a} \frac{f(x)}{g(x)}
$$

leading to ratios of 

$$
\frac{0}{0} and \frac{\infty}{\infty}.
$$

This is when both **L1** and **L2** in our example above are either 0 or $$\infty$$ at the same time. Limits of such expressions cannot be determined only by the limits of the functions in the expression. Such an expression is in an **indeterminate form**.

## Examples of indeterminate forms


$$
\lim_{x \to 0} \frac{kx}{x}
$$

where k is a real number and

$$
\lim_{x \to 0} \frac{sin(x)}{x}
$$

leads to an expression of form $$\frac{0}{0}$$.

Similarily,

$$
\lim_{x \to \infty} \frac{kx}{x}
$$

and

$$
\lim_{x \to \infty} \frac{log(x)}{x}
$$

creates an expression of form $$\frac{\infty}{\infty}$$


## The 7 indeterminate forms

There are 7 different indeterminate forms. $$\frac{0}{0}$$ and $$\frac{\infty}{\infty}$$ are the most common out of these 7. Other indeterminate forms are $$0\times\infty$$, $$\infty - \infty$$, $$0^0$$, $$1^\infty$$, and $$\infty^0$$.

> Note: $$\frac{0}{\infty}$$ is not an indeterminate form. If $$\lim_{x \to \infty} f(x) = 0$$ and $$\lim_{x \to \infty} g(x) = \infty$$, then $$\lim_{x \to \infty} \frac{f(x)}{g(x)}$$ is of the form $$\frac{0}{\infty}$$. If you however rewrite this equation in the form $$\lim_{x \to \infty} f(x)\times\frac{1}{g(x)}$$ it is easy to see that both the multiplicands are tending to $$0$$. $$0 \times 0$$ is $$0$$ and hence the limit of this expression is $$0$$.

For forms of the type $$\frac{0}{0}$$, one could use the definition of a derivative to resolve the situation. By defintion, derivative $$f'(x)$$ is

$$
\lim_{h \to 0} \frac{f(x+h) - f(x)}{h}
$$

which is of the form $$\frac{0}{0}$$.

## L'Hospital's Rule

L'Hospital's rule tells us that if we have an expression of the indeterminate form $$\frac{0}{0}$$ or $$\frac{\infty}{\infty}$$ then we must differentiate the numerator and differentiate the denominator separately and then take the limit.

In this case we get:

$$
\lim_{x \to a} \frac{f(x)}{g(x)} = \frac{f'(x)}{g'(x)}
$$

As an example, 

$$
\lim_{x \to 0} \frac{sin(x)}{x} = \lim_{x \to 0} \frac{cos(x)}{1} = \frac{1}{1} = 1
$$

L'Hospital's rule can also be applied to indeterminate expressions of the form $$0\times\infty$$. In fact, such an expression can easily be converted to $$\frac{0}{0}$$ or $$\frac{\infty}{\infty}$$ form.

$$
\lim_{x \to a} f(x) = 0
$$

$$
\lim_{x \to a} g(x) = \infty
$$

$$
\lim_{x \to a} {f(x)}{g(x)}
$$

is then of the form $$0\times\infty$$. 

A simple transformation to $$\lim_{x \to a} \frac{f(x)}{\frac{1}{g(x)}}$$ converts it to $$\frac{0}{0}$$ form.

For a more detailed explanation and a few examples of the application of L'Hospital's rule please read [Paul's Online Notes on the subject](https://tutorial.math.lamar.edu/Classes/CalcI/LHospitalsRule.aspx){:target="_blank"} or [math24.net's tutorial on L'Hospital's rule](https://www.math24.net/lhopitals-rule/){:target="_blank"}.

## Solving indeterminate forms

L'Hospital's rule is the best method to solve indeterminate forms. In some cases an initial transformation allows an expression to be converted to $$\frac{0}{0}$$ or $$\frac{\infty}{\infty}$$ form after which the L'Hospital's rule can be applied to the expression.

In some other cases, transformation could lead to forms where one can easily calculate the limits. Here is an example:

$$
\lim_{x \to \infty} (\sqrt{x^2+x+5} - x) = \lim_{x \to \infty} (\sqrt{x^2+x+5} - x) \times \frac{\sqrt{x^2+x+5} + x}{\sqrt{x^2+x+5} + x}
$$

$$
= \lim_{x \to \infty} \frac{x^2+x+5 - x^2}{\sqrt{x^2+x+5} + x} = \lim_{x \to \infty} \frac{x+5}{\sqrt{x^2+x+5} + x} = \lim_{x \to \infty} \frac{\frac{x}{x}+\frac{5}{x}}{\sqrt{\frac{x^2}{x^2}+\frac{x}{x^2}+\frac{5}{x^2}} + \frac{x}{x}}
$$

$$
= \lim_{x \to \infty} \frac{1+\frac{5}{x}}{\sqrt{1+\frac{1}{x}+\frac{5}{x^2}} + 1} = \frac{1}{2}
$$



## Conclusion

There are 7 different types of indeterminate forms. In order to find the limits of expressions in indeterminate forms, transform them to a $$\frac{0}{0}$$ or $$\frac{\infty}{\infty}$$ form and apply the L'Hospital's rule.


