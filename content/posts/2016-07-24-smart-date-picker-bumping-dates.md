---
title: 'Smart Date Picker: Selecting and bumping dates'
date: 2016-07-24T13:18:33+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/they-fled-the-setting-sun-henk-sijgers.jpg
categories:
  - Programming
---
This is a direct continuation of the [last post on using FRP primitives](/smart-date-picker-period-type-frp-primitives/) to model changing the period type. I will jump straight into the next part of adding more smart date picker functionality. You might want to review the [entire series](/?s=smart+date+picker) on making a smart date picker if this is your first visit.

<!--more-->

## Code on GitHub

This week’s fully-functional code is up on the [GitHub project](https://github.com/zemien/smart-date-picker), under the 02 – Date Range Picker folder. Each folder corresponds to a week’s post and is self-contained. You will need a .Net 4.6.1 compiler to be able to build and run the project.

## Data Availability Dates

One of the control’s external data source is the dates that the system has data for. I am modelling it in a simple way in this exercise by way of a service class. This DataAvailabilityService exposes a Cell of a DateRange. A DateRange is an immutable struct that contains a start and end DateTime. Immutability is an important concept in functional programming, and a good practice to consider in general.

I use a CellSink in this class. A CellSink and StreamSink are the primary ways to sending data into FRP logic. They extend a Cell or Stream with a send() method that pushes data into FRP-land. There is no other way to instantiate a Cell or a Stream without going through the sinks.

In my simple service I have initialised the CellSink with a default value, but I’ve also added a method that simulates fetching new dates from the external world. I am not using this bit yet so we can ignore this for now.

```csharp
public class DataAvailabilityService
{
    private readonly CellSink<DateRange> mDataAvailabilitySink;

    /// <summary>
    /// Initializes a Data Availability Service
    /// </summary>
    public DataAvailabilityService()
    {
        mDataAvailabilitySink = new CellSink<DateRange>(new DateRange(new DateTime(2014, 1, 1), new DateTime(2016, 07, 31)));
    }

    /// <summary>
    /// Dates that we can run reports for
    /// </summary>
    public Cell<DateRange> DataAvailability
    {
        get
        {
            return mDataAvailabilitySink;
        }
    }
}
```

## Adding date pickers

Once again I am using the SWidget library from the [Sodium FRP library](https://github.com/SodiumFRP/sodium) book examples. It includes a rudimentary date picker control that consists of three combo boxes, instead of a fancy calendar picker. It exposes a SelectedDate Cell property, which contains the user-selected date.

## lift() FRP primitive

lift() is another FRP primitive that works like map() and merge() for multiple cells – you provide a function that combines multiple cells into one. It is called ‘lift’ “because we are ‘lifting’ a function that operates on values into the world of cells.” It is a general functional programming concept that I am still not familiar with.

## The bump dates function

Alright – let’s bring all those Cells together and make a smart date picker, shall we? We now have the following Cells:

  * StartDate
  * EndDate
  * DataAvailability
  * PeriodType

As mentioned, we will use the lift() function to combine all those cells and output a DateRange.

```csharp
//Combine the different cells into one DateRange using a Lift() call to BumpDates()
Cell<DateRange> cDateRange = startDateField.SelectedDate.Lift(
    endDateField.SelectedDate, dataAvailabilityService.DataAvailability, 
    cPeriodTypeCell, ReportDateRangeBumper.BumpDates);
```

The last method argument is a method I’ve defined in a separate helper class.

## Date bumping helper function

ReportDateRangeBumper is a helper class that is the logical ham in this FRP sandwich. I won’t go into details as it’s pretty simple once you read it. What’s important is how simple it is – it doesn’t maintain state or have external dependencies. This retains referential transparency and also makes it easy to unit test. In other words, it will always return the same result with the same input parameters.

This is a massive improvement over the non-FRP way of doing things. Previously, I had logic scattered in various event handlers that made it impractical to unit test. I also could not guarantee the order of the events.

## Displaying the bumped dates

Finally, I display the bumped dates by using another map() call to convert the DateRange to a string representation. I add this to a SLabel control on the screen.

```csharp
SLabel selectedDate = new SLabel(cDateRange.Map(dr => dr.ToString()));

//Some other code

ResultsContainer.Children.Add(selectedDate);
```

## Next steps

There are a few important things I still need to figure out. I’ve added TODO notes throughout the code.

  * Differentiate between changing the start date vs. the end date picker.
  * Update the SDateField once I’ve bumped the selected dates.
  * Simulate changing data availabilities while the program is running.

Comments on how to achieve those will be much appreciated!

_This is part of an ongoing series where I review the book_[ _Functional Reactive Programming by Stephen Blackheath and Anthony Jones_](https://www.manning.com/books/functional-reactive-programming)_. Kindly note that anything I write here may be incorrect and not indicative of the author’s intent. After all, making mistakes is the mother of learning and this book review is meant to be a truthful reflection of my own experiences grasping a new concept. The review copy I received was version 11 in their Manning’s Early Access Program, so the book’s contents may differ at the time of publication._