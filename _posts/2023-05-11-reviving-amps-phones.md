---
layout: post
title: "Reviving AMPS Phones (April Fools' Post Explained)"
date:   2023-05-11 0:00:01 -0400
image: /assets/img/2023-05-11-reviving-amps-phones/amps-01.jpg
categories: cellular mobile
---

After publishing [our April Fools' Day post](https://philtel.org/2023/04/01/announcing-philtel-cellular.html), some people were surprised to learn that we could actually get old AMPS "bagphones" working and communicating with one another. There are posts online regarding how to do this, but I've found them to be slightly incomplete and wanted to walk through the process of getting everything working. Many thanks can be given to [Dmitrii Eliuseev for his 2021 post *HowTo: Running the 1G Analog Phone from 1997*](https://medium.com/geekculture/howto-running-the-1g-analog-phone-from-1997-3caec77a9df9) and [the wonderful folks at the *osmocom-analog* mailing list](https://lists.osmocom.org/hyperkitty/list/osmocom-analog@lists.osmocom.org/)

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-01.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-01-sm.jpg"></a><figquote>Two AMPS phones: A Radio Shack 17-1002 and a Panasonic EF-6106EA.</figquote></center></p>{% endraw %}  

# What is AMPS? {#what-is-amps}

Advanced Mobile Phone System (AMPS) is an analog mobile phone standard that was introduced in the Americas in late 1983. It was the most popular analog standard in North America until it was finally shut down in 2008. AMPS is part of the first generation of cellular technology (1G), and operated in the 850 MHz Cellular band. 

AMPS utilized 60kHz duplex channels, each of which was composed of two frequencies with 30kHz one-way bandwidth. As this technology was analog, it was subject to noise and static and offered no protection from eavesdropping (which could even be done from older televisions about to pick up UHF TV channels 70–83, as AMPS reused some of the older allocation), or cellular cloning.

While AMPS phones have been replaced by digital successors these days, they were fairly popular in their time and mostly exist now as paperweights.

# What do I need? {#what-do-i-need}

To get an AMPS phone(s) running we will need the following:

* One or more AMPS phones, and some way to power them (more on this later)
* A LimeSDR Mini (A non-mini also works, and potentially other SDRs, but this guide focuses on the Mini)
* Cellular antennas for the LimeSDR Mini
* A computer running Linux (We'll be showing Ubuntu, but this could possibly work on other POSIX operating systems)
* Antennas or (preferably) dummy loads for our phone(s)

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-11.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-11-sm.jpg"></a><figquote>Bags of bag phones.</figquote></center></p>{% endraw %} 

Obviously we'll need an AMPS phone to get running, but powering it can be a bit tricky. Often, AMPS phones on the secondhand market come with their original power cable, meant to be plugged into the lighter socket of a car (as most people used these exclusively as "car phones). This means we can use a suitable bench power supply [like this one from Pyramid](https://www.amazon.com/Pyramid-PS9KX-Universal-Regulated-Converter/dp/B0002BA570?th=1) with a built-in lighter socket to supply adequate power to our phone. Alternatively one could hack the plug off of the end of the power cable and wire the leads directly to a bench supply's terminals to deliver somewhere between 12V and 13.8V. We'll discuss battery options later in this guide.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-02.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-02-sm.jpg"></a><figquote>Two power supplies with lighter sockers.</figquote></center></p>{% endraw %}  

The LimeSDR Mini is probably the most critical and hard-to-find item in this list. While it originally retailed for $100 USD, the current value of the Mini on the secondhand market is between $200 and $300 if one can even be found. Other SDRs like the full-sized LimeSDR also work, but they are considerably more expensive. The key feature about the LimeSDR models that make them suitable for interfacing with AMPS phones is that they are "full-duplex" SDRs, meaning they can transmit and receive simultaneously. While it is likely feasible to use two inexpensive SDRs (one for transmit and one for receive), this likely isn't possible with the software we will be using.

My SDR did not come with antennas, and instead of opting for telescoping antennas that I would have to appropriately size, I purchased two [Quad-band Cellular Antennas](https://www.sparkfun.com/products/8347) that cover the frequency range for AMPS, and feature an SMA connector which is what the LimeSDR Mini uses.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-03.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-03-sm.jpg"></a><figquote>A LimeSDR Mini in case, with the two cellular antennas connected.</figquote></center></p>{% endraw %}  

We will need a computer to run [Osmocom-Analog](http://osmocom-analog.eversberg.eu/), the software that will interface with our SDR and get the phones operational. I'm using a laptop with Ubuntu installed, and this guide assumes a Debian-based operating system. Osmocom-Analog may run on a different POSIX operating system with some modifications, but there are many dependencies that need to be satisfied. I'll be using a ThinkPad X230 laptop as the host computer for its portability. A top-of-the-line computer is not needed to support the LimeSDR Mini.

An AMPS phone should never be powered on without an antenna attached as this can damage the radio in the phone. However, it is also important to note that **these days the AMPS frequencies have been reallocated for emergency radio use and it would be incredibly bad if you were to interfere with these frequencies. That said, consider this guide for informational purposes only, and follow it at your own risk.**

All of my phones luckily have removable antennas, so I opted to buy dummy loads to replace the current antennas. To get an idea of output power for any AMPS phone, take note of the FCC ID for the phone, usually located on a sticker somewhere on the unit. Then, plug this id into a tool like [fccid.io](https://fccid.io/) to get the operating frequency range(s) (to assure that this is indeed an AMPS phone) and the power output of the phone in watts. 

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-04.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-04.jpg"></a><figquote>Searching for the FCC ID of the Radio Shack phone.</figquote></center></p>{% endraw %}  

All of my phones seem to operate at 3 watts (perhaps this was a standard) so I purchased [5 Watt BNC Dummy Loads](https://www.amazon.com/BECEN-Watt-BNC-Dummy-Load/dp/B08K2FRPC8/?th=1) for each phone. Some of my phones have a BNC connector which made this incredibly convenient, but others had different connector types like TNC which meant I had to purchase a [TNC Male to BNC Female Adapter](https://www.amazon.com/gp/product/B0932V73K3/) for that phone. There are phones with neither BNC or TNC connectors (some use mini UHF), so be sure to determine which connector the phone uses before getting an adapter (or just buy an assortment).

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-05.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-05-sm.jpg"></a><figquote>A pile of dummy loads and connectors.</figquote></center></p>{% endraw %}

Hand-held phones often do not have removable antennas, and I don't have a good way to limit their power output. Because of this, I don't use them. In my testing, operating an AMPS phone with a dummy load limits the use to the room that the LimeSDR Mini is in and I haven't observed the signals being able to travel through walls but this may not be the same for everyone. If you have access to a Faraday cage, that would be the safest place to operate your SDR and phones.

# Installing Osmocom-Analog {#installing-osmocom-analog}

The below commands are performed on a computer running Ubuntu Linux with a non-root, sudo user.

First, we need to install SoapySDR version 7 which is used to interface with the SDR hardware:

```
$ cd ~
$ sudo apt-get update
$ sudo apt-get install libusb-1.0-0-dev build-essential autoconf gcc make cmake libasound2-dev pkg-config git
$ wget https://github.com/pothosware/SoapySDR/archive/refs/tags/soapy-sdr-0.7.2.tar.gz
$ tar -xf soapy-sdr-0.7.2.tar.gz
$ cd SoapySDR-soapy-sdr-0.7.2 && mkdir build && cd build
$ cmake ..
$ make -j4 && sudo make install && sudo ldconfig
$ cd ../..
```

Next, we will install the LimeSuite library:

```
$ wget https://github.com/myriadrf/LimeSuite/archive/refs/tags/v20.10.0.tar.gz
$ tar -xf v20.10.0.tar.gz
$ cd LimeSuite-20.10.0
$ mkdir build && cd build
$ cmake ..
$ make -j4 && sudo make install && sudo ldconfig
$ cd ../..
```

Lastly, we will install Osmocom-Analog:

```
$ git clone git://git.osmocom.org/osmocom-analog
$ cd osmocom-analog
$ autoreconf -if
$ ./configure
$ make -j4
$ sudo make install
$ cd ..
```

That's it, we are ready to test out a phone!

# Calling Our Phone {#calling-our-phone}

We're ready to call our phone! Launch Osmocom-Analog with the following command:

```
$ sudo amps --limesdr-mini --channel 334 --channel 332 --fast-math --sdr-rx-gain -12 --sdr-tx-gain -6
```

The above command opens a control channel on channel 334 and a voice channel on channel 332. We can also enable a few options such as `--fast-math` which speeds up computation, `--sdr-rx-gain -12` which decreases our receive power, and `--sdr-tx-gain -6` which decreases our transmit power.

After a few seconds of loading, we will be presented with an ASCII graphic and a log of cellular traffic will start to appear. At this point we can power up our phone. The "Roam" light on the phone should go on as we are not recognized as the phone's original service area. Within a few seconds, we should see some logs in the console as the phone is registering and the phone's number will be printed. If we enter the phone number in the console followed by the `d` key, Osmocom-Analog will call the phone!

Soon the phone should start ringing and if we answer it we should hear some music from *Computer Chronicles*.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-06.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-06-sm.jpg"></a><figquote>Calling the Panasonic phone from the Osmocom console.</figquote></center></p>{% endraw %}

To hangup, press the `h` key in the console or hangup from the phone itself.

Below is an example log of making a call to a phone and hanging up from the console:

```
$ sudo amps --limesdr-mini --channel 334 --channel 332 --fast-math --sdr-rx-gain -12 --sdr-tx-gain -6
options.c  118 options-info   : Config file '/root/.osmocom/analog/amps.conf' seems not to exist, using command line options only.
options.c  284 options-info   : Command line option '--limesdr-mini'
options.c  284 options-info   : Command line option '--sdr-soapy'
options.c  282 options-info   : Command line option '--sdr-device-args', parameter 'driver=lime'
options.c  282 options-info   : Command line option '--sdr-rx-antenna', parameter 'LNAW'
options.c  282 options-info   : Command line option '--sdr-tx-antenna', parameter 'BAND2'
options.c  282 options-info   : Command line option '--sdr-rx-gain', parameter '25'
options.c  282 options-info   : Command line option '--sdr-tx-gain', parameter '30'
options.c  282 options-info   : Command line option '--sdr-samplerate', parameter '5000000'
options.c  282 options-info   : Command line option '--sdr-bandwidth', parameter '15000000'
options.c  268 options-info   : Command line option '-s' ('--samplerate'), parameter '200000'
options.c  282 options-info   : Command line option '--channel', parameter '334'
options.c  282 options-info   : Command line option '--channel', parameter '332'
options.c  284 options-info   : Command line option '--fast-math'
options.c  282 options-info   : Command line option '--sdr-rx-gain', parameter '-12'
options.c  282 options-info   : Command line option '--sdr-tx-gain', parameter '-6'
Selected System ID (SID) 40 belongs to:
Salinas, CA (GTE Mobile)
San Francisco, CA (GTE Mobile)
San Jose, CA (GTE Mobile)
San Rosa, CA (GTE Mobile)
Santa Barbara, CA (GTE Mobile)
jitter.c  142 jitter-info   : (334) Reset jitter buffer.
jitter.c  121 jitter-info   : (334) Created jitter buffer. (samplerate=8000, target_window=50ms, max_window=1000ms, flag:latency=true flag:repeat=true)
jitter.c  142 jitter-info   : (334) Reset jitter buffer.
jitter.c  121 jitter-info   : (334) Created jitter buffer. (samplerate=200000, target_window=50ms, max_window=1000ms, flag:latency=true flag:repeat=true)
amps.c  683 amps-info   : (chan 334) Entering IDLE state, sending Overhead/Filler frames on combined control & paging channel.
dsp.c  936 dsp-info   : (chan 334) Change mode to FOCC
amps.c  636 amps-notice : Created channel #334 (System B) of type 'CC/PC' = combined control & paging channel
Base station on channel 334 ready (combined control & paging channel), please tune transmitter to 880.020 MHz and receiver to 835.020 MHz. (-45.000 MHz offset)
amps.c  556 amps-notice : Channel number 332 belongs to a control channel, but your channel type 'voice channel' requires to be on a voice channel number. Some phone may reject this, but all my phones don't.
jitter.c  142 jitter-info   : (332) Reset jitter buffer.
jitter.c  121 jitter-info   : (332) Created jitter buffer. (samplerate=8000, target_window=50ms, max_window=1000ms, flag:latency=true flag:repeat=true)
jitter.c  142 jitter-info   : (332) Reset jitter buffer.
jitter.c  121 jitter-info   : (332) Created jitter buffer. (samplerate=200000, target_window=50ms, max_window=1000ms, flag:latency=true flag:repeat=true)
amps.c  690 amps-info   : (chan 332) Entering IDLE state (sending silence / no RF) on voice channel.
dsp.c  954 dsp-info   : (chan 332) Change mode from FVC to OFF
amps.c  636 amps-notice : Created channel #332 (System A) of type 'VC' = voice channel
Base station on channel 332 ready (voice channel), please tune transmitter to 879.960 MHz and receiver to 834.960 MHz. (-45.000 MHz offset)
sdr.c  168 sdr-info   : Require bandwidth of each channel is 2 * (8.0 deviation + 10.0 modulation) = 36.0 KHz
sdr.c  345 sdr-info   : We move center freqeuency between the two channels in the middle, to prevent them from overlap with DC level.
sdr.c  133 sdr-info   : TX Spectrum:
                           2                       1                          
---------------------------------------+---------------------------------------
sdr.c  135 sdr-info   : Frequency 1 = 880.0200 MHz
sdr.c  135 sdr-info   : Frequency 2 = 879.9600 MHz
sdr.c  357 sdr-info   : Total bandwidth (two side bands) for all TX Frequencies: 96000 Hz
sdr.c  365 sdr-info   : Using center frequency: TX 879.990000 MHz
sdr.c  391 sdr-info   : Using gain: TX -6.0 dB
sdr.c  457 sdr-info   : We move center freqeuency between the two channels in the middle, to prevent them from overlap with DC level.
sdr.c  133 sdr-info   : RX Spectrum:
                           2                       1                          
---------------------------------------+---------------------------------------
sdr.c  135 sdr-info   : Frequency 1 = 835.0200 MHz
sdr.c  135 sdr-info   : Frequency 2 = 834.9600 MHz
sdr.c  469 sdr-info   : Total bandwidth (two side bands) for all RX Frequencies: 96000 Hz
sdr.c  477 sdr-info   : Using center frequency: RX 834.990000 MHz
sdr.c  492 sdr-info   : Using gain: RX -12.0 dB
sdr.c  525 sdr-info   : Using local oscillator offseet: -1000000 Hz
soapy.c  109 soapy-info   : Using device args "driver=lime"
soapy.c  113 soapy-info   : Using stream args ""
soapy.c  117 soapy-info   : Using tune args ""
[INFO] Make connection: 'LimeSDR Mini [USB 2.0] 1D4C3927379912'
[INFO] Reference clock 40.00 MHz
[INFO] Device name: LimeSDR-Mini
[INFO] Reference: 40 MHz
[INFO] LMS7002M register cache: Disabled
[INFO] RX LPF configured
[INFO] RX LPF configured
[INFO] Filter calibrated. Filter order-4th, filter bandwidth set to 5 MHz.Real pole 1st order filter set to 2.5 MHz. Preemphasis filter not active
[INFO] TX LPF configured
soapy.c  371 soapy-notice : Given TX gain -6.000 is not supported, we use -6.021
[INFO] Filter calibrated. Filter order-4th, filter bandwidth set to 15 MHz.Real pole 1st order filter set to 2.5 MHz. Preemphasis filter not active
[INFO] TX LPF configured

                                                      |   |
           |                                         /|===|\
         -=O=-                                      / |\ /| \               ~
           |                    AMPS               /  | X |  \      ~
                                                  /   |===|   \
                                                 /    |   |\    \
                                                /    /|   | \    \       ~
              |--|                            /    /  |===|  \    \
             /|\/|\                          /    /   |   |    \    \
            / |--| \                        /    /    |   |     \    \
           /  |  |\  \                    /    /      |===|       \    \
__        /  /|--|  \  \                /    /        |   |         \    \
  \_    /  /  |  |    \  \            /   /           |   |           \    \
    \ /  /    |  |       \  \______/   /              |   |             \|   \|
     \=========================================================================
      \_______|/\|______        _***_*__**_***___**_*_| X |              |MMMM|
         \__  |\/|      \______/*  **   *  * * **  *  |/ \|______________|MMMM|
 _____      \_|/\| *    *     *  *  ** * *  * *  *  * |\ /|*____/  ~    ~ ~   ~
/ ~  ~\_______I__I_________*______*_*___*__*___*__*_*_| X |/   ~  ~   _________
_______\ _      _  _    _       _ _   _ _    _    _   |/_\/  ~ ______/
 _         _         _        _   _      _      _   _/~ ~ ____/  ~       ~   ~
_  _    _   _          _        _         _        /_____/     ~   ____________
__/~\______________  _     _         _        _  _/ ~ ~ __________/  ~  ~   ~
 ~      ~   ~      \____________________________/      /  ~   ~      ~    ~  ~

[ERROR] Tx Calibration: MCU error 5 (Loopback signal weak: not connected/insufficient gain?)
[INFO] Rx calibration finished
soapy.c  543 soapy-error  : SDR RX overflow, seems we are too slow. Use lower SDR sample rate, if this happens too often.
frame.c 2722 frame-info   : Transmit: Word 1 - System Parameter Overhead
frame.c 2722 frame-info   : Transmit: Word 2 - System Parameter Overhead
frame.c 2722 frame-info   : Transmit: Registration ID Message
frame.c 2722 frame-info   : Transmit: Registration Increment Global Action Message
frame.c 2722 frame-info   : Transmit: Access Type Parameters Global Action Message
frame.c 2722 frame-info   : Transmit: Control-Filler Message
frame.c 3514 frame-info   : (chan 334) Subsequent system/filler frames are not show, to prevent flooding the output.
frame.c 3759 dsp-info   : (chan 334) RX Level: 133% Quality: 46% Polarity: NEGATIVE
frame.c 3759 dsp-info   : (chan 334) RX Level: 53% Quality: 85% Polarity: POSITIVE
frame.c 3728 frame-info   : (chan 334) RX RECC: DCC=0 (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word A - Abbreviated Address Word
frame.c 3336 frame-info   : (chan 334) expecting 2 more word(s) to come
frame.c 3736 frame-info   : (chan 334) RX RECC: (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word B - Extended Address Word
frame.c 3336 frame-info   : (chan 334) expecting 1 more word(s) to come
frame.c 3736 frame-info   : (chan 334) RX RECC: (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word C - Serial Number Word
frame.c 3336 frame-info   : (chan 334) expecting 0 more word(s) to come
amps.c  850 amps-info   : (chan 334) Registration 2159208040 (ESN = 88022f39, Class 1 / Continuous / 20 MHz, TIA/EIA-553 or IS-54A mobile station)
amps.c  856 amps-info   : (chan 334)  -> Home country: USA
amps.c  858 amps-info   : (chan 334)  -> Home number: 2159208040
transaction.c  257 transaction-info   : Transaction state IDLE -> REGISTER ACK
transaction.c  162 transaction-info   : Created transaction for subscriber '2159208040'
amps.c 1206 amps-info   : (chan 334) Sending Register acknowledge
transaction.c  257 transaction-info   : Transaction state REGISTER ACK -> REGISTER ACK SEND
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
transaction.c  175 transaction-info   : Destroying transaction for subscriber '2159208040'
transaction.c  257 transaction-info   : Transaction state REGISTER ACK SEND -> IDLE
frame.c 3759 dsp-info   : (chan 334) RX Level: 144% Quality: 46% Polarity: NEGATIVE
console.c  493 cc-info   : Outgoing call to '2159208040'
endpoint.c  963 cc-info   : Handle message CC-SETUP-REQ at state IDLE (callref 1)
message.c  471 cc-info   :   IE_CALLED type=0(unknown) plan=1(telephony) number='2159208040'
message.c  531 cc-info   :   IE_BEARER coding=0(ITU-T) capability=16(audio) mode=0(circuit)
message.c  585 cc-info   :   IE_SDP payload=v=0\no=- 3892817989 3892817989 IN IP4 127.0.0.1\ns=-\nc=IN IP4 127.0.0.1\nt=0 0\nm=audio 16384 RTP/AVP 96\na=rtpmap:96 L16/8000\n
call.c  744 call-info   : Received OSMO-CC call from fixed network '' to mobile '2159208040'
call.c  441 call-info   : Indicate OSMO-CC call confirm towards fixed network
endpoint.c  963 cc-info   : Handle message CC-PROC-IND at state INIT-OUT (callref 1)
message.c  585 cc-info   :   IE_SDP payload=v=0\no=- 3892817989 3892817989 IN IP4 127.0.0.1\ns=-\nc=IN IP4 127.0.0.1\nt=0 0\nm=audio 16386 RTP/AVP 96\na=rtpmap:96 L16/8000\n
message.c  555 cc-info   :   IE_PROGRESS coding=0(ITU-T) location=10(beyond interworking) progress=8(inmand information available (audio))
call.c  751 call-info   : Outgoing call from '' to '2159208040'
amps.c  971 amps-info   : (chan 334) Call to mobile station, paging station id '2159208040'
transaction.c  257 transaction-info   : Transaction state IDLE -> PAGE
transaction.c  162 transaction-info   : Created transaction for subscriber '2159208040'
amps.c 1246 amps-info   : (chan 334) Paging the phone
transaction.c  257 transaction-info   : Transaction state PAGE -> PAGE SEND
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC == 11)
transaction.c  257 transaction-info   : Transaction state PAGE SEND -> PAGE REPLY
frame.c 3759 dsp-info   : (chan 334) RX Level: 55% Quality: 85% Polarity: POSITIVE
frame.c 3728 frame-info   : (chan 334) RX RECC: DCC=0 (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word A - Abbreviated Address Word
frame.c 3336 frame-info   : (chan 334) expecting 2 more word(s) to come
frame.c 3736 frame-info   : (chan 334) RX RECC: (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word B - Extended Address Word
frame.c 3336 frame-info   : (chan 334) expecting 1 more word(s) to come
frame.c 3736 frame-info   : (chan 334) RX RECC: (5 of 5 CRCs are ok)
frame.c 3073 frame-info   : Received: Word C - Serial Number Word
frame.c 3336 frame-info   : (chan 334) expecting 0 more word(s) to come
amps.c  871 amps-info   : (chan 334) Paging reply 2159208040 (ESN = 88022f39, Class 1 / Continuous / 20 MHz, TIA/EIA-553 or IS-54A mobile station)
transaction.c  257 transaction-info   : Transaction state PAGE REPLY -> MT CALL ASSIGNMENT
amps.c 1233 amps-info   : (chan 334) Assigning channel to call to mobile station
transaction.c  257 transaction-info   : Transaction state MT CALL ASSIGNMENT -> MT CALL ASSIGNMENT SENDING
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC != 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC != 11)
frame.c 2722 frame-info   : Transmit: Word 1 - Abbreviated Address Word
frame.c 2722 frame-info   : Transmit: Word 2 - Extended Address Word (SCC != 11)
amps.c 1171 amps-info   : Moving to voice channel 332
amps.c 1239 amps-info   : (chan 334) Assignment complete, waiting for SAT on VC
transaction.c  257 transaction-info   : Transaction state MT CALL ASSIGNMENT SENDING -> MT CALL ASSIGNMENT WAIT CONFIRM
dsp.c  949 dsp-info   : (chan 332) Change mode from OFF to FVC
amps.c  800 amps-info   : Detected SAT signal with quality=99.
amps.c  815 amps-info   : (chan 332) Confirm from mobile (SAT) received
transaction.c  257 transaction-info   : Transaction state MT CALL ASSIGNMENT WAIT CONFIRM -> MT CALL ALERT
amps.c 1283 amps-info   : (chan 332) Sending alerting
transaction.c  257 transaction-info   : Transaction state MT CALL ALERT -> MT CALL ALERT SENDING
frame.c 2722 frame-info   : Transmit: Mobile Station Control Message Word 1 (SCC == 11)
amps.c 1289 amps-info   : (chan 332) Alerting was sent, continue waiting for ST or timeout
transaction.c  257 transaction-info   : Transaction state MT CALL ALERT SENDING -> MT CALL ALERT WAIT CONFIRM
dsp.c  771 dsp-notice : (chan 332) SAT level 105.21% quality 100%
amps.c  729 amps-info   : (chan 332) Detected Signaling Tone with quality=100.
call.c  526 call-info   : Call is alerting
call.c  451 call-info   : Indicate OSMO-CC alerting towards fixed network
endpoint.c  963 cc-info   : Handle message CC-ALERT-IND at state PROCEEDING-OUT (callref 1)
console.c  287 cc-info   : Call alerting
transaction.c  257 transaction-info   : Transaction state MT CALL ALERT WAIT CONFIRM -> MT CALL ANSWER WAIT
dsp.c  771 dsp-notice : (chan 332) SAT level 105.35% quality 99%
dsp.c  771 dsp-notice : (chan 332) SAT level 105.36% quality 100%
console.c  522 cc-info   : Call hangup  
endpoint.c  963 cc-info   : Handle message CC-REL-REQ at state ALERTING-OUT (callref 1)
message.c  567 cc-info   :   IE_CAUSE location=0(user) isdn_cause=16(normal call clearing) sip_cause=0 socket_cause=0(unset)
endpoint.c  800 cc-info   : Changing message to CC-DISC-REQ.
```

# Operating Two (Or More) Phones {#operating-two-phones}

If we have multiple phones, we can have them call one another by slightly modifying the `amps` command:

```
$ amps --limesdr-mini --channel 334 --channel 332 --channel 330 --fast-math --sdr-rx-gain -12 --sdr-tx-gain -6 -s 500000 -x
```

Here, we have added another channel, channel 330, which is used as an additional voice channel for a second phone. Remember that we will always need one more channel than the number of phones we have since a control channel is always needed. Additionally, we have added options `-s 500000` to explicitly set the sample-rate to accommodate another channel, and `-x` which enables cross-connect so the phones can call one another.

Just as we did previously, we can run the command and then power up both phones. The logs in the console should report phone numbers for each phone, and we can direct-dial one phone from the other to make them ring and hold a conversation!

{% raw %}<p><center><iframe title="PhilTel Cellular Demonstration" src="https://diode.zone/videos/embed/71fa4a49-bc12-4ea0-baab-ae320b3caf84" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups" width="736" height="414" frameborder="0"></iframe><br><figquote>Demonstration of one phone calling the other.</figquote></center></p>{% endraw %} 

# Interface an AMPS Phone with SIP {#interface-with-sip}

Sure, calling one AMPS phone with another is fun, but it would be more fun to make/place actual phone calls to the PSTN.

This assumes that we have a SIP endpoint to connect to for placing/receiving calls. This could by an Asterisk PBX, or a SIP provider like Voip.ms. Setting up a PBX or registering with a SIP provider is out-of-scope here, and the below assumes a SIP server address, username, and secret are already known for connection.

Before we can dive right into connecting, we need to install an additional piece of software [Osmocom Call Control](https://osmocom.org/projects/osmo-cc), a protocol-agnostic way of interfacing call control of various different protocols with each other, and its dependency Sofia-SIP, an open-source SIP User-Agent library.

First, we will install Sofia-SIP:

```
$ cd ~
$ sudo apt install make gcc
$ wget http://download.eversberg.eu/sofia/sofia-sip-1.12.11.tar.gz
$ tar -xzf sofia-sip-1.12.11.tar.gz
$ cd sofia-sip-1.12.11/
$ ./configure CFLAGS=-O0
$ make
$ sudo make install
$ sudo ldconfig
```

Next we can install Osmocom Call Control:

```
$ sudo apt install aclocal m4 autom4te
$ git clone git://git.osmocom.org/cc/osmo-cc-sip-endpoint
$ cd osmo-cc-sip-endpoint
```

I had an issue here where I needed to manually modify `configure.ac` and comment out the line `AC_CONFIG_MACRO_DIR([m4])` due to a duplicate definition error. You only need to comment this out if you receive an error running `autoreconf --install` below. Others didn't report having this issue.

```
$ nano configure.ac
...
AC_CONFIG_MACRO_DIRS([m4])
#AC_CONFIG_MACRO_DIR([m4])
...
```

Now we can proceed:

```
$ autoreconf --install
$ ./configure
$ make
$ sudo make install
```

Finally we can run Osmocom Call Control.

Below is an example connecting to a PBX on the LAN. Note that `192.168.1.22` is our internal IP address, `192.168.1.33` is the internal IP address of our PBX, `lamepassword` is our secret, and `login` is our username.

```
$ osmo-cc-sip-endpoint --local 192.168.1.22 --remote 192.168.1.33 -R login@192.168.1.33 -A login lamepassword 192.168.1.33 -v 0 *
```

Now in a new console window, we will run Osmocom-Analog with the options `-o --cc "rtp-peer 192.168.1.22"` so it connects to the call control service running on our local machine.

```
$ sudo amps --limesdr-mini --channel 334 --channel 332 --fast-math --sdr-rx-gain -12 --sdr-tx-gain -62 -o --cc "rtp-peer 192.168.1.22"
```

Now we will be able to interface with the PBX as though we were any other phone on the system.

{% raw %}<p><center><iframe title="SIP Call with an AMPS Cellular Phone" src="https://diode.zone/videos/embed/ab0d57c5-040b-4d23-8862-7a99a1e65482" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups" width="736" height="414" frameborder="0"></iframe><br><figquote>Making a SIP call with an AMPS phone.</figquote></center></p>{% endraw %} 

If our SIP endpoint is NOT within our local network, we will need to modify the Osmocom Call Control command to use the `-P` option to specify our public IP address. For example, if our endpoint was `newyork.voip.ms` and our public IP address was `1.2.3.4` (and all other information was the same), our new command would be:

```
$ osmo-cc-sip-endpoint --local 192.168.1.22 --remote newyork.voip.ms -R login@newyork.voip.ms -A login lamepassword newyork.voip.ms -P 1.2.3.4 -v 0 *
```

# Make A Phone Portable Again {#make-a-phone-portable-again}

As previously mentioned we can run these phones off of a bench power supply, but what if we wanted to take one on-the-go? 

Many of these phones (especially the ones that come in soft bags) seem to make use of a common form-factory lead acid battery utilized by '90s camcorders. They are still for sale today, such as the [Raion Power 12V 2.3Ah Camcorder Battery](https://www.ebay.com/itm/224240998547) which is a perfect replacement.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-07.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-07-sm.jpg"></a><figquote>The camcorder battery slides right into the Panasonic phone.</figquote></center></p>{% endraw %}

For a more "universal" (though clumsy) option, we can purchase several [ExpertPower 12 Volt 2.3 Ah Rechargeable batteries](https://www.amazon.com/gp/product/B000LQDN52/) along with [2x 12V Female Socket cables](https://www.amazon.com/gp/product/B08KW5NX7C/) and a pack of [Nylon Spade Quick Disconnect Connectors] to fit tit the F2 connectors on the batteries. From here, we cut the lugs off of the cables, crimp on the quick disconnect connectors, and connect the cable to the battery. Now, any phone's lighter adapter can be plugged into the socket cable and receive power from the battery. Of course, we will also need a lead-acid battery charger. The [NOCO GENIUS1, 1A Smart Car Battery Charger](https://www.amazon.com/gp/product/B07W46BX31/) is a fairly inexpensive option that works well to charge the batteries, but many trickle chargers should do the job. Do NOT use a high-amperage charger here, it will ruin the batteries.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-08.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-08-sm.jpg"></a><figquote>The battery setup successfully powering the Panasonic phone.</figquote></center></p>{% endraw %}

For other phones we may be able to substitute internal batteries if we can find suitable replacements in the proper dimensions. For example, a Radio Shack 17-002 used two 6V batteries originally manufactured by Radio Shack. There isn't a direct equivalent but these [Power Sonic PS-612 6V 1.4AH SLA batteries](https://www.amazon.com/gp/product/B09DDK2G59/) fit into the battery compartment of the phone and use the same F1 connectors the original batteries did.

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-09.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-09-sm.jpg"></a><figquote>New batteries in the Radio Shack phone. The fuse and fuse holder were also replaced.</figquote></center></p>{% endraw %}

Of course, we could likely replace batteries in these phones with Lithium-Ion equivalents, but sticking to the original chemistry means that the original charging circuitry can be used without modification. Always remember to stick to **sealed** lead-acid batteries so they can be used in any orientation if going with lead-acid chemistry.

# Eavesdrop on Phone Calls {#eavesdrop-on-phone-calls}

In the early 1990's it was really common for technically-savvy people to eavesdrop on cellular calls with completely off-the-shelf hardware. Oftentimes, radio scanners were leveraged as they were already the de facto devices for moniroing on other radio transmissions. While most scanners of the time would explicitly block out the cellular telephone range, there was one popular model that could easily be modified to allow receipt of cellular traffic with no tools required and no need to even open the device!

This scanner was the [Radio Shack PRO-51](https://rigreference.com/documents/rr-5eb71c9c1e15e4.34936870), a compact and portable handheld scanner.

To receive AMPS cellular frequencies with the PRO-51, we need to put it into "Test Mode 1" by holding the `2` key, `9` key, and `L/OUT` key while powering on the scanner. This will fill channels 1-25 with test frequencies. We want to check out channel 23 which is now programmed with 888.960MHz, right in the range for cellular traffic! After powering on the scanner, we can navigate to this channel by pressing the `MANUAL` button repeatedly until reaching `23`. Then, using the `▲` and `▼` buttons we can step in proper 30kHz increments to our desired frequency. For precision, it helps if Squelch is opened when stepping to the frequency.

{% raw %}<p><center><iframe title="Eavesdropping on AMPS Cellular Calls" src="https://diode.zone/videos/embed/32056644-a02a-44e9-8908-2dfd3e13a9cc" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups" width="736" height="414" frameborder="0"></iframe><br><figquote>Listening in on an AMPS call.</figquote></center></p>{% endraw %} 

Keep in mind that when intercepting phone calls, we will only hear one side of the conversation. AMPS phones transmit and receive on different frequencies so unless we have two scanners, we won't be able to hear both parties talking. Additionally, while we likely want to listen to the voice frequencies, we can also listen in on the control channel to hear (one side at a time of) the data the phone and transmitter send one another for handshaking, disconnection, etc. To know which frequencies we can tune to, we can check the Osmocom-Analog logs at startup to see the frequencies being utilized .

{% raw %}<p><center><a href="/assets/img/2023-05-11-reviving-amps-phones/amps-10.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-05-11-reviving-amps-phones/amps-10-sm.jpg"></a><figquote>The Radio Shack PRO-51 tuned to 879.960MHz. This is the audio coming from the base station (What the AMPS phone user hears).</figquote></center></p>{% endraw %}

Of course, after a while the FCC figured out people were doing this and in 1994 they forced Radio Shack to provide a fix for this test mode so that the cellular frequencies were once again inaccessible. From [anecdotal information I have gathered](https://docplayer.net/125651608-Modifications-for-the-radio-shack-pro-51.html), any scanner manufactured in August of 1994 or later will be updated to fully block cellular frequencies. However, there are reports that Uniden-manufactured Radio Shack scanners actually have a workaround for this where they can be put into "Test Mode 3" and then quickly put into "Test Mode 1" to restore cellular frequency access. [This guide](http://cd.textfiles.com/hackersencyc/RADIO/SCANNER/PRO_51.TST) goes into more details on the specifics, and also lets us know that the previously mentioned test mode trick potentially works for other Uniden-manufactured Radio Shack scanners like the PRO-23 and PRO-46. However, other manufacturers may have built PRO scanners for Radio Shack so it's possible that a scanner from August 1994 simply won't work for cellular. The easiest thing to do in order to verify if a scanner was made before the FCC-mandated update is to check the date code on the scanner's back label that will be in the format `MMAY`. For example, the scanner pictured above has the code `11A3` which means it was manufactured in November 1993. As long as we have a scanner with code `7A4` or earlier it should work fine!

# Conclusion {#conclusion}

AMPS is now a thing of the past, but it is fun to bring life back to the old hardware and once again make these phones usable.

In the future it may be interesting to explore [gr-amps](https://github.com/unsynchronized/gr-amps), a set of blocks for GNU Radio which emulate an AMPS base station, as an alternative to Osmocom-Analog.
