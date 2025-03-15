---
title: 'Smart Date Picker: Cells and Streams'
date: 2016-07-10T13:43:58+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/outsidein-layered-reflected-connected-henk-sijgers.jpg
categories:
  - Programming
---
I covered the [requirements of the smart date picker](/smart-date-picker-requirements/) in my last post, which ended with a data flow diagram. Today I aim to translate that conceptual view to primitives within [Sodium, a functional reactive programming (FRP) library](https://github.com/SodiumFRP/sodium) supported on multiple languages. The only types in Sodium are Cells and Streams. I will attempt to provide a brief explanation.

<!--more-->

## Cells

In essence, a Cell is a value that changes over time. Specifically, it is a generic type Cell<T> that can contain almost any type you specify. Other FRP systems might call it a Behavior, Property or a Signal.

The reason Sodium calls it a Cell is because it is conceptually similar to a cell in a spreadsheet. Individual cells in a spreadsheet contain a value but they are not constant. They can change over time, especially in response to changes in other parts of the workbook. Let me illustrate that with Excel’s personal budget template:

<img class="alignnone wp-image-187 size-large" src="/wp-content/uploads/sites/2/2016/07/personal-budget-template-1.png" alt="personal-budget-template-1" width="640" height="439" srcset="/wp-content/uploads/sites/2/2016/07/personal-budget-template-1.png 1024w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-1.png 300w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-1.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

This is a simple template where you can put in your income, expenses, and savings. It then shows you a summary for the month with a chart and totals. Let’s trace the precedents and dependents of the Total Monthly Income cell:

<img class="alignnone size-large wp-image-188" src="/wp-content/uploads/sites/2/2016/07/personal-budget-template-2.png" alt="personal-budget-template-2" width="640" height="446" srcset="/wp-content/uploads/sites/2/2016/07/personal-budget-template-2.png 1024w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-2.png 300w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-2.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

It is straightforward. Total Monthly Income is a **function** that sums up all income sources, and its result is in turn used by the Cash Balance cell to determine how much extra you have left in your bank account. It is also used by the chart data to construct a delicious pie chart. See what happend when I add a new line of income:

<img class="alignnone size-large wp-image-189" src="/wp-content/uploads/sites/2/2016/07/personal-budget-template-3.png" alt="personal-budget-template-3" width="640" height="451" srcset="/wp-content/uploads/sites/2/2016/07/personal-budget-template-3.png 1024w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-3.png 300w, /wp-content/uploads/sites/2/2016/07/personal-budget-template-3.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

When I do that, the Total Monthly Income, Cash Balance, and pie chart are all updated **simultaneously**. There is no need to manually recalculate because Excel knows the **dependency** between the cells and so knows the correct order to update them.

This brings us to the concept of Streams.

## Streams

A Stream<T> in the world of Sodium represents a “stream of events”. A single mouse click on a button is an event, but multiple mouse clicks on the button throughout the program life cycle would be represented by a stream of mouse clicks.

I find it easiest to think of a Stream as an event handler/listener. I’m sure there are differences between them but I view it as a functional event handler that plugs in to the rest of the Sodium framework. Realistically, one would provide a function that executes when the specified stream fires. This is no different from writing an event handler method but with the added ability to work with Cells and other Sodium primitive operations.

I believe it is called a Stream like the body of water that continuously meanders across the earth. Similarly, a Stream<T> event flows through the program and creates a ripple of changes. In the spreadsheet example above it is logical to conceive of my user input as a stream that leads to other cells being updated.

## Smart Date Picker

So what are the Cells and Streams of the smart date picker I am trying to implement? I would look at the data flow diagram I created last week and ponder upon what values are being held and what sources of events are present.

<img class="size-large wp-image-177" src="/wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png" alt="Smart Date Picker Data Flow Diagram" width="640" height="564" srcset="/wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 1024w, /wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 300w, /wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

At a theoretical level, I think they would be:

Cells:

  * Data Availability Dates
  * Period Type (whether it is Weeks, Months, or Quarters)
  * Date Range (the selected start and end date for the report)

Streams:

  * Select Weeks
  * Select Months
  * Select Quarters
  * Select Start Date
  * Select End Date

You might have noticed a few omissions in my cells and streams. I did not put Start Date and End Date as a Cell because ultimately the date pickers are only sources of events. Whatever the user picks are merely a suggestion. The date bumping logic still needs to decide if it is valid, which then updates the date picker and the Date Range cell with the final outcome.

Changes in Data Availability Dates might resemble a Stream, but I did not model it as one because the changes are initiated from outside the application, i.e. database updates. There is a strict requirement of **referential transparency** in Sodium, which means:

  * you must not perform any I/O;
  * you must not throw any exceptions unless they are caught and handled within the  function;
  * you must not read the value of any external variable if its value can change, but  constants are allowed and encouraged;
  * you must not modify any externally visible state;
  * you must not keep any state between invocations of the function;

(The above list is excerpted from the [book Functional Reactive Programming](https://www.manning.com/books/functional-reactive-programming))

Because checking the Data Availability Dates would require I/O (query the DB), it needs to be done outside of FRP. I know you’re asking: How would one integrate FRP into an application that needs to do I/O and throw exceptions? That is a question I have often asked myself as I read the book but the authors insist on keeping those details under wraps until halfway through the book in Chapter 8. This is done under the guise of keeping the reader’s mind ‘pure’ in FRP-Land before the messy reality of integration hits. I have not formed an opinion regarding this approach yet as I’m still reading the book, but will return to this later.

My list of cells and streams for the smart date picker might change as I start implementing it and discover better ways of looking at the problem. Next week: I will write about some of the operational primitives I may need in Sodium to transform cells and streams from one form to another.

_This is part of an ongoing series where I review the book_[ _Functional Reactive Programming by Stephen Blackheath and Anthony Jones_](https://www.manning.com/books/functional-reactive-programming)_. Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._