---
layout: post
title:  "Port Forward from the GL-AR300M16"
date:   2022-11-08 20:35:00 -0400
categories: openwrt ata router
---

We may want to get into our router or ATA remotely so it is a good idea to port forward necessary ports to our VPN network.

## Setting an IP Reservation

Navigate to the web interface at <http://192.168.1.1> and go to *Network* --> *DHCP and DNS*.

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-01.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-01.jpg"></a><figquote>DHCP and DNS.</figquote></center></p>{% endraw %} 

Select the tab for *Static Leases*, view the current DHCP leases in the *Active DHCP Leases* list. Identify the entry for the ATA (reference the *MAC address* field against the sticker on the bottom of the ATA) and note the MAC address before clicking on the *Add* button.
 
{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-02.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-02.jpg"></a><figquote>Static Leases.</figquote></center></p>{% endraw %} 

In the resulting popup, select the corresponding MAC address from the *MAC Address* dropdown. In the *IPv4 Address* dropdown, enter the IP address for the ATA you want (likely `191.168.1.2`). In the *Hostname* field enter in `ATA` and in the *Lease time* field enter `infinite`. Next, press the "Save" button. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-03.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-03.jpg"></a><figquote>Save the Addition.</figquote></center></p>{% endraw %} 

Then on the bottom of the *DHCP and DNS* page press the button for *Save & Apply*.

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-04.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-04.jpg"></a><figquote>Save & Apply.</figquote></center></p>{% endraw %} 

You may need to unplug/replug the ethernet cable for the ATA to have the new IP address used.

## Set up Port Forwarding for One Port

Go to *Network* --> *Firewall*.

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-05.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-05.jpg"></a><figquote>Firewall.</figquote></center></p>{% endraw %} 

Navigate to the *Port Forwards* tab and press the button for *Add*. 

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-06.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-06.jpg"></a><figquote>Add Port Forward.</figquote></center></p>{% endraw %} 

On the *General Settings* tab of the resulting dialog, enter:
* `ata_http` into the *Name* field
* `TCP` into the *Protocol* field
* `philtel_fw` in the *Source zone* field
* `8080` in the *External port* field
* `lan` in the *Destination zone* field
* The IP address of the ATA (likely `192.168.1.2`) in the *Internal IP address* field
* `80` in the *Internal port* field. 

Then press the *Save* button.

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-07.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-07.jpg"></a><figquote>Save Forward.</figquote></center></p>{% endraw %} 

Finally on the bottom of the *Port Forwards* page press the button for *Save & Apply*.

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-08.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-08.jpg"></a><figquote>Save & Apply.</figquote></center></p>{% endraw %} 

Now, from the VPN server you should be able to remotely access the ATA's web interface through port `8080` on the internal VPN `10.8.0.x` IP address if needed.

## More Port Forwarding

We should repeat the steps above to port foward more services for the ATA and the router itself. Create one entry in *Port Fowards* for each item in the table below (except the first one which you just did):

| Name              | Protocol | Source zone | External port | Destination zone | Internal IP address | Internal port |
| ----------------- | -------- | ----------- | ------------- | ---------------- | ------------------- | ------------- |
| ata_http          | TCP      | philtel_fw  | 8080          | lan              | 192.168.1.2         | 80            |
| ata_https         | TCP      | philtel_fw  | 4443          | lan              | 192.168.1.2         | 443           |
| ata_ssh           | TCP      | philtel_fw  | 2222          | lan              | 192.168.1.2         | 22            |
| router_http       | TCP      | philtel_fw  | 80            | lan              | 192.168.1.1         | 80            |
| router_https      | TCP      | philtel_fw  | 443           | lan              | 192.168.1.1         | 443           |
| router_ssh        | TCP      | philtel_fw  | 22            | lan              | 192.168.1.1         | 22            |
| router_prometheus | TCP      | philtel_fw  | 9100          | lan              | 192.168.1.1         | 9100          |

{% raw %}<p><center><a href="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-09.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-port-forward-from-the-glar300m16/3-gl-ar300m16-port-forwarding-09.jpg"></a><figquote>All Port Forwards.</figquote></center></p>{% endraw %} 