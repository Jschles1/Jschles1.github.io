---
layout: post
title:      "Using Hash Maps To Solve Problems"
date:       2017-12-13 01:36:33 +0000
permalink:  using_hash_maps_to_solve_problems
---

One of the most common problems one might face in a algorithm or whiteboard problem is the task of counting or verifying either the number that a certain character appears in a string, or the number that a certain element appears in an array. For those who are inexperienced in dealing with these types of problems, this task may be overwhelming at first to complete in code. The first instinct may be to create a seperate counter variable for each character/element, but that would obviously be an extremely inefficient way.

Fortunately, a trick exists that makes these types of questions very simple to solve: the hash map. It involves simply creating an object, where the string characters / array elements are the keys, and the occurrences of each character / element are the values. Here's an example:

```
"hello world!"

=>

{ h: 1, e: 1, l: 3, o: 2, " ": 1, w: 1, r: 1, d: 1, !: 1 }
```

So, how do you build one? Here's one way to do so in JavaScript:

```
const map = {};
let str = "abcd";
str.split('').forEach(char => {
  // If the key for the current char doesn't already exist:
  if (!map[char]) {
    // Initialize it at 0.
    map[char] = 0;
  }
  // Incrememnt the value of current char by 1.
  map[char]++;
});
	
=>
	
{ a: 1, b: 1, c: 1, d: 1 }
```

If you want to get fancy, you can get the same result as above by using `reduce()`:

```
const map = str.split('').reduce((obj, char) => {
  if (!obj[char]) {
    obj[char] = 0;
  }
  obj[char]++;
  return obj;
}, {});
```

Of course, since `forEach()` and `reduce()` are array helpers, you have to convert the string to an array first. You could always iterate through the string using a normal `for` loop if you prefer to do it that way.

Hash maps aren't only good for counting occurences of characters or elements. Say you had an array of numbers, and you wanted to create a hash map where the keys were each of the numbers, and the values were each number's index. Here's how you would do that:

```
const numbers = [1, 2, 3, 4, 5];
const map = {};

numbers.forEach((num, index) => { map[num] = index });

=>

{ 1: 0, 2: 1, 3: 2, 4: 3, 5: 4 }
```

If you're a beginner, then I highly recommend you memorize how to build a hash map. Doing this will greatly increase your confidence when it comes to solving algorithm challenges, because it will allow you to solve many more types of problems that you wouldn't have been able to solve before.
