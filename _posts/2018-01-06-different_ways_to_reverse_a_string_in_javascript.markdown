---
layout: post
title:      "Different Ways To Reverse a String in JavaScript"
date:       2018-01-06 19:20:59 -0500
permalink:  different_ways_to_reverse_a_string_in_javascript
---


Perhaps one of the most common interview questions that is asked of junior-level developers is to create an algorithm that reverses an input string and returns that reversed string.  In this blog post, I will walk through multiple possible solutions to this problem.

**The "Easy" Solution:**

I call this the "easy" solution because if you know about JavaScript's `reverse()` array method, the solution will simply look like this:

```
function reverseString(str) {
    return str.split('').reverse().join('');
}
```

Here, we take the string, split it into an array, reverse the array, then join the reversed array back into a string. Unfortunately, many interviewers who ask this question will forbid you from using `reverse()`, and will require you to take a more in-depth approach.

**The Iterative Solution:**

One common way of solving this problem involves the use of a `for` loop:

```
function reverseString(str) {
    let reverse = "";
		for (let i = 0; i < str.length; i++) {
		    reverse = str[i] + reverse;
		}
		return reverse;
}
```

Here, we define an empty string called `reverse`. Then, we initiate our `for` loop, pushing the current character of the string we're iterating over and pushing that character onto the beginning of `reverse`. At the end of the loop, we return the final value of `reverse`.

**Can We Do Better?**

Believe it or not, there's actually a way to double the effiency of the iterative solution. How, you might ask?

```
function reverseString(str) {
    str = str.split('')
    for (let i = 0; i < str.length / 2; i++) {
        let current = str[i];
        let opposite = str[str.length - i - 1];
        str[i] = opposite;
       str[str.length - i - 1] = current;
    }
    return str.join('');
}
```

While with this approach, you have to split the string into an array, you double the effiency by switching each letter with its' opposite position. We only have to iterate through the loop half the amount of times of the string's length.

**Using Reduce:**

You can also use `reduce()` to print out the iterative solution if you want to impress your interviewer.

```
function reverseString(str) {
    return str.split('').reduce((reversed, char) => {
        return char + reversed;
    }, '');
}
```

**Bonus: Recursion:**

The recursive solution might be overkill, but it will show your interviewer that you *really* know how to reverse a string:

```
function reverseString(str) {
    if (str.length < 2) {
        return str;
    } else {
        return reverseString(str.slice(1)) + str.charAt(0);
    }
}
```

Knowing how to reverse a string is an absolutely crucial algorithm to know for interviews. If you know of any other solutions that aren't listed here, feel free to reach out to me.


