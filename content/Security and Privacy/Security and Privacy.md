---
draft: true
tags:
  - security
---
## Tier 3 - Controlling Your Data
By now, you should understand or be starting to understand why "I have nothing to hide" is naive at best. Your data is yours and should be treated as such. Protecting your data is as important as protecting your financial information, your social security number, what school your children go to. Information, and likewise data, can't be taken back once it's given. You don't have to hide it from every person, but you should be in control of who gets it.

This tier will explore deepening your understanding of the concepts and tools in tiers 1 and 2, like VPNs and password managers, you will start learning about [self](https://www.reddit.com/r/selfhosted/)-[hosting](https://www.reddit.com/r/SelfHosting/), [proxies](https://www.reddit.com/r/proxies/) and [I2P](https://www.reddit.com/r/i2p/), and encryption. In tier 2, I ended with, "whenever you use a service, search for an alternative." In tier 3, we'll shift this a bit to, "whenever you use a service, try to figure out how to self-host it." To be clear, self-hosting doesn't necessarily mean you create \[the thing], just that it is hosted in an environment fully controlled by you.

Except for email. Email is notoriously a horrific pain in the ass to set up, monitor, maintain, and troubleshoot. I would recommend looking into it to learn, because learning will always benefit you in the long run. But, just know that you're in for a long ride complete with a lot of cursing under your breath until you get it working, or get it working again.
### Encryption
So, this is where things start to get a bit technical. But, if you want to protect yourself, you have to learn some of the underlying technologies that power privacy, security, and the tools that make this possible to make educated decisions down the road. Plus, you've already read this far, I may as well actually teach something.

Encryption – or more specifically called cryptographic solutions, aka crypto (no, not the currency) – is an incredibly wide net that is at the core of security, hence why I'm starting here. There are many cryptographic solutions, from PKI, digital signatures, and certificates to obfuscation, salting, and hashing. These are not basic, so I will cover them in a later post.

Yes, I did use an en-dash. No, this wasn't written by AI. Although, I did learn they existed from AI and how to use them.

Narrowing encryption down even further, there are a few categories that would be helpful to outline for the purposes of this post; asymmetric vs symmetric encryption, encryption algorithms and key length, and levels of encryption.
#### Asymmetric vs Symmetric Encryption
Data encryption is employed when you want to protect data from someone you haven't authorized to access or use said data. Data like your health information or your banking details. Encryption works by taking what's known as plaintext (if you can read it, it's in plaintext), running it through some kind of cipher, and outputting it as ciphertext.

In order to not lose the original message, it's useful to use a kind of cipher that is somehow reversible. Ciphers have been used ever since information has been worth stealing. From the simple substitution cipher like the classic Caesar cipher, where letters are shifted by a fixed number, to more complex ones, like the Vigenère cipher, which uses a repeating key to change the shift amount for each letter.

The more complex the cipher used, the more difficult it is to "crack" the ciphertext back into plaintext. In modern times, the ciphers used are mathematical equations that rely on very large prime numbers. The reason for this is outside the scope of this post, and, to be quite honest, are far outside the scope of my current understanding. I'll eventually learn this as well, but feel free to get a head start.

Okay, with the introductions out of the way, let's talk symmetries. Symmetric encryption is basically passwords. You use a password to encrypt something, you use that password to decrypt that something. Just like logging in your favorite social media site. Top-level, what's happening in the background is that you enter your desired password and the encryption algorithm uses that as a kind of key for the thing you are encrypting. Whenever you want to decrypt that thing, you enter the password and the encryption algorithm, like a lock, magically decrypts.

Asymmetric encryption is a bit different in that you generate a key **pair**. Understanding this kind of encryption takes some setting up and mental gymnastics, but is pretty straight forward once you get it. One key "locks" the data and only the other one can "unlock" it. Think of it kind of like a key and padlock, but in this scenario, we're going to keep the key well guarded. If someone wants to send me something secret and wants to make sure only I can see it, they can ask me for a copy of my padlock. I make it for them and send it over. They put the McGuffin in a box and lock it with my padlock, which only I can open (assuming the box can't be broken, yada yada). They now know with certainty that since I'm the only one with the key, they can safely assume only I can see the McGuffin.

Asymmetric encryption has another built-in feature as well. The *private key* that only I have can also act as a kind of "monarch's seal." I can use my key to digitally sign my work. The process is the same as someone putting the McGuffin in the box and sealing it with something that only I can decrypt, but in reverse. My key can also encrypt a McGuffin that anyone can decrypt. Since I'm the only one that can encrypt this McGuffin in a way that my *public key* can decrypt, it must stand to reason that I am the originator of this McGuffin.

The logical next step is discussing how powerful this private key can be. It can sign work digitally and decrypt secret messages intended for a specific person. Sounds like something precious. All things precious make valuable targets for theft. How do we protect the private key? Well, there's no right answer here. Often, the most useful way to protect it is to encrypt it with a password. Yes, you can, and should, combine asymmetric and symmetric encryption. Together, they create a sum that is far greater than the of their parts.
#### Levels of Encryption
What can you do with this? And how does this pertain to tier 3?

There are various levels of encryption that each have their place and use-case. Starting with the highest level, full-disk encryption, or FDE, is used to encrypt your entire hard drive. You may have heard of BitLocker. That's Microsoft's FDE solution. Linux has LUKS and Mac has FileVault. Often, when you lock up the whole disk, you have to enter a password just to boot into it, though not always necessarily. With Windows 11, the hard drive is fully encrypted when the machine is shutdown – always shutdown your device when not in use, especially mobile devices like laptops.

Next step down is partition level encryption. This is pretty self-explanatory for technical people, but 

---

Which tier starts?
- Passwords // Emails // Account names
	- Same within compartments
	- Everything different
- Compartmentalization (tier3)
	- Compartmentalizing emails and accounts
- Compartmentalization (tier4)
	- Example, instead of switching from the Google or Apple eco-system to the privacy-focused Proton ecosystem in its entirety, use Proton for email, Wasabi for cloud storage, Bitwarden for your password manager, etc
- Compartmentalization (tier5)
	- Creating sock puppet accounts and online identities
- Data poisoning (tier3)
- Data removal (tier3)
- Data masking (tier3) - also, this is not data masking
	- Email (addy.io)
	- Address (iPostal1)
- De-Corporatizing your life (tier2)
	- Google
	- Apple
	- Microsoft
- Password manager (tier1)
	- Or at least one password for each part of your life: finances, social media, shopping, hobbies, etc.
- Logging
- Sandboxing with firejail/kasm (tier3)
- Self-hosting
	- LLM Studio // Hugging Face // Ollama
	- Password manager