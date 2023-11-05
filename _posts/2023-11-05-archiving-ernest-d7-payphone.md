---
layout: post
title: "Archiving Ernest Telecom D7 Payphone Firmware and Manual"
image: /assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-01-sm.jpg
date:   2023-11-05 0:00:01 -0400
categories: payphone software rom ernest
---

Back in September of this year, [bkobe](https://twitter.com/kobeski1906){:target="_blank"} donated an Ernest Telecom D7 mainboard, keypad, and manual which all date back to 1987!

{% raw %}<p><center><a href="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-03.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-03-sm.jpg"></a><figquote>The Ernest 7D keypad and mainboard.</figquote></center></p>{% endraw %}

Ernest was a fairly popular COCOT manufacturer back in the 1980's, selling their payphones exclusively though Amway, but has essentially faded into obscurity. There are two EPROM chips on the board housing the firmware and voice data respectively. There is also a Dallas-branded battery-backed NVSRAM chip that likely houses rate information and other custom programming. Interestingly, this phone had an on-board MOZER 54104 chip for speech synthesis which processes whole words instead of stringing fragments of speech together to create words and other sounds like other speech synthesizers commonly do. This speech is likely used for on-board menus or voice prompts.

{% raw %}<p><center><a href="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-01.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-01-sm.jpg"></a><figquote>The Ernest 7D mainboard.</figquote></center></p>{% endraw %}

We were able to dump the two ROMs and the contents of the NVSRAM chip and [upload them to the Internet Archive for safe keeping](https://archive.org/details/ernest-7d-roms){:target="_blank"}. Surprisingly, the NVSRAM chip still contained data, though we aren't sure if it is corrupted or otherwise decayed. These chips are rated for 10 years of retention without power and we don't know for sure how long the phone had been out of service. We have [scanned and uploaded the manual for the phone to the Internet Archive for safe keeping as well](https://archive.org/details/ernest7d-operations-manual){:target="_blank"}.

{% raw %}<p><center><a href="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-02.jpg"><img style="width: 80%; max-width: 600px; display: block; margin: 0 auto; border 0" src="/assets/img/2023-11-05-archiving-ernest-d7-payphone/payphone-02-sm.jpg"></a><figquote>The Ernest 7D manual.</figquote></center></p>{% endraw %}

As far as we can tell, nobody has made these ROM/NVRAM dumps or manual scans available publicly until now and we are happy to share them with the community! 

Do you have any payphone related manuals, boards, ROMs, or other software? We'll work with you to get it preserved and online, just [let us know](https://philtel.org/contact/){:target="_blank"}!