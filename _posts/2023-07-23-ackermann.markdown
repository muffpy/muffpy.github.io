---
layout: post
title:  "Reducing 2-ary Ackermann recurrences using Lisp notation"
date:   2023-07-23 17:56:57 +0100
categories: sicp
---

This post is a result of me reaching Exercise 1.10 in [_Structure and Interpretation of Computer Programs_](sicp) and finding out that GitHub released support for [mathemtical expressions on Markdown](math-support). There will probably be more posts on SICP exercises as I progress through the book.

Some familiarity with Lisp syntax is needed. The opening pages of the book gives us a suitable way to do this by stating that, 
> when we describe a language, we should pay particular attention to the means that the language provides for combining simple ideas to form more complex ideas.

And this idea can be limited to three ideas:
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
However, the notation above is not practised where the brackets are lined up to elucidate the syntax. Since, this can lead to really long files to read, we would write it instead as:

```lisp
(define (abs x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```

And that's all we need to tackle Exercise 1.10 which is given as follows.
<br><br>
{:refdef: style="text-align: center;"}
![](/img/ex1.10.png){:width="500"}
{: refdef}

We will deal with the second half of the question.

[sicp]: https://web.mit.edu/6.001/6.037/sicp.pdf
[math-support]: https://github.blog/2022-05-19-math-support-in-markdown/