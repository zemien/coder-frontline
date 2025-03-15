---
title: Cutting-Edge Technology Cuts
date: 2016-04-24T04:51:09+12:00
featuredImg: /wp-content/uploads/sites/2/2016/04/barbed-wire.jpg
categories:
  - Programming
---
I bring you a cautionary tale this week about the dangers of using cutting-edge technology and how it can cut you. Like many programmers I get excited with the prospect of using new technology, frameworks, or languages – especially for commercial uses. It’s one thing to try stuff out on the weekend and build tutorial projects, but it’s another ball game to actually build something someone would buy. So I rubbed my hands with glee when the opportunity arose to redesign a web app that was written in ASP .Net MVC 3.

<!--more-->

While there are some firms that like to upgrade their technology with every major release, they are the exception from the norm. For the majority, technology is merely the vehicle to deliver business value to customers, and there is no reason to change vehicles while the current one is still working fine. That’s why there are still a lot of production software running on COBOL or VB6.

The web app product that I’ve been working on (let’s call it Firewatch – no relation to the [interesting game by Campo Santo](http://www.firewatchgame.com/)) was created about 5 years ago on ASP .Net MVC 3. Cutting-edge technology at that time, but in the intervening years there’s been a bit of an explosion in the web development world with the rise of mature frameworks like Bootstrap, Node JS, and AngularJS. They have redefined how most web apps are developed these days, and Microsoft hasn’t been quiet either. They introduced Web API to support creating RESTful services and the upcoming ASP .Net Core further moves the framework towards being cross-platform and use de-facto web standards.

In late 2015 I was asked to look into redesigning the Firewatch frontend and saw it as a chance to move things up a notch. I looked around and read about the upcoming ASP .Net 5 (codename vNext) and it looked really promising. According to the [product roadmap](https://github.com/aspnet/Home/wiki/Roadmap) at the time, they were releasing RC1 in November followed by RTM in the first quarter of 2016. I thought “Hmm that fits the project schedule just fine. I can start with RC1 and transition onto the RTM when it’s released.”

I did an impassioned presentation to my colleagues on why going onto the cutting-edge technology of ASP .Net 5 was the right choice and got them quite excited. The first couple of weeks building up the basic views went fine, but then I hit a major roadblock – referencing projects built on classic .Net was a real hassle. There were some real oddities in how it tries to wrap existing projects into NuGet packages, and I wasn’t the only one having [issues with wrapping](https://github.com/aspnet/Tooling/issues/45#issuecomment-182812165) either. That thread was created May 2015 and is still getting occasional posts from frustrated developers 11 months later. Even when I managed to do the workaround on my machine, the build machine did not recognise the wrapped packages.

The last straw that broke the camel (i.e. me) was the announcement that they’re [renaming ASP .Net 5 to ASP .Net Core](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). It finally dawned on me that vNext was not just a major step up, it’s a major step to the side. I had picked ASP .Net 5 on the basis that it is the next major release, as its name would suggest. This announcement finally acknowledges that this change is foundational and developers should not assume their prior knowledge is transferable to the new wild west of .Net Core.

I threw my hands up and backtracked on my recommendations. I proposed moving down to ASP .Net 4.5.1 with MVC 5 and Web API 2 while Microsoft ironed out the kinks. Indeed, as I write this, the roadmap has been updated to have an RC2 with a TBD delivery date. The only issue is once Firewatch gets all its needs met by the existing framework, there would be little reason to do a major architectural update just to go onto .Net Core.

The lesson I learned is: cutting-edge technology might be cool, but be prepared to bleed.

Reminders to self:

  * Don’t trust product roadmaps even from established vendors like Microsoft.
  * When evaluating any technology, consciously seek out existing issues and what many developers constantly complain about. GitHub issues page is a fantastic resource.
  * Rapidly prototype a test project containing unique idiosyncrasies of my project and not just stick within the sample projects playground which always works brilliantly.

Photo credit: [Fernando Reyes Palencia](https://www.flickr.com/photos/fernandoreyes/2532211558/) via [Visualhunt.com](https://visualhunt.com/) / [CC BY-SA](http://creativecommons.org/licenses/by-sa/2.0/)