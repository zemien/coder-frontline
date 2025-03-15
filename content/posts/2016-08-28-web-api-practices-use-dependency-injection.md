---
title: 'Web API Practices: Use Dependency Injection'
date: 2016-08-28T10:18:20+12:00
featuredImg: /wp-content/uploads/sites/2/2016/08/syringe-red-book.jpg
categories:
  - Programming
---
I've been working on a web project for the past year that used both ASP .Net (4.5.2) Web API and MVC. The Web API provides a foundation for flexible data access, while MVC uses it to power a rich web application. I recently gave a presentation to my team on the Top 5 Web API <del>Best</del> Practices, and I will reproduce portions of it over the next five entries. I struck out the word &#8216;Best' intentionally. There are so many best practices out there but not all of them will suit your unique circumstance. These practices happened to work very well for this project, and we'll start with a popular one - dependency injection.

<!--more-->

Dependency injection is one particular pattern that implements the Inversion-of-Control principle. To paraphrase [Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)&#8216;s excellent page on it, a class should have all its dependencies provided rather than create them itself. For example, you are provided a complete object to use instead of instantiating a business layer object yourself:

```csharp
public class AwesomeController : ApiController {
    private IBusinessLayer mBusinessLayer;

    public AwesomeController (IBusinessLayer businessLayer){
        mBusinessLayer= businessLayer;
    }

//Logic code that uses mBusinessLayer
}
```

In this case, your dependency on a business layer object is injected into the controller. Your controller can go on with its life completely oblivious to the exact implementation as long as it matches the IBusinessLayer contract.

The concept of dependency injection is pretty simple. Actually linking it to Web API and MVC is slightly more complicated, however. There are many dependency injection libraries in the market - I personally use StructureMap, but others like NInject work in a similar manner. Because they are platform-agnostic, you still need to hook it up to the overall Web API lifecycle. The general steps are:

  1. Configure dependency injection container
  2. Configure Web API/MVC/other system components to use the container
  3. Use it in your code

I usually struggle with Step 2. The number of custom NuGet packages and blog tutorials out there tell me I'm not the only one. I won't add to the cacophony as I find each project to be slightly different. This project is using Web API + MVC, while another project used MVC + SignalR. The key thing to keep in mind is that those technologies have built-in support for dependency injection, and it is a matter of finding the right combination of switches. This is what I used in my project to link the same StructureMap container to both MVC and Web API:

```csharp
public static void Initialize()
{
    SetupService(); //Setup StructureMap mappings
    Resolver = new StructureMapDependencyResolver(Container); //Implements both versions of IDependencyResolver
    DependencyResolver.SetResolver(Resolver); //For MVC Controllers
    GlobalConfiguration.Configuration.DependencyResolver = Resolver; //For Web API Controllers
}
```

It's very smooth sailing once the plumbing is hooked up! I add new dependencies in the container and have confidence it gets used correctly anywhere in my web project. My code is now decoupled from my dependencies. (A valid criticism is that I am now dependent on dependency injection, but it's gotta start somewhere right?)

ASP .Net Core takes this one step further and implements its own simple dependency injection system. This will greatly increase adoption as the difficult Step 2 is more or less removed. However, I am still waiting with bated breath for a stable production version after my [last run-in with ASP .Net Core](/cutting-edge-technology-cuts/).

Photo credit: [SlipStreamJC](https://www.flickr.com/photos/slipstreamjc/2060643626/) via [Visual Hunt](https://visualhunt.com/) / [CC BY-NC](http://creativecommons.org/licenses/by-nc/2.0/)