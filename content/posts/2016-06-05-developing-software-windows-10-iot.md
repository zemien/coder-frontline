---
title: Developing Software on Windows 10 IoT
date: 2016-06-05T09:35:42+12:00
featuredImg: /wp-content/uploads/sites/2/2016/06/apple-pie.jpg
categories:
  - Programming
---
It’s taken me awhile to get into the Internet of Things due to my lack of knowledge around interfacing with the hardware. Luckily, developing software on Windows 10 IoT is almost a breeze for .Net developers thanks to Microsoft’s great tooling support. There are a lot of great tutorials online so I will focus instead on highlighting unique aspects about programming applications for the Raspberry Pi 3.

<!--more-->

## Universal Windows Platform

Commonly shortened to UWP, this is Microsoft’s latest effort to create a common set of system APIs across different Windows devices and architecture. I still remember the circus when Windows 8 introduced Win RT. Time will tell if UWP lasts the distance or gets superseded by the next major Windows release. For the moment though, you will need to create a Universal Windows Platform application in order to run it on Windows 10 IoT.

UWP provides a rich set of APIs, but it is by no means a complete replacement for the full Windows desktop API. The standard UWP template reveals WPF window classes with standard XAML support – or so it seems. Upon further inspection, UWP XAML is missing some features such as Triggers, which many developers like to use. There are workarounds in both XAML and code-behind, but it is something to be aware of. Namespaces may also differ from what desktop developers are used to, and some UWP-specific NuGet packages might be required.

## Visual Studio 2015 debugging experience

Visual Studio 2015 provides a first-rate debugging and deployment experience, in all editions. I created a project in both the Ultimate edition I use at work, and the free Community edition on a home laptop. There are no differences between them in terms of working with the Pi. I was able to pick the ARM platform and run it on a Remote Device. It prompts you to pick an auto-detected device, or manually enter an IP address. And it just works – no fiddling with app certificates or manually selecting executables to send over the wire.

## Raspberry Pi remote connection

One current issue with the auto-detection is that the listening server on the RasPi sometimes times out. It does not show up in the [IoT Core Dashboard](/configuring-windows-10-iot-raspberry-pi-3/) app and you can’t access the web configuration page either. The running app usually remains operational, but it will need to be soft reset to be able to deploy and debug again.

## Communicating with hardware

The next step is to communicate with external hardware. This is an area I have yet to investigate in any depth but online code snippets suggest it is quite trivial to send signals to the RasPi’s GPIO pins. I also bought a [Sense HAT](https://www.raspberrypi.org/products/sense-hat/), which is a convenient add-on board that conveniently sits on top of the Pi and has an 8&#215;8 RGB LED matrix, gyroscope, accelerometer, magnetometer, temperature, barometric pressure, and humidity sensors. Even more convenient is the [open-source .Net library created by Mattias Larsson](https://www.hackster.io/laserbrain/windows-iot-sense-hat-10cac2) to conveniently poll the Sense HAT’s sensors. It comes with a great samples library to kick-start any Sense HAT project.

## Summary

While it will always have its detractors, I am personally a fan of what Microsoft is doing in the IoT space and I feel empowered to finally bring my software skills into the hardware world. I will have lots more to write about my experiences with developing software on Windows 10 IoT after I learn more about it. I would encourage you to try it too because the Internet is coming to your things. Are you ready for it?

For the next few weeks we will completely switch tacks. I received a reviewer’s copy of [Functional Reactive Programming](https://www.manning.com/books/functional-reactive-programming), authored by Kiwi technologists Stephen Blackheath and Anthony Jones, creators of the Sodium FRP library. I’m finding time to work through it and will be sharing my thoughts on the book and the pattern itself.

Photo credit: [Caitlinator](https://www.flickr.com/photos/caitlinator/2971879868/) via [Visual Hunt](https://visualhunt.com/photos/travel/) / [CC BY](http://creativecommons.org/licenses/by/2.0/)