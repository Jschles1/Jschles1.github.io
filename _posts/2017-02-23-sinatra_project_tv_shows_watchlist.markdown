---
layout: post
title:  "Sinatra Project: TV Shows Watchlist"
date:   2017-02-23 00:22:12 +0000
---


For my sinatra project, I decided that I would build a simple app that would allow users to create a list of their favorite TV shows. Even though the only TV show I watch regularly is The Walking Dead, I figured this app would be useful for people who like to watch multiple shows on multiple networks over the course of the week. This app can help them remember when their shows are on so they don't forget to watch them. Overall I am pretty satisfied with how this project turned out. 

The project consists of two models, users and shows. A user has many shows, while a show belongs to a user. Both users and shows have their own corresponding controllers, which both inherit from an application controller.

For me personally, the hardest part of this project were constructing the views pages. I wanted to implement some CSS styling on my project even though it wasn't a requirement. It had been a few months since I completed the HTML/CSS sections of the Learn Verified course, so I was really rusty when it came to that and had to go re-read a few of the lessons in that section. Furthermore, I decided to challenge myself a little bit and take a shot at using the Bootstrap framework, which until now I had never attemped using. I used it mainly to implement button links on my pages, which really improved the overall look of my app. I used the CSS page on the Bootstrap website as my main source of information ([Here](http://getbootstrap.com/css/)).

One problem I was unable to resolve after a day of trying was one regarding the time format of a show. When creating a show, the user is asked for the show's name, network, weekday, and the time the show airs. For the time I implemented a special time input which would force the user to select the hour and minutes from a drop down menu, as well as whether the time was in the A.M. or P.M. The problem was that when the created show is listed, the time would be inexplicably converted from the A.M./P.M. format to the 24 hour format (also known as the military format). I tried to create a method for the show model that would convert the 24 hour format back into the A.M./P.M. format, but encountered a multitude of errors with each method I tried to implement. I'll probably look to fix this issue at a later time. I currently specify in the app when the time is displayed in a 24 hour format to resolve any possible confusion.

[Here](https://github.com/Jschles1/sinatra-shows-towatch) is a link to my repo on GitHub if you're interested in taking a look.
