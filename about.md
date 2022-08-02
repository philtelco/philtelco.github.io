---
layout: page
title:  About
permalink : /about/
---
{% raw %}<p><img style="width: 100%; display: block; margin: 0 auto; border 0" src="/assets/img/philtel-head.jpg"></p>{% endraw %} 

Want to know how the Philtel network actually works?

To power the telephone network that will drive the payphones, we are looking to make use of commodity hardware and open-source software where available. The workhorse of our network is an [Asterisk](https://www.asterisk.org/){:target="_blank"}-based Public Branch eXchange (PBX) running on a virtual private server. Asterisk acts a bit like a bridge between the Public Switched Telephone Network (PTSN) and our collection of payphones; we can facilitate connections between the payphones and the outside world seamlessly. To connect to the PSTN, we get service through a Voice over Internet Protocol (VoIP) provider (think of them like an Internet Service Provider but for telephony). However, Asterisk is much more powerful than a simple traffic controller, and it lets us create our own services or connections to private telephone networks that any caller can access!

Each site where a payphone is installed will connect to our Asterisk server through two pieces of hardware: a router and an Analog Telephone Adapter (ATA). The router runs software called [OpenWRT](https://openwrt.org/){:target="_blank"} that allows for more robust features than typical router software, and provides a consistent experience for the administrator configuring it (even if we had many routers made by different brands). Another piece of software called [WireGuard](https://www.wireguard.com/){:target="_blank"} is installed on both the router and our Asterisk server to provide an encrypted Virtual Private Network (VPN) so devices connected to the router can communicate with services on our server as though they were physically on the same Local Area Network (LAN). 

But how does the payphone, or any old phone with an RJ-11 jack for that matter, communicate with our Asterisk server? This is where the ATA comes in. The ATA has two jacks on it: one RJ-11 jack for a phone and one RJ-45 jack to connect to a router. The ATA can be configured to talk to our Asterisk server using the popular Session Initiation Protocol (SIP), allowing Asterisk a way to send/receive calls to our payphone over the Internet! To the payphone, the ATA behaves just like the PSTN would, so everything works the same way it would if the phone was connected to a standard land line.

{% raw %}<p><img style="width: 100%; display: block; margin: 0 auto; border 0" src="/assets/img/network-diagram.png"></p>{% endraw %} 
