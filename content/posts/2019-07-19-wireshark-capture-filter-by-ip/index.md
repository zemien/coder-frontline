---
title: Wireshark Capture Filter by Host IP
date: 2019-07-19T13:28:00+12:00
categories:
  - Programming
---

I keep forgetting this simple syntax and hopefully this will come in handy. In Wireshark you can specify a capture filter to only log traffic to/from a specific IP address with:
```
host {ipAddress}
```

{{< 
  figure src="wireshark.png" 
  title="Wireshark Capture Filter" 
  alt="Screenshot of Wireshark's Capture Filter input" 
>}}

I needed to use a capture filter because I was running captures on a low-spec server, and without a capture filter Wireshark eats up all the memory.

Related tip: Use nslookup to get the IP address that is resolved to. You would usually do this from the server itself as CDNs may send you to a different IP.

```
nslookup {www.domain.tld}
```