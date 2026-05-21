---
draft: false
tags:
  - security
---
# Introduction
What would you do if I told you that there existed a second version of you? This second you is exactly like you in every way. Your interests. Your hobbies. Your beliefs. A perfect, digital replica. What's more, what if I told you that anyone that had custody over this second you would be able to flawlessly predict every decision in any given situation. What's more, what if I said that anyone with custody could influence those decisions in any given situation? As a final hypothetical, what if I said that *you* are actively building this digital shadow for others to control?

Except, this isn't a hypothetical. Digital shadows exist of all of us. And, without being aware of your online behaviors, you *are* actively constructing this replica. And it's being sold on the open market for an absurd price, none of which you see. And even more diabolically, some of which you pay for.

Luckily, at this moment, and for the vast majority, the only thing this is really being used for is influencing how people spend their money. Most people know this phenomena as "targeted ads." Like when you are discussing kids with a friend and all of a sudden you start seeing ads for baby clothes on every website you visit.
# The Basics
###### If you skip the bill on safety, disaster will readily settle the tab. -DrBenMiles in [The Bhopard Disaster](https://www.youtube.com/shorts/7D6Q2HnqUM0) Video
Contrary to popular belief, security and privacy aren't just a couple buttons you push or settings you configure on a few devices. To proclaim "such-and-such is a lifestyle" has always irked me, especially when in arrogance, but I will make an exception for these two. Both require being mindful and modifying your internet behavior.

More specifically, security and privacy is a journey. You aren't going to wake up tomorrow as a ghost in the wires with perfect operational security (OpSec, as it's known in the industry). Nor should you want to. Trying to do so will only make you paranoid. 

I am speaking from experience regarding the paranoia. I grew up seeing ghosts behind every tree. When PCs shrank enough to fit in homes, this translated to seeing ghosts in every wire. It was about time to figure out what deserved my paranoia and what doesn't matter.

Start small. Start with one thing. When you understand that, move to the next, then to the next. Follow this pattern until you've reached the level of security and the level of privacy you are comfortable living with.
# Tiers of Play
I am going to split this into different tiers. Tier 0 will be the average users of the internet; the "I have nothing to hide," crowd. They shop online and use various services without anything protecting their identity. Facebook, Indeed, Amazon, TikTok. Everything is tied to a single person and that person can be linked to those around them. Everything is known about them: who they are, who they hang out with, who are they related to, who they work with, where they are, where they hang out, where their relatives are, where they work. They are completely oblivious to the function I will call the Data Machine and how much of themselves they willingly either give away or pay for companies to sell their data. Yes, many services that you pay for, also make money off the data they harvest from you. Yes, you do legally allow them to do this.

Tier 0.5, I will define as the average Tier 0 user that does know the Data Machine exists, but only surface level, doesn't know what to do about it, and for various reasons, aren't able to figure out what to do about it (fear, not enough time, don't know where to look, overwhelmed, etc). Tier 0.5 is likely loosely connected to someone who practices higher tiers of privacy and security, have read a book or two like "This is How They Tell Me the World Ends" by Nicole Perlroth (great read, I highly recommend), or watched a few shows like Black Mirror or Mr. Robot and took them seriously. Tier 0.5 users have picked up a couple practices themselves through social osmosis and have likely dabbled in some other higher tier practices. When Tier 0.5 users post on social media, they know not to include every bit of information they have in the post. They know not to fill out internet quizzes; "find out which Harry Potter house you belong in" or "post your badass score by counting how many badass things you've done in your lifetime." Instead of posting their location while on vacation, they know to post these pictures when they get home. But, they are not quite Tier 1. The Data Machine still knows everything about Tier 0.5 as Tier 0, but at least their every move and thought isn't immediately posted to every social media account that exists.
## Tier 1 - Starting the Journey
Tier 1 users know the Data Machine exists and have found, stumbled upon, or been directed towards ways to learn what to do about it. Tier 1 users find blogs like this one, organizations with self-teaching material like [eff.org](https://eff.org), and have either looked into or started using services that are privacy-first, like [Proton Mail](https://proton.me). They notice the Data Machine in more places than they originally thought. An overwhelming feeling starts to creep in of always been watched, someone always listening. They start to feel exposed, like those dreams where we are in school and realize we're naked and get laughed at.

To quell these fears, here's a list in order of how I thought of them (aka in no particular order) to start implementing into your life. Using any of one these puts you far ahead of Tier 0 users, so don't worry about which to start with or stressing about having to do all of them right now. I'll keep adding to this list as I think of new things.
- Stop falling for FOMO campaigns.
- Stop filling out online quizzes.
- Don't download things from anywhere. Yes, from anywhere.
- If you want to look at things that are risqué or visit a website you don't know is safe, learn about virtual machines and do it there. Oracle's VirtualBox is free and pretty self explanatory. For instructions, see YouTube.
- Be careful whenever you see Fear, Uncertainty, and Doubt campaigns. FUD sells more than sex. If you see FUD, it's probably a scam. If you see FUD and a price tag, it's definitely a scam.
- Set social media accounts to whatever the platform's highest privacy settings are that you are willing to deal with.
- Start learning how to use a password manager
	- The ultimate goal here is to stop reusing passwords, with every account getting a unique password
	- For now, learn how to generate passwords and use different passwords for different areas of your life
		- Shopping accounts get a password
		- Financial accounts get a different password
		- Social media accounts
		- etc
- Enable mfa / 2fa
	- Yes, it's an extra step. Do it.
	- Password managers often include this
	- You can also look for "authenticator apps." I've heard [Aegis Authenticator](https://getaegis.app/) is pretty good, but haven't tried it. YMMV
	- Try not to use SMS 2fa, as it's incredibly vulnerable to attack. But, SMS 2fa is better than no 2fa
- Buy a subscription using your favorite YouTuber's discount code:
	- A cheap VPN
		- All VPN companies are the same and they all keep logs. Just pick the cheapest.
	- Data removal services
- Don't do anything while connected to you company's network or VPN. You do not manage that network and anything you look up is logged and recorded by your sysadmin team
- In the same vein, don't use a company device for personal searches or a personal device for company-related tasks.
- When not in use, turn off signal broadcasters on your devices:
	- Bluetooth when nothing's connected
	- Wifi when using mobile data
	- Mobile data when using Wifi
	- Location when you know where you are
	- NFC when you're not using tap-to-pay
	- Hotspot (there is no "when" here, just turn it off)
	- Air Drop / Quick Share / Music Share when you're not sharing anything
	- Discovery mode
- Audit your app permissions
	- Deny all app permissions when installing apps
	- Assume all app permissions are unnecessary
	- If an app needs a permission, click "only this time" when needed
- Don't plug your phone in places you don't trust. If you need to plug your phone in somewhere get a power bank or a set of data blockers, known less formally as [phone condoms](https://a.co/d/00UeaXcy).
	- Data blockers work by having onlyddd power pins, the data pins are entirely absent. Thus, without data pins, no data can flow through them. You can test this out by sticking a mouse dongle into it and plugging it into your computer. You'll see that you're not able to use the mouse.
- Don't use unsecured WiFi
## Tier 2 - CorpFree
You've seen them all in the news: Microsoft, Tesla, Apple, Google, Meta. Breached, leaked, hacked. For the vast majority of people, these are just buzz words; and that's all they'll ever be. Some are aware these leaks can lead to some consequences. For a few unfortunates, however, it has lead to dire consequences: Identity theft, financial fraud, privacy invasions, targeted attacks. Any one of these is devastating to have to deal with and resolve. The internet has made all of this easier, now more than ever, and for even the non-professional, to perform all of these attacks against a single person.

Whether you're playing catch-up or trying to prevent these anxieties, the next step in not having your data leaked or stolen is to not give it to Corporations to begin with, or to remove it from Corporations you've already given it to. This means finding alternative platforms to use. Ideally, you would audit yourself to figure out which of these Corporations you actually need, find privacy-focused alternatives to those, and stop using the rest.

Living in a single ecosystem is akin to having the same password for every account. If someone compromises your Google / Apple / Microsoft account, they can easily and systematically compromise every service you've used this account to sign into. How many websites have you seen allow you to sign in using Google / Apple / Microsoft? How many of these websites have you actually signed into using this instead of creating a separate account for that website? This is called a [federated identity](https://en.wikipedia.org/wiki/Federated_identity). The website uses Google's / Apple's / Microsoft's authentication to verify you are who you say you are instead of leaning on their own.

Federating your identity is just another internet feature that makes everything incredibly convenient. Less button clicks, less accounts to manage, less passwords to remember. But, convenience often comes with the cost of being less secure and poking holes in your privacy. A good rule to live by: if it's convenient for you, it's convenient for an attacker. Security is not about convenience. It's about finding out which conveniences you are comfortable living without.

The following tools are intended to replace what you are already doing online: browsing, surfing, communicating, etc. You're still not quite hiding, yet. The goal of this tier is to add another layer that needs to be broken to compromise you, your life, and your family.

- [Proton.me](https://proton.me/) has been making an effort as a privacy-first Google- or Apple-style ecosystem. They offer a large suite of tools that assist in de-Corporatizing your life and are relatively device agnostic, which, in my honest opinion, makes them the best beginner-friendly de-Corporatization solution. Make sure when signing up for things you use an identifier in the email you use: "username+service@example.com" instead of "username@example.com"
- Download a flavor of Linux. I wouldn't necessarily recommend switching over fully at this point to Linux as a daily driver, but this is a good time as any to start experimenting. Which Linux to use for whatever use-case (including which is "the best" beginner-friendly) is a widely debated topic. Personally, my favorite to recommend for a starter distro (short for distribution) is [Linux Mint](https://linuxmint.com/).
- Stop using chrome and edge as shipped by the manufacturers. [Brave](https://brave.com/) is great beginner-friendly privacy browser. Note that it is a chromium based browser, however. But, it will give you the chrome feeling while cutting back significantly on Google surveillance. For more information, look up "browser hardening." A hardened Firefox is your best bet for privacy and security.
- Start learning about [TOR](https://www.torproject.org/). On android you can route your traffic through Tor using [Orbot](https://play.google.com/store/apps/details?id=org.torproject.android&hl=en_US&pli=1). As with everything, Tor is not an end-all-be-all privacy/security setting. Experimentation leads to understanding, but don't do anything stupid or illegal. This is just another tool you can put in your toolbox.
- [Signal](https://signal.org/) is a messaging app with a similar feel to WhatsApp. But, it isn't owned by Meta. It's run by people who actually care about security and privacy, and is another tool that removes you and your family from an ecosystem.
- At this point you know [Google tracks everything](https://www.lifewire.com/how-to-stop-google-from-tracking-you-11740472) about you. But, did you know that "everything" includes your Google searches? What you search, how you search, search patterns, ads you click, images you look at. Based on IP addresses, geographical location, and more. You don't even have to be signed in. Whenever habits that look like yours are detected, every scrap of data (IP address, location, whether your walking or in a vehicle, which near-by phones are often near-by, etc) is added to your digital shadow. Just use [DuckDuckGo](https://duckduckgo.com/).
- [PrivacyTools.io](https://www.privacytools.io/), [r/DigitalEscapeTools](https://www.reddit.com/r/DigitalEscapeTools/) and [r/CorpFree](https://www.reddit.com/r/CorpFree/) are great places for gathering more understanding. A good rule to live by: whenever you use a service, look online for alternatives. Literally put into the search bar "\[service] alternatives."
# Closing Thoughts
Originally, I was going to outline all of the tiers at a high level. But, after getting through only about a third of tier 3, I noticed that this post was already sitting at around 3,500 words. I had so much more to say and I'm already a week late with this post. At this time, I would like to keep my posts between 2,500 and 3,500 words. Long enough to get my point across, but short enough that reading it doesn't impact anyone's day.

The other reason I decided not to go into higher tiers is because anything above tier 2 starts to get into the weeds of how things work, and can't easily be summed up anymore at a high level. In tiers 3 and up, I will discuss topics such as self-hosting, the difference between proxies and VPNs and how to know when to use which, compartmentalization, encryption, intrusion and detection, and risk assessment.
# Conclusion
We've discussed why "I have nothing to hide" is naive at best, touched on how dangerous that thought process can be, and how to begin increasing your privacy and security posture. Implementing any of the behaviors in Tier 1 puts you leagues ahead of Tier 0's, but implementing all of them as habit takes time, practice, and patience. Once you get a handle on Tier 1, start moving into Tier 2. Again, this takes time. Habits don't form over night. At these stages, don't be hard on yourself for making mistakes. This is the time to make them. Recognizing a mistake was made is more important than not making it.

Layers make security. The goal here is not to put up a shield and go about your day. The goal is to wear layers that need to be penetrated in order to get in. The more layers you have, the more costly it will be to hack you, track you, and maintain a digital shadow of you.