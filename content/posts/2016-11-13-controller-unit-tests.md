---
title: 'Web API Practices: Controller Unit Tests'
date: 2016-11-13T20:58:37+12:00
featuredImg: /wp-content/uploads/sites/2/2016/11/mixer-amplifier-amp-power-mixer-controller-audio.png
categories:
  - Programming
---
I'm sharing some of the best practices that I used in a recent ASP .Net Web API and MVC project. I recently gave a presentation to my team on the Top 5 Web API <span style="text-decoration: line-through;">Best</span> Practices, and I continue the series by discussing controller unit tests. I struck out the word &#8216;Best' intentionally. There are so many best practices out there. These practices happened to work very well for this project, but your mileage may vary. However, I think most people understand the value of unit testing, and I’m here to show you some tips in regards to writing controller unit tests.

<!--more-->

If you’ve followed the series thus far, you will find writing controller unit tests to be a cinch because you have [dependency injection](/web-api-practices-use-dependency-injection/) on your ASP .Net controllers. This means the controller itself does not instantiate any of its dependencies such as repositories or business layers. Instead you can use a mocking/stubbing framework such as RhinoMock or Moq to simulate the other layers. I won’t go into detail today as there are great tutorials out there, plus I am starting another series on unit test patterns in the new year.

I will focus instead on how to mock certain bits of ASP .Net ‘magic’ that happens behind the scene. I will give two examples: first for mocking the UrlHelper object and another for mocking the logged in user. Those two scenarios come up quite regularly when crafting controller unit tests.

## UrlHelper

There’s a nifty Url helper in ASP .Net Web API controllers whereby you can generate a hyperlink based off the controller action name and any parameters that you give it. This is handy if you’re making [Hypermedia APIs](/hypermedia-api-responses/) as I discussed last week. Imagine you have a POST method and you wish to respond with the link to the newly created user account. This will generate a working link for the action “GetAccountById” with the new ID specified in the route.

```csharp
//Returns something like http://mybank:8085/api/account/15
Url.Link("GetAccountById", newAccountId)
```

Here is an example method you can call from controller unit tests that set up the UrlHelper object so it has a route table to reference when running your tests:

```csharp
/// <summary>
/// Setups the controller for tests, mainly to create a UrlHelper class and register action names.
/// </summary>
/// <param name="controller">The controller.</param>
private void SetupControllerForTests(ApiController controller)
{
    var config = new HttpConfiguration();
    var request = new HttpRequestMessage(HttpMethod.Post, "http://mybank/api/");
    var route = config.Routes.MapHttpRoute("GetAccountById", "account/{id}");
    var routeData = new HttpRouteData(route, new HttpRouteValueDictionary
    {
        {"id", 15}
    });
    controller.ControllerContext = new HttpControllerContext(config, routeData, request);
    UrlHelper urlHelper = new UrlHelper(request);
    controller.Request = request;
    controller.Request.Properties[HttpPropertyKeys.HttpConfigurationKey] = config;
    controller.Request.Properties[HttpPropertyKeys.HttpRouteDataKey] = routeData;

    controller.Url = urlHelper;
}
```

&nbsp;

## Logged-in User

The logged-in user is an IPrincipal object as part of the HttpContext in every controller. Here’s one way to mock it, according to a [StackOverflow answer](https://stackoverflow.com/a/17236476/824036):

```csharp
// create mock principal
var mocks = new MockRepository(MockBehavior.Default);
Mock<IPrincipal> mockPrincipal = mocks.Create<IPrincipal>();
mockPrincipal.SetupGet(p => p.Identity.Name).Returns(userName);
mockPrincipal.Setup(p => p.IsInRole("User")).Returns(true);

// create mock controller context
var mockContext = new Mock<ControllerContext>();
mockContext.SetupGet(p => p.HttpContext.User).Returns(mockPrincipal.Object);
mockContext.SetupGet(p => p.HttpContext.Request.IsAuthenticated).Returns(true);

// create controller
var controller = new MvcController() { ControllerContext = mock.Object };
```

However, Scott Hanselman has mentioned another way to inject the IPrincipal object into individual methods instead. This would avoid having to mock the HttpContext as well. His blog post talks about [passing IPrincipal objects into MVC controllers](http://www.hanselman.com/blog/IPrincipalUserModelBinderInASPNETMVCForEasierTesting.aspx) but I see no problems getting this to work with Web APIs.

## Summary

It is certainly quite easy to write controller unit tests if you begin with that end in mind. There are other things in the ASP .Net pipeline model that aren’t as easily tested, but it is my opinion that they are part of the framework. Hence it is the responsibility of the framework provider (Microsoft) to test those, not us developers.