---
draft: true
tags:
  - networking
  - segmentation
date: 2026-03-14
---
# Introduction
I'm starting off my network segmentation journey with the IoT sinkhole. As discussed in [[Networking Overview]], my original idea was to split the wireless network into three and trunk them all through the same ethernet port to the firewall for everything to be handled there. I can definitely still do that, but my understanding isn't really to the point where I'd be doing anything more than blindly finding and following someone else's work instruction. There is nothing wrong with that manner of doing things, but I apparently enjoy making life difficult for myself and thus wanted to understand the buttons to push before pushing them.
# The New Map
###### If at first you don't succeed...reduce your expectations until you're a success - StuffMadeHere
It occurred to me that I have two ethernet ports on the device I put OpenWrt on. Even though they are *labeled* WAN and LAN on the tin, doesn't mean that's what they have to do. Especially since I ripped the stock OS off in place of a custom one. Now the new mapping of the wireless networks is as follows:
- Trusted network pretty much has carte blanche to connect to whatever (currently, this will not configured for carte blanche until I get to fully hardening) and uses one of the ethernet ports
- Guest network will use the second ethernet port, and those that connect to it will be met with a captive portal (more on what that is in another post) before they can use the internet
- This leaves no ports left for the IoT network, which is perfectly fine, since I don't want them leaving the box anyway
# IoT Split to Sinkhole
Which leads me here. Why send the traffic across the wires when I can just configure it to not even leave the box. By doing that, I can, in fact, have three networks and only two ports. This will require that I take the box that I set up as a dumb ap, and make it...well...less dumb. I configure this as a second DHCP server that only serves IoT devices, then resolve all DNS requests to a sinkhole. There is a fantastic tool, called the PiHole, which affectively does this for you, but I will not be using it. I don't have a spare rPi (the one I have will be for something else), and, more importantly, I want to learn how to do this bit myself.

You will see this a lot here. I want to learn how to do these things myself. Any tool that I use and any tool I decide to build myself will be for the express purpose of teaching myself how to do things. My suspicion is that simple toolsets, like building a sinkhole, will be built by hand, where as complex tools, like OSs, will be vetted and utilized until a time I feel I am ready to build it myself. Further, I know how I operate; I will likely build the more simple aspects of the complex toolsets until I find myself with the suite I've vetted. At which point, I will polish this for use in place of the vetted one.
# Configuring The Bits
So, with a plan in place and my new understanding of concepts, it's time to get a-building. First thing to do is to configure the box so it is no longer a dumb ap regarding IoT traffic. This means firewall rules, traffic rules, and making it a miniature, enclosed (aka single-purpose or dedicated) DHCP server. Since the OpenWrt box will still be a dumb AP regarding all other traffic, I am free to configure the box however I want because the other network devices and interfaces will be left "unmanaged," meaning "unmanaged by OpenWrt;" or more specifically "pass everything not on the IoT network to the main router to manage."
## Configuring DHCP and DNS
Remember in [[Networking Overview]] when I disabled the `odhcpd`, `firewall`, and `dnsmasq` services in `System -> Startup`? Well, leave the `odhcpd` service disabled and reenable `firewall` and `dnsmasq`. As for DHCP, go to `Network -> DHCP and DNS` and set `Addresses` to `/#/192.168.x.1`. 

Breaking this down, this instructs `dnsmasq` to resolve any DNS resolution request (the `#`) as `192.168.x.1`. This means that no matter what is asked for, make it go to `192.168.x.1`. To be clear, the `x` is actually a number. It doesn't really matter what number goes here, so long as it's between 0 and 255 (because subnetting, which I will further go into in a later post). What's important is that you remember the address you chose in this step as we will be using it later. 

As an aside, I wonder if this means we can build something at `192.168.x.1` to resolve to so it doesn't break cloud-first devices, like Arlo and Blink cameras. Yes, doing this will make these so you can't send them instructions. Cloud-first means that you don't directly connect to the device to give it instructions like "arm" or "disarm." 

Taking Blink as an example, when you open the Blink app and set your cameras to "arm," your phone is connecting to the Blink cloud infrastructure, then giving the command to this infrastructure "set any camera connected to this account to armed." The infrastructure then passes this command to the cameras in this account. And finally, the cameras connect to the infrastructure to synchronize their states, which is how they eventually get set to "armed." This also pertains to setting camera names, adding cameras to different locations, and everything else you can configure.

Back to configuration, go to the `Log` tab and check the `log queries` and `extra DHCP logging` boxes. Choose any `log facility` that isn't being used, or configure your own. Log everything. Everything. The more logs you have, the easier it is to a) debug *when* something goes wrong and b) catch intruders. Log everything.
## Firewall Configuration
Firewalls. Many find configuring a firewall tedious. If I had to configure enterprise-level firewalls, I would probably also find configuring firewalls tedious. Luckily, for consumer-level security, configuring a firewall can be pretty simple: `default deny-all inbound` and `default allow-out outbound`. With this configuration, you are already obscenely ahead of the curve with regards to securing your devices. You can also make your outbound rules more restrictive, like only allowing outbound browser traffic, time synchronization, and DNS requests, as well as any remoting you may have to do, but this can largely be ignored unless you need stronger levels of security. I will cover the levels of security (from average-user to nation-state paranoia levels) in another post.

For purposes of this, however, we can stick to the follow rules:

| Zone => Forwards | Input  | Output | Intra zone forward | Enable Logging |
| ---------------- | ------ | ------ | ------------------ | -------------- |
| iot => reject    | accept | accept | reject             | Yes            |
| trusted2iot      | accept | accept | accept             | Yes            |

"Woah, woah, woah. What is this `trusted2iot` rule?? I thought we were just configuring one network!!" I (am pretending to) hear you say! Well, hang on, don't get too feisty, now, person-that-is-definitely-saying-this. What if I told you that you can administer the IoT network without ever connecting to it? If you never connect to the network, you never compromise your device. But, by allowing traffic to flow into the sinkhole network from the outside, you can still send commands to the devices (unless they are cloud-first). "Well...yeah. Duh," is what you would likely actually say.
## Traffic Rules Tab
The final configuration to make is the actual drop-traffic rules. Drop All IoT DNS Requests. Any time a device makes a DNS request from the IoT network over ports 53, 67, or 68, drop the forward entirely. You may need to make three separate rules: one for each port.

Whoof that's a dense sentence:

- We've already covered what is a DNS request, but to briefly reiterate, human types URL address, machine asks DNS server to translate this to an IP address
- A forward is when a device requests something that lives on a different device or network. The traffic is then forwarded to that location
- A drop action is when a request is rejected silently. The other type of request rejection action is "reject," which tells the requesting device that the request was rejected (great for debugging)
- Port 53 is the standard port used by the DNS to translate human-readable domain names (like google.com) into machine-readable IP addresses (like 142.250.180.14)
- Ports 67 (go ahead, get it out of your system) and 68 are the ports used by BOOTP to assign devices IP addresses on a network. This is a little noodley, but I'd still like to explain.
### BOOTP: A Tangent
I had to do a little digging with this one to put what this does into words, so now you get to read my ramblings. This is not pertinent to making the machine go, so feel free to skip to [[#Going Live]].

As explained in Wikipedia, when a Bootstrap Protocol, abbreviated as BOOTP, client (device) is started, i.e. when it boots, it has no IP address, so it broadcasts a message containing its MAC address (which is a thing that can be thought of as a device's identification number) onto the network. This message is called a "BOOTP  request." 

As I've become more entrenched in this industry and my understanding more clear, I've noticed the internet and networks via requests and responses that are directed over routers. Devices request \<thing\> then passes \<thing\> through various routers to its destination. Then, the destination sends back the response to a request back through various routers to the source of the request. Everything is routers. Everything is requests and responses.

Anyways, the message is then picked up by the BOOTP server, which replies to the client with the following information that the client needs:

1. The client's IP address, subnet mask, and default gateway address assignments
2. The IP address and host name of the BOOTP server
3. The IP address of the server that has the boot image, which the client needs to load its OS

When the client receives this information from the BOOTP server, it configures and initializes its TCP/IP protocol stack, and then connects to the server on which the boot image is shared. The client then loads the boot image and uses this information to load and start its OS.

The Dynamic Host Configuration Protocol (DHCP) was developed as an extension of BOOTP. BOOTP is defined in the Requests for Comments (RFC) 951 and 1084.

This is specifically in reference to when clients (devices) and servers (like the DHCP server) are on the same network. There's a whole other case where all of this needs to securely take place when clients and servers are on different networks, but as that doesn't pertain to my use-case, I will not be going over that here. For more information, your favorite search engine is your friend. Or email me and we can research the answer together.
# Going Live
Alright, you configured the DHCP/DNS server, the firewall, and the traffic rules. Now what? The next steps are to create the device, the interface, and finally broadcast the wireless network to connect to, and test/validate. This is going to be more of a list than the usually explanatory sections.

1. Create the device in `Network -> Interfaces -> Devices tab`
	- Name: br_iot
	- Type: Bridge Device
	- Bridge ports: unspecified
2. Create the interface in `Network -> Interfaces`
	- Name: iot
	- Protocol: static address
	- Device: br-iot
	- IPv4: 192.168.x.1 (use the one discussed earlier)
	- IPv4 Network: 255.255.255.0
		- Or CIDR: 192.168.x.1/24
	- DHCP Server: checked
	- DHCP Options: 6,192.168.x.1
	- IPv6: Disable everything here
		- IPv6 is incredibly insecure at this time
		- Disable IPv6 everywhere by default always
3. Create and broadcast the wireless network
	- Frequency: Legacy mode Channel 6
	- Mode: Access Point
	- Encryption: WPA2/3
	- 802.11w: optional
	- Enter an SSID (the thing you see under the "Connect to WiFi" menu on the device)
	- Enter a key (this is the password for the SSID)
4. Verify the following
	- Firewall rules
	- Traffic rules
	- DHCP address
	- Client isolation is disabled, you'll need this for devices that require some kind of hub to disperse instructions
	- `dnsmasq` and `firewall` are enabled in `System -> Startup`
		- This tripped me up for a while; I had everything configured properly, but nothing was working. Turns out, I never reenabled these.
5. Test and validate everything is working...erm...not working...but like, in the way that we want things to not be working. Because if it's not working, then it's working, but if it's broken, then it's not working, but in a bad way. Anyways:
	- Connect your phone or another device to the IoT network and go to the network settings to find its IP address. It should start with `192.168.x`
		- Visit [SpeedTest by Ookla](https://speedtest.net/). This should fail
	- When connecting cloud-first devices, you will have to temporarily change the Network drop-down menu under the Interface Configuration to a network that doesn't sinkhole connections
		- After they are successfully on the network, you can switch the Network back to the IoT network
		- I have another network I've configured with strict outbound rules that allow initial setup
# Conclusion
I wanted to close this off with a little vulnerability (pun intended). I've always been of the opinion that it's important when going through your process with someone else (like in the case of me publicly releasing my journal) that including the struggles you went through is as important as your triumphs. I am not and will never be better than anyone else, nor do I strive to be.

This took me a good several months of tinkering, thinking, and testing on repeat to do this. Many people would find this to be child's play regarding setting stuff up. And, reductively, I fell into a trap of comparing myself to these self-perceived "others:" "Such-and-such type-of-person wouldn't have had this much trouble doing this."

To this and to those who fall into this trap, I will say that it is important to remind yourself that you are not them. You will have your own struggles through any journey; and further, you are going to find things incredibly easy that these "others" will struggle with.

One of the more satisfying things I've noticed, and I touched on this a bit in [[Networking Overview]] at the end when discussing keeping a journal, is in reviewing my journal. I get to see the progress I've made throughout my journey. I see the decisions I made. I see the mistakes I made. And, most importantly, I can now see the solutions in my mind to the challenges I faced. This culminates to an incredibly empowering state-of-mind.