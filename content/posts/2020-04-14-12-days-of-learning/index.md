---
title: 12 Days of Learning
date: 2020-04-17T11:08:00+12:00
featuredImg: /12-days-of-learning/books.jpg
categories:
  - Programming
---

The greatest challenge for developers who want to stay on the forefront of technology is the sheer amount of innovation happening everywhere. It is simply overwhelming for anyone to know everything. My advice for people starting out is to focus on one stack, e.g. React web apps, and get really comfortable with it so you build a solid technical foundation. It is still natural to feel like you're falling behind when others talk about some cool tech they used. I feel it all the time, to be honest.

I am in the middle of changing jobs and had roughly two weeks of 'me' time. I was planning to take a holiday but the Covid-19 pandemic scuppered any thoughts of that nature. Therefore, I decided to make this a time of learning instead. I called it my "12 Days of Learning" - one new tech each workday. My goal was not to become an expert on each topic by the end of 8 hours, but to know enough to hold a conversation about it. This means I:

- Understand the rationale behind this technology
- Know the right scenarios to apply this technology
- Possess basic hands-on knowledge and can read the syntax

I had set out an ideal plan of what I wanted to learn but real life has a way of throwing banana peels to slip on. I adapted my plans, and the following is a summary of my thoughts on each technology I picked up, along with the learning resources I used. Here's why I picked those subjects:

- Must cover a large surface area. I selected topics around web, backend, databases, and infrastructure.
- I wanted to learn about the clever architectures, designs, and decisions made by people much smarter than I am.
- It would be unlikely for me to use some of those subjects professionally, e.g. Ethereum blockchain programming and Kafka.

## 12 Days of Learning Topics
1. [Aspect-Oriented Programming (AOP)](#aspectoriented-programming-aop)
2. [GraphQL](#graphql)
3. [Kafka](#kafka)
4. [Ethereum Blockchain programming](#ethereum-blockchain-programming)
5. [SSIS](#ssis)
6. [AWS Lambda with .Net](#aws-lambda-with-net)
7. [Amazon CloudSearch](#amazon-cloudsearch)
8. [Terraform](#terraform)
9. [React state management](#react-state-management)
10. [PWA](#pwa)
11. [SRE](#sre)
12. [GoLang](#golang)
13. [Feature Management](#feature-management)

## Aspect-Oriented Programming (AOP)
Learning sources:
- This fantastic [Dependency Injection e-book](https://www.manning.com/books/dependency-injection-principles-practices-patterns)

Thoughts:
- AOP is a really interesting technique, which many developers will be uncomfortable with until they encounter the problems that AOP tries to solve
- I am &#x1F4AF;% going to recommend this approach when I come across suitable scenarios

## GraphQL
Learning sources:
- Pluralsight: [Building GraphQL APIs with ASP.NET Core](https://app.pluralsight.com/library/courses/building-graphql-apis-aspdotnet-core/table-of-contents)

Thoughts:
- GraphQL is a useful paradigm in itself, and it makes building web clients so much easier when you can succintly describe the shape of the data you want
- However, it feels very unnatural to integrate it with Entity Framework and SQL Server. I end up introducing another abstraction layer - essentially re-implmenting EF methods in a format GraphQL understands. My many 'repository' methods still remain rather than be replaced by GraphQL, which is what I was hoping for.
- I see a bright future for GraphQL-like technologies, but it must be paired with a data store that has native support for GraphQL. I have so far heard of FaunaDB and Dgraph. I might add them to my next learning backlog.

## Kafka
Learning sources:
- Pluralsight: [Designing Event-driven Applications Using Apache Kafka Ecosystem](https://app.pluralsight.com/library/courses/designing-event-driven-applications-apache-kafka-ecosystem/table-of-contents)
- [Confluent Community Quick Start](https://docs.confluent.io/current/quickstart/cos-docker-quickstart.html#cos-docker-quickstart)

Thoughts:
- Kafka appeared on my tech radar after a presentation by Confluent. I like the idea of a messaging model but Kafka appeared to take this many steps further.
- It is definitely an "enterprise" solution. Entirely overkill for small startups based on the number of servers needed and maintenance required. This can be a niche filled by cloud providers such as [Amazon Managed Streaming for Apache Kafka (Amazon MSK)](https://aws.amazon.com/msk/) but the pricing is still geared towards customers with cash to spend.
- The possibility of using Kafka to do event sourcing intrigued me but I did not learn how to do this in the course.
- I am especially impressed with how easy it is to use KSQL to set up streams and queries with a SQL-like language.

## Ethereum Blockchain programming
Learning sources:
- Pluralsight: [Developing Applications on Ethereum Blockchain](https://app.pluralsight.com/library/courses/975932a6-f99a-4c21-97ea-0353070a007d)

Thoughts:
- It was quite easy to get started with learning with a browser IDE like [Remix](https://remix.ethereum.org/)
- The Solidity programming language is close enough to C-based languages which makes it easy to pick-up
- I like Solidity's blockchain-specific enhancements to the language. They are quite intuitive.
- Truffle framework supports unit-testing Solidity. Yes!
- Ethereum blockchain is secure, but there are still loopholes and you can still write insecure code. It is not automatically secure.
- Cost is calculated based on the amount of 'gas' your code needs to run, so it really forces the developer to be efficient.

## SSIS
Learning sources:
- Pluralsight: [Extracting and Transforming Data in SSIS](https://app.pluralsight.com/library/courses/extracting-transforming-data-ssis/table-of-contents)

Thoughts:
- SSIS has been around a long time but its ease of use and flexibility will ensure its longevity while relational databases are a thing.
- My main concerns are around testability and error handling. It requires very manual testing approaches.
- It has to drag legacy support along, such as multiple database connection drivers and Windows-only runtime, which will exclude it from many people's minds when starting a greenfield project. It really shouldn't.
- I can see how it has influenced modern low-code platforms like Zapier and Microsoft Flow (Power Automate).
- I also watched a few clips on Azure Data Factory, and it's clear they work in tandem, and ADF does not replace SSIS.

## AWS Lambda with .Net
Learning sources:
- Official docs, e.g. [Building Lambda Functions with C#](https://docs.aws.amazon.com/lambda/latest/dg/lambda-csharp.html) and [.Net Core CLI](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-cli.html)

Thoughts from someone used to Microsoft Azure:
- Visual Studio (Windows) [AWS Toolkit](https://aws.amazon.com/visualstudio/) is pretty good - it makes deployment as easy as deploying to Azure Functions
- Lambdas do not have a HTTP endpoint built-in, unlike Azure Functions. I need to bolt on AWS API Gateway to call it. This requires changing how I deal with the default lambda function parameters significantly, which confused me for a quite awhile.
- Resource groups in AWS are an optional construct unlike Azure whereby everything must be in a Resource Group.
- IAM policies are very confusing. I gave up and kept my APIs open. From what I gather, I need to grant permissions on both the calling resource and the called resource, but I'm not sure how to construct the policy code.

## Amazon CloudSearch
Learning sources:
- Official docs, e.g. [Developer Guide](https://docs.aws.amazon.com/cloudsearch/latest/developerguide/what-is-cloudsearch.html) and [.Net SDK Reference](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/Index.html)

Thoughts:
- Documentation for .Net SDK is basic. It is very comprehensive but hard to find what you need. It took me so long to find out where to specify the CloudSearch search endpoint in the SDK (hint: it's the `ServiceURL` property of the `AmazonCloudSearchDomainConfig` class)
- The CloudSearch service itself was easy to get started and configure. I could upload my CSV dataset and it parsed it without trouble.
- The [pricing](https://aws.amazon.com/cloudsearch/pricing/) page notes "Pricing is per instance-hour consumed for each search instance, from the time the instance is launched until it is terminated. Each partial instance-hour consumed is billed as a full hour." The second sentence puzzles me. I interpret it as: if CloudSearch gets exactly one search that takes 5 seconds to execute, Amazon will still charge me for a full hour. This could be an expensive service on a cost per search basis if your site/app only gets a few searches a day.

I used AWS Lambda with API Gateway to expose a REST API to an Amazon CloudSearch domain. This was used to power the item search backend in the [Essential Or Not website](https://essentialornot.org.nz) created by my partner Gage during the SARS-Cov-2 lockdown:

{{< 
  figure src="eon-search-1.png" 
  title="Search suggestions" 
  alt="Essential Or Not website item search page showing search suggestions when a user types cupcake in the search box" 
>}}

{{< 
  figure src="eon-search-2.png" 
  title="Search result showing whether an item is essential or not" 
  alt="Essential Or Not website item search result for American Muffins showing that yes, it is an essential item" 
>}}

## Terraform
Learning sources:
- Official [HashiCorp Terraform Getting Started course](https://learn.hashicorp.com/terraform?track=getting-started#getting-started)

Thoughts:
- I like it. It can be hard to justify using Terraform if the organisation is only using one cloud provider, but that's quite a rare case.
- Azure has ARM templates that work very well, but do not have the state awareness that Terraform maintains.
- The definition language is easy to read, similar to ARM templates but not as nested.
- I can see Terraform relies a lot on 'magic' by expecting certain things such as files that must be named a certain way. It can be confusing for a newcomer to pick up all the built-in assumptions that drive a lot of its smarts.
- I did not try any examples that work with on-prem servers, besides provisioning and starting a Docker container.
- State awareness has a downside - you need to maintain it in a central location if you're working in a team. HashiCorp then tries to upsell you into Terraform Cloud to be the centralized remote state store.

## React state management
Learning sources:
- CodeMash Conference 2020 talk: [React State: Redux & Context & Hooks, Oh My: CodeMash](https://app.pluralsight.com/library/courses/codemash-session-60/table-of-contents)

Thoughts:
- What annoys me about React is that good state management was not built in to the initial releases, which led to an explosion of different frameworks like Redux, MobX, and StateX over the years. React in itself is simple to learn until you start throwing state at it. That is usually all the time.
- The 40 minute talk by Michael Moran is a useful primer to the most common state management approaches in React but doesn't go into the nitty gritty. This was perfect for me.
- The summary is:
  - He categorizes state into short-term (user input), medium-term (identity, shopping cart), and long-term (customer data)
  - You can mix and match approaches for different types of state
  - He would use React Hooks if he was starting from scratch today.
  - A useful library of custom hooks is the [react-use project](https://github.com/streamich/react-use)

## PWA
Learning sources: 
- [Building Performant Progressive Web Apps from Scratch](https://app.pluralsight.com/library/courses/building-performant-progressive-web-apps-scratch/table-of-contents)

Takeaways:
- To paraphrase the instructor, all web apps can be made into Portable Web Apps, but it doesn't mean that you should. Most apps really need to be rethought in "native" app terms.
- Browser support is growing, however it is still very hidden on iOS. I think it scares Apple that there can be mobile apps that circumvent their tight grip on the App Store.
- The basic implementation steps are pretty straightforward, and the Service Worker approach to support offline mode is probably a good thing to consider in any mobile-optimised app, even without adding to home screen.
- PWAs can be considered a developer specialty because there is decent technical depth required to make production-quality PWAs, especially when it comes to supporting offline workloads.

## SRE
Learning sources:
- O'Reilly Webinar [Site Reliability Engineering Fundamentals](https://www.oreilly.com/webcasts/lot/2248020-site-reliability-engineering-fundamentals.html)

Takeaways:
- The speakers define Site Reliability Engineering as: SRE is thinking of reliability from a business perspective and engineering towards it. Key term "business perspective" is making sure the reliability solutions you implement help fulfill a real business requirement.
- The analogy they gave was for an airline aiming to be on-time at all times - it will probably lead to over-resourcing and be really expensive, but is it really what the customer expects?
- All systems will fail: What is your acceptable level of failure, and what will you set up to handle it?
- Stripe had "game days" - pick a day to deliberately fail systems to observe impact, practice incident response, and update the run book.
- How reliable should I be? What services touch customers?
- Choose the right SLIs (Service Level Indicators) for the satisfaction of your user. It should be specific, measurable, and actionable.
- SLOs (Service Level Objectives) are your targets for your SLIs, e.g. 98.50% uptime. Consult multiple perspectives to calculate this number, and acknowledge what is realistic, e.g. user internet connections out of your control.
- SLAs (Service Level Agreements) usually involve financial consequences and should not be your SLOs!
- Toil: repetitive, predictable, constant stream of tasks related to maintaining a service. (Google)
- We want to automate toil away, but realize that automation is a spectrum. A simple checklist is also a form of automation and is a step towards fully scripted solutions.
- "The root cause of falling is gravity" perfectly describes why we want to move beyond Root Cause Analysis to blameless Post Incident Reviews.

## GoLang
Learning sources:
- Pluralsight: [Go: Getting Started](https://app.pluralsight.com/library/courses/getting-started-with-go/table-of-contents)

Thoughts:
- I can see why some people like the simplicity of Go. But as the instructor says, simple doesn't mean easy. It doesn't provide a lot of 'magic' that heavier frameworks like Java and .Net provide out of the box. This means it's easier to read a Go program and understand the execution flow. I remember my initial confusion with ASP .Net MVC's routing system, which is largely by convention. Go, however, requires you to implement that logic flow yourself.
- I like that Go discourages the use of panics, which are analogous to exceptions. It instead encourages returning error types. This, again, improves the predictability of the execution.
- THe main thing that feels unnatural to me is how to implement some object-oriented concepts like struct methods and constructors, mainly because they are not actually classes.
- The Pluralsight course was a beginner-level course so it did not go into concurrency, which I keep hearing is one of the main highlights of the language.

## Feature Management
Learning sources:
- Pete Hodgson: [Feature Toggles (aka Feature Flags)](https://www.martinfowler.com/articles/feature-toggles.html)
  - Referenced in that article is a [good DevOps cautionary tale](https://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/)
- Pluralsight: [Feature Toggles module of this wider course](https://app.pluralsight.com/library/courses/azure-devops-feature-toggles-package-management-versioning/table-of-contents)
- [Effective Feature Management e-book](https://launchdarkly.com/effective-feature-management-ebook/) by O'Reilly and LaunchDarkly
- THAT Conference '19: [Feature Flags for Better DevOps](https://app.pluralsight.com/library/courses/that-conference-2019-session-28/table-of-contents)

Takeaways:
- There are many categories of toggles which affect how they should be stored and managed. The first blog post in my learning sources is a good foundational reading.
- Feature Flags as a Service (FFaaS) is a thing and is [quite pricey](https://launchdarkly.com/pricing/) but there is also a [free tier for Azure App Configuration](https://azure.microsoft.com/en-us/pricing/details/app-configuration/) which is fairly new at the time of writing. However LaunchDarkly has a lot of features and integrations with different platforms such as Azure DevOps.
- Feature flags decouple deployment from release, which enables canary releases, dark launches, and A/B testing.
- Architecture patterns need to be considered to avoid tightly coupling your execution code with feature management code.
- "Savvy teams view their Feature Toggles as inventory which comes with a carrying cost, and work to keep that inventory as low as possible."

----

And that's a wrap, folks! I enjoyed doing the 12 Days of Learning because it's like curating and attending my own conference from the comfort of my own home. I definitely think this is a worthwhile investment for my personal growth - I will continue to organise something like this in the future but for a shorter timeframe, such as a "Long weekend of learning" instead of 12 days.