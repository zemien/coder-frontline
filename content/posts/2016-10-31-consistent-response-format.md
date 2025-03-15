---
title: 'Web API Practices: Consistent Response Format'
date: 2016-10-31T09:43:56+12:00
featuredImg: /wp-content/uploads/sites/2/2016/10/mistake-spill-slip-up-accident-error-careless.jpg
categories:
  - Programming
aliases:
  - /consistent-response-format/
---
I'm sharing some of the best practices that I used in a recent ASP .Net Web API and MVC project. I recently gave a presentation to my team on the Top 5 Web API <span style="text-decoration: line-through;">Best</span> Practices, and I continue the series by discussing AutoMapper this week. I struck out the word &#8216;Best' intentionally. There are so many best practices out there but not all of them will suit your unique circumstance. These practices happened to work very well for this project, but your mileage may vary. Consistent response formats, however, are pretty much universal and something that all of us should be doing already.

<!--more-->

RESTful APIs are meant for external consumption although it does not have a traditional user interface as such. This means it should cater for various user levels, some of whom may not be familiar with the inner workings of the API. Hence, it is important to return useful and clear messages especially when an error occurs. I think this is pretty clear to most developers.

It is more difficult to ensure consistent response formats when there are multiple programmers building a single API. It is also rare for error message phrasings to be specified up-front in this increasingly Agile world. What I have experienced is each coder coming up with his or her own approach to showing error messages. Common formats include:

  * Just returning HTTP status codes: I am not a fan of this as it can be open to misinterpretation, especially when it comes to input validation errors (e.g. POST-ing an email address with invalid characters).
  * Including a message with the appropriate HTTP status code: This is better, and I suspect is the approach taken by most APIs. This is even provided in the standard [HTTP Results types](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx).
  * Including a message and detailed message with the appropriate HTTP status code: This is the approach I went for, and the response is a JSON object that looks something like:

```js
{
    "Message": "Input validation errors",
    "MessageDetail": "Please fix the following issues: The email address contains invalid characters and the zip code is missing"
}
```

I went with this approach to provide two layers of error messages that can be used to drive a visual error dialog. The Message can be used as the dialog box title, while the MessageDetail forms the dialog box content itself.

Alternatively, I could just display the MessageDetail in a Bootstrap status message at the top of the screen. I effectively open up the options for my API consumers to understand the results and consequently display it to their users in the most appropriate manner.

The next step after deciding on a return format that works, is to make it easy for all developers to use it. The most basic way to do this is to create a POCO that has both fields:

```csharp
public struct ReturnMessage {
    public string Message { get; set; }
    public string MessageDetail { get; set; }
}
```

Developers can then instantiate a ReturnMessage when a success or error message is desired. This is of course just a starting point. A further extension is to create helper types similar to the IHttpActionResult implementations defined in the [System.Web.Http.Results namespace](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) for the most commonly used HTTP codes in your application.

The main takeaway is to make it easy for team members to return consistent response messages. This helps your users and their users down the line. It doesn’t have to be using a ReturnMessage type as I had, but consistent messages go a long way in making a user-friendly Web API.

Photo via [Visual Hunt](https://visualhunt.com/)