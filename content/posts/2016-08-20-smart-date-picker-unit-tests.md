---
title: 'Smart Date Picker: Unit Tests'
date: 2016-08-20T22:13:02+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/space-telescope-mirror-segments-james-webb-cosmos.jpg
categories:
  - Programming
---
I finally add some unit tests for my date range bumper logic. I based it on my earlier blog [post on test cases](/smart-date-picker-requirements/) with some additions. I subsequently found bugs that I wouldn’t have found otherwise. I also added FRP code to differentiate between start and end date selection, which I’ll briefly talk about later. But first, a community service message regarding unit tests.

<!--more-->

I’ll admit it – I don’t write unit tests as much as I would like to. I’ve only worked at companies with a lot of legacy code that use various levels of automated unit tests. Test-Driven Development (TDD) emphasises writing tests to guide the implementation. Many people consider that as working backwards. I used to be a doubter until I actually tried it for myself. The big advantage of TDD is that it creates testable code up front. A lot of legacy code works fine, but was designed with lots of internal dependencies that make it hard to mock and stub without contorting oneself. This leads to people giving up writing and maintaining unit tests, as the cost becomes too high. Most people mean well – but if the right [culture](/why-care-about-culture/) and code conditions are not present unit testing becomes difficult to practice.

I now make it a point to write unit tests where I can. This little FRP project is no different. FRP helped me to unify the logic into one public method which I could unit test vigorously. This is now up on the [GitHub project under the 03 – Unit Tests](https://github.com/zemien/smart-date-picker) folder. Just a note that I’m using NUnit 3.x, which may require updated test suite runners on your environment.

I also modified the SWidget DateField to expose a Stream<DateTime> property in addition to the existing Cell<DateTime>. I’m not sure if I did it correctly, but I needed a Stream for when the date changes in order to merge() them into a single Stream denoting which date to prioritise. There is slightly different bumping behaviour depending on which date picker the user picked.

The next and final step is to loop the resultant DateRange back into the DateField control. This is one part I’m not sure how to implement. I will need to read the advanced chapters of the book, which I don’t have the time for until I’m done with my Marketing Management course. This is a wrap on FRP for now!

_This is part of an ongoing series where I review the book_[ _Functional Reactive Programming by Stephen Blackheath and Anthony Jones_](https://www.manning.com/books/functional-reactive-programming)_. Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._