---
layout: post
title: "Free Community Protel Programming Line is Live"
image: /assets/img/2024-08-26-fixing-a-protel-pt4-xnet-config/rpviewer1.png
date:   2024-02-10 0:00:01 -0400
categories: payphone software expressnet xnet protel
---

*Updated 2024-10-08 with SIP information*

After a little over a year from having the idea to run a communal Protel programming line, we now have one running on both the PSTN and [PhreakNet](https://portal.phreaknet.org/){:target="_blank"}! Using this line, most Protel payphones can be programmed to rate all calls free, essentially turning them into standard "dumb" telephones. Note that we haven't tested many firmwares or different board types, but our understanding is that most boards are supported as long as they have later firmware versions.

The number for the line is **225-2PROTEL** (PSTN) and **263-0500** (PhreakNet). We have tested this both from a Cable-ISP VoIP line and an ATA (forcing ulaw codec, setting jitter buffer fixed and low, and disabling echo cancellation) connected to a separate exchange on PhreakNet and have had programming successfully work most of the time. Your mileage may vary, and this service is offered with no guarantees or warranty. 

Special thanks to John Porter and Peter for their help with ExpressNet along the way!

## How is this set up?

From a technical standpoint, we are running ExpressNet 1.55 on Windows 98SE on a Wyse thin client hooked up to a Protel UPMS 1200 modem. The modem is hooked into an Adit 600 channel bank, which is connected to an Asterisk PBX by way of a Sangoma T1/E1 card. A video explaining the whole setup is available below:

{% raw %}<p><iframe width="560" height="315" src="https://www.youtube.com/embed/yRDab94jheM?si=GWcb96ulaDOHEaJI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></p>{% endraw %}

## Programming

To program your phone, first make sure it is on-hook.  
Now, hold down the program button and take the phone off-hook, you should hear a single beep. You can now release the program button.  
On the keypad, dial `005555555555*`, you should then hear a single beep.  
Next, dial `252252776835*` (for PSTN) or `252630500*` (for PhreakNet), you should then hear a single beep.  
If you hear more than one beep on the above steps, simply repeat the step.  
Finally, dial `*#3`. You should hear the phone say the phone number of the phone (in this case __555-555-5555__) and then you will hear the phone connect to ExpressNet and download the necessary configuration. This should take around 5 minutes total.  
After the download finishes, dial `*#6`, and you should hear a timestamp of the last successful download.  

If your download fails, try repeating the steps above again. Otherwise, make sure you have working (and charged) payphone batteries (test this by calling your payphone and seeing if the internal modem picks up), a working keypad, etc. **Note: You may need to retry five times or so depending on a number of factors, VoIP can be temperamental. For additional help, don't hesitate to [contact us](/contact/).**

## Direct SIP Connection

We now have a direct SIP connection into our PBX for programming. Connection information is supplied below.

* Server: programming.philtel.org
* Port: 16556
* Username: protel
* Password: protel
* Codec(s): ulaw

The context is set so that any phone number dialed over this connection will be sent to out programming setup. You can follow the programming steps above as written with this SIP setup and the behavior should be identical.

If you are connecting via an ATA, make sure you use the G.711 ulaw codec, disable echo cancellation and fax options, and set the jitter buffer to be fixed (different people have different success rates with high/medium/low buffer so you may need to play around with this).

## Next Steps

We are currently working on documenting how we set this up so that others can follow and recreate the setup. We also want to see how we can modify the setup to virtualize the software environment and use more abundant modems to lower the barrier for entry and allow others to set up their own programming environments quickly and easily.

While the line only rates calls for free at the moment, we ultimately hope to come up with a system so that people can submit custom programming requests and we can set up site records in ExpressNet to accommodate them. 