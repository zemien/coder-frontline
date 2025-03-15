---
title: Windows 10 IoT on Raspberry Pi 3 – A Toe Dip
date: 2016-05-15T11:04:32+12:00
featuredImg: /wp-content/uploads/sites/2/2016/05/urban-bicycles-bikes.jpg
categories:
  - Programming
---
Being a software engineering graduate, I’ve always looked at hardware as the other side of the river from me. It sounds a bit surprising considering my first job was at Agilent (now [Keysight](http://jobs.keysight.com/country-malaysia/#section-overview)), a top electronics measurement company, but I’ve always treated the hardware component of my work with a little apprehension. However, the Internet of Things (IoT) is becoming very prolific and it’s no longer something I can easily ignore. I’ve thus decided to start dipping my toes in that cold, cold river, beginning with installing Windows 10 IoT on a Raspberry Pi 3. I will share my experiences as I learn more about it.

<!--more-->

I consider Windows 10 IoT as Microsoft’s acknowledgement that IoT is poised to start a revolution. They missed the smartphone train, and are barely holding on to their niche in the tablet space (Surface Pro), but they’re definitely taking big steps forward to make sure the Windows brand name lives on in small and headless (zero interface) devices. Thus, Windows 10 IoT is their entry into defining a Universal Windows Platform (UWP) for popular IoT platforms like the Raspberry Pi. Briefly speaking, UWP promises a consistent set of Windows APIs to target, regardless of the device. The idea being that code that targets UWP APIs are platform-agnostic, and should [look and perform the same](https://msdn.microsoft.com/en-us/windows/uwp/layout/design-and-ui-intro) no matter where the code is run.

Hearing about Windows 10 IoT being available was a great motivation for me to get started. Sure, it’s gotten some [bad reviews](http://hackaday.com/2015/08/13/raspberry-pi-and-windows-10-iot-core-a-huge-letdown/) (takeaway: run!) but that’s inevitable because people expected the full Windows 10 experience. So did I, which meant some disappointment when I discovered there’s no Windows desktop or ability to run multiple foreground apps (slightly ironic considering the Windows moniker – what? No windows?).

However, that overlooks some key points. The main one for me is that I can use development tools (Visual Studio 2015), languages (C#), and frameworks (WPF) that I’m familiar with onto a new platform. I plan to get around to developing on Linux, like on the excellent Raspbian distro, but I see Windows 10 IoT as helping to remove a major barrier between ideas and implementation.

I highly doubt I would get started without having Windows 10 IoT because there is just so much to consider when doing IoT – different processor architecture, resource constraints, communicating with external sensors/peripherals, and finally the software. I felt that Windows 10 IoT gives me a confident footing with its software platform (as feature-lite as it still is), and so I can focus more on the other pillars of what makes up IoT. I suspect this is the same for a lot of software developers who are already struggling to learn about breadboards and resistor colour codes, which bodes well for Microsoft if they can quickly solidify the Windows 10 IoT development experience.

I will be sharing my experiences with developing on the Raspberry Pi 3 over the next few weeks, and what you should watch out for.