---
layout: post
title:      "Building A Flashcards Application In React & Rails"
date:       2017-12-23 00:10:44 -0500
permalink:  building_a_flashcards_application_in_react_and_rails
---


In the weeks after graduating from Flatiron's online program, I found myself scrambling to cram all the knowledge I had learned relating to JavaScript to prepare for interviews. I found myself using flashcards to help me be better able to describe concepts such as closure, scope, first-class functions, etc. Then I came up with an idea; why not build a flashcards app myself? I would be able to study these concepts while being able to put them into practice at the same time. 

Also, I was sort of disappointed with the way my final project, the Workout Planner, turned out. The last minute addition of the Suggested Workouts feature using seed data made it so the app didn't really allow for a later implementation of other features (such as user accounts and authentication) without having to go back and rewrite that feature completely. I was more concerned with meeting the project requirements so I could graduate as soon as possible. I planned to take what I learned from this mistake I made and build my flashcards app structure in a way that I could easily come back and add those features later. I see this flashcards app as sort of a redo of the final project.

For the most part, my Flashcards app structure is almost identical to the Workout Planner's; it implements React and Redux on the front-end, with a Rails API implemented on the back-end. Users can create and remove decks of cards relating to a subject of their choice. Users can then add or remove cards (each with a question and answer) to each deck.
I again used the Semantic-UI CSS framework for styling.

Now, here's the fun part: once a user creates and fills their deck with cards, they can then enter into an interactive quiz game using that deck. Users are first shown the question side of the card. Once they think they know the answer, they click the "Show Answer" button. The card is flipped, and answer to the question is shown. The user then clicks either the "Correct" or "Incorrect" button to document whether they got the question right or not and to move onto the next question. At the end, the user is provided with a percentage score of the questions they got correct.

Here's a screenshot:

![](https://photos.app.goo.gl/E8px9WT5N2CSrzTS2)

[Here's](https://github.com/Jschles1/react-rails-flashcards) a link to the GitHub repo to the project if you're interested in checking it out.
