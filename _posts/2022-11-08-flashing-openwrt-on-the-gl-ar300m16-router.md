---
layout: post
title:  "Flashing OpenWRT on the GL-AR300M16 Router"
date:   2022-11-08 20:10:00 -0400
categories: openwrt router glinet
---

The GL-AR300M16 is a low cost "travel router" manufactured by GL.iNet that replaces the GL-AR300M. The main difference here seems to be that the GL-AR300M16 lacks any NAND memory so the 16MB NOR memory must be used. Unfortunately the original model is no longer available.

This device seems to be cost-effective at around $27 USD. This is around the same price as a used Archer A7/C7, which is a great router, but the GL device is much less power-hungry and incredibly small.

{% raw %}<p><center><a href="/assets/img/2022-11-08-flashing-openwrt-on-the-gl-ar300m16-router/glar300m.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2022-11-08-flashing-openwrt-on-the-gl-ar300m16-router/glar300m.jpg"></a><figquote>The Gl-AR300M16.</figquote></center></p>{% endraw %} 

Note: This device is very different than the GL-MT300N-V2 (Mango) device even though they are offered at the same price point and seem to have an identical formfactor. Thile the GL-AR300M16 has a well supported Qualcomm Atheros QCA9531 chipset, the GL-MT300N-V2 has a MediaTek MTK7628NN chipset.

Official site: https://www.gl-inet.com/products/gl-ar300m/

## Flashing OpenWRT

We will be installing the powerful, open-source [OpenWRT](https://openwrt.org/) software on this router due to its performance, extensibility, and community support. Additionally, OpenWRT provides an identical interface regardless of underlying hardware so it would provide a seamless administrator experience even with heterogeneous hardware deployed in the field.

### Initial Flash

The AR300M16 can be flashed via the stock GL.iNet web interface. Connect an ethernet cable between your computer and the LAN jack on the router and navigate to `http://192.168.8.1` to login. Once in the web interface, use the left navigation to continue to the *Upgrade* page. Choose *Local Upgrade* and uncheck the option for *Keep Settings*. Drag/drop an OpenWRT `ramfs` image targeted for the router.

At the time of writing, https://downloads.openwrt.org/releases/21.02.3/targets/ath79/generic/openwrt-21.02.3-ath79-generic-glinet_gl-ar300m16-initramfs-kernel.bin was used. 

Finally, press the *Install* button and wait a few minutes for the firmware to be flashed. Do not unplug anything during this process.

After a few minutes you should be able to access luci (OpenWRT's web interface) through http://192.168.1.1

### Second Flash

If all goes well, we will now want to flash with a `squashfs` image so we can make our changes persistent and not boot from a ramdisk. In the OpenWRT web interface, navigate to *System* --> *Backup / Flash Firmware* and then scroll down to the *Flash new firmware image* section. Press the button for *Flash image...* and choose the squashfs image to flash with.

At the time of writing, we will use https://downloads.openwrt.org/releases/21.02.3/targets/ath79/generic/openwrt-21.02.3-ath79-generic-glinet_gl-ar300m16-squashfs-sysupgrade.bin

Follow the prompts to upload the image and then before flashing, uncheck the option for *Keep settings and retain the current configuration.* Finally, press the *Continue* button and flash the router. Again, this may take a few minutes but after some time the web interface should come back up at http://192.168.1.1

## Set root Password

Before we wrap up, be sure to set the root password. This can be done by navigating to *System* --> *Administration*. Then on the *Router Password* tab, enter a password and a confirmation before pressing the *Save* button.