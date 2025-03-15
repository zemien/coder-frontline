---
title: My static CMS site architecture
date: 2019-02-06T15:00:00+13:00
featuredImg: /my-static-cms-site-architecture/jam-glass-breakfast.jpg
categories:
  - Programming
---
I mentioned in my last post that I started 2019 by [moving this blog off WordPress](/new-year-new-beginnings/) and into a Hugo static CMS set up. I will go into more detail about the particular approach I took to migrate the site over the next several posts. I will start the series off by quickly showing you the main architectural differences between a traditional CMS like WordPress and a modern static CMS like Hugo or Jekkyl.

{{< 
  figure src="wordpress.svg" 
  title="A typical WordPress site architecture" 
  alt="Architecture diagram showing both blogger and visitor connecting to a LAMP stack server running WordPress" 
>}}

The image above shows a typical WordPress server on the web. The blogger connects to the WordPress administrator console running on the web server to manage content, while site visitors connect to the same server, either directly or via a CDN (Content Delivery Network). The CMS builds the page on-the-fly when a request for a particular page is received.

It is most likely an open-source LAMP stack server, meaning it is composed of Linux (Operating System), Apache (Web Server Software), MySQL (Database), and PHP (Programming language). You can run WordPress on Windows and SQL Server, but a LAMP server starts at $0 licensing costs. This is a major factor for the LAMP stack's continuing popularity.

A static CMS pushes the page building process up the production line, to when the blogger is writing content. The image below shows a basic static CMS architecture using Hugo. The blogger will create the content on their local environment such as a PC or Mac, and then run the Hugo command-line tool to generate the static .html files. These files are then pushed onto any web server, since all web servers are capable of serving up normal .html, .css, and .js files.

{{< 
  figure src="hugo-basic.svg" 
  title="A basic Hugo site architecture" 
  alt="Architecture diagram showing a simple Hugo site setup where the blogger publishes pre-built .html files to web server" 
>}}

This is certainly adequate and most closely resembles the traditional low-cost WordPress server set up. However, the rise of cloud computing and serverless technologies is creating new possibilities for how we host content-rich websites that are scalable and yet cost very little.

{{< 
  figure src="hugo-advanced.svg" 
  title="An advanced Hugo site architecture" 
  alt="Architecture diagram showing an advanced Hugo site setup" 
>}}

The architecture diagram above shows how I am currently building this blog. I, the blogger, write my content on my Mac and push changes up to a Git repository hosted on [Azure DevOps](https://dev.azure.com) (previously known as Visual Studio Team Services or Visual Studio Online). GitHub is a more popular option, but I am familiar with Azure DevOps and like its all-in-one build and release pipelines.

I use the [complimentary build and release minutes](https://docs.microsoft.com/en-us/azure/devops/organizations/billing/buy-more-build-vs?view=azure-devops) included in the free tier. The amount changes from time to time but is currently 30 hours a month, which is more than adequate for my needs. My site is built in the cloud and then released into an Azure Storage Blob account, with [static website hosting](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) turned on.

Lastly, I place a CDN service in front of the static website to get the benefits of a cheap, hihgly-scalable, and highly-responsive content delivery system. I have specifically chosen the Premium Verizon CDN offering, and I will explain the reasons why in a future post. A site visitor will be served my content from the closest edge node, so I do not have to worry about distribution or load balancing myself.

The diagram also has an optional App Service API that I have not implemented because I have no need for it. This is another common component of what's known as the [JAMstack](https://jamstack.org/) - an alternative to the LAMP stack. JAM in this case stands for JavaScript (Programming language), API (Backend), and Markup (Content definition language). There are a lot of well-written information about what makes a good JAMstack on that site. My blog is more a J-Mstack as it stands, but I am considering adding an API layer for site searches.

Next time: I write about how each component is set up.