---
layout: post
title:  "SIM Unlock a Franklin T9 Hotspot"
date:   2022-11-08 20:40:00 -0400
categories: franklin t9 hotspot
---

In the PhilTel demo rig, we make use of an older Franklin T9 LTE hotspot for Internet access. These hotspots are widely available as they were given out for free by T-Mobile and are able to be unlocked and hacked to support multiple carriers provided they are operating in GSM bands.

{% raw %}<p><center><a href="/assets/img/2022-11-08-sim-unlock-a-franklin-t9-hotspot/franklint9.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-sim-unlock-a-franklin-t9-hotspot/franklint9.jpg"></a><figquote>The Franklin T9 RT717.</figquote></center></p>{% endraw %} 

Note that the below content is sourced heavily from the following two web pages:
* [Rooting and Unlocking the T-Mobile T9 (Franklin Wireless R717)](https://snt.sh/2020/09/rooting-the-t-mobile-t9-franklin-wireless-r717/) 
* [Rooting the T-Mobile T9 (Franklin Wireless R717) – Again!](https://snt.sh/2021/09/rooting-the-t-mobile-t9-franklin-wireless-r717-again/)

## Perform the SIM Unlock

To perform this unlock you will need a Linux/Mac machine and the IMEI of your hotspot. The IMEI is usually on a sticker inside the battery department, but it may also be on the web interface somewhere (I think the *About* page).

In a terminal on your computer, run the following commands, replacing `YOURIMEIGOESHERE` with the IMEI of your device:

```
export IMEI=YOURIMEIGOESHERE
echo -n "${IMEI}simlock" | sha1sum | cut -c1-8
```

After performing the above, a code should be output in the terminal.

Log in to your Franklin device (the web interface is usually at `192.168.0.1` and the password is usually either `admin` or `password`) and click on the *Settings* icon from the top navigation. Now from the left navigation, go to *Mobile Network* --> *SIM* and scroll down the the *Carrier Unlock* section. Under *Desired Action* select *Carrier Unlock* from the dropdown and under *Enter Unlock Code* enter the code you generated in the terminal earlier. Finally, press the button for *Unlock* to unlock your device. 