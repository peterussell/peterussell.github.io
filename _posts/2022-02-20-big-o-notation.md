---
title: Big O Notation
date: 2022-02-20 13:04:00 +1300
categories: [study, cs]
tags: [cs-study, big-o-notation]
---

### Resources

 - [Introduction to Big O Notation and Time Complexity - CS Dojo (YouTube)](https://www.youtube.com/watch?v=D6xkbGLQesk)
 - [Big O Notation - HackerRank (YouTube)](https://www.youtube.com/watch?v=v4cd1O4zkGw)

### Overview

Big O Notation is a way of describing how the time it takes to run your
function grows as the size of the input to that function grows.

Its purpose is to answer the question "how long does it take to run this function?",
while removing variable factors like how fast your computer is, are other
programs running at the same time, which programming language is being used,
etc.

### Constant time: **O(1)**

The processing time of the function doesn't increase, even when the size of the
input increases.

```java
// Constant time - O(1)
public void printFirstItem(ArrayList<String> fruits) {
  System.out.println(fruits.get(0));
}
```

### Linear time: **O(n)**

The processing time scales linearly with respect to the size of the input, ie.
doubling the input size means the algorithm will take twice as long to run.

```java
// Linear time - O(n)
public void printFruits(ArrayList<String> fruits) {
  for (String fruit : fruits) {
    System.out.println(fruit);
  }
}
```

### Quadratic time: **O(n<sup>2</sup>)**

The processing time grows as a *square* of the size of the input. eg. if
my input contains 1 item, then it may take 1ms to process. If the input
size is 2 items it will take 4ms, 4 items = 16ms, etc.

```java
// Quadratic time - O(n^2)
public void printFruitCombos(ArrayList<String> fruits) {
  for (String fruit1 : fruits) {
    for (String fruit2 : fruits) {
      System.out.println(
        "I'm going to have a fruit salad with " +
        fruit1 + " and " + fruit2 + "."
      );
    }
  }
}
```

## Rules

 1. #### Different steps in the algorithm are added together
    ```java
    public void doTwoSteps() { // O(a+b)
      doStep1(); // O(a)
      doStep2(); // O(b)
    }
    ```

 2. #### Drop constants, ie. O(2n) or O(3n) become O(n)
    ```java
    public void printTwoFruits(ArrayList<String> fruits) { // O(2n) == O(n)
      for(String fruit: fruits) {
        System.out.println(fruit); // O(n)
      }
      for (String fruit: fruits) {
        System.out.println(fruit); // O(n)
      }
    }
    ```
    We could add up the two inner for loops to give us a combined runtime of O(2n), but
    if we drop the constant then we can say `printTwoFruits(...)` is an O(n) algorithm. The
    reason is we're only interested in generally how the algorithm scales with a change
    in input size - both of these (O(n) and O(2n)) are scaling linearly.

 3. #### Different inputs get different variable names
    ```java
    public void printSums(int[] firstItems, int[] secondItems) {
      for (int firstItem : firstItems) {
        for (int secondItem : secondItems) {
          System.out.println("Sum: " + (firstItem + secondItem));
        }
      }
    }
    ```
    In this case, `firstItems` and `secondItems` can be (and probably are) different sizes,
    so we can't treat them as the same input (ie. O(n<sup>2</sup>)). Instead, assign
    `firstItems = a`, and `secondItems = b`, making the complexity of `printSums` O(a x b).

 4. #### Only care about the fastest growing term
    If an algorithm has one part whose complexity is O(n) and a second part whose complexity
    is O(n<sup>2</sup>), then we only care about the slowest part of the algorithm, ie.
    O(n<sup>2</sup>). In this case, we say the complexity of the algorithm is O(n<sup>2</sup>).


## Other notes

 - 'n' is just a variable used by convention. You can swap it for some other variable if
   you want, eg. given an input size of `a` items to an algorithm with linear complexity,
   we can write the Big O notation as O(a).
