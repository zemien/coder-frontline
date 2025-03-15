---
title: First Impressions Functional Reactive Programming with Sodium
date: 2016-06-19T12:26:07+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/building_underpass_painting.jpg
categories:
  - Programming
aliases:
  - /first-impressions-functional-reactive-programming-sodium/
---
_This is part of an ongoing series where I review the book [Functional Reactive Programming by Stephen Blackheath and Anthony Jones](https://www.manning.com/books/functional-reactive-programming). Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._

With that pseudo-disclaimer out of the way, I wanted to share my first impressions of the book’s writing style and organisation. I mentioned last week how I [stumbled upon Functional Reactive Programming (FRP)](/events-are-evil/) and wanted to try implementing a “smart date picker”. I’m just on Chapter 4 and am not ready to start working on that yet. Instead here are some quick-fire thoughts of the book so far:

## What is Functional Reactive Programming?

This is a comparatively young field and so I appreciate that you, dear reader, may not be familiar with this concept. You’re not alone – neither do I! Instead of trying to come up with a definition that will most certainly be incorrect, I encourage readers to check out the first chapter of this book ([see the sidebar](https://www.manning.com/books/functional-reactive-programming)). It is free to read and gives a comprehensive overview of where the book aims to take you.

## Sodium FRP and Java

The authors of the book created their own [BSD-licensed FRP library called Sodium](https://github.com/SodiumFRP/sodium), which is implemented in several languages including Java and .Net. The book itself uses Sodium and Java as the main instructional vehicles, even though the concepts should be applicable to other languages and similar FRP libraries. However, my own background as a C# .Net developer makes things slightly less pleasant. While there are many similarities between Java and C#, the differences are like tiny stones stuck in my shoe. Not painful enough to stop walking, but a niggling nuisance nonetheless. Consequently, I find the code samples to be quite verbose from what I’m used to, and I have yet to identify which parts are verbose because of FRP and which are just Java’s eccentricities.

Pro Tip: The [book samples](https://github.com/SodiumFRP/sodium/tree/master/book) are also implemented in C#, which are all part of the GitHub project. I would recommend running the C# examples instead of the Java ones from the book. If you’re so inclined.

## Target Audience

This book is targeted towards “programmers familiar with object oriented programming. No prior knowledge of functional programming is needed.” That fits my profile almost perfectly. This is by no means a “For Dummies” book though. It is not ashamed to throw readers like myself in the deep-end, and then stand and laugh as I fumble to stay afloat.

OK, that last line was a tad melodramatic. I’m only mildly exaggerating of course. While Chapter 1 was a gentle introduction into what FRP is and isn’t, Chapter 2 dives into the primitives that make up an FRP system. I appreciate the use of examples and diagrams to illustrate new concepts such as cells and streams, but I always feel like I am missing something due to a lack of prior functional programming experience.

There is no time to catch my breath though as the hits keep coming with one concept after another. Looking back, this is one of the densest Chapter 2 of any book I’ve read. This either says something about the depth of this book, or the shallowness of my mind. Take it how you will.

Pro Tip: Having _some_ awareness of what functional programming is will absolutely help readers understand this book better.

I will continue sharing more impressions about this book next week and hopefully start working on my date picker widget.

<div class="subsection subsection-embed">
  Photo credit: <a href="https://www.flickr.com/photos/henk-sijgers/15625028439/">henk.sijgers (on when I can)</a> via <a href="https://visualhunt.com/photos/city/">Visual hunt</a> / <a href="http://creativecommons.org/licenses/by-nc/2.0/">CC BY-NC</a>
</div>