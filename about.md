---
layout: page
title:  About
permalink : /about/
---
{% raw %}<p><img style="width: 100%; display: block; margin: 0 auto; border 0" src="/assets/img/philtel-head.jpg"></p>{% endraw %} 

PhilTel is an amateur phone collective looking to install new (to us) payphones within the city of Philadelphia. Any payphone installed will be completely free-to-use, allowing the user to place calls within North America. Additionally, we would like to provide various experiences and services through the phones; these could be as simple as voicemail, fun as a phone number that randomly calls other payphones, or as complex as an integration with phone collectors' networks to allow [phone-phreaking](https://en.wikipedia.org/wiki/Phreaking){:target="_blank"} and exploration!

## Frequently Asked Questions

### How is PhilTel funded?

PhilTel is funded 100% by donations. If you would like to donate, please check out our [donations page](../donate) for options!

We do not play advertisements before, during, or after phone calls.

### Are my calls private?

We collect no data from users and have no record or insight into the contents of calls. At most, we may capture the phone number dialed if we are actively debugging an issue, but this data is not kept long term.

Further, calls are encrypted from the phone site to our server. Any bad actors at the phone location will not be able to inspect call traffic even if they are on the same Internet connection.

### I have an old payphone in my basement, do you want it?

Probably! Please reach out to us via our [contact page](../contact)!

All payphones we utilized will either be acquired from donations or the used market. Contrary to anything you may have heard, it is perfectly legal to own/buy/sell a payphone as a private individual or organization.

### Are there other projects like this?

[Futel](https://futel.net){:target="_blank"} is a Portland, Oregon-based phone company started in 2014 by Karl Anderson. Futel currently operates twelve free-to-use phones (both payphones and armored phones) within Oregon, Washington, and Michigan, including one at Right 2 Dream Too, a self-managed houseless encampment in Portland. Fore more information, visit 

[Talk To Me](http://talktomenyc.com/){:target="_blank"} is a network of five public payphones located in each of the five boroughs of New York City, that only call each other. Participants who pick up a TTM receiver set the other four ringing, inviting passing New Yorkers to answer the call. Upon connection, two unsuspecting strangers are thrust into conversation and left to navigate a serendipitous interaction with their neighbor. Talk To Me was launched in June 2022 by Jordan Seiler. For more information visit

### How does the PhilTel network work?  

To power the telephone network that drives the payphones, we are making use of commodity hardware (especially used/discarded and low-power-consumption hardware) and open-source software where available. The workhorse of our network is an [Asterisk](https://www.asterisk.org/){:target="_blank"}-based Public Branch eXchange (PBX) running on a virtual private server (VPS) on the Internet. Asterisk acts a bit like a bridge between the Public Switched Telephone Network (PSTN) and our collection of payphones; we can facilitate connections between the payphones and the outside world seamlessly. To connect to the PSTN, we get service through a Voice over Internet Protocol (VoIP) provider (think of them like an Internet Service Provider but for telephony). However, Asterisk is much more powerful than a simple traffic controller, and it lets us create our own services or connections to private telephone networks that any caller can access!  
Each site where a payphone is installed will connect to our Asterisk server through two pieces of hardware: a GL.iNet GL-AR300M16 router and a Grandstream HT801 Analog Telephone Adapter (ATA). These devices connect to mains power (pulling less than 1A at 5V in total), though we hope to explore solar options in the future.  

{% raw %}<p><center><a href="/assets/img/philtel-demo-hardware.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/philtel-demo-hardware.jpg"></a><figquote>This box has been used to demo the PhilTel project at various events. The right side of the box shows the ATA (top) and router (bottom) which would be the components installed at a remote site. The left side features an LTE modem access (left) and a battery (right) to give us temporary Internet and power on-the-go.</figquote></center></p>{% endraw %}  

The router (similar to your Internet router at home) runs software called [OpenWRT](https://openwrt.org/){:target="_blank"} that allows for more robust features than typical router software and provides a consistent experience for the administrator configuring it (even if we had many routers made by different brands). Another piece of software called [OpenVPN](https://openvpn.net/){:target="_blank"} is installed on both the router and our Asterisk server to provide an encrypted Virtual Private Network (VPN) so devices connected to the router can communicate with services on our server as though they were physically on the same network.  

{% raw %}<p><img style="width: 100%; display: block; margin: 0 auto; border 0" src="/assets/img/network-diagram.png"></p>{% endraw %} 

With the VPN, any bad actors using the same network as our phone at the installation site will not be able to investigate any traffic related to a call being made. Further, PhilTel does not monitor who is using the phone or the contents of any call, though phone numbers may reside in our server logs until they are cycled out.  

But how does the payphone, or any old phone with an RJ-11 jack for that matter, communicate with our Asterisk server? This is where the ATA comes in. The ATA has two jacks on it: one RJ-11 jack for a phone and one RJ-45 jack to connect to a router.  

{% raw %}<p><center><a href="/assets/img/grandstream-ht801-rear.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/grandstream-ht801-rear.jpg"></a><figquote>The back of the Grandstream HT801 ATA showing both the ethernet and telephone ports.</figquote></center></p>{% endraw %}

Essentially, the ATA acts as an analog-to-digital converter and helps our old payphone speak the popular Session Initiation Protocol (SIP), allowing Asterisk a way to send/receive calls to our payphone over the Internet! To the payphone, the ATA behaves just like the PSTN would, so everything works the same way it would if the phone was connected to a standard landline.  

{% raw %}<p><center><a href="/assets/img/coinless.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/coinless-sm.jpg"></a><figquote>This coinless circuit board allows a payphone to be used without needing coins to be inserted!</figquote></center></p>{% endraw %}

For now, PhilTel phones make use of a "coinless" circuit board that replaces the existing electronics in the phone that would determine how much to charge for calls and handle payments. The new circuit board has a much smaller footprint than the older board and mounts cleanly to the back of the phone’s dialpad. In the future, we hope to repurpose the older phone electronics to reduce waste and still keep calls completely free!  