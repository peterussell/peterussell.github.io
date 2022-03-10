---
title: Big O Notation (Part 2)
date: 2022-02-20 21:46:00 +1300
categories: [study, cs]
tags: [cs-study, big-o-notation]
---

This is part 2/2 on a series of posts covering Big O Notation.
[Part 1 here]({% post_url 2022-02-20-big-o-notation %}) covered
constant, linear, and quadratic time. In this post we'll do a
quick revision of what a logarithm is, and then look at
how they're applied to Big O.

## Logarithms refresher

### Example

I find it easier to understand logarithms by starting with an
exponential equation, which might be more familiar. For our
example we're going to use **2<sup>3</sup> = 8**.

A *logarithm* is the inverse of an exponential expression like the one above.
In this case, the inverse logarithm is **log<sub>2</sub>8 = 3**.

### Explanation

In the example above, the "2" in log<sub>2</sub>8 = 3 is called the **base**,
so if we write this in a more general way, we can say something like:

> log<sub>base</sub>a = b

The question we're trying to answer with a logarithm is:

> What value **b** do we have to raise the **base** value to, in order to get **a**?

In our example, raising 2 (the base) to the power of 3 (the value of b) gives us
8 (the value of a). We'd say this as "the base 2 log of 8 is 3".

In computer science we tend to use a base of 2 given we're usually dealing with
binary math (especially with Big O notation).

Other examples:
 - log<sub>2</sub>1 = 0 (*"what do I raise 2 to, in order to get 1? Answer: 0"*)
 - log<sub>2</sub>2 = 1
 - lob<sub>2</sub>32 = 5

## Logarithmic time: **O(logn)**

Logarithmic time describes a function where the processing time halves each time
the function iterates internally. Put another way: doubling the amount of data
only requires one extra iteration.

One example is searching a sorted array of numbers, eg. `[1,8,20,35,41,97,105]`
where we want to see if the number `20` is in the array.

We can start in the middle of the array (`35`) and see whether the number we're
trying to find is bigger or smaller. `20 < 35`, so we know the value `20` - if
it exists - must be in the lower half of the array. We can discard the upper
half of the array, making our remaining search set half the size.

Then we repeat this: pick the middle value of the remaining values (now
halved) and check whether our search term is higher or lower, again discarding
the half we know it's not in.

We can keep doing this until we either find our search term `20`, or don't find it
(for example if our search term was 19).

## n * logarithmic time: **O(nlogn)**

This describes the situation above, but repeated `n` number of times. This
comes up in merge sort, which we'll look at in a future post.

### Resources

 - [Big O Part 4 - Logarithmic Complexity - Computer Science](https://www.youtube.com/watch?v=Hatl0qrT0bI)
 - [Big O Notation, O(log n) - freeCodeCamp.org](https://youtu.be/Mo4vesaut8g?t=1593)
