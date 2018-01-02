---
layout: post
title:      "Intro To Big O & Time Complexity"
date:       2017-12-30 20:21:00 -0500
permalink:  intro_to_big_o_and_time_complexity
---

To finish up 2017, I figured I would write a short introductory blog post about an important topic when it comes to constructing algorithms: Time complexity.

**Time complexity** (also called **runtime complexity**) describes the performance of an algorithm. Specifically, it describes how the processing power required to run the algorithm grows as the input to that algorithm increases, usually referring to the longest the algorithm can possibly take to execute (called the **worst-case scenario**). 

Algorithms with lower time complexities are considered "better" solutions, since they require less processing power as the input scales. The time complexity is algorithm is represented in a format referred to as **big O notation**. 

Time complexity and big O notation may be an intimidating concept to those of us who don't have a strong computer science. In the context of technical interviews, the important thing to know about these topics is how to determine the time complexity of an algorithm. The good news is this can be easily done without any math if you can spot certain patterns in your code that give away whether it is a certain time complexity or not.

## Constant Time (O(1)):

No matter what the input size is, an algorithm in constant time will  always take the same amount of time. An example of this would accessing an element in an array at a specific index.

```
function firstElement(array) {
    return array[0];
}
```


## Linear Time (O(n)):

With linear time, there is a direct, 1-to-1 relationship between number of inputs and amount of work needed to process it. A dead giveaway for this time complexity would be an algorithm that iterates over a collection of data, such as a `for` loop that goes from `0` to `array.length`.

## Logarithmic Time (O(log(n)) and Quasilinear Time (O(n * log(n))

These two time complexities involve logarithms. To keep things simple, just know that your algorithm probably meets one of these two time complexities if doubling the input to your algorithm doesn't double the processing power required. Logarithmic time usually refers to an algorithm that uses some sort of searching operation (such as binary search), while quasalinear time refers to efficient sorting algorithms (such as merge sort).

## Quadratic Time (O(n^2))

As the input `n` increases by 1, the amount of work needed increases by `n^2`. A dead giveaway for this type of time complexity would be a `for` loop nested inside of another `for` loop; every element in the collection is being compared in some way to every other element in the collection. You should always seek to improve the efficiency of your algorithm in some way if it ends up in quadratic time.

## Exponential Time (O(2^n))

With exponential time, adding a single element to the input collection will result in the processing power required to double. I think it goes without saying that you should avoid algorithms that run in exponential time, as they are horribly inefficient. A classic example would be the recursive solution to the Fibonacci algorithm.

[Here's](http://bigocheatsheet.com/) a great source if you want to learn more about big O and time complexity.




