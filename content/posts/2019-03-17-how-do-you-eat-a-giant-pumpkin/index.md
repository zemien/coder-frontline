---
title: How do you eat a giant pumpkin?
date: 2019-03-17T09:00:00+13:00
featuredImg: /how-do-you-eat-a-giant-pumpkin/giant-pumpkin-aresauburn.jpg
categories:
  - Programming
---
Here's a useful analogy I use to explain how to break a feature down into its unit tests. I am using it in a TDD (Test-Driven Development) workshop I'm conducting in my team. Putting aside the fact that giant pumpkins are mainly bred for show, this will hopefully help new programmers when they're faced with any programming challenge. I created this analogy as an alternative to "How do you eat an elephant?" because elephants are majestic creatures and what are you, a monster? /s

{{< 
  figure src="giant-pumpkin-aresauburn.jpg" 
  title="A giant pumpkin" 
  alt="A giant pumpkin sitting in a field" 
>}}

So how do you eat a giant pumpkin? Piece-by-piece!

It may seem like an uphill climb when staring up a new programming challenge. Honestly it's a normal feeling among experienced developers like myself. The difference is we quickly start carving that pumpkin up into large segments, otherwise known as a feature breakdown. We ask ourselves some leading questions, like:

* What does the user want to achieve?
* What are we interested within our input?
* Do our inputs have things we want to exclude?
* What are the edge cases?
* How do we start transforming the input into output?

This will effectively chop our pumpkin up into large segments. You can call this a feature, user story, task, etc.

{{< 
  figure src="pumpkin-segments-joe-shlabotnik.jpg" 
  title="Pumpkin segments" 
  alt="Large pumpkin segments placed to the left of some hollowed out whole pumpkins" 
>}}

The pumpkin segments are still too large to eat though. You need to break down each segment into even smaller pieces. I ask leading questions like:

* What are our inputs?
* What are our expected outputs?
* What errors might happen here?
* Which parts of the implementation are we writing our own logic, and which are we handing off to another API or library?

This thought process helps to separate distinct parts of the program. For example, you would want to encapsulate your own algorithms away from external API calls. This way you can write unit tests for your own code without worrying about coding against the API. Over time this will break the feature down to individual methods that meet the Single Responsibility Principle. In other words, they are diced into a bite-sized morsels.

{{< 
  figure src="diced-pumpkin-lalibela.jpg" 
  title="Diced pumpkin" 
  alt="Diced pumpkin pieces" 
>}}

I should conclude before I stretch this analogy out too much. New (and old!) software developers should cultivate their pumpkin-dicing skills in order to create applications that are more maintainable and readable. I did not specifically call out any unit test approaches, but this is extremely well-suited to how we can answer that perplexing conundrum: "How do I write a test for a method that does not exist yet?"

Hope you enjoy that delicious pumpkin pie at the end!

{{<
  figure src="pumpkin-pie-autumn-holiday-baked-delicious.jpg" 
  title="Delicious pumpkin pie" 
  alt="A slice of pumpkin pie next to the whole pie" 
>}}

Top and background photo credit: <a href="https://www.flickr.com/photos/aresauburnphotos/1865650749/">aresauburn™</a> on <a href="https://visualhunt.com">Visual hunt</a> / <a href="http://creativecommons.org/licenses/by-sa/2.0/"> CC BY-SA</a>

Pumpkin segments photo credit: <a href="https://www.flickr.com/photos/joeshlabotnik/10864721445/">Joe Shlabotnik</a> on <a href="https://visualhunt.com">Visual hunt</a> / <a href="http://creativecommons.org/licenses/by-nc-sa/2.0/"> CC BY-NC-SA</a>

Diced pumpkin photo credit: <a href="https://www.flickr.com/photos/lalibela_ethiopian_restaurant/15534775139/">Lalibela Ethiopian Restaurant</a> on <a href="https://visualhunt.com">VisualHunt.com</a> / <a href="http://creativecommons.org/licenses/by-nd/2.0/"> CC BY-ND</a>