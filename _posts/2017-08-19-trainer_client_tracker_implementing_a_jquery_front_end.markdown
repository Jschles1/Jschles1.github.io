---
layout: post
title:  "Trainer Client Tracker: Implementing a jQuery Front End"
date:   2017-08-18 21:52:04 -0400
---


Initially when I read the instructions for the Rails App with a jQuery Front End project, my heart sank when I saw that I'd be working again with my Rails app that I had a lot of difficulty building. I seriously considered building a new app from scratch that didnt have the complexities contained in my Rails app, as I had heard horror stories from other students where they had great difficulty implementing the requirements for this project since they made their Rails app too ambitious and complex.

In the end, I decided to stick with my original Trainer Client Tracker app after thinking through thoroughly how I would implement the requirements. I am very happy that I made this decision, as this project ended up not taking that much time at all to complete (almost 1 week compared to my Rails app which took 1 month), and the end result is an app that I am very proud of.

The first noticeable change I made was that I added styles to make the app look good. I added a background image and some borders to make it look more smooth. Nothing major, but it definitely looks much better than my previous Rails app which had almost no styling. 

The main change I made to my app was the addition of a "Dashboard" page. On this page, the user can now click "My Clients" to render the Client index resource, or the "My Appointments" link which will render the Appointment resource, both through jQuery and an Active Model Serialization JSON backend. The user can alternate between these two index resources without having to refresh the page.

To fulfill the fourth requirement of this project, I added new model, `Note`, which `belongs_to` a `Client`. A `Client` `has_many` `Notes`. Say your new client preivously had surgery for a shoulder injury that you need to take into account when you end up making a training program for that client. On the a client's show page, you can make a note for that client and render it immediately onto the page. 

On a client's show page, you also can now click either the "Next" or "Previous" buttons to sift through all your clients, with the next/previous client being requested and rendered to the page via jQuery & AJAX. When I was implementing this feature, I ended up running into an interesting problem. When we implemented a similar feature in our labs, clicking the "Next" button would simply request the resource with an `id` attribute 1 greater than the one currently on the page. Say there are 5 unique resources with their own `id`. If we deleted the resource with the `id` of `3`, the app would break if we clicked "Next" while viewing the show resource with the `id` of `2` (since `3` no longer exists).

Here's what I implemented to fix this problem:

```
function getNext(id, array) {
  var next = id + 1
  while (next <= array[array.length - 1]) {
    if (array.includes(next)) {
      renderClient(next)
      break;
    } else {
      ++next
    }
  }
}
```

This function takes 2 arguments: the id of the current client (which is retrieved by a matching `data-id` attribute of the "Next" button), and an array. This array contains the `id` attribute of each client that belongs to the current user. We define the variable `next` as being equal to the `id` plus 1 (which equals the `id` of the client resource we want to retrieve). 

Next, we initiate a `while` loop. While `next` is less than or equal to the last element of the array (which is always the highest numbered client `id` that exists), we check if the current value of `next` matches any of the `id`'s in the array. If so, we call `renderClient()` with `next` passed in as an argument, (which will make the AJAX GET request to the backend, using `next` as the `id` in the request URL) and then break from the loop. If not, we increment `next` by 1 and check again.

Here's the version of the function that gets called when the user clicks on "Previous"

```
function getPrevious(id, array) {
  var descArray = array.reverse()
  var prev = id - 1
  while (prev >= descArray[descArray.length - 1]) {
    if (descArray.includes(prev)) {
      renderClient(prev)
      break;
    } else {
      --prev
    }
  }
}
```

Working on this project greatly improved my confidence with using jQuery, and JavaScript in general. I can see how powerful it is when it comes to improving the front end and user experience of a website. Now it's time to get into the home stretch of this curriculm, the React and Redux sections.


