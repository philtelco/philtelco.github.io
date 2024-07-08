--
layout: post
title: "Fixing a Protel Payphone Part 2: Installing ExpressNet"
date:   2024-07-07 0:00:01 -0400
categories: payphone software expressnet xnet protel
---

Initially, I attempted to program the phone with call cost rates via the keypad according to some programming instructions I found online targeted at the Protel CTR201 series boards. There is a handy programming quick reference distributed by Protel that walks a user through the programming steps as well as [an old text file from 2000 written by El Oscuro](/assets/docs/prtl201.txt){:target="_blank"}.  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/protel-manual.jpg"><img style="width: 80% max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/protel-manual.jpg"></a><figquote>Programming instructions for a Protel CTR201.</figquote></center></p>{% endraw %}  

After trying to program  my Protel 7000 board using the keypad, I learned that this board doesn’t support programming via this method and that this board would have to be programmed via computer.  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/protel-7000-board.jpg"><img style="width: 80% max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/protel-7000-board-sm.jpg"></a><figquote>A Protel 7000 board.</figquote></center></p>{% endraw %}  

Protel boards make use of a configuration and management packaged known as ExpressNet (or simply XNet) that interfaces with the board via a modem connection. ExpressNet is an older piece of software, designed to run on DOS machines, with little support for modern operating systems. After some research, I concluded that I would need some sort of lab machine to host XNet running either DOS or an older Windows 9X release to be sure I could run it properly.  

The first logical place to start with this was creating a virtualized environment. I have previously worked fairly extensively with VirtualBox, so I thought it would be a good candidate for this project as to not tie up a physical machine. To keep this as FOSSy as possible, I decided to install FreeDOS into a VM where I could then install XNet.  

Luckily, there is installation media for ExpressNet version 1.55 floating around the Internet, so it was readily available. From what I understand, the floppy image for this software was originally released by Protel themselves online as a free upgrade disk from a lesser version, but crackers were quick to discover they could modify a file or two and turn the disk into a full-fledged installer.  

I’ll be the first person to say that I’m not well-versed in DOS as my first home computer ran Windows 95 and I only became familiar with the most basic idea of it when running games as a kid. In the years that would pass I became more experienced with CLIs, but I never had the experience of a straight-up DOS machine to work on.  

Almost immediately I ran into issues with the XNet installer failing due to not finding `share.exe`, an executable normally included with MS-DOS to facilitate file locking. I tried to copy over a legitimate `share.exe` and even a third-party work-alike executable, but I would still get the same failure message.  

Reading more about experiences people had with XNet online, I decided to try running Windows 98 instead of FreeDOS, which many people reported to be working perfectly with no issues whatsoever. Instead of digging out my old installation media, I found that there was already a Windows 98 VDI file for VirtualBox available on the Internet Archive, so I decided to use this as the basis of the installation.  

While XNet installed just fine, no matter what I did I could not get it to recognize a modem. VirtualBox has a really handy feature that allows you to map a serial port on the host machine to a “virtual” serial port on the VM. This worked just fine as I could access and use the modem via HyperTerminal, but XNet wouldn’t budge.  

At this point I decided the best thing to do would be to use bare metal and try running Windows 98 directly on a machine with a real serial port. The choice to stick with Windows 98 was was mostly for familiarity, but also for future compatability. Other payphone software like Elcotel’s PNM Manager needs a Windows environment to run, so having a machine that can be something of a “swiss army knife” for payphone programming was an attractive idea.  

The problem with running Windows 98 on bare metal is that it generally works best on period-appropriate hardware. From some of my other projects, most notably an attempt at a dialup ISP, I knew that running 25-year-old (if not older) hardware was a recipe for unexpected issues like disk crashes or power supplies going poof in the night. Of more concern, a lot of older PCs were very power-hungry and likely to make an impact on my power bill if I decided to run one 24/7. What could I do about a stable, power-efficient machine that could run Windows 98?  

The answer was thin clients. For a little while I had heard of people repurposing thin clients as servers for their home lab and retro gaming machines. The latter use-case was just what I needed, and enthusiasts had already done a lot of leg-work to document a number of thin clients that supported both DOS and Windows 98. These thin clients generally draw low power, run passive cooling, and and have no moving parts like fans or conventional hard disks.  

Luckily, there is a great website out there documenting how to repurpose different thin clients, [parkytowers.me.uk/thin/](https://www.parkytowers.me.uk/thin/){:target="_blank"} After checking the site, I settled on a [Wyse V90LE](https://www.parkytowers.me.uk/thin/wyse/vx0/WyseV90LE.shtml){:target="_blank"} thin client that was thoroughly documented in its support and easily available on eBay. It also had a real serial port, which is much harder to find on more modern thin clients. While I could perform hardware upgrades and software installation myself, I found someone selling units with a larger DOM (disk on module), more RAM, and both Windows 98 and Windows XP installed dual-booted with all drivers. Surprisingly, these units were priced only slightly higher than a wiped unit plus the memory and disk upgrade.  

After I received the thin client, the next step was to install XNet. Initially I decided to use a differnet, older Windows 98 machine with a 3.5” internal floppy drive to write the XNet .ima image file directly to a blank diskette. I have two external floppy drives, but neither seemed to be able to read the floppy on the thin client. I tried a different disk, and tried writing the disk with the external floppy drives on a modern Windows computer but still couldn’t get the disk to read so I’m wondering if there is something strange about the image. The next step was to try to mount the disk image directly on the thin client via Daemon Tools, but I quickly found that the image format wasn’t supported. Then, I tried to extract the image and copy the contents over to the thin client, but running the installer off of the thin client didn’t seem to progress far. What finally ended up working was to dump the contents of the image on a flash drive (luckily the Windows installation already had the mass storage drivers) thin/and then run the installer directly off of the flash drive on the thin client.  

The setup is incredibly straight-forward. Below are some screenshots documenting the process.  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-00.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-00.JPG"></a><figquote>ExpressNet installation files are loaded into the root directory of a flash drive. <pre>Install.exe</pre> can be launched to start installation.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-01.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-01.JPG"></a><figquote>We have no existing data, so proceed with New Install.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-02.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-02.JPG"></a><figquote>Most "wild" payphones out there already run DA/DD firmware, I'm actually not sure if I've ever heard of any that don't. We can press any key to continue. If you have an older firmware on your phone you may need an older XNet version.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-03.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-03.JPG"></a><figquote>Press any key to continue.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-04.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-04.JPG"></a><figquote>A base installation in the <pre>C:\</pre> directory works just fine.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-05.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-05.JPG"></a><figquote>Enter your company name.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-06.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-06.JPG"></a><figquote>Pick which serial ports XNet will use.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-07.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-07.JPG"></a><figquote>Select a time zone.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-08.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-08.JPG"></a><figquote>Enter <pre>0</pre> as to disable password expiration.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-09.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-09.JPG"></a><figquote>Choose if you would like to add XNet to the system path, and/or start automatically.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-10.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-10.JPG"></a><figquote>Enter <pre>Y</pre> to allow XNet to modify <pre>AUTOEXEC.BAT</pre>.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-11.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-11.JPG"></a><figquote>XNet will show you the changes to <pre>AUTOEXEC.BAT</pre>.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-12.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-12.JPG"></a><figquote>Enter <pre>Y</pre> to allow XNet to modify <pre>CONFIG.SYS</pre>.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-13.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-13.JPG"></a><figquote>XNet will show you changes to <pre>CONFIG.SYS</pre>.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-14.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-14.JPG"></a><figquote>Installation is completed, press any key to continue.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-15.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-15.JPG"></a><figquote>When starting XNet, it will query for a connected modem on the port(s) you specified during installation and attempt to reset it.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-16.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-16.JPG"></a><figquote>Login with user <pre>PROTEL</pre> and password <pre>PROTELX3</pre>.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-17.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-17.JPG"></a><figquote>Press <pre>Enter</pre> to change the password.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-18.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-18.JPG"></a><figquote>Enter and confirm a new password, then press <pre>F2</pre> to save.</figquote></center></p>{% endraw %}  

{% raw %}<p><center><a href="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-19.JPG"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2024-07-07-fixing-a-protel-pt2/xnet-install/expressnet-19.JPG"></a><figquote>You are now running ExpressNet!</figquote></center></p>{% endraw %}  

Part II

The next step to getting XNet running was connecting up the modem. I have a Protel UPMS 1200 modem which was built specifically to program payphones via XNet. When I had asked online about whether or not I needed one of these modems to program my Protel 7000 board, many people told me that the 7000-series boards could only be programmed with the official modem and anything else simply wouldn’t work.

I’ve later learned that this isn’t true, but I can now understand where the confusion on this came from. A Protel 7000 series board has a modem that supports a 1200 baud connection, but unlike most modems which will use FSK modulation at 1200 baud (Bell 202), Protel boards use PSK modulation which makes them incompatible. Somehow, this morphed into the myth that the 7000-series boards could only be programmed at 1200 baud when in-fact they can also be programmed at 300 baud (standard Bell 102) if the software is configured to use a non-Protel modem.

At the time though, I didn’t know this. When an opportunity came up to get a UPMS 1200 modem I jumped on it. I figured best case scenario I would be able to use the UPMS to make sure that my setup worked and then experiment from that point to show definitively wether other modems would or wouldn’t work. Worst case scenario I still had all the “proper” components for a functioning setup.

Upon receiving the UPMS modem, I hooked up the phone line, wired the data port to the serial port on the thin client, and powered it up. Immediately I head a loud pop from the internal speaker and then a loud dial-tone. It seemed like the modem immediately went off-hook without being configured to do so. This wasn’t a good sign. Starting XNet will send some initialization configuration to the modem so I loaded it up and watched it open communication and send the init string, but the modem still wouldn’t give up the load dial-tone.

At this point I knew that the modem was broken, so I decided to open it up and investigate for any failures. Cosmetically, the inside of the unit looked fine and all of the components seemed to be intact. I noticed that the board had a handful of electrolytic capacitors so I decided to record the values of these and order replacements. This was only a few dollars worth of parts and luckily all of the capacitors are through-hole so replacement wouldn’t be too difficult.

When the capacitors arrived I went ahead and replaced each one on the board one-by-one with a soldering iron, desoldering vacuum tool, and solder wick over the course of 20-30 minutes.
