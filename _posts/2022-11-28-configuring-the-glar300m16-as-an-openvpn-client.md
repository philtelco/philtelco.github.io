---
layout: post
title: "Configuring the GL-AR300M16 as an OpenVPN Client"
date:   2022-11-28 00:01:00 -0400
categories: openwrt router glinet
---

*Note: The following guide has been deprecated in our configuration as we have switched to using OpenVPN. OpenVPN is supported natively by our ATAs, so in some installations, we may not even need a router installed*

Now that [we have our router connected to the WAN]({{ site.baseurl }}/2022/11/09/configuring-the-glar300m16-as-a-routed-client.html) and [the OpenVPN server is installed]({{ site.baseurl }}/2022/11/09/configuring-an-openvpn-server.html), we need to install OpenVPN on our router to allow it to access the VPN.

## Installing OpenVPN

`ssh` into your router at `192.168.1.1`. 

Note: Make sure you set a password for root before doing this step, you will not be able to perform an update until you do.

```
$ ssh root@192.168.1.1
```

Next, perform an update and install OpenVPN packages and reboot:

```
# opkg update
# opkg install openvpn-openssl luci-app-openvpn
# reboot
```

## Configuration

After a few minutes the router should be rebooted. Navigate to the web interface at http://192.168.1.1 and go to  *VPN* --> *OpenVPN*. Then in the *OVPN configuration file upload* section, press the *Browse...* button to select our `.ovpn` file and then in the *instance name* field enter `philtel_iffybooks`. Now press the *Upload* button. The page should refresh with our new configuration now showing in the *OpenVPN instances* list.

In the list, check the box for *Enabled* next to our instance and press the button for *Save & Apply*. After the page refreshes, press the *Start* button next to our instance and the list should update to show the instance running.

## Configuring the Firewall

We want to pass all traffic through OpenVPN and also allow connections direct to the OpenVPN server so we must make changes in the firewall.

Navigate to *Network* --> *Firewall* and press the *Add* button in the *Zones* section of the page.

In the *Name* field enter `philtel_fw`. From the *Input* dropdown choose `Reject`, from the *Output* dropdown choose `Accept`, and from the *Forward* dropdown choose `Reject`.

Check the boxes for *Masquerading* and *MSS clamping*. In *Covered Networks* select `unspecified`. For *Allow Forward to destination zones* leave this as `unspecified` but for *Allow forward from source zones* select `lan`.

Press the button for *Save* and then on the *Firewall* page press the button for *Save & Apply*.

## Configuring an Interface

Now we need to set up an interface for our OpenVPN adapter so we can add it to the our new `philtel_fw` zone and pass Internet traffic.

Click on *Network* --> *Interfaces*. Next, press the button *Add new Interface…* and fill the form with the following values: *name* = `tun0`, *Protocol* = `Unmanaged`, *Interface* = `tun0`. Then press the *Create Interface* button. When the page reloads, press the *Edit* button for the interface we just created.

In the resulting popup, in the *General Settings* panel, deselect the checkbox for *Bring up on boot*. In panel for *Firewall Settings*, set *Assign Firewall-zone* to `philtel_fw` and then press the *Save* button. Finally, press the *Save & Apply*. Then, reboot the router.

Afterwards from any LAN client connected to the router should be able to ping a public IP and the internal IP of our OpenVPN server:

```
> ping 1.1.1.1
> ping 10.8.0.1
```

## Create a Reset Script

It appears that there is an issue in newer versions of OpenWRT where when there is a network drop/hiccup on the WAN, a VPN connection does not gracefully recover. To work around this, I have created a script that will ping a public IP every ~30 seconds and it it doesn't get a response (meaning no network connectivity) it restarts OpenVPN. [This forum post](https://forum.openwrt.org/t/openvpn-regular-time-out/128016) seems to describe the issue we are currently facing.

First, we will ssh into the router:

```
$ ssh root@192.168.1.1
```

Now install `nano` to create/edit our script. Skip this is you want to use `vi` or something.

```
# opkg update
# opkg install nano
```

Now, we can create our file at `/etc/openvpn/restart_openvpn.sh and fill it with the contents as shown below:

```
# cat /etc/openvpn/restart_openvpn.sh
#!/bin/sh /etc/rc.common
n=4

logger "[restart_openvpn] starting restart_openvpn.sh"
sleep 120
while sleep 25; do
        t=$(ping -c $n 8.8.8.8 | grep -o -E '\d+ packets r' | grep -o -E '\d+')
        #logger "[restart_openvpn] done ping"
        if [ "$t" -eq 0 ]; then
                logger "[restart_openvpn] restarting openvpn service"
                /etc/init.d/openvpn restart
        fi
done
```

Next, made it executable:

```
# chmod +x /etc/openvpn/restart_openvpn.sh
```

Now, modify `/etc/rc.local` and place the line `/bin/sh /etc/openvpn/restart_openvpn.sh &` before `exit 0`:

```
# cat /etc/rc.local
# Put your custom commands here that should be executed once
# the system init finished. By default this file does nothing.
/bin/sh /etc/openvpn/restart_openvpn.sh &
exit 0
```

Finally, reboot:

```
# reboot
```

After rebooting the script should now be running. You can verify via `syslog`:

```
# logread | grep 'restart_openvpn'
Sun Nov 27 05:32:59 2022 user.notice root: [restart_openvpn] starting restart_openvpn.sh
Sun Nov 27 12:25:40 2022 user.notice root: [restart_openvpn] restarting openvpn service
```