---
title: 'Smart Date Picker: Requirements and Test Cases'
date: 2016-07-02T22:49:16+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/in-royal-cloacks-they-stood-henk-sijgers.jpg
categories:
  - Programming
aliases:
  - /smart-date-picker-requirements/
---
I hinted three weeks ago that I wished to [learn functional reactive programming](/events-are-evil/) by implementing a smart date picker. Since then I shared my [impressions on the book](/first-impressions-functional-reactive-programming-sodium/) but have yet to delve into actually working on the control. I haven't had much time to read the book (curse you, Uncharted 4), and so I am still a bit clueless about where to start. It's time to take the plunge though, and I will outline my stumbles through thinking about business requirements from a data perspective. In this post I flesh out the requirements and test cases a bit more and think about how they rely on each other.

<!--more-->

## Requirements

Let's first revisit the smart date picker itself and explain the requirements a bit more. It looks something like this:<figure id="attachment_148" style="width: 300px" class="wp-caption aligncenter">

<img class="size-medium wp-image-148" src="/wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png" alt="Smart date picker" width="300" height="153" srcset="/wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png 300w, /wp-content/uploads/sites/2/2016/06/Smart-Date-Picker.png 570w" sizes="(max-width: 300px) 100vw, 300px" /><figcaption class="wp-caption-text">Smart Date Picker</figcaption></figure> 

This control is used as part of a larger data reporting app. Users select how they wish to view the data – by weeks, months, or calendar quarters. They then pick the dates they wish to run the report for. The start and end dates will automatically adjust (also known as bumping) to ensure a valid report range. Specifically, weeks will always be Sunday to Saturday, months will always go from the first to the end of a month, and quarters will go from the first of a quarter to the end of another quarter.

## Test Cases

Here are some test cases (dates in dd/MM/yy):

  1. By weeks: Currently showing 12/06/16 (Sunday) to 18/06/16 (Saturday) 
      1. Change start date to 29/06/16 (Sunday). It now shows 29/05/16 to 18/06/16.
      2. Change start date to 01/06/16 (Wednesday). It now shows 29/05/16 to 18/06/16.
      3. Change start date to 27/06/16 (Monday). It now shows 26/06/16 to 02/07/16 (bumps both date to ensure a valid range).
  2. By months: Currently showing 01/06/16 (first of the month) to 30/06/16 (last of the month). 
      1. Change end date to 27/06/16. It now shows 01/06/16 to 30/06/16 (no change).
      2. Change end date to 31/07/16. It now shows 01/06/16 to 31/07/16.
      3. Change end date to 28/02/16. It now shows 01/02/16 to 29/02/16.
  3. By quarters: Currently showing 01/01/16 (first day of quarter 1) to 30/06/16 (last day of quarter 2). 
      1. Change start date to 15/03/16. It now shows 01/01/16 to 30/06/16 (no change).
      2. Change end date to 15/03/16. It now shows 01/01/16 to 31/03/16.
      3. Change start date to 01/10/16. It now shows 01/10/16 to 31/12/16.

An additional constraint to the basic rules is the amount of data available. It can only show dates for when data can be reported, and bump dates as necessary. The requirements state that this comes from a SQL query, but for the purposes of this exercise, it will be mocked to only have data for 2016.

Extending the test cases above for data availability 01/01/16 to 31/12/16:

  1. By weeks: Currently showing 12/06/16 (Sunday) to 18/06/16 (Saturday) <ol start="4">
      <li>
        Change end date to 15/02/17. It now shows 12/06/16 to 31/12/16 (Saturday).
      </li>
      <li>
        Change start date to 15/02/17. It now shows 25/12/16 (Sunday) to 31/12/16 (Saturday).
      </li>
    </ol>

  2. By months: Currently showing 01/06/16 (first of the month) to 30/06/16 (last of the month). <ol start="4">
      <li>
        Change start date to 15/02/17. It now shows 01/12/16 to 31/12/16.
      </li>
      <li>
        Change end date to 20/12/15. It now shows 01/01/16 to 31/01/16.
      </li>
    </ol>

  3. By quarters: Currently showing 01/01/16 (first day of quarter 1) to 30/06/16 (last day of quarter 2). <ol start="4">
      <li>
        Change end date to 15/05/15. It now shows 01/01/16 to 31/03/16.
      </li>
      <li>
        Change start date to 23/08/17. It now shows 01/10/16 to 31/12/16.
      </li>
    </ol>

There are cases where there wouldn’t be enough data to create a valid range. In those instances, it stops bumping and selects the minimum or maximum date of the data availability.

Tests might look like this:

<ol start="4">
  <li>
    Control currently set to Weeks and showing 12/06/16 to 18/06/16. Data only available for 05/06/16 to 30/08/16. <ol>
      <li>
        The user changes to Quarterly reporting. It now shows 05/06/16 to 30/06/16 even though it is not a whole quarter because that is the most valid it can get.
      </li>
      <li>
        The user changes to Monthly reporting. It now shows 05/06/16 to 30/06/16.
      </li>
    </ol>
  </li>
</ol>

## Data and Dependencies

Those are the general requirements of the smart date picker. I have implemented this in regular C# WPF MVVM architecture, but it produced hard-to-understand code and I ran into issues with some edge cases. I now want to see how it might look like if I redid it using FRP.

A key difference with functional-style programming is the focus on thinking about the problem in terms of how data depends on each other, and then declaring those relationships in code. This contrasts wildly from traditional imperative-style programming where we tell the computer what we want to do and how to do it.

It’s been hard grasping FRP concepts because it required me to change my paradigm from thinking about the sequence of events to just focusing on how data relates to each other. I still remember trying to learn object-oriented programming in uni. It threw me into a mental loop because I had been happy using Visual Basic and couldn’t understand why coding objects was “better” than coding modules and forms. And one day I just got it. I suspect the same struggle lies ahead.

So the first baby step towards thinking in the “problem space”, as the FRP book authors put it, is to map out the data and how they depend on each other. This exercise is surprisingly rare in modern OOP programming even though it’s a logical next step after you have the requirements. Instead, TDD/BDD practitioners jump straight into crafting unit tests that map to requirements, while others segue into defining a sequence of events.

The data and dependency diagram for the smart date picker might look something like this:<figure id="attachment_177" style="width: 600px" class="wp-caption aligncenter">

<img class="size-medium wp-image-177" src="/wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png" alt="Smart Date Picker Data Flow Diagram" width="600" height="530" srcset="/wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 300w, /wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 768w, /wp-content/uploads/sites/2/2016/07/Smart-Date-Picker-Data-Flow.png 1024w" sizes="(max-width: 600px) 100vw, 600px" /><figcaption class="wp-caption-text">Smart Date Picker Data Flow Diagram</figcaption></figure> 

Consider the above a first draft. I might move things around as I understand more FRP.

_This is part of an ongoing series where I review the book [Functional Reactive Programming by Stephen Blackheath and Anthony Jones](https://www.manning.com/books/functional-reactive-programming). Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._

Photo credit: [henk.sijgers (on when I can)](https://www.flickr.com/photos/henk-sijgers) via [VisualHunt](https://visualhunt.com/photos/city/) / [CC BY-NC](http://creativecommons.org/licenses/by-nc/2.0/)