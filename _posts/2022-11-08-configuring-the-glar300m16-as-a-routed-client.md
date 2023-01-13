---
layout: post
title: "Configuring the GL-AR300M16 as a Routed Client"
date:   2022-11-08 20:20:00 -0400
categories: openwrt router glinet
---

In some cases, the WAN connection for the router may not be provided via ethernet cable and a WWAN connection will need to be performed so the router can get Internet access via an existing wireless network at the installation site.

## Configure the Network

Login to the router at <https://192.168.1.1/> and proceed to *Network* --> *Wireless*. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-01.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-01.jpg"></a><figquote>Navigate to Wireless Settings.</figquote></center></p>{% endraw %} 

Here you will see an entry for `radio0,` now press the *Scan* button to get a list of available wireless networks. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-02.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-02.jpg"></a><figquote>Scan for networks.</figquote></center></p>{% endraw %} 

Find the one you would like to use and press the *Join Network* button next to it.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-03.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-03.jpg"></a><figquote>Select a network.</figquote></center></p>{% endraw %} 

On the *Joining Network* dialog, check the box for *Replace wireless configuration*, ensure the network name is `wwan` and enter the passphrase to access the network. In *Create / Assign firewall-zone*, be sure that the `wan` zone is selected before pressing the *Submit* button. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-04.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-04.jpg"></a><figquote>Join the network.</figquote></center></p>{% endraw %} 

On the following dialog the defaults should be enough to establish connection, so press the *Save* button. In some cases you may need to edit the *Wireless Security* section to ensure the proper encryption is set.  

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-05.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-05.jpg"></a><figquote>Save the network.</figquote></center></p>{% endraw %} 

NOTE: If you want to have multiple WAN networks, repeat the above steps but then go to *Interfaces* --> *Add new interface* and in the resulting dialog supply a unique network `Name` and choose your newly created interface under `Device` before pressing the *Create Interface* button. Afterwards, press the *Save & Apply* button. You may need to restart the WireGuard interface after this step.  

After the dialog closes, press the *Save & Apply* button and after a few seconds of waiting the router should connect to the network and display connection status in the *Associated Stations* section of the page.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-06.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-06.jpg"></a><figquote>Save and apply the changes.</figquote></center></p>{% endraw %} 