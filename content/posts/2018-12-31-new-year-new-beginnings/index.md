---
title: New Year, New Beginnings
date: 2018-12-31T09:00:00+13:00
featuredImg: /new-year-new-beginnings/city-night-explosion-firework.jpg
categories:
  - General
---
A new year presents new opportunities to transform ourselves. New changes are afoot for this blog. This is the first new post since I migrated this blog from my old WordPress installation to [Hugo](https://gohugo.io/) static CMS. 

## Minimum Viable Product
I still need to refine many aspects of the blog so what you see as of Dec 31st, 2018 will surely change. I am taking an Agile approach by launching with my MVP (Minimum Viable Product) to gauge interest and refine my product as I go along. Plus, there's always more motivation to work on a live product rather than one that is stuck in draft forever.

You will most likely read this once I've improved on this version, so here are some screenshots of my old WordPress blog and the new blog (as of post date):

{{< 
  figure src="wp-homepage-menu.png" 
  title="WordPress-based homepage and menu" 
  alt="WordPress-based homepage and menu" 
>}}
{{< 
  figure src="hugo-homepage.png" 
  title="New Hugo-based homepage" 
  alt="New Hugo-based homepage" 
>}}
{{< 
  figure src="hugo-post.png" 
  title="New Hugo-based post" 
  alt="New Hugo-based post" 
>}}

## Reasons for change
1. I wanted to massively reduce the risk of being hacked since I am not diligent with updating WordPress with the latest patches.
2. I wanted to cut my hosting costs down to the minimum because this is only a personal blog.
3. I wanted to learn new technology.

## Current technology stack
- CMS: Hugo
- Editor: Visual Studio Code with [Azure Storage extension](https://github.com/Microsoft/vscode-azurestorage) to simplify static website publishing
- Hosting: Azure Storage with [Static Website hosting](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) behind Azure Premium CDN for scalability and managed TLS certificates
- Code repository: Azure DevOps with automated build on master branch updates. I will work on automated releases in the near future.

## New journey
I will write more about my migration journey in future posts, especially as I attempt to bundle up the Azure resource configuration into an ARM template that anyone can reuse.

You can also refer to Chris' blog post on [Go Make Things](https://gomakethings.com/migrating-from-wordpress-to-hugo/) on how he migrated his site. Our approaches differ in several key areas, so his experience may be more relevant to your WordPress site setup.