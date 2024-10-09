---
layout: post
title: "Protel Programming - Now Available over SIP"
date:   2024-10-08 0:00:01 -0400
categories: payphone software expressnet xnet protel
---

Earlier this year we released our [public Protel payphone programming line]({{ site.baseurl }}/2024/02/10/protel-programming-line.html), and today we're adding another way people can connect to it.

Now, the programming line will also be available over SIP, so anyone can directly connect to our PBX and access the line.

Connection details are as follows:

* Server: programming.philtel.org
* Port: 16556
* Username: protel
* Password: protel
* Codec(s): ulaw

The context is set so that any phone number dialed over this connection will be sent to out programming setup.

If you are connecting via an ATA, make sure you use the G.711 ulaw codec, disable echo cancellation and fax options, and set the jitter buffer to be fixed (different people have different success rates with high/medium/low buffer so you may need to play around with this).

Happy programming!

