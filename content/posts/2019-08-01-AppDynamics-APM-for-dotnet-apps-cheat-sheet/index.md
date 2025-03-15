---
title: AppDynamics APM for .Net Apps Cheat Sheet
date: 2019-08-01T17:28:00+12:00
categories:
  - Programming
---

I'm getting involved with Cisco AppDynamics APM (Application Performance Monitoring) at work and have been finding lots of little tricks and gotchas with instrumenting ASP .Net web applications on Windows servers. I have compiled a list of quick tips that may help you set yourself up for APM bliss.

# .Net Agent Installation
The .Net Agent Setup on Windows is a breeze to go through. It even installs a Windows application called AppDynamics Agent Configuration that lets you easily re-configure the agent afterwards. I recommend running this right after installation because one of the final steps it does is restart IIS, which is required for the agent to start collecting information about your ASP .Net applications.

{{< 
  figure src="dotnet-agent.png" 
  title=".Net Agent Configuration Wizard" 
  alt=".Net Agent Configuration Wizard dialog box" 
>}}

# Machine Agent Installation
The .Net Agent comes with basic server monitoring features (CPU, memory, disk usage levels). The Server Visiblity license, if you have it, will open up new insights such as detailed Processes list. This means you need to install a Machine Agent on the same server as the .Net Agent. There are a few configuration settings you need to change which you can [refer to AppDynamics documentation on .Net Compatibility Mode](https://docs.appdynamics.com/display/PRO45/.NET+Compatibility+Mode).

In a nutshell:

1. Set dotnet-compatibility-mode to true
2. Set unique-host-id to the same unique-host-id of the Application. You can find the Application's unique-host-id by viewing your AppDynamics Agents:

{{< 
  figure src="appdynamics-agents-menu.png" 
  title="AppDynamics Agents menu" 
  alt="AppDynamics Agents menu" 
>}}

{{< 
  figure src="app-agent-id.png" 
  title="AppDynamics Agents Unique Host ID" 
  alt="AppDynamics Agents Unique Host ID" 
>}}

Lastly, it is perfectly fine for the machine agent status to have the "-java-MA" suffix. This means it is running in .Net Compatibility Mode. I also noticed that the status is always INITIALIZING even though it is running normally.

{{< 
  figure src="machine-agent-summary.png" 
  title="AppDynamics Machine Agent summary window" 
  alt="AppDynamics Machine Agent summary window" 
>}}

Hope this helps!