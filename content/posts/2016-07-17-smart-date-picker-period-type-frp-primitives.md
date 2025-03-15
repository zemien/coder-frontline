---
title: 'Smart Date Picker: Changing the period type using FRP Primitives'
date: 2016-07-17T15:05:55+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/the-beacon-henk-sijgers.jpg
categories:
  - Programming
aliases:
  - /smart-date-picker-period-type-frp-primitives/
---
I’ve faffed about at the fringes of functional reactive programming (FRP) for a while now, but I think it’s time to take a cold plunge into actually using some Sodium FRP primitives. FRP primitives are the fundamental operations (methods or functions) that make up the core of FRP. What makes them primitive is that they cannot be expressed using any other combination of operations.

My objective over the next few weeks is to reimagine the smart date picker’s data flow/dependency diagram by using FRP primitives. [Sodium](https://github.com/SodiumFRP/sodium), the library I am using for this lesson, has ten FRP primitives. There isn’t enough space here to explain each one in detail (I recommend the book [Functional Reactive Programming](https://www.manning.com/books/functional-reactive-programming) for this purpose), but I will briefly introduce them as I go along.

Today I start implementing the changes to the period type.

<!--more-->

## Changing the Period Type

This portion of the logic is concerned with how selecting one of the three period types (Weeks/Months/Quarters) registers a change in the Period Type cell, which then flows on to other parts of the program. You may wish to refer to the [DFD](/smart-date-picker-requirements/) and [cells/streams](/smart-date-picker-cells-and-streams/) breakdown covered in previous weeks.

First, we’ll have an `enum` called `PeriodType` which has three values: Weeks, Months, and Quarters. Each radio button, when clicked, will output its corresponding enum type. Because the click event is a stream, what we want to do is **convert** the stream of one type (a click event) to another (PeriodType). This is the basis of the `map()` primitive. For C# developers who have used LINQ, this is quite similar to the [`select` clause](https://msdn.microsoft.com/en-us/library/bb384087.aspx).

We then want to **merge** the three separate streams into a single stream that will update the cell **holding** the selected PeriodType. We do this using two aptly-named primitives `merge()` and `hold()`.

`merge()` and its close sibling `orElse()` takes in as many streams as you need but outputs one stream. You provide a function to `merge()` that combines two streams in some way, whereas `orElse()` just takes the first stream that occurs. In our case, `orElse()` would do the job as users will only pick one of the three PeriodType options at a time.

Next, we use a `hold()` function to store the value passing through the stream as a cell. In this case, we are holding the selected PeriodType. This cell can then be used by other parts of the program such as generating a sales report.

## Putting it together

I’ve created a [smart-date-picker repository on GitHub](https://github.com/zemien/smart-date-picker) for this exercise. They are organised in self-contained folders for each week and I invite you to do a checkout and run it for yourself. I have based it on the SWidgets custom controls that power the book samples, as they have added the necessary FRP hooks to the outside world. This leaves me free to focus on writing the FRP logic.

Check out the folder 01 - Period Type in GitHub. The relevant code bits I added in to a new WPF window are:

```csharp
public MainWindow()
{
    InitializeComponent();

    //Wrap FRP initialisation code in a transaction
    Transaction.RunVoid(() =>
    {
        //Create selection controls
        SButton weeks = new SButton { Content = "Weeks", Width = 75 };
        SButton months = new SButton { Content = "Months", Width = 75, Margin = new Thickness(5, 0, 0, 0) };
        SButton quarters = new SButton { Content = "Quarters", Width = 75, Margin = new Thickness(5, 0, 0, 0) };

        //Convert click event streams into PeriodType streams
        Stream<PeriodType> sWeeks = weeks.SClicked.Map(_ => PeriodType.Weeks);
        Stream<PeriodType> sMonths = months.SClicked.Map(_ => PeriodType.Months);
        Stream<PeriodType> sQuarters = quarters.SClicked.Map(_ => PeriodType.Quarters);

        //Merge the streams into a single stream with orElse
        Stream<PeriodType> sPeriodType = sWeeks.OrElse(sMonths).OrElse(sQuarters);

        //Hold a value of the stream in a cell, and also specifies Weeks as the default value
        Cell<PeriodType> cPeriodTypeCell = sPeriodType.Hold(PeriodType.Weeks);

        //Display the contents of the cell by mapping it to a string that SLabel can display
        Cell<string> cPeriodTypeString = cPeriodTypeCell.Map(p => p.ToString());
        SLabel lbl = new SLabel(cPeriodTypeString) { Width = 75, Margin = new Thickness(5, 0, 0, 0) };

        //Add controls to WPF StackPanel container
        Container.Children.Add(weeks);
        Container.Children.Add(months);
        Container.Children.Add(quarters);
        Container.Children.Add(lbl);
    });
}
```

I relied heavily on the book’s RedGreen sample project and SWidgets library to build this. You should note that I have used the provided SButton component instead of implementing my own Sodium-enabled radio button because I want to keep my code simple for now. Nevertheless, they operate the same despite some visual differences.

It looks very different from the usual OOP code one would normally write. I have to say that it does read quite logically from top to bottom when laid out using FRP primitives. The best part? No mutable PeriodType property that can get accidentally overridden without our explicit permission.

## Next Week

I will go through a similar exercise with expressing the date pickers and data availability dates using FRP primitives, before combining them together in the following weeks. Comments, questions, and corrections are always welcome.

_This is part of an ongoing series where I review the book_[ _Functional Reactive Programming by Stephen Blackheath and Anthony Jones_](https://www.manning.com/books/functional-reactive-programming)_. Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._