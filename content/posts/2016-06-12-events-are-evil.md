---
title: Events Are Evil
date: 2016-06-12T12:57:28+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/water-drop-ripples.jpg
categories:
  - Programming
---
Events are evil. Yes, that is quite the incendiary statement to make. To clarify, I don’t mean events in the physical world like rock concerts and art festivals. I am referring to the common event handling patterns most programmers are familiar with, such as button click event handlers and JavaScript callbacks. They are popular because they are simple to set up – most languages support some form of asynchronous event handling out of the box. However, its prevalence does not mean it is perfect, and I have recently come across a different event handling paradigm that promises to rehabilitate events.

<!--more-->

I first heard about Functional Reactive Programming last month at an [Techweek AKL](/techweek-akl-2016/) event. The blurb went like this:

> “Event-based application logic is a special challenge with today's ever greater demands on the programmer's art. Functional programming (FP) is a powerful general approach that uses composability to tame application complexity, but event handling has remained tricky. Functional Reactive Programming (FRP) applies functional methods to transform the event handling problem.”

I’ve certainly experienced my fair share of event handling angst. One of my most recent descent into event handling hell is to implement a pair of smart date pickers that look something like this:<figure id="attachment_148" style="width: 300px" class="wp-caption aligncenter">

<img class="wp-image-148 size-medium" src="/wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png" alt="Smart date picker" width="300" height="153" srcset="/wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png 300w, /wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png 570w" sizes="(max-width: 300px) 100vw, 300px" /><figcaption class="wp-caption-text">Smart Date Picker Mockup</figcaption></figure> 

The user picks the date range that they would like a report for. They can constrain it by weeks, months, or financial quarters. The date pickers obey that setting, e.g. if the user is reporting on months, selecting 5 August as the start date will bump it to 1 August. To add another layer of complexity, the user is constrained to picking dates that the system has data for. For example, if the system only has data beginning 3 August, the date picker would be fixed at 3 August even though it wanted to select the first of the month.

Sounds simple? It started innocently enough but bugs started appearing when unexpected combinations were selected. A big component of the date bumping process is the date picker change event handler that tries to maintain a valid date range. The start date picker’s change event handler can change the end date, which would trigger the end date’s own event handler, which may change the start date, which would trigger the… (and so on and so forth). To avoid an infinite loop, an ‘amnesty’ is used to break the loop, which may leave the date pickers with an invalid range.

Debugging was also a pain as change event handlers could be triggered from multiple sources, and it’s not clear which event path is ‘correct’. It’s now in a stable state, but god forbid I have to troubleshoot it again!

That digression serves to illustrate why I was intrigued and went along to the [Functional Programming Auckland Meetup](http://www.meetup.com/Functional-Programming-Auckland/) on FRP. Stephen Blackheath did a quick-fire demo, implementing a block stacking game in Java using FRP. I will admit that most of the demo flew over my head, as Stephen launched into the code with very little exposition. That is partly due to time constraints and the audience (who are mainly functional programmers familiar with the ‘functional’ way). My experience with functional constructs is limited to C#’s lambda expressions, which I find very useful but not sufficient enough for me to follow along with his demo.

Of course, one does not need to understand the intricacies of classical art to appreciate the Mona Lisa. So it is with FRP. I was curious about what Functional Reactive Programming might herald, and I was determined to learn more. I approached Stephen during post-event drinks and I offered to review his upcoming [Manning’s book on Functional Reactive Programming](https://www.manning.com/books/functional-reactive-programming) (co-authored with Anthony Jones). It is currently on the Manning’s Early Access Program, which means it is still subject to revisions. The first chapter ‘Stop Listening!’ is free (check the sidebar), and I highly recommend it to get Stephen and Anthony’s take on why events are evil. The part about the six plagues of listeners is fantastic and provides a solid argument as to why we need to consider new patterns.

Over the next few weeks I will chronicle my attempt to learn FRP by implementing the Smart Date Picker above using Streams and Cells (FRP primitives). I will rely only on the [book](https://www.manning.com/books/functional-reactive-programming), the [book’s forum](https://forums.manning.com/forums/functional-reactive-programming), the [Sodium library forum](http://sodium.nz/) and minimal web searching in order to give an honest book review from the perspective of a developer who’s mainly done imperative coding in C#. I will set up a GitHub page so you can follow my progress.

Many thanks to Stephen for allowing me to review his book. I am grateful that New Zealand has some great minds doing some great work in emerging areas.