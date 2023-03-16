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

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-02.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-02.jpg"></a><figquote>Scan for Networks.</figquote></center></p>{% endraw %} 

Find the one you would like to use and press the *Join Network* button next to it.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-03.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-03.jpg"></a><figquote>Select a Network.</figquote></center></p>{% endraw %} 

On the *Joining Network* dialog, check the box for *Replace wireless configuration*, ensure the network name is `wwan` and enter the passphrase to access the network. In *Create / Assign firewall-zone*, be sure that the `wan` zone is selected before pressing the *Submit* button. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-04.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-04.jpg"></a><figquote>Join the Network.</figquote></center></p>{% endraw %} 

On the following dialog the defaults should be enough to establish connection, so press the *Save* button. In some cases you may need to edit the *Wireless Security* section to ensure the proper encryption is set.  

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-05.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-05.jpg"></a><figquote>Save the Network.</figquote></center></p>{% endraw %} 

After the dialog closes, press the *Save & Apply* button and after a few seconds of waiting the router should connect to the network and display connection status in the *Associated Stations* section of the page.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-06.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-06.jpg"></a><figquote>Apply Changes.</figquote></center></p>{% endraw %} 

## Add a Second Network

If you want to have multiple WWAN networks defined, proceed to *Network* --> *Wireless*. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-01.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-01.jpg"></a><figquote>Navigate to Wireless Settings.</figquote></center></p>{% endraw %} 

Scroll down to any currently enabled WWAN network and press the *Disable* button next to it.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-07.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-07.jpg"></a><figquote>Disable.</figquote></center></p>{% endraw %} 

Now press the *Scan* button next to the entry for `radio0`.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-07.5.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-07.5.jpg"></a><figquote>NScan.</figquote></center></p>{% endraw %} 

Find the new wireless network you want to connect to and press the *Join Network* button to the right of it.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-08.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-08.jpg"></a><figquote>Join Network.</figquote></center></p>{% endraw %} 

In the resulting dialog, enter a name in the `Name of the new network` field and enter the network password in the `WPA passphrase` field before pressing the *Submit* button.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-09.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-09.jpg"></a><figquote>New Network.</figquote></center></p>{% endraw %} 

In the next dialog, press the *Save* button.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-10.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-10.jpg"></a><figquote>Save Network.</figquote></center></p>{% endraw %} 

Back on the *Wireless Overview* page, press the *Save & Apply* button.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-11.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-11.jpg"></a><figquote>Save & Apply.</figquote></center></p>{% endraw %} 

The new network should connect!

If you already have OpenVPN running during this step, you will want to either power cycle the router, or by stopping and starting the OpenVPN connection. To do the latter, navigate to *VPN* --> *OpenVPN*.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-12.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-12.jpg"></a><figquote>OpenVPN.</figquote></center></p>{% endraw %} 

On the OpenVPN page, press the *stop* button in the row for our OpenVPN instance. After it has stopped, press the *start* button that will replace it in the same row.

{% raw %}<p><center><a href="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-13.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-configuring-the-glar300m16-as-a-routed-client/1-gl-ar300m16-routed-client-13.jpg"></a><figquote>Stop & Start.</figquote></center></p>{% endraw %} 