---
layout: post
title:  "Understanding The DOM"
date:   2017-07-10 06:12:54 +0000
---

After passing my Rails assessment last week, I immediately jumped straight into the JavaScript portion of the curriculum. It was very refreshing to start over learning the basics of a new language after having solely worked with HTML, CSS, and Ruby the past couple of months. At first, it was a little difficult to adjust  from Ruby to JavaScript, particularly when it came to things such as variable and function declaration, and hoisting. Now, being at the end of the JavaScript Foundations section, I can say that JavaScript is starting to grow on me and I'm starting to enjoy working with it more.

The biggest eye opener I learned from this section was the DOM, or the **Document Object Model**. One of the things I had always wondered about websites in general was how certain intercative features could be used without making a brand new request to a new URL route, such as loading a list of comments from an article on Facebook, or making a tweet on Twitter. I always knew that JavaScript played a role in these features, but I never knew exactly how.

Learning about the DOM pulled back the curtains and revealed a huge part of the magic for me. As it turns out, what I thought was HTML being displayed in the browser was actually the DOM, which is a representation of the HTML source created by the browser. The DOM is what enables us to use JavaScript to dynamically changed what is displayed on the webpage without changing any of the actual HTML itself. For example, through JavaScript, we can both add and remove elements on the page, or we could change any of the various CSS styles.

The DOM represents the elements contained in the HTML as these things called **nodes**. We can had further dynamic functionality to our webpages by adding **event listeners** to these nodes, which will, as their name implies, listen for certain "events" to occur, and then execute any accompanying function of code.

To illustrate a short example, say we had a single `<p>` element on our page. If we wanted an alert of `"Hello World!"` to be displayed, we could write the following code:

```
// sets a variable equal to the <p> element:
var element = document.getElementByTagName("p");
// adds the event listener to the element:
element.addEventListener('click', function(event) {
  alert('Hello World!')
})
```

Even though I know this is only scratching the surface of JavaScript, I'm excited to see how I can use this new language to expand my abilities as a developer in the future.



