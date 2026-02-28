---
draft: true
tags:
  - networking
  - segmentation
---
# Introduction
I'm starting off my segmentation journey with the IoT sinkhole. As discussed in [[Networking Overview]], my original idea was to split the wireless network into three and trunk them all through the same ethernet port to the firewall for everything to be handled there. I can definitely still do that, but my understanding isn't really to the point where I'd be doing anything more than blindly finding and following someone else's work instruction. There is nothing wrong with that manner of doing things, but I apparently enjoy making life difficult for myself and thus wanted to understand the buttons to push before pushing them.
# The New Map
###### If at first you don't succeed...reduce your expectations until you're a success - StuffMadeHere
It occurred to me that I have two ethernet ports on the device I put OpenWrt on. Even though they are *labeled* WAN and LAN on the tin, doesn't mean that's what they have to do. Especially since I ripped the stock OS off in place of a custom one. Now the new mapping of the wireless networks is as follows:
- Trusted network pretty much has carte blanche to connect to whatever (this is hardened to the fullest extent at this time) and uses one of the ethernet ports
- Guest network will use the second ethernet port, and those that connect to it will be met with a captive portal before they can use the internet
- This leaves no ports left for the IoT network, which is perfectly fine, since I don't want them leaving the network anyway
# IoT Split to Sinkhole
Which leads me here. Why send the traffic across the wires when I can just configure it to not even leave the box. By doing that, I can, in fact, have three networks and only two ports. This will require I take the box set up as a dumb ap, and make it...well...less dumb. I configure this as a second DHCP server that only serves IoT devices, then DNS to a sinkhole. There is a fantastic tool, called the PiHole, which affectively does this for you, but I will not be using it. I don't have a spare rPi (the one I have will be for something else), and, more importantly, I want to learn how to do this bit myself.

You will see this a lot here. I want to learn how to do these things myself. Any tool that I use and any tool I decide to build myself will be for the express purpose of teaching myself how to do things. My suspicion is that simple toolsets, like building a sinkhole, will be built by hand, where as complex tools, like OSs, will be vetted and utilized until a time I feel I am ready to build it myself. Knowing how I operate, I will likely build the more simple aspects of the complex toolsets until I find myself with the toolset suite I've vetted. At which point, I will polish the complex toolset for use in place of the vetted one.
# Configuration
So, with a plan in place and my new understanding of concepts, it's time to get a-building. First thing to do is to configure the box so it is no longer a dumb ap regarding IoT traffic. This means firewall rules, routing rules, and making it a miniature, enclosed (aka single-purpose or dedicated) DHCP server.

| Firewall | Routing | DHCP     |
| -------- | ------- | -------- |
|          |         | Reenable |


# Some Terminology
So, to start off with, there are a few terms that will be useful to know before we move forward. Well, I say, "before," but I get bored reading the glossary first. I'll still put it here so you can decide whether you want to read it first or come back.
## IoT
## Sinkhole
## DNS
## rPi
## PiHole
## DHCP, though we already went over this
