---
layout: post
title:  "Reducing 2-ary Ackermann recurrence functions using Lisp notation"
date:   2023-07-23 17:56:57 +0100
categories: sicp
usemathjax: true
---

This post is a result of me reaching Exercise 1.10 in [_Structure and Interpretation of Computer Programs_](https://web.mit.edu/6.001/6.037/sicp.pdf) (SICP) and finding out that GitHub released support for [mathemtical expressions on Markdown](https://github.blog/2022-05-19-math-support-in-markdown/). There will probably be more posts on SICP exercises as I progress through the book.

## Lisp Primer
Some familiarity with Lisp syntax is needed to understand this post. The opening pages of SICP gives us a suitable way to do this by stating that,
> when we describe a language, we should pay particular attention to the means that the language provides for combining simple ideas to form more complex ideas.

So we will do just that. This idea can be limited to three mechanisms:
- **primitive expressions**, which represent the simplest entities the
language is concerned with,
- **means of combination**, by which compound elements are built
from simpler ones, and
- **means of abstraction**, by which compound elements can be named
and manipulated as units.

Primitive expressions in Lisp are nothing you wouldn't find in any other language: integers, strings, booleans and floats. You can assign names to these values using `define`,

```lisp
(define radius 3.5)
> radius
3.5
```

combinations of these expressions with primitive procedures are created using _prefix notation_,
```lisp
> (* 25 4 12)
1200

> (+ 2.7 10)
12.7

> (/ 10 5)
2
```

and such operations can be nested to produce compound operations.

```lisp
> (+ (* 3 5) (- 10 6))
19
```

Functions, called _procedures_ in Lisp, are an abstraction technique by which such compound operations can be named and called.
```lisp
(define (square x) (* x x))

> (square 5)
25

(define (sum-of-squares x y)
  (+ (square x) (square y)))

> (sum-of-squares 3 4)
25
```

Control flow is described in Lisp using the `cond` or `if` where the former allows for an _n-ary_ case analysis.

```lisp
(define (abs x)
  (cond 
    ((> x 0) x)
    ((= x 0) 0)
    ((< x 0) (- x))
  )
)

> (abs -3)
3
```
However, the notation above is not practised where the brackets are lined up to elucidate the syntax since this can lead to really long files to read as functions get bigger. We will write it instead as:

```lisp
(define (abs x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```

And that's all we need to tackle Exercise 1.10 which is given below.

## Exercise 1.10
Before we start writing down the math, let us add MathJax support to our static site generator Jekyll using Vincent Zhang's [tutorial](http://webdocs.cs.ualberta.ca/~zichen2/blog/coding/setup/2019/02/17/how-to-add-mathjax-support-to-jekyll.html). MathJax is JavaScript library that scans the page for mathematical markup, and typesets the mathematical information accordingly. All we need to do is add the public CDN `<script>` tags to our base `head.html`.

<br><br>
{:refdef: style="text-align: center;"}
![](/img/ex1.10.png){:width="500"}
{: refdef}

We will use the _substitution model_ to evaluate $ (A \ x \ y) $ at the given parameters.

_`(A 1 10)`_
```lisp
(A 0 (A 1 9))
(A 0 (A 0 (A 1 8)))
(A 0 (A 0 (A 0 (A 1 7))))
(A 0 (A 0 (A 0 (A 0 (A 1 6)))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 1 5))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 4)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 3))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 8)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 16))))))
(A 0 (A 0 (A 0 (A 0 (A 0 32)))))
(A 0 (A 0 (A 0 (A 0 64))))
(A 0 (A 0 (A 0 128)))
(A 0 (A 0 256))
(A 0 512)
1024
```

We observe that the multiplications operations which reduce the chain of function compositions to our answer is _deferred_ and the max length of this chain can be pre-determined using the parameters (input size). Such a function is called a _primtive recursive_ function. Computability theorists care about this quality of functions.

We also observe from the behaviour of this function instance that Ackerman functions of the form $(A \ 1 \ n)$ can be represented using the closed-form expression $2^n$. Thereby, we now know $ g(n) $ as well.

_`(A 2 4)`_
```lisp
(A 1 (A 2 3))
(A 1 (A 1 (A 2 2)))
(A 1 (A 1 (A 1 (A 2 1))))
(A 1 (A 1 (A 1 2)))
```
We could stop here and use g(n) to write this expression as $ g(g(g(2))) $. Looking at the number of compositions of $g(n)$ should give you a clue about finding a closed-form for $h(n)$ but let us continue.

```lisp
(A 1 (A 1 (A 0 (A 1 1))))
(A 1 (A 1 (A 0 2)))
(A 1 (A 1 4))
(A 1 (A 0 (A 1 3)))
(A 1 (A 0 (A 0 (A 1 2))))
(A 1 (A 0 (A 0 (A 0 (A 1 1)))))
(A 1 (A 0 (A 0 (A 0 2))))
(A 1 (A 0 (A 0 4)))
(A 1 (A 0 8))
(A 1 16)
```
We can stop here and use the first subsection to get our answer.

$$
\begin{align}
2^{16} = 65356 \hspace{50cm} 
\end{align}
$$

Note also that the max size of the deferred operation chain (16) is not readily apparent from our input size. We are no longer dealing with primitive recursive functions.

_`(A 3 3)`_
```lisp
(A 2 (A 3 2))
(A 2 (A 2 (A 3 1)))
(A 2 (A 2 2))
(A 2 (A 1 (A 2 1)))
(A 2 (A 1 2))
(A 2 (A 0 (A 1 1)))
(A 2 (A 0 2))
(A 2 4)
```

For the second half of the question, we already know that $g(n) = 2^n$ and $f(n) = 2n$ is obvious. To solve for $h(n)$,

$$
\begin{aligned}
h(n) &= (A \ 2 \ n) \\
&= (A \ 1 \ (A \ 2 \ (n-1))) \\
&= (A \ 1 \ (A \ 1 \ (A \ 2 \ (n-2)))) \\
&= \underbrace{(A \ 1 \ (A \ 1 \ \ldots \ldots}_{k \text{ times}} (A \ 2 \ (n-k)) \ldots \ldots)) \\
&= (A \ 1 \ (A \ 1 \ \ldots (A \ 2 \ 1) \ldots )) \ \ \textrm{for k = n - 1} \\
&= \underbrace{(A \ 1 \ (A \ 1 \ \ldots}_{n \text{ times}} (2) \ldots )) \\
\end{aligned}
$$


By representing $k$ function compositions using $\ f^k(n)$, we have,


$$
\begin{aligned}
&= g^n(2) = \underbrace{2^{2^{2^{⋰^{2}}}}}_{n \text{ times}} \hspace{4.5cm}
\end{aligned}
$$

## Ackermann functions of the form $A(k, n)$



And we're done.