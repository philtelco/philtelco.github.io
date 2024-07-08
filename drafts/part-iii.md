Part III

The next step to getting XNet running was connecting up the modem. I have a Protel UPMS 1200 modem which was built specifically to program payphones via XNet. When I had asked online about whether or not I needed one of these modems to program my Protel 7000 board, many people told me that the 7000-series boards could only be programmed with the official modem and anything else simply wouldn’t work.

I’ve later learned that this isn’t true, but I can now understand where the confusion on this came from. A Protel 7000 series board has a modem that supports a 1200 baud connection, but unlike most modems which will use FSK modulation at 1200 baud (Bell 202), Protel boards use PSK modulation which makes them incompatible. Somehow, this morphed into the myth that the 7000-series boards could only be programmed at 1200 baud when in-fact they can also be programmed at 300 baud (standard Bell 102) if the software is configured to use a non-Protel modem.

At the time though, I didn’t know this. When an opportunity came up to get a UPMS 1200 modem I jumped on it. I figured best case scenario I would be able to use the UPMS to make sure that my setup worked and then experiment from that point to show definitively wether other modems would or wouldn’t work. Worst case scenario I still had all the “proper” components for a functioning setup.

Upon receiving the UPMS modem, I hooked up the phone line, wired the data port to the serial port on the thin client, and powered it up. Immediately I head a loud pop from the internal speaker and then a loud dial-tone. It seemed like the modem immediately went off-hook without being configured to do so. This wasn’t a good sign. Starting XNet will send some initialization configuration to the modem so I loaded it up and watched it open communication and send the init string, but the modem still wouldn’t give up the load dial-tone.

At this point I knew that the modem was broken, so I decided to open it up and investigate for any failures. Cosmetically, the inside of the unit looked fine and all of the components seemed to be intact. I noticed that the board had a handful of electrolytic capacitors so I decided to record the values of these and order replacements. This was only a few dollars worth of parts and luckily all of the capacitors are through-hole so replacement wouldn’t be too difficult.

When the capacitors arrived I went ahead and replaced each one on the board one-by-one with a soldering iron, desoldering vacuum tool, and solder wick over the course of 20-30 minutes.
