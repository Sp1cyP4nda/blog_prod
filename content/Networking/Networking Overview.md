---
draft: false
tags:
  - networking
date: 2026-02-11
---
# Introduction
Welp, this took far longer to figure out than I originally thought. [[Breaking Things Down]] was really tested with this. I wanted to learn how to set up my network. I got that up and running without much of a fuss. Dope! Then, I wanted to learn how to segment it...And now my whole network's down. Great =\ What did I do wrong? How do I fix it? What was my goal again?
## The Beginning
###### The only way to learn is by playing. The only way to win is by learning. And the only way to begin is by beginning - Sam Reich, Game Changer intros
While I was bartending, I would ask regulars if they had any tech equipment that was taking up space in their house that they didn't want to pay to have thrown out. More people than I thought were far more happy to oblige. They got to get rid of old crap and I got to build my tool chest of tech while staying in budget. Two of these devices are relevant for this story: A ThinkCentre mini PC and a single node of a Google Mesh Wifi device.

Originally, I had wanted to install a WiFi card in the ThinkCentre and use that as a wireless access point. The most difficult parts of this is a) sourcing antenna with the correct connector size (apparently this is about as standardized as woman's clothing sizes) and b) once you do find the right size, connecting it to the chip without destroying connectors is an absolute nightmare.

Flash forward a few years, after wading through the sea of information describing all sorts of tech and tech-related setups, devices, etc. it had occurred to me that I had most of what I needed to create my own local network and stop paying for my ISP's router rental ($15 every month!!). I let go of the idea of using the ThinkCentre as the wireless access point, opting instead to use the Google Mesh device. Yes, this took a couple years to dawn on me. Shush.
## The Planning
Okay, what do I need?
- A modem
	- I decided to just start with the ISP's router in bridged mode
	- I eventually found a dedicated modem, which was surprisingly difficult
- Firewall
- Router
- Switch
	- I was told to get a managed switch. At the time, I didn't really know what that meant, but smarter people than me said to do it, so I listened. (More on this later)
- Wireless Access Point (WAP, WAP, WAP, WAP)
Neat, this is the bare minimum. Can these be segmented into their own dedicated machine? Yes; granularly. Is it necessary for all of them to be? No. Which setup is the best for me? After scouring the internet, I decided to marry the firewall and router, building a switch or a modem required more skill and budget than I had at the time, and the Google Device is already technically a WAP, so I didn't need one of them. I just needed to figure out how to fit it into my network that way.

This means, for purchases I just need to source a managed switch. Oh boy, was this more difficult than it needed to be. I found out there's all sorts of marketing terms that are used for this. Smart, Intelligent, Easy Smart, Smart Easy, Enterprise-Managed, Fully Managed, holy crap dude, the only word I didn't see was "managed." I was buried in Whytho memes. So, what does that mean? What do I need? All I need is a switch that can handle VLANs. I don't care about anything else. I found one labeled Smart Switch with "VLAN capable" written on the box somewhere, crossed my fingers, and brought it home.
### Mapping It Out
Now, with all the basic physical equipment, I could get started. After researching [FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software) routers, firewalls, and access points, I decided [pfSense](https://www.pfsense.org/) and [OpenWrt](https://openwrt.org/start) were my best options. pfSense on the ThinkCentre to act as my gateway, firewall, and router, and OpenWrt on the Google Device as what I would later learn is known as a [Dumb AP](https://openwrt.org/docs/guide-user/network/wifi/wifiextenders/bridgedap).

Here's the first network diagram I drew:

ISP -> Modem -> Firewall/Router (ThinkCentre) -> Switch
From the Switch:
- Syslog server
- NAS
- Wired Devices
- WAP (Google Device) VLANs
	- Trusted
	- Guest
	- IoT
It may not be the prettiest, and calling it a diagram is definitely a stretch, but this is pretty much how bad it looks in my notebook, and it worked for me. I also made an A-or-B diagram that I knew at the time was out of scope for the initial setup that expanded on "wireless devices" and included an unmanaged switch to increase my port count.
For security, I wrote down:
- Obfuscate with DNS Sinkholes
- Non-broadcasted SSIDs
- Close inbound ports
- Tighten outbound ports
Necessary PoCs to get this working:
- Google Device -> Flash w/ OpenWrt (Need USB-C PD Hub)
- ThinkCentre -> pfSense (Need compatible USB NIC)
- RPi -> Set up syslog server
This means I needed to go back to the store...I needed a USB NIC, a USB-C PD Hub, and a bunch of 2 inch ethernet cables. All easy to find.
## Connecting and Configuring Devices
Okay, so now for the first course. Let's just get this network up and running. Connect the ISP device into the switch, then the pfSense and OpenWrt, and finally my main rig. Configuration, configuration, configuration aaandd....nothing works. After much fuss, here is a list of ~~issues~~ behaviors, in no particular order, that I ran into that others will too.
1. Realizing there's a better way to arrange my devices and which device ran which OS
2. Double NATting
3. Locking myself out of All of the devices
4. Having two DHCP servers on the same subnet
5. Locking myself out of all of the devices again, but in a different way
6. Bringing down the entire network by locking it down incorrectly
7. Locking myself out of all of the devices 3: electric boogaloo
### 1. Rearranging Devices
Never tie yourself to a map. When I finished mapping out my network, I spent so long doing so, that changing the map felt like that time spent was wasted. Time spent learning is never time wasted. Once I allowed myself to change the map, everything aligned soo much better.

I let go of trying (and failing) to install a wireless NIC into the ThinkCentre and reassigned it as the firewall/router, which left the mesh router as my wireless access point. I also found out that pfSense is too resource heavy for the mesh router, anyway, so that definitely helped with that decision.
### 2. Lockouts
Big oof, I'll tell ya what; it's obnoxiously easy to lock yourself out. Challenge yourself to not just hit the factory reset button when this happens. This will take longer, and is more difficult, but boy, do you learn a lot. Undo what you did, don't reset. This may not always be feasible, as an example, some machines may not be on-prem, but for machines you do have access to, find out what was messed up and learn how to fix it.
### 3. Multiple DHCP Servers
#### Journal Entry
"It looks like pfSense may reset LAN IP addresses. If it does, configure Services -> DHCP Server -> LAN then Interfaces -> LAN. If this happens, unplug the OpenWrt device." - 09Aug25 - Everything is Connected

Before I knew what DHCP was, or at least understood it to the point where I could fix it, I was experiencing the issue where, after connecting all of my devices, the IP addresses kept resetting after a period of time.
#### What is DHCP?
The [Dynamic Host Configuration Protocol](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol), or DHCP, is defined by Wikipedia (I tried to find the NIST definition, but couldn't) as a network management protocol used on Internet Protocol networks for automatically assigning IP addresses and other communication parameters to devices connected to a network using a client-server architecture. In practice, this means that networks need a way to assign pathways for devices to communicate with one another.
#### Small Home Networks
This can be achieved by many different methods, depending entirely on use-case and network size, among a plethora of other variables. The one I will be focusing on is the use-case that matched my needs: the small home network. 

For small home networks, you would set up a single DHCP server, I am (currently) using my pfSense for this, and that one server is configured with a admin-defined pool of IP addresses from which it can assign to however many devices connect to the network. My issue is that I...had two: the pfSense device, and the OpenWrt device (which is a fully capable OS that can be used by itself as a full suite of routing and access points).
#### Easy Fix
The way to disable OpenWrt acting as a DHCP server is to configure it to be what is colloquially known as a Dumb AP. After much fuss and research, it boils down to basically disabling three services in System -> Startup:

- odhcpd
- firewall
- dnsmasq

Then, when creating interfaces under Network -> Interfaces, make sure to set the protocols to Unmanaged. And, in the Firewall Settings tab of the interface, set to Unspecified. Everything else, leave default.

> [!Note]
> It's been a minute since I did this and I didn't write it in my journal, so there may be a few other nuance bits that I didn't put in this part. Make sure you do your homework before you bring down your network again!

### 4. Double NATting
#### A Bit of ~~A Tangent~~ History
There are plenty of places to find know what NATting is, but I'm going to explain it from my understanding. This will be partly a history lesson. Understanding what decisions were made during the creation of things, why those decisions were made, and the impact of those decisions are, in my opinion, paramount to not only learning about a topic, but also to solidify that topic in the mind.

Starting off, during the initial creation of the "experiment" that we now know as the internet, IP addresses (annotated by four numbers separated by a period e.g. 192.168.0.69, nice) were handed out basically willy-nilly because the internet was seen more as "nerd stuff" that was really only useful in enterprise environments and thus was believed would never actually catch on in the more public manner that we see today. This meant that each device connected to this internet could have it's own of about 4.3 billion available addresses assigned to it: workstations, printers, routers, etc. Cell phones — or mobile workstations, as the router sees them — were not around yet in the capacity that required them to have an IP address. Yes, I used an em-dash, no I didn't use AI. Get over it. I will be using them throughout this blog. Although, I did learn how to properly use em-dashes because of AI.

In an effort to keep things organized, corporations that had many devices that required IP address (this would be considered an enterprise-level network), also referred to as nodes, were assigned entire families of addresses. A family of addresses would be any set of addresses that begin with the same numbers; up to the first three, e.g. 10.x.x.x, 172.16.x.x, and 192.168.0.x. It is important to note that 10.x.x.x has more addresses that can be assigned to nodes than 192.168.0.x for reasons out of the scope of this article to explain (I will eventually make a subnet article, stay tuned!), but is easy to find. Because of this, larger companies were granted the addresses that contained more addresses, such as Comcast being legally assigned every address included in 10.x.x.x.

Regarding companies, this is perfectly fine, with well more than plenty of addresses to hand out to their devices. No correction needed...well...now there's personal computers (PCs). Oh, now every household has a PC...erm...two PCs...now laptops and cell phones. More and more devices are starting to connect to the World Wide Web (yes, this is what "www" stands for). Each one needs an IP address so that a device can accurately send and receive data. The issue is that all of the IP addresses were already legally given to different companies and can no longer be used to identify devices on a public network, in a process referred to as [IPv4 address exhaustion](https://en.wikipedia.org/wiki/IPv4_address_exhaustion).
#### ~~Who Cares?~~ Enter NAT
Now, in the mid '90's (yes, the 1990's, you youngins. I'm not old, shush!), a few solutions to this problem were proposed: CIDR blocks, IPv6, and the one relevant to this post, NAT. The first two, I'll cover in a future post. [Network Address Translation,](https://csrc.nist.gov/glossary/term/network_address_translation)  as defined by NIST, is a function by which internet protocol addresses within a packet are replaced with different internet protocol addresses. There are a few definitions in the linked citation, but this is the one I'll be going with. And hoahboy that's a lot of jargon packed into a single sentence. Let's break this down:

What is a "[packet](https://csrc.nist.gov/glossary/term/packet)?" Think of a packet like a letter (or more relevant to modern-times, a utility bill, but that's less fun). You *can* go deliver this letter yourself to the friend you wrote it for. But, if you live across the country (or like me, you are too lazy to go to the next town over), you can, instead, put this letter into an envelope, write down where your friend lives, and hand it to your mail carrier for them to do it. The mail carrier then looks at your friend's *address* to find where to deliver the letter.

Tying this in. The packet is what you want to send, the mail carrier is the way it gets there, and the address is, well, the internet protocol address (IP address). We can break this down even further by saying your mailbox is like your router (that ugly box that your internet service provider, ISP, gave you when you first moved in to your current pad that's now just collecting dust because you don't know what to do with it because it doesn't fit the decoration in anyone's house). 

For completeness, here is the full lifecycle of a packet (letter): Some packet of data is created, this could be you using your browser (Chrome, Edge, Firefox, Safari) to access your favorite news or social media website, or simply clicking on a link; this packet is wrapped in metadata that describes where it's going and is then handed to your router. The router then hands it to your ISP, who then uses magic that is, again, out of the scope of this post to deliver this packet to the router of its intended recipient, but can be thought of as sending your letter through the mail carrier facility. Finally, the recipient's router then looks for the intended machine of the packet and delivers it. There is then a return journey that's kind of like your friend writing back to you where the entire process is reversed.
#### What is Double NATting
In our metaphor, we have written a letter and sent it to our friend. Our friend then read the letter and wrote and sent one in return. What if we wanted to send our friend a letter at work? Maybe it's a special day for them or they've been having a rough week. If they work at a small company, great! The process is exactly the same. Write letter, mailbox, carrier, facility, delivery carrier, mailbox, friend. But, if the friend works at a bigger corporation, it gets a bit more complex. Let's say this corporation sits on a campus and has multiple buildings with multiple floors and departments (we can make this more extreme by introducing multiple campuses, across multiple states or countries, but we'll keep it bound to just this). Admittedly, this metaphor would probably work better as a university campus, but I've already written it and ~~am too lazy to change it~~ want to challenge myself to stay in this metaphor.

Nothing on your end changes, you write your letter and send it off. It's when the letter gets to the main receiving mailbox (externally facing router) that the route to your friend changes. The campus likely has some kind of internal system that allows mail to be routed to the appropriate locations (machines), thus the letter goes through a similar process as finding the main mailbox, but this time, depending on the complexity of the campus' (mail) network, it's being delivered to a specific building, to a specific floor, then to a specific department, before making it's way to the location that your friend has access to for picking up their mail.

To translate, the packet is received by the main router, which figures out where to send it. If the destination of the packet resides deeper within the architecture of the environment, the router translates that address to an internal address corresponding to that area and hands it to the next router. This is then repeated until the correct subnet is found. The router of that subnet then sends the packet to its intended recipient.
#### The Issue With My Double NAT
Finally, the crux of my issue. Yes, I'm aware I'm long winded. Thank you for being patient.

Mine is the equivalent to the "small company" mentioned in the first part of the first paragraph of the last section. However, when I first set up my network, I told pfSense to be my NAT, but never instructed my ISP's router to stop being my NAT. This means that both routers thought they were the only NAT router on the network.

This boils down to a few things. My PC (my phone, a different device, doesn't matter which) sent a packet to pfSense which sent it to the mail carrier facility by way of the ISP's router which tried to send the packet via a different route. This discrepancy in instruction caused my packets to be dropped entirely, like two mail carriers being hired to deliver the same letter.
#### The Fix
I wrote it perfectly in my journal, so I'll just iterate it here, "I found out the issue was double NATting. I put my modem into bridge mode and rebooted. When I saw that public IP on my firewall that I built, I must admit I was startled at first. Fear and adrenaline. The only thing that stood between the safety and sanctity of my home was no longer the professionals at the ISP and their firewall. Now, it was up to me. I was the 'professional' that protected my network. It's time to lock everything down. Into and out of." -15Aug25 - Realization
## Closing Thoughts
It is just now occurring to me while reviewing my journal to write this post that I actually had three DHCP servers: the pfSense device, the OpenWrt device, and my ISP's router. Configuring the OpenWrt device to be a dumb AP brought that down to two, and by setting my ISP's router to bridge mode for the purpose of getting rid of the double NAT, I had unknowingly also fixed the multiple DHCP server issue. That's pretty cool. Not intended, but definitely neat.

I've always been of the opinion that keeping yourself measurable during a journey is how you keep yourself from getting lost. Take the extra time to keep notes that remind you of what you've learned on the way. Look back over the progress that you've made during the time of your studies. I may have always been of the opinion that this is an incredibly valuable skill, but wasn't until writing this post that I understood just how powerful this practice really is. 

Reviewing even this small section has reminded me not only of how little I knew of these topics, but also how much I've learned. More importantly, it is a measurable show of how much I'm capable of learning and of doing. I have a sizable imposter syndrome. Even that can't argue with measurable results.