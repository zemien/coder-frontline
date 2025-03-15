---
title: Taking Shortcuts Make Us Look Stupid
date: 2017-06-10T22:24:11+12:00
featuredImg: /wp-content/uploads/sites/2/2017/06/st-patrick-day-gone-awry.jpg
categories:
  - General
---
I came across this error message when I was setting up a new Visual Studio Team Services build and found it quite funny. A valid ASP .Net Core project does not need a web.config file when it's not hosted in IIS, and it definitely doesn't need a wwwroot folder if it's an API that does not serve static files.<figure id="attachment_341" style="width: 640px" class="wp-caption aligncenter">

[<img class="wp-image-341 size-large" src="https://www.coderfrontline.com/wp-content/uploads/sites/2/2017/06/dotnet-core-build.png" alt="" width="640" height="143" srcset="/wp-content/uploads/sites/2/2017/06/dotnet-core-build.png 1024w, /wp-content/uploads/sites/2/2017/06/dotnet-core-build.png 300w, /wp-content/uploads/sites/2/2017/06/dotnet-core-build.png 768w" sizes="(max-width: 640px) 100vw, 640px" />](https://www.coderfrontline.com/wp-content/uploads/sites/2/2017/06/dotnet-core-build.png)<figcaption class="wp-caption-text">Error when running dotnet publish command</figcaption></figure> 

It made me reflect on my past choices when designing software solutions. We sometimes take shortcuts, whether due to time constraints or insufficient business analysis. It usually makes us look stupid, sooner or later. No profound lessons in this post. It's just something to remind me that I should look where I can put in that extra 5%.

Featured photo credit: [eilonwy77](https://www.flickr.com/photos/eilonwy77/6847865750/) via [Visual Hunt](https://visualhunt.com/re/17524f) /  [CC BY-SA](http://creativecommons.org/licenses/by-sa/2.0/)