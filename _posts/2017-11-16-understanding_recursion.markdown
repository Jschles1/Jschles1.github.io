---
layout: post
title:      "Understanding Recursion"
date:       2017-11-16 05:34:32 +0000
permalink:  understanding_recursion
---


When it comes to understanding algorithms, one of the most important concepts to learn is recursion.

In the simplest terms, a function is recursive if it calls itself in the body of the function.

Well, you might be asking, how does a function that calls itself return a value? Won’t a function that calls itself keep calling itself and go on forever?

The answer is yes, but we can fix this by adding a condition that returns a value and terminates the function. This is called a base case, and every recursive function needs to keep it from continuing on in an infinite loop, like this:

![Recursion without a base case...](https://qph.ec.quoracdn.net/main-qimg-519d64ee652a4ced76ca89b49dfb7e6d-c)

So how is this written in code? consider a basic function that returns the factorial of the argument n:

```
function factorial(n) {
	if (n === 1) {
		return 1;
	} else {
		return n * factorial(n - 1)
	}
}
```


Executing will return 6. We know this is because 3 * 2 * 1 is equal to 6, but the computer executes this problem  in a slightly different process using something referred to as the call stack.

Knowing how the call stack works is essential to writing effective recursive functions. I know that I had great difficulty in completing the recursion lab provided by the FlatIron CS module until I fully understood it. The best way to explain the call stack is to illustrate how the computer solves factorial(3) through a step by step process.

1. Invoke factorial(3). Since 3 !== 1, we return 3 * factorial(2).
2. Invoke factorial(2). Since 2 !== 1, we return 2 * factorial(1).
3. Invoke factorial(1). Since 1 === 1, we return 1.
4. Notice how we made 3 recursive calls of factorial(n) before we met our base case. Imagine that when we make a recursive call, we stack that call on top of the previous call. This is why it’s called the call stack.
5. Now what we’ve reached the base case, we traverse back down the call stack, taking the return value (1) and passing it to the call 1 level lower.
6. Since we know that the return value of factorial(1) is 1, we know that the return value of factorial(2) is 2 * 1 (2).
7. Now that we know the final return value of factorial(2) is 2, we can use that to get the return value of factorial(3), which is 3 * 2 (6). This is our final answer.

If you’re still struggling to understand recursion, consider a real life example I drew from my previous career as a personal trainer.

When you think about it, performing a set of an exercise is actually a recursive function:

```
function performExercise(energy, targetRepetitions, repetitions = 0) {
	if (!energy || repetitions === targetRepetitions) {
		return repetitions
	} else {
		return performExercise(energy—, repetitions++)
	}
}
```

So when you go to do a set of an exercise, such as the bench press, you want to have an idea of how many repetitions you want to perform (usually around 8 or 10 for beginners). Basically, you do repetitions of that exercise until you hit your target amount of repetitions, or if you run out of energy to perform the next repetition. 

Recursion may be a hard concept to get down at first, but once you do, it will become a very powerful tool you can utilize in your software development career.
