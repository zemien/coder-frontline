---
title: 'Web API Practices: AutoMapper for Object-Object Mapping'
date: 2016-09-04T14:51:18+12:00
featuredImg: /wp-content/uploads/sites/2/2016/08/new-york-map-new-york-city.jpg
categories:
  - Programming
---
I'm sharing some of the best practices that I used in a recent ASP .Net Web API and MVC project. I recently gave a presentation to my team on the Top 5 Web API <span style="text-decoration: line-through;">Best</span> Practices, and I continue the series by discussing AutoMapper this week. I struck out the word &#8216;Best' intentionally. There are so many best practices out there but not all of them will suit your unique circumstance. These practices happened to work very well for this project, but your mileage may vary.

<!--more-->

Most projects of a decent complexity will usually need to deal with object to object mapping. This is when an object needs to be converted to another type. Some common scenarios include translating a model to a view-model and flattening a complex object into simple fields for database persistence. This task is traditionally done using a simple mapping method, for example:

```csharp
public static SimpleDatabaseObject Flatten(BusinessLayerObject blo)
{
    return new SimpleDatabaseObject {
        Id = blo.Id;
        Title = blo.Title;
        CategoryId = (int) blo.CategoryEnum;
    };
}
```

While this works well, note that the code is pretty simple – all it does is assign some property as-is or cast an enum to an int. A similar method would also be needed if you wanted to convert it back the other way. A disadvantage is apparent once new properties are added – the developer would need to remember to update both methods if it is meant to be mapped back and forth.

Alternatively, convention-based object to object mappers like AutoMapper take the frustration out of this common scenario with simple code like this:

```csharp
var flattenedDto = Mapper.Map<SimpleDatabaseObject>(someBusinessLayerObjectHere);
```

This is possible once AutoMapper is configured before the application starts. Registering maps is dead simple if it follows common conventions, but it is also fairly easy to customize mapping. Here is an example for the BusinessLayerObject to SimpleDatabaseObject mapping:

```csharp
Mapper.CreateMap<BusinessLayerObject, SimpleDatabaseObject>()
.ForMember(d => d. CategoryId, o => o.MapFrom(y => (int) y.CategoryEnum));
```

Properties that have the same name and compatible types are mapped automatically, such as Id and Title strings. Those that follow conventions happen automagically too, e.g. BookAuthorLastName can be resolved by Book.Author.LastName. Properties that have a different name or special conversion rules will need a bit of help as I’ve shown above.

Using AutoMapper has greatly simplified our Web API development. It is normal for us to expose only a subset of the model through the RESTful interface. There are also rules around how to return certain data types. True, we still need to make sure new properties are added on both the source and destination types, but we only need to configure all the mapping rules in one place.

A practical disadvantage we have encountered is around combining multiple objects into one. I am sure there are ways to configure AutoMapper to do so, but we have found it easier to specify manual mapping methods if we need to construct an object using multiple source objects. The lesson here is to recognise the boundaries of any framework, so we can break away when we need to.