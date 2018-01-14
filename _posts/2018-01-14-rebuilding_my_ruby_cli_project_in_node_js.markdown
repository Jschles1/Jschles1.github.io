---
layout: post
title:      "Rebuilding My Ruby CLI Project in Node.js"
date:       2018-01-14 21:17:36 +0000
permalink:  rebuilding_my_ruby_cli_project_in_node_js
---


In my best efforts to keep up with current industry trends, I have decided to learn to learn Node.js. 

**Node.js** is a run time environment which is built over V8 Javascript engine of Chrome, and it can effectively serve  both as a backend HTTP server and/or a templating engine for your application. There are 3 main advantages to using Node.js over a framework such as Ruby on Rails:

* Node.js uses **non-blocking I/O** (input/output) to execute server processes. This means that processes can be executed by the server asynchronously, and the server doesn't have to wait until one process is completed before it starts to execute another one. In comparison, Ruby on Rails uses **blocking I/O**, where one process must be completed before another can be executed. This makes Node.js much more efficient since it can handle more than one running process at a time.
* Node.js allows you to write your whole application in JavaScript, both frontend and backend. You don't have to switch between languages when working on a full-stack application.
* The npm package ecosystem. If there's a certain functionality you want to implement in your program, chances are somebody already built a package or library for it.

To put what I learned to good use, I decided to rebuild all of my Ruby projects using Node.js, starting with my CLI project. I pushed myself to get a working version done as quick as possible, and built the whole project start to finish in 5 hours.

To get user input, I utilized the **readline** module which is provided by Node by default. It allows you to get user input from the command line and use that input in many different ways. I made heavy use of the `readline.question()` method, which takes 2 arguments: a string representing a question you want to ask the user, and the user's input which is then passed to a callback function.

```
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

...

rl.question("Which system's current bestselling games would you like to see? (Xbox One, PS4, Switch, Wii U, 3DS): ", (input) => {
  listBestsellers(input);
});
```

Since there isn't a Nokogiri npm package, I had to find a new way to implement web scraping. Fortunately, I found an approach that uses two packages: 

* **request-promise:** Allows us to make HTTP requests to a webpage.
* **cheerio:** Allows you to use jQuery in Node.js.

To scrape data off of a webpage, we make the request using request-promise. We then use cheerio to select the elements to scrape the data off of. [This](https://codeburst.io/an-introduction-to-web-scraping-with-node-js-1045b55c63f7) blog post explains the process in more detail.

[Here](https://github.com/Jschles1/bestselling-games-cli-node) is a link to the GitHub repo of the project if you want to check it out.
